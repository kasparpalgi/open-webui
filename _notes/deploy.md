Persistent Volumes
Add these paths under "Volumes":

/app/backend/data → open-webui-data
/app/rag → dog-knowledge-base # For RAG documents

Add Open WebUI to Hasura as remote schema:
yaml
# In hasura/metadata/remote_schemas.yaml
- name: openwebui
  url: http://perroz-webui:8080/graphql
  comment: Open WebUI GraphQL API
Query dog data in Pipe Functions:
python
# In Open WebUI Pipe Function
import requests

def get_dog_data(dog_id: str, user_id: str):
    query = f"""
    query GetDogData {{
      dogs(where: {{id: {{_eq: "{dog_id}"}}, owner: {{id: {{_eq: "{user_id}"}}}}) {{
        name
        weight_history
        vet_visits
      }}
    }}
    """
    response = requests.post(
        os.environ["HASURA_GRAPHQL_URL"],
        json={"query": query},
        headers={"x-hasura-admin-secret": os.environ["HASURA_ADMIN_SECRET"]}
    )
    return response.json()["data"]["dogs"][0]
Option 2: URL Parameter Injection

Launch chat with contextual data:
text
https://webui.perroz.app?dog_id=123&user_id=456


Spending Limit Implementation

1. Create Usage Tracker Table in Hasura

sql
CREATE TABLE ai_usage (
  user_id UUID REFERENCES users(id),
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  model VARCHAR(50),
  input_tokens INT,
  output_tokens INT,
  cost NUMERIC(10,4)
);
2. Pipe Function for Spending Control

python
# cost_monitor.py
class Pipe:
    class Valves(BaseModel):
        MAX_MONTHLY_SPEND: float = 50.00

    def pipe(self, body: dict):
        user_id = body["user_id"]
        month = datetime.now().strftime("%Y-%m")
        
        # Query monthly spend
        query = f"""
        SELECT SUM(cost) AS total 
        FROM ai_usage 
        WHERE user_id = '{user_id}' 
          AND to_char(timestamp, 'YYYY-MM') = '{month}'
        """
        result = run_hasura_query(query)
        current_spend = result["data"]["ai_usage_aggregate"]["sum"]["cost"] or 0

        if current_spend > self.valves.MAX_MONTHLY_SPEND:
            return {
                "error": "Budget exceeded",
                "message": f"${current_spend} spent of ${self.valves.MAX_MONTHLY_SPEND} limit"
            }
        
        return body
3. Attach to All Models
In Open WebUI Admin:

Go to Functions → Cost Monitor → Enable
Set priority to "Pre-Processing"
RAG Setup for Dog Knowledge

Add Documents:
Place e-books in /app/rag volume (PDFs, EPUBs)
Supported formats: PDF, DOCX, TXT, EPUB
Configure in UI:
text
Settings → RAG → 
  Collection Name: dog_encyclopedia
  Chunk Size: 512
  Embedding Model: all-MiniLM-L12-v2
Query in Chats:
markdown
/attach dog_encyclopedia
How do I handle separation anxiety in terriers?
Model Routing Strategy

1. Create Model Router Pipe

python
# model_router.py
MODEL_MAP = {
    "medical": "gpt-4-turbo",
    "behavior": "claude-3-opus",
    "training": "gemini-1.5-pro",
    "general": "mixtral-8x7b"
}

def pipe(self, body: dict):
    prompt = body["messages"][-1]["content"]
    
    # Classify query type
    classifier = llm(f"""
    Categorize this dog query:
    {prompt}
    
    Options: medical, behavior, training, general
    """)
    
    # Select best model
    body["model"] = MODEL_MAP.get(classifier.lower(), "gpt-4-turbo")
    return body
2. Verification System

python
# response_verifier.py
def pipe(self, body: dict):
    responses = get_multiple_responses(body["messages"])
    
    # Get consensus
    verified = llm(f"""
    Compare these responses about dog care:
    {responses}
    
    Return ONLY the most accurate response. Flag uncertainties.
    """)
    
    return {"messages": [{"role": "assistant", "content": verified}]}
Development Setup

.env Configuration

env
# Open WebUI
OPENAI_API_KEY=sk-xxx
ANTHROPIC_API_KEY=claude-xxx
GOOGLE_API_KEY=gemini-xxx
OLLAMA_BASE_URL=http://localhost:11434

# Hasura
HASURA_GRAPHQL_URL=http://localhost:8080/v1/graphql
HASURA_ADMIN_SECRET=your_secret

# Business Rules
MAX_MONTHLY_SPEND=50
DOG_DATA_ENDPOINT=https://api.perroz.app/graphql
Local Run Command

bash
docker run -d \
  -p 3000:8080 \
  --env-file .env \
  -v ./rag:/app/rag \
  -v ./data:/app/backend/data \
  --name perroz-webui \
  ghcr.io/open-webui/open-webui:main
Cost Optimization Path

Phase 1: Use cloud models (OpenAI/Anthropic) for core features
Phase 2: Add Ollama with:
bash
docker run -d -gpu all -p 11434:11434 ollama/ollama
ollama pull mixtral:8x7b-instruct-q4_K_M
Phase 3: Hybrid routing:
Simple queries → Local Ollama
Complex cases → Cloud models

Component	Integration Method
User Auth	JWT via Hasura webhooks
Dog Data	GraphQL queries in Pipe Functions
Spending Limits	Pre-processing Pipe + Hasura
Knowledge Base	Built-in RAG with PDF indexing
Training Plans	JSON output via function calling

For production monitoring, add:

bash
docker run -d \
  -p 3001:3000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name open-webui-monitor \
  containrrr/watchtower