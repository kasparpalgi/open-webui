# Perroz.app

We have a dog app where we collect user's dog data. All kinds of dog's info, weight history, vet visit history, training history and progress, etc. We pass it to the Opus 4 model currently and want to also pass some other top models and then one really good model to summarise and verify and get one best answer combined. 

Also, want to feed into RAG tons of e-books and articles related dog veterinary, psychology and training and then get also one version of ansver from there. We already use AI to provide pre-defined JSON formatted learning paths.

We want nice chat UI and obviously loads of out of the box functionality. We use SvelteKit and Hasura GraphQL. We have auth there and we need info how much someone has spent on AI and we need to limit it somehow. Ollama best models and own RAG in future for cheaper AI costs (just server costs).

Want to keep things simple from the other hand and lightweight. At the moment Open WebUI seems a good choice. Need to re-search also other options before further development.

Shall be easy to deploy to Hetzner VPS via CapRover and to run/develop locally. 

Pick the best tool and provide detailed instructions to deploy to server (just gpt-5, sonnet-4, opus-4.1, gemini-pro-2.5 for now). Tell me what's needed to set up in .env and how to pass dog data and limitations info from my app to Open WebUI and what's the best method. Maybe sync somehow my Hasura and WebUI databases or make WebUI to use my Hasura/PostgreSQL or have the WebUI database as a remote schema in Hasura or pass somehow as a url parameteres data?

Install instructions: 

### Installing Open WebUI with Bundled Ollama Support

This installation method uses a single container image that bundles Open WebUI with Ollama, allowing for a streamlined setup via a single command. Choose the appropriate command based on your hardware setup:

- **With GPU Support**:
  Utilize GPU resources by running the following command:

  ```bash
  docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
  ```

- **For CPU Only**:
  If you're not using a GPU, use this command instead:

  ```bash
  docker run -d -p 3000:8080 -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:ollama
  ```

Both commands facilitate a built-in, hassle-free installation of both Open WebUI and Ollama, ensuring that you can get everything up and running swiftly.

After installation, you can access Open WebUI at [http://localhost:3000](http://localhost:3000). Enjoy! 😄

### Other Installation Methods

We offer various installation alternatives, including non-Docker native installation methods, Docker Compose, Kustomize, and Helm. Visit our [Open WebUI Documentation](https://docs.openwebui.com/getting-started/) or join our [Discord community](https://discord.gg/5rJgQTnV4s) for comprehensive guidance.

Look at the [Local Development Guide](https://docs.openwebui.com/getting-started/advanced-topics/development) for instructions on setting up a local development environment.

### Troubleshooting

Encountering connection issues? Our [Open WebUI Documentation](https://docs.openwebui.com/troubleshooting/) has got you covered. For further assistance and to join our vibrant community, visit the [Open WebUI Discord](https://discord.gg/5rJgQTnV4s).

#### Open WebUI: Server Connection Error

If you're experiencing connection issues, it’s often due to the WebUI docker container not being able to reach the Ollama server at 127.0.0.1:11434 (host.docker.internal:11434) inside the container . Use the `--network=host` flag in your docker command to resolve this. Note that the port changes from 3000 to 8080, resulting in the link: `http://localhost:8080`.

**Example Docker Command**:

```bash
docker run -d --network=host -v open-webui:/app/backend/data -e OLLAMA_BASE_URL=http://127.0.0.1:11434 --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

### Keeping Your Docker Installation Up-to-Date

In case you want to update your local Docker installation to the latest version, you can do it with [Watchtower](https://containrrr.dev/watchtower/):

```bash
docker run --rm --volume /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once open-webui
```

In the last part of the command, replace `open-webui` with your container name if it is different.

Check our Updating Guide available in our [Open WebUI Documentation](https://docs.openwebui.com/getting-started/updating).

### Using the Dev Branch 🌙

> [!WARNING]
> The `:dev` branch contains the latest unstable features and changes. Use it at your own risk as it may have bugs or incomplete features.

If you want to try out the latest bleeding-edge features and are okay with occasional instability, you can use the `:dev` tag like this:

```bash
docker run -d -p 3000:8080 -v open-webui:/app/backend/data --name open-webui --add-host=host.docker.internal:host-gateway --restart always ghcr.io/open-webui/open-webui:dev
```

### Offline Mode

If you are running Open WebUI in an offline environment, you can set the `HF_HUB_OFFLINE` environment variable to `1` to prevent attempts to download models from the internet.

```bash
export HF_HUB_OFFLINE=1
```
--

For local development maybe easiest to install with Pinokio.computer Installation

For installation via Pinokio.computer, please visit their website:

https://pinokio.computer/

Support for this installation method is provided through their website.

--

Provide simple instructions for Hetzner Caprover installation.

---

 Starting With Ollama

Overview

Open WebUI makes it easy to connect and manage your Ollama instance. This guide will walk you through setting up the connection, managing models, and getting started.

Step 1: Setting Up the Ollama Connection

Once Open WebUI is installed and running, it will automatically attempt to connect to your Ollama instance. If everything goes smoothly, you’ll be ready to manage and use models right away.

However, if you encounter connection issues, the most common cause is a network misconfiguration. You can refer to our connection troubleshooting guide for help resolving these problems.

Step 2: Managing Your Ollama Instance

To manage your Ollama instance in Open WebUI, follow these steps:

Go to Admin Settings in Open WebUI.
Navigate to Connections > Ollama > Manage (click the wrench icon).
From here, you can download models, configure settings, and manage your connection to Ollama.
Here’s what the management screen looks like:

Ollama Management Screen

Ollama Management Screen

A Quick and Efficient Way to Download Models

If you’re looking for a faster option to get started, you can download models directly from the Model Selector. Simply type the name of the model you want, and if it’s not already available, Open WebUI will prompt you to download it from Ollama.

Here’s an example of how it works:

Ollama Download Prompt

This method is perfect if you want to skip navigating through the Admin Settings menu and get right to using your models.

All Set!

That’s it! Once your connection is configured and your models are downloaded, you’re ready to start using Ollama with Open WebUI. Whether you’re exploring new models or running your existing ones, Open WebUI makes everything simple and efficient.

If you run into any issues or need more guidance, check out our help section for detailed solutions. Enjoy using Ollama! 🎉


Starting with Llama.cpp

Overview

Open WebUI makes it simple and flexible to connect and manage a local Llama.cpp server to run efficient, quantized language models. Whether you’ve compiled Llama.cpp yourself or you're using precompiled binaries, this guide will walk you through how to:

Set up your Llama.cpp server
Load large models locally
Integrate with Open WebUI for a seamless interface
Let’s get you started!

Step 1: Install Llama.cpp

To run models with Llama.cpp, you first need the Llama.cpp server installed locally.

You can either:

📦 Download prebuilt binaries
🛠️ Or build it from source by following the official build instructions
After installing, make sure llama-server is available in your local system path or take note of its location.

Step 2: Download a Supported Model

You can load and run various GGUF-format quantized LLMs using Llama.cpp. One impressive example is the DeepSeek-R1 1.58-bit model optimized by UnslothAI. To download this version:

Visit the Unsloth DeepSeek-R1 repository on Hugging Face
Download the 1.58-bit quantized version – around 131GB.
Alternatively, use Python to download programmatically:

# pip install huggingface_hub hf_transfer

from huggingface_hub import snapshot_download

snapshot_download(
    repo_id = "unsloth/DeepSeek-R1-GGUF",
    local_dir = "DeepSeek-R1-GGUF",
    allow_patterns = ["*UD-IQ1_S*"],  # Download only 1.58-bit variant
)


This will download the model files into a directory like:

DeepSeek-R1-GGUF/
└── DeepSeek-R1-UD-IQ1_S/
    ├── DeepSeek-R1-UD-IQ1_S-00001-of-00003.gguf
    ├── DeepSeek-R1-UD-IQ1_S-00002-of-00003.gguf
    └── DeepSeek-R1-UD-IQ1_S-00003-of-00003.gguf

📍 Keep track of the full path to the first GGUF file — you’ll need it in Step 3.

Step 3: Serve the Model with Llama.cpp

Start the model server using the llama-server binary. Navigate to your llama.cpp folder (e.g., build/bin) and run:

./llama-server \
  --model /your/full/path/to/DeepSeek-R1-UD-IQ1_S-00001-of-00003.gguf \
  --port 10000 \
  --ctx-size 1024 \
  --n-gpu-layers 40


🛠️ Tweak the parameters to suit your machine:

--model: Path to your .gguf model file
--port: 10000 (or choose another open port)
--ctx-size: Token context length (can increase if RAM allows)
--n-gpu-layers: Layers offloaded to GPU for faster performance
Once the server runs, it will expose a local OpenAI-compatible API on:

http://127.0.0.1:10000

Step 4: Connect Llama.cpp to Open WebUI

To control and query your locally running model directly from Open WebUI:

Open Open WebUI in your browser
Go to ⚙️ Admin Settings → Connections → OpenAI Connections
Click ➕ Add Connection and enter:
URL: http://127.0.0.1:10000/v1
(Or use http://host.docker.internal:10000/v1 if running WebUI inside Docker)
API Key: none (leave blank)
💡 Once saved, Open WebUI will begin using your local Llama.cpp server as a backend!

Llama.cpp Connection in Open WebUI

Quick Tip: Try Out the Model via Chat Interface

Once connected, select the model from the Open WebUI chat menu and start interacting!

Starting with OpenAI-Compatible Servers

Overview

Open WebUI isn't just for OpenAI/Ollama/Llama.cpp—you can connect any server that implements the OpenAI-compatible API, running locally or remotely. This is perfect if you want to run different language models, or if you already have a favorite backend or ecosystem. This guide will show you how to:

Set up an OpenAI-compatible server (with a few popular options)
Connect it to Open WebUI
Start chatting right away
Step 1: Choose an OpenAI-Compatible Server

There are many servers and tools that expose an OpenAI-compatible API. Here are some of the most popular:

Llama.cpp: Extremely efficient, runs on CPU and GPU
Ollama: Super user-friendly and cross-platform
LM Studio: Rich desktop app for Windows/Mac/Linux
Lemonade: Fast ONNX-based backend with NPU/iGPU acceleration
Pick whichever suits your workflow!

🍋 Get Started with Lemonade

Lemonade is a plug-and-play ONNX-based OpenAI-compatible server. Here’s how to try it on Windows:

Download the latest .exe

Run Lemonade_Server_Installer.exe

Install and download a model using Lemonade’s installer

Once running, your API endpoint will be:

http://localhost:8000/api/v0

Lemonade Server

See their docs for details.

Step 2: Connect Your Server to Open WebUI

Open Open WebUI in your browser.

Go to ⚙️ Admin Settings → Connections → OpenAI Connections.

Click ➕ Add Connection.

URL: Use your server’s API endpoint (for example, http://localhost:11434/v1 for Ollama, or your own Llama.cpp server’s address).
API Key: Leave blank unless required.
Click Save.

Tip: If running Open WebUI in Docker and your model server on your host machine, use http://host.docker.internal:<your-port>/v1.

For Lemonade: When adding Lemonade, use http://localhost:8000/api/v0 as the URL.

Lemonade Connection

Step 3: Start Chatting!

Select your connected server’s model in the chat menu and get started!

That’s it! Whether you choose Llama.cpp, Ollama, LM Studio, or Lemonade, you can easily experiment and manage multiple model servers—all in Open WebUI.

Getting Started with Functions

Overview

Did you know Open WebUI can connect to almost anything—not just OpenAI-compatible APIs?

Thanks to Pipe Functions, you can bring in services that don’t support the OpenAI API (like Anthropic, Home Assistant, Google Search, or any Python codebase). No restrictions on LLMs or AI models: if you can automate it in Python, you can turn it into a plugin for Open WebUI!

This guide walks you through setting up your first Pipe Function, using the Anthropic Pipe plugin as an example.

What are Pipe Functions?

Pipe Functions are “bring-your-own-model (or tool)” plugins:

Act like models: They show up as selectable models in your Open WebUI sidebar.
Flexible: Integrate with any backend, API, or workflow—no OpenAI compatibility required.
No LLM required: You can build plugins for search, home automation, weather, databases, or whatever you like.
Pure Python: All logic is Python code that runs directly inside your WebUI (so be cautious with what you enable!).
Step 1: Find a Pipe Function to Try

Let’s integrate Anthropic with Open WebUI—even though Anthropic only supports its own native API (not OpenAI-compatible endpoints)!

Go to the Anthropic Chat function page.

Click Get.

Anthropic Pipe Function Page

Step 2: Import the Function to Open WebUI

A modal will appear:

Enter your Open WebUI URL (e.g., http://localhost:3000) in the prompt.
Click Import to Open WebUI.
Import Modal Screenshot

You’ll be redirected directly to the Functions Editor within your running instance of Open WebUI.

Step 3: Review & Save

You’ll see all of the Pipe Function’s Python code and configuration.
Important: Functions run arbitrary Python! Review the code for safety, and only install from sources you trust.
If you’re happy with the code, click Save to add it to your instance.
Function Editor Screenshot

Step 4: Enable the Function

Your new Pipe Function is now available, but must be enabled:

Switch the toggler to enable the function.
Enable Function Screenshot

Step 5: Enter any Required API Keys via Valves

Some functions need credentials (like Anthropic’s API key):

Click on the Gear icon next to the switch to open the Valves configuration.

Input your required API key(s) for the Pipe Function.

Valves/API Key Screenshot

Step 6: Start Using Your New Plugin!

The new function now appears as a selectable “model” in the chat interface.
Select Anthropic (or whatever you installed), and start chatting!
Select Pipe Function as Model Screenshot

🎉 That’s It—You’re Plugged Into Anything!

Pipe Functions open Open WebUI to any API, model, or automation—not just OpenAI-compatible endpoints.
Think beyond LLMs: Integrate tools, APIs, local scripts, or your entire smart home.
⚠️ Security Notes

Always review function code before enabling.
Only use plugins from trusted sources.
You have the power to enhance (or break!) your WebUI—use responsibly.

 Pipe Function: Create Custom "Agents/Models"

Welcome to this guide on creating Pipes in Open WebUI! Think of Pipes as a way to adding a new model to Open WebUI. In this document, we'll break down what a Pipe is, how it works, and how you can create your own to add custom logic and processing to your Open WebUI models. We'll use clear metaphors and go through every detail to ensure you have a comprehensive understanding.

Introduction to Pipes

Imagine Open WebUI as a plumbing system where data flows through pipes and valves. In this analogy:

Pipes are like plugins that let you introduce new pathways for data to flow, allowing you to inject custom logic and processing.
Valves are the configurable parts of your pipe that control how data flows through it.
By creating a Pipe, you're essentially crafting a custom model with the specific behavior you want, all within the Open WebUI framework.

Understanding the Pipe Structure

Let's start with a basic, barebones version of a Pipe to understand its structure:

from pydantic import BaseModel, Field

class Pipe:
    class Valves(BaseModel):
        MODEL_ID: str = Field(default="")

    def __init__(self):
        self.valves = self.Valves()

    def pipe(self, body: dict):
        # Logic goes here
        print(self.valves, body)  # This will print the configuration options and the input body
        return "Hello, World!"


The Pipe Class

Definition: The Pipe class is where you define your custom logic.
Purpose: Acts as the blueprint for your plugin, determining how it behaves within Open WebUI.
Valves: Configuring Your Pipe

Definition: Valves is a nested class within Pipe, inheriting from BaseModel.
Purpose: It contains the configuration options (parameters) that persist across the use of your Pipe.
Example: In the above code, MODEL_ID is a configuration option with a default empty string.
Metaphor: Think of Valves as the knobs on a real-world pipe system that control the flow of water. In your Pipe, Valves allow users to adjust settings that influence how the data flows and is processed.

The __init__ Method

Definition: The constructor method for the Pipe class.
Purpose: Initializes the Pipe's state and sets up any necessary components.
Best Practice: Keep it simple; primarily initialize self.valves here.
def __init__(self):
    self.valves = self.Valves()

The pipe Function

Definition: The core function where your custom logic resides.
Parameters:
body: A dictionary containing the input data.
Purpose: Processes the input data using your custom logic and returns the result.
def pipe(self, body: dict):
    # Logic goes here
    print(self.valves, body)  # This will print the configuration options and the input body
    return "Hello, World!"


Note: Always place Valves at the top of your Pipe class, followed by __init__, and then the pipe function. This structure ensures clarity and consistency.

Creating Multiple Models with Pipes

What if you want your Pipe to create multiple models within Open WebUI? You can achieve this by defining a pipes function or variable inside your Pipe class. This setup, informally called a manifold, allows your Pipe to represent multiple models.

Here's how you can do it:

from pydantic import BaseModel, Field

class Pipe:
    class Valves(BaseModel):
        MODEL_ID: str = Field(default="")

    def __init__(self):
        self.valves = self.Valves()

    def pipes(self):
        return [
            {"id": "model_id_1", "name": "model_1"},
            {"id": "model_id_2", "name": "model_2"},
            {"id": "model_id_3", "name": "model_3"},
        ]

    def pipe(self, body: dict):
        # Logic goes here
        print(self.valves, body)  # Prints the configuration options and the input body
        model = body.get("model", "")
        return f"{model}: Hello, World!"


Explanation

pipes Function:

Returns a list of dictionaries.
Each dictionary represents a model with unique id and name keys.
These models will show up individually in the Open WebUI model selector.
Updated pipe Function:

Processes input based on the selected model.
In this example, it includes the model name in the returned string.
Example: OpenAI Proxy Pipe

Let's dive into a practical example where we'll create a Pipe that proxies requests to the OpenAI API. This Pipe will fetch available models from OpenAI and allow users to interact with them through Open WebUI.

from pydantic import BaseModel, Field
import requests

class Pipe:
    class Valves(BaseModel):
        NAME_PREFIX: str = Field(
            default="OPENAI/",
            description="Prefix to be added before model names.",
        )
        OPENAI_API_BASE_URL: str = Field(
            default="https://api.openai.com/v1",
            description="Base URL for accessing OpenAI API endpoints.",
        )
        OPENAI_API_KEY: str = Field(
            default="",
            description="API key for authenticating requests to the OpenAI API.",
        )

    def __init__(self):
        self.valves = self.Valves()

    def pipes(self):
        if self.valves.OPENAI_API_KEY:
            try:
                headers = {
                    "Authorization": f"Bearer {self.valves.OPENAI_API_KEY}",
                    "Content-Type": "application/json",
                }

                r = requests.get(
                    f"{self.valves.OPENAI_API_BASE_URL}/models", headers=headers
                )
                models = r.json()
                return [
                    {
                        "id": model["id"],
                        "name": f'{self.valves.NAME_PREFIX}{model.get("name", model["id"])}',
                    }
                    for model in models["data"]
                    if "gpt" in model["id"]
                ]

            except Exception as e:
                return [
                    {
                        "id": "error",
                        "name": "Error fetching models. Please check your API Key.",
                    },
                ]
        else:
            return [
                {
                    "id": "error",
                    "name": "API Key not provided.",
                },
            ]

    def pipe(self, body: dict, __user__: dict):
        print(f"pipe:{__name__}")
        headers = {
            "Authorization": f"Bearer {self.valves.OPENAI_API_KEY}",
            "Content-Type": "application/json",
        }

        # Extract model id from the model name
        model_id = body["model"][body["model"].find(".") + 1 :]

        # Update the model id in the body
        payload = {**body, "model": model_id}
        try:
            r = requests.post(
                url=f"{self.valves.OPENAI_API_BASE_URL}/chat/completions",
                json=payload,
                headers=headers,
                stream=True,
            )

            r.raise_for_status()

            if body.get("stream", False):
                return r.iter_lines()
            else:
                return r.json()
        except Exception as e:
            return f"Error: {e}"


Detailed Breakdown

Valves Configuration

NAME_PREFIX:
Adds a prefix to the model names displayed in Open WebUI.
Default: "OPENAI/".
OPENAI_API_BASE_URL:
Specifies the base URL for the OpenAI API.
Default: "https://api.openai.com/v1".
OPENAI_API_KEY:
Your OpenAI API key for authentication.
Default: "" (empty string; must be provided).
The pipes Function

Purpose: Fetches available OpenAI models and makes them accessible in Open WebUI.

Process:

Check for API Key: Ensures that an API key is provided.
Fetch Models: Makes a GET request to the OpenAI API to retrieve available models.
Filter Models: Returns models that have "gpt" in their id.
Error Handling: If there's an issue, returns an error message.
Return Format: A list of dictionaries with id and name for each model.

The pipe Function

Purpose: Handles the request to the selected OpenAI model and returns the response.

Parameters:

body: Contains the request data.
__user__: Contains user information (not used in this example but can be useful for authentication or logging).
Process:

Prepare Headers: Sets up the headers with the API key and content type.
Extract Model ID: Extracts the actual model ID from the selected model name.
Prepare Payload: Updates the body with the correct model ID.
Make API Request: Sends a POST request to the OpenAI API's chat completions endpoint.
Handle Streaming: If stream is True, returns an iterable of lines.
Error Handling: Catches exceptions and returns an error message.
Extending the Proxy Pipe

You can modify this proxy Pipe to support additional service providers like Anthropic, Perplexity, and more by adjusting the API endpoints, headers, and logic within the pipes and pipe functions.

Using Internal Open WebUI Functions

Sometimes, you may want to leverage the internal functions of Open WebUI within your Pipe. You can import these functions directly from the open_webui package. Keep in mind that while unlikely, internal functions may change for optimization purposes, so always refer to the latest documentation.

Here's how you can use internal Open WebUI functions:

from pydantic import BaseModel, Field
from fastapi import Request

from open_webui.models.users import Users
from open_webui.utils.chat import generate_chat_completion

class Pipe:
    def __init__(self):
        pass

    async def pipe(
        self,
        body: dict,
        __user__: dict,
        __request__: Request,
    ) -> str:
        # Use the unified endpoint with the updated signature
        user = Users.get_user_by_id(__user__["id"])
        body["model"] = "llama3.2:latest"
        return await generate_chat_completion(__request__, body, user)


Explanation

Imports:

Users from open_webui.models.users: To fetch user information.
generate_chat_completion from open_webui.utils.chat: To generate chat completions using internal logic.
Asynchronous pipe Function:

Parameters:
body: Input data for the model.
__user__: Dictionary containing user information.
__request__: The request object from FastAPI (required by generate_chat_completion).
Process:
Fetch User Object: Retrieves the user object using their ID.
Set Model: Specifies the model to be used.
Generate Completion: Calls generate_chat_completion to process the input and produce an output.
Important Notes

Function Signatures: Refer to the latest Open WebUI codebase or documentation for the most accurate function signatures and parameters.
Best Practices: Always handle exceptions and errors gracefully to ensure a smooth user experience.
Frequently Asked Questions

Q1: Why should I use Pipes in Open WebUI?

A: Pipes allow you to add new "model" with custom logic and processing to Open WebUI. It's a flexible plugin system that lets you integrate external APIs, customize model behaviors, and create innovative features without altering the core codebase.

Q2: What are Valves, and why are they important?

A: Valves are the configurable parameters of your Pipe. They function like settings or controls that determine how your Pipe operates. By adjusting Valves, you can change the behavior of your Pipe without modifying the underlying code.

Q3: Can I create a Pipe without Valves?

A: Yes, you can create a simple Pipe without defining a Valves class if your Pipe doesn't require any persistent configuration options. However, including Valves is a good practice for flexibility and future scalability.

Q4: How do I ensure my Pipe is secure when using API keys?

A: Never hard-code sensitive information like API keys into your Pipe. Instead, use Valves to input and store API keys securely. Ensure that your code handles these keys appropriately and avoids logging or exposing them.

Q5: What is the difference between the pipe and pipes functions?

A:

pipe Function: The primary function where you process the input data and generate an output. It handles the logic for a single model.

pipes Function: Allows your Pipe to represent multiple models by returning a list of model definitions. Each model will appear individually in Open WebUI.

Q6: How can I handle errors in my Pipe?

A: Use try-except blocks within your pipe and pipes functions to catch exceptions. Return meaningful error messages or handle the errors gracefully to ensure the user is informed about what went wrong.

Q7: Can I use external libraries in my Pipe?

A: Yes, you can import and use external libraries as needed. Ensure that any dependencies are properly installed and managed within your environment.

Q8: How do I test my Pipe?

A: Test your Pipe by running Open WebUI in a development environment and selecting your custom model from the interface. Validate that your Pipe behaves as expected with various inputs and configurations.

Q9: Are there any best practices for organizing my Pipe's code?

A: Yes, follow these guidelines:

Keep Valves at the top of your Pipe class.
Initialize variables in the __init__ method, primarily self.valves.
Place the pipe function after the __init__ method.
Use clear and descriptive variable names.
Comment your code for clarity.
Q10: Where can I find the latest Open WebUI documentation?

A: Visit the official Open WebUI repository or documentation site for the most up-to-date information, including function signatures, examples, and migration guides if any changes occur.


Filter Function: Modify Inputs and Outputs

Welcome to the comprehensive guide on Filter Functions in Open WebUI! Filters are a flexible and powerful plugin system for modifying data before it's sent to the Large Language Model (LLM) (input) or after it’s returned from the LLM (output). Whether you’re transforming inputs for better context or cleaning up outputs for improved readability, Filter Functions let you do it all.

This guide will break down what Filters are, how they work, their structure, and everything you need to know to build powerful and user-friendly filters of your own. Let’s dig in, and don’t worry—I’ll use metaphors, examples, and tips to make everything crystal clear! 🌟

🌊 What Are Filters in Open WebUI?

Imagine Open WebUI as a stream of water flowing through pipes:

User inputs and LLM outputs are the water.
Filters are the water treatment stages that clean, modify, and adapt the water before it reaches the final destination.
Filters sit in the middle of the flow—like checkpoints—where you decide what needs to be adjusted.

Here’s a quick summary of what Filters do:

Modify User Inputs (Inlet Function): Tweak the input data before it reaches the AI model. This is where you enhance clarity, add context, sanitize text, or reformat messages to match specific requirements.
Intercept Model Outputs (Stream Function): Capture and adjust the AI’s responses as they’re generated by the model. This is useful for real-time modifications, like filtering out sensitive information or formatting the output for better readability.
Modify Model Outputs (Outlet Function): Adjust the AI's response after it’s processed, before showing it to the user. This can help refine, log, or adapt the data for a cleaner user experience.
Key Concept: Filters are not standalone models but tools that enhance or transform the data traveling to and from models.
Filters are like translators or editors in the AI workflow: you can intercept and change the conversation without interrupting the flow.

🗺️ Structure of a Filter Function: The Skeleton

Let's start with the simplest representation of a Filter Function. Don't worry if some parts feel technical at first—we’ll break it all down step by step!

🦴 Basic Skeleton of a Filter

from pydantic import BaseModel
from typing import Optional

class Filter:
    # Valves: Configuration options for the filter
    class Valves(BaseModel):  
        pass

    def __init__(self):
        # Initialize valves (optional configuration for the Filter)
        self.valves = self.Valves()

    def inlet(self, body: dict) -> dict:
        # This is where you manipulate user inputs.
        print(f"inlet called: {body}")
        return body  

    def stream(self, event: dict) -> dict:
        # This is where you modify streamed chunks of model output.
        print(f"stream event: {event}")
        return event

    def outlet(self, body: dict) -> None:
        # This is where you manipulate model outputs.
        print(f"outlet called: {body}")

🆕 🧲 Toggle Filter Example: Adding Interactivity and Icons (New in Open WebUI 0.6.10)

Filters can do more than simply modify text—they can expose UI toggles and display custom icons. For instance, you might want a filter that can be turned on/off with a user interface button, and displays a special icon in Open WebUI’s message input UI.

Here’s how you could create such a toggle filter:

from pydantic import BaseModel, Field
from typing import Optional

class Filter:
    class Valves(BaseModel):
        pass

    def __init__(self):
        self.valves = self.Valves()
        self.toggle = True # IMPORTANT: This creates a switch UI in Open WebUI
        # TIP: Use SVG Data URI!
        self.icon = """data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIGZpbGw9Im5vbmUiIHZpZXdCb3g9IjAgMCAyNCAyNCIgc3Ryb2tlLXdpZHRoPSIxLjUiIHN0cm9rZT0iY3VycmVudENvbG9yIiBjbGFzcz0ic2l6ZS02Ij4KICA8cGF0aCBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIGQ9Ik0xMiAxOHYtNS4yNW0wIDBhNi4wMSA2LjAxIDAgMCAwIDEuNS0uMTg5bS0xLjUuMTg5YTYuMDEgNi4wMSAwIDAgMS0xLjUtLjE4OW0zLjc1IDcuNDc4YTEyLjA2IDEyLjA2IDAgMCAxLTQuNSAwbTMuNzUgMi4zODNhMTQuNDA2IDE0LjQwNiAwIDAgMS0zIDBNMTQuMjUgMTh2LS4xOTJjMC0uOTgzLjY1OC0xLjgyMyAxLjUwOC0yLjMxNmE3LjUgNy41IDAgMSAwLTcuNTE3IDBjLjg1LjQ5MyAxLjUwOSAxLjMzMyAxLjUwOSAyLjMxNlYxOCIgLz4KPC9zdmc+Cg=="""
        pass

    async def inlet(
        self, body: dict, __event_emitter__, __user__: Optional[dict] = None
    ) -> dict:
        await __event_emitter__(
            {
                "type": "status",
                "data": {
                    "description": "Toggled!",
                    "done": True,
                    "hidden": False,
                },
            }
        )
        return body


🖼️ What’s happening?

toggle = True creates a switch UI in Open WebUI—users can manually enable or disable the filter in real time.
icon (with a Data URI) will show up as a little image next to the filter’s name. You can use any SVG as long as it’s Data URI encoded!
The inlet function uses the __event_emitter__ special argument to broadcast feedback/status to the UI, such as a little toast/notification that reads "Toggled!"
Toggle Filter

You can use these mechanisms to make your filters dynamic, interactive, and visually unique within Open WebUI’s plugin ecosystem.

🎯 Key Components Explained

1️⃣ Valves Class (Optional Settings)

Think of Valves as the knobs and sliders for your filter. If you want to give users configurable options to adjust your Filter’s behavior, you define those here.

class Valves(BaseModel):
    OPTION_NAME: str = "Default Value"

For example:
If you're creating a filter that converts responses into uppercase, you might allow users to configure whether every output gets totally capitalized via a valve like TRANSFORM_UPPERCASE: bool = True/False.

Configuring Valves with Dropdown Menus (Enums)

You can enhance the user experience for your filter's settings by providing dropdown menus instead of free-form text inputs for certain Valves. This is achieved using json_schema_extra with the enum keyword in your Pydantic Field definitions.

The enum keyword allows you to specify a list of predefined values that the UI should present as options in a dropdown.

Example: Creating a dropdown for color themes in a filter.

from pydantic import BaseModel, Field
from typing import Optional

# Define your available options (e.g., color themes)
COLOR_THEMES = {
    "Plain (No Color)": [],
    "Monochromatic Blue": ["blue", "RoyalBlue", "SteelBlue", "LightSteelBlue"],
    "Warm & Energetic": ["orange", "red", "magenta", "DarkOrange"],
    "Cool & Calm": ["cyan", "blue", "green", "Teal", "CadetBlue"],
    "Forest & Earth": ["green", "DarkGreen", "LimeGreen", "OliveGreen"],
    "Mystical Purple": ["purple", "DarkOrchid", "MediumPurple", "Lavender"],
    "Grayscale": ["gray", "DarkGray", "LightGray"],
    "Rainbow Fun": [
        "red",
        "orange",
        "yellow",
        "green",
        "blue",
        "indigo",
        "violet",
    ],
    "Ocean Breeze": ["blue", "cyan", "LightCyan", "DarkTurquoise"],
    "Sunset Glow": ["DarkRed", "DarkOrange", "Orange", "gold"],
    "Custom Sequence (See Code)": [],
}

class Filter:
    class Valves(BaseModel):
        selected_theme: str = Field(
            "Monochromatic Blue",
            description="Choose a predefined color theme for LLM responses. 'Plain (No Color)' disables coloring.",
            json_schema_extra={"enum": list(COLOR_THEMES.keys())}, # KEY: This creates the dropdown
        )
        custom_colors_csv: str = Field(
            "",
            description="CSV of colors for 'Custom Sequence' theme (e.g., 'red,blue,green'). Uses xcolor names.",
        )
        strip_existing_latex: bool = Field(
            True,
            description="If true, attempts to remove existing LaTeX color commands. Recommended to avoid nested rendering issues.",
        )
        colorize_type: str = Field(
            "sequential_word",
            description="How to apply colors: 'sequential_word' (word by word), 'sequential_line' (line by line), 'per_letter' (letter by letter), 'full_message' (entire message).",
            json_schema_extra={
                "enum": [
                    "sequential_word",
                    "sequential_line",
                    "per_letter",
                    "full_message",
                ]
            }, # Another example of an enum dropdown
        )
        color_cycle_reset_per_message: bool = Field(
            True,
            description="If true, the color sequence restarts for each new LLM response message. If false, it continues across messages.",
        )
        debug_logging: bool = Field(
            False,
            description="Enable verbose logging to the console for debugging filter operations.",
        )

    def __init__(self):
        self.valves = self.Valves()
        # ... rest of your __init__ logic ...


What's happening?

json_schema_extra: This argument in Field allows you to inject arbitrary JSON Schema properties that Pydantic doesn't explicitly support but can be used by downstream tools (like Open WebUI's UI renderer).
"enum": list(COLOR_THEMES.keys()): This tells Open WebUI that the selected_theme field should present a selection of values, specifically the keys from our COLOR_THEMES dictionary. The UI will then render a dropdown menu with "Plain (No Color)", "Monochromatic Blue", "Warm & Energetic", etc., as selectable options.
The colorize_type field also demonstrates another enum dropdown for different coloring methods.
Using enum for your Valves options makes your filters more user-friendly and prevents invalid inputs, leading to a smoother configuration experience.

2️⃣ inlet Function (Input Pre-Processing)

The inlet function is like prepping food before cooking. Imagine you’re a chef: before the ingredients go into the recipe (the LLM in this case), you might wash vegetables, chop onions, or season the meat. Without this step, your final dish could lack flavor, have unwashed produce, or simply be inconsistent.

In the world of Open WebUI, the inlet function does this important prep work on the user input before it’s sent to the model. It ensures the input is as clean, contextual, and helpful as possible for the AI to handle.

📥 Input:

body: The raw input from Open WebUI to the model. It is in the format of a chat-completion request (usually a dictionary that includes fields like the conversation's messages, model settings, and other metadata). Think of this as your recipe ingredients.
🚀 Your Task:
Modify and return the body. The modified version of the body is what the LLM works with, so this is your chance to bring clarity, structure, and context to the input.

🍳 Why Would You Use the inlet?

Adding Context: Automatically append crucial information to the user’s input, especially if their text is vague or incomplete. For example, you might add "You are a friendly assistant" or "Help this user troubleshoot a software bug."

Formatting Data: If the input requires a specific format, like JSON or Markdown, you can transform it before sending it to the model.

Sanitizing Input: Remove unwanted characters, strip potentially harmful or confusing symbols (like excessive whitespace or emojis), or replace sensitive information.

Streamlining User Input: If your model’s output improves with additional guidance, you can use the inlet to inject clarifying instructions automatically!

💡 Example Use Cases: Build on Food Prep

🥗 Example 1: Adding System Context

Let’s say the LLM is a chef preparing a dish for Italian cuisine, but the user hasn’t mentioned "This is for Italian cooking." You can ensure the message is clear by appending this context before sending the data to the model.

def inlet(self, body: dict, __user__: Optional[dict] = None) -> dict:
    # Add system message for Italian context in the conversation
    context_message = {
        "role": "system",
        "content": "You are helping the user prepare an Italian meal."
    }
    # Insert the context at the beginning of the chat history
    body.setdefault("messages", []).insert(0, context_message)
    return body


📖 What Happens?

Any user input like "What are some good dinner ideas?" now carries the Italian theme because we’ve set the system context! Cheesecake might not show up as an answer, but pasta sure will.
🔪 Example 2: Cleaning Input (Remove Odd Characters)

Suppose the input from the user looks messy or includes unwanted symbols like !!!, making the conversation inefficient or harder for the model to parse. You can clean it up while preserving the core content.

def inlet(self, body: dict, __user__: Optional[dict] = None) -> dict:
    # Clean the last user input (from the end of the 'messages' list)
    last_message = body["messages"][-1]["content"]
    body["messages"][-1]["content"] = last_message.replace("!!!", "").strip()
    return body


📖 What Happens?

Before: "How can I debug this issue!!!" ➡️ Sent to the model as "How can I debug this issue"
Note: The user feels the same, but the model processes a cleaner and easier-to-understand query.

📊 How inlet Helps Optimize Input for the LLM:

Improves accuracy by clarifying ambiguous queries.
Makes the AI more efficient by removing unnecessary noise like emojis, HTML tags, or extra punctuation.
Ensures consistency by formatting user input to match the model’s expected patterns or schemas (like, say, JSON for a specific use case).
💭 Think of inlet as the sous-chef in your kitchen—ensuring everything that goes into the model (your AI "recipe") has been prepped, cleaned, and seasoned to perfection. The better the input, the better the output!

🆕 3️⃣ stream Hook (New in Open WebUI 0.5.17)

🔄 What is the stream Hook?

The stream function is a new feature introduced in Open WebUI 0.5.17 that allows you to intercept and modify streamed model responses in real time.

Unlike outlet, which processes an entire completed response, stream operates on individual chunks as they are received from the model.

🛠️ When to Use the Stream Hook?

Modify streaming responses before they are displayed to users.
Implement real-time censorship or cleanup.
Monitor streamed data for logging/debugging.
📜 Example: Logging Streaming Chunks

Here’s how you can inspect and modify streamed LLM responses:

def stream(self, event: dict) -> dict:
    print(event)  # Print each incoming chunk for inspection
    return event

Example Streamed Events:
{"id": "chatcmpl-B4l99MMaP3QLGU5uV7BaBM0eDS0jb","choices": [{"delta": {"content": "Hi"}}]}
{"id": "chatcmpl-B4l99MMaP3QLGU5uV7BaBM0eDS0jb","choices": [{"delta": {"content": "!"}}]}
{"id": "chatcmpl-B4l99MMaP3QLGU5uV7BaBM0eDS0jb","choices": [{"delta": {"content": " 😊"}}]}


📖 What Happens?

Each line represents a small fragment of the model's streamed response.
The delta.content field contains the progressively generated text.
🔄 Example: Filtering Out Emojis from Streamed Data

def stream(self, event: dict) -> dict:
    for choice in event.get("choices", []):
        delta = choice.get("delta", {})
        if "content" in delta:
            delta["content"] = delta["content"].replace("😊", "")  # Strip emojis
    return event


📖 Before: "Hi 😊"
📖 After: "Hi"

4️⃣ outlet Function (Output Post-Processing)

The outlet function is like a proofreader: tidy up the AI's response (or make final changes) after it’s processed by the LLM.

📤 Input:

body: This contains all current messages in the chat (user history + LLM replies).
🚀 Your Task: Modify this body. You can clean, append, or log changes, but be mindful of how each adjustment impacts the user experience.

💡 Best Practices:

Prefer logging over direct edits in the outlet (e.g., for debugging or analytics).
If heavy modifications are needed (like formatting outputs), consider using the pipe function instead.
💡 Example Use Case: Strip out sensitive API responses you don't want the user to see:

def outlet(self, body: dict, __user__: Optional[dict] = None) -> dict:
    for message in body["messages"]:
        message["content"] = message["content"].replace("<API_KEY>", "[REDACTED]")
    return body 


🌟 Filters in Action: Building Practical Examples

Let’s build some real-world examples to see how you’d use Filters!

📚 Example #1: Add Context to Every User Input

Want the LLM to always know it's assisting a customer in troubleshooting software bugs? You can add instructions like "You're a software troubleshooting assistant" to every user query.

class Filter:
    def inlet(self, body: dict, __user__: Optional[dict] = None) -> dict:
        context_message = {
            "role": "system", 
            "content": "You're a software troubleshooting assistant."
        }
        body.setdefault("messages", []).insert(0, context_message)
        return body


📚 Example #2: Highlight Outputs for Easy Reading

Returning output in Markdown or another formatted style? Use the outlet function!

class Filter:
    def outlet(self, body: dict, __user__: Optional[dict] = None) -> dict:
        # Add "highlight" markdown for every response
        for message in body["messages"]:
            if message["role"] == "assistant":  # Target model response
                message["content"] = f"**{message['content']}**"  # Highlight with Markdown
        return body


🚧 Potential Confusion: Clear FAQ 🛑

Q: How Are Filters Different From Pipe Functions?

Filters modify data going to and coming from models but do not significantly interact with logic outside of these phases. Pipes, on the other hand:

Can integrate external APIs or significantly transform how the backend handles operations.
Expose custom logic as entirely new "models."
Q: Can I Do Heavy Post-Processing Inside outlet?

You can, but it’s not the best practice.:

Filters are designed to make lightweight changes or apply logging.
If heavy modifications are required, consider a Pipe Function instead.
🎉 Recap: Why Build Filter Functions?

By now, you’ve learned:

Inlet manipulates user inputs (pre-processing).
Stream intercepts and modifies streamed model outputs (real-time).
Outlet tweaks AI outputs (post-processing).
Filters are best for lightweight, real-time alterations to the data flow.
With Valves, you empower users to configure Filters dynamically for tailored behavior.
🚀 Your Turn: Start experimenting! What small tweak or context addition could elevate your Open WebUI experience? Filters are fun to build, flexible to use, and can take your models to the next level!

Happy coding! ✨