<div
  style="
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
  "
>
  <div style="display: flex; align-items: center; gap: 15px">
    <img
      src="https://github.com/aniketppanchal/blax-ml-mcp/raw/refs/heads/main/assets/logo.png"
      width="200px"
    />
    <div>
      <h1 style="font-size: 40px; margin: 0">BlaxML MCP</h1>
      <span style="font-size: 20px">
        Autonomous Machine Learning MCP server powered by
        <a
          href="https://blaxel.ai"
          target="_blank"
          style="color: #ff8b3d; text-decoration: none"
        >
          Blaxel
        </a>
      </span>
    </div>
  </div>
</div>

# 1. Demo Video

<video width="50%" controls>
  <source
    src="https://github.com/aniketppanchal/blax-ml-mcp/raw/refs/heads/main/py-mcp/demo.mp4"
    type="video/mp4"
  />
  Your browser does not support the video tag.
</video>

# 2. Social Media Post

[LinkedIn Post](https://www.linkedin.com/posts/aniketppanchal_blaxml-mcp-a-hugging-face-space-by-mcp-activity-7401009033425600514-5ND_)

# 3. What is 🏀 BlaxML MCP Server?

`BlaxML MCP Server` = `Blaxel` + `Autonomous Machine Learning` + `MCP Server`

The BlaxML MCP Server is a fully autonomous machine learning system built on top of Blaxel's sandboxing infrastructure, enabling AI agents to transform raw CSV datasets into completely trained and deployed machine learning models. And it doesn’t stop there; every deployed model comes with a built-in interactive Gradio GUI and a REST API endpoint, making inference accessible for both humans and applications.

# 4. Key Features

1. Automated ML Workflow: BlaxML handles everything except the agent’s reasoning. Once the agent decides what needs to be done, BlaxML takes over the entire workflow from profiling the CSV, training the model, managing sandboxes, and deploying the model automatically. Each deployed model runs inside its own isolated Blaxel sandbox, ensuring strong security and clean separation.

2. Powerful Dataset Profiler: BlaxML includes a dedicated CSV profiler that provides rich statistical insights, distributions, missing-value reports, outlier detection reports, and schema summaries. These insights give the agent the precise context it needs to choose the most effective training strategy.

3. Flexible Model Trainer: BlaxML includes a dedicated model trainer that supports various models like `Random Forest`, `SVM`, and `Linear` for both regression and classification. Agents define the task, target, and optional feature set, while the trainer applies the specified preprocessing, dataset split, and model-specific settings to produce a reproducible training run.

4. Automatic Model Deployment: BlaxML doesn’t just train models; it deploys them on demand. When the agent requests a deployment, BlaxML provisions a fresh, isolated Blaxel sandbox, transfers all model artifacts into it, launches an inference server, and creates a preview URL that is returned to the user.

5. Powerful Gradio Inference UI: BlaxML’s inference UI is fully type aware and schema driven. When a model is trained, BlaxML saves metadata for every feature's data types, valid categorical values, numeric ranges, and smart imputed defaults. This allows the deployed Gradio interface to automatically adapt; for example, if your CSV had a gender column with values male and female, the UI automatically renders a dropdown instead of a text box. Numeric fields display their observed training ranges, ideal for production use. The result is a self-adapting, intelligent inference UI with no manual setup required.

6. Optimized for Speed: You might think creating sandboxes, transferring files, profiling a CSV, training a model, and spinning up an inference server would take a lot of time, but it doesn’t. Blaxel sandboxes are lightweight, Docker-based environments that start up extremely fast, and both the profiler and trainer are built to run quickly even in non-GPU environments. The whole BlaxML workflow feels surprisingly snappy from start to finish.

# 5. Architecture

## 5.1 Components of BlaxML

BlaxML is built around three core components, each responsible for a specific stage in machine learning lifecycle:

1. CSV Profiler: Profiles the dataset and generates rich statistical insights.

2. Model Trainer: Trains machine learning models based on agent-provided configuration.

3. Inference Server: Serves the trained model with an intelligent, schema-aware UI and REST inference endpoint.

## 5.2 Sandbox Images

BlaxML follows a two-sandbox architecture. Before understanding how the workflow operates, it’s important to know what these sandbox images contain and how they are used:

1. BlaxML Model Trainer Image: This image contains both the CSV profiler and model trainer. It runs inside a single shared training sandbox that is reused across all user workflows. This sandbox handles:
   - CSV profiling
   - Model training
   - Metadata generation
   - Transfer of artifacts for deployment

2. BlaxML Model Server Image: This image contains the inference server. Unlike the training sandbox, the model server runs inside a brand-new sandbox created per deployment. Each deployed model gets its own isolated server sandbox that includes:
   - Trained model artifacts
   - Dynamic Gradio UI

# 5.3 Architecture Summary

At a high level:

- The AI agent drives the logic of choosing features, interpreting the dataset profile, deciding how to train, and deciding when to deploy.

- The BlaxML MCP Server exposes tools `generate_request_id`, `upload_and_profile_csv`, `train_model` and `deploy_model` that the agent calls to execute each step.

- The Model Trainer Sandbox (a shared, persistent Blaxel sandbox) handles all heavy processing of CSV profiling, model training, metadata extraction, and artifact preparation.

- The Model Server Sandbox (a fresh sandbox created per deployment) hosts the final trained model along with a schema-aware Gradio UI and a REST inference endpoint.

- Every workflow is isolated using a unique request ID, ensuring clean separation of different training jobs.

> Agent thinks. BlaxML does the work. Blaxel runs the work safely and fast.

# 6. Workflow

1. Agent Requests a New Workflow: The agent calls `generate_request_id`, and BlaxML prepares a fresh workspace inside the shared training sandbox.

2. Agent Uploads the CSV: The agent calls `upload_and_profile_csv`, sending the dataset to the training sandbox.
   BlaxML validates the file and stores it under `/blaxel/<request_id>/dataset.csv`.

3. BlaxML Profiles the Dataset: The CSV profiler runs inside the training sandbox and produces a detailed `csv_profile.json`. This report gives the agent everything it needs: column summaries, types, distributions, missing values, and more.

4. Agent Decides Training Parameters: Using the profiler’s insights, the agent chooses the best configuration (task type, features, target, scalers, etc.) and calls `train_model`.

5. BlaxML Trains the Model: The model trainer loads the CSV, applies preprocessing, trains the model, saves the artifacts, and generates metadata for the inference UI.

6. Agent Requests Deployment: The agent calls `deploy_model`, and BlaxML spins up a brand-new model server sandbox, isolated per deployment.

7. BlaxML Deploys the Model: The trained model and metadata are moved into the new server sandbox, the Gradio inference server launches, and a public preview URL is returned.

8. User Runs Inference: The deployed model becomes instantly usable via:
   - A schema-aware Gradio web UI, or
   - A RREST inference endpoint. both powered by the isolated model server sandbox.

# 7. BlaxML Integration with Claude and LangChain

### 7.1. Claude Desktop (Requires npx)

Add/update your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "py-mcp": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp-1st-birthday-blaxml-mcp.hf.space/gradio_api/mcp"
      ]
    }
  }
}
```

### 7.2 LangChain

```bash
pip install langchain langchain-mcp-adapters langchain-openai
```

```python
import asyncio

from langchain.agents import create_agent
from langchain_mcp_adapters.client import MultiServerMCPClient


async def main() -> None:
    client = MultiServerMCPClient(
        {
            "py-mcp": {
                "transport": "streamable_http",
                "url": "https://mcp-1st-birthday-blaxml-mcp.hf.space/gradio_api/mcp",
            },
        }
    )

    tools = await client.get_tools()
    agent = create_agent(
        model="openai:gpt-5.1",
        tools=tools,
    )

    query = "Please profile this CSV: https://example.com/dataset.csv"
    response = await agent.ainvoke({"messages": query})
    print(response["messages"][-1].content)

if __name__ == "__main__":
    asyncio.run(main())
```

# 8. Acknowledgements

- [Blaxel](https://blaxel.ai/): A massive thank you for providing the Agentic Infrastructure and Sandboxes. Their serverless environment allows this MCP server to spin up isolated docker containers in seconds, making the "Autonomous" part of this project possible.

- [Anthropic](https://www.anthropic.com/): For developing the Model Context Protocol (MCP) open standard, which is rapidly changing how AI agents interact with the world.

- [Gradio](https://www.gradio.app/): For the robust UI framework and the streamable_http transport layer that makes serving this MCP over the web seamless.

- [Hugging Face](https://huggingface.co/): For providing the Spaces infrastructure to host the demo.

- The Hackathon Organizers: For bringing the community together to celebrate the 1st birthday of MCP!

# 9. License

- [MIT License](https://huggingface.co/spaces/MCP-1st-Birthday/BlaxML-MCP/blob/main/LICENSE)

# 10. Contact

Created by Aniket Panchal

- 📧 Email: [aniket.prakash.panchal@gmail.com](mailto:aniket.prakash.panchal@gmail.com)
- 💼 LinkedIn: [linkedin.com/in/aniketppanchal](https://www.linkedin.com/in/aniketppanchal)
