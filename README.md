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

2. Powerful Dataset Profiler: BlaxML includes a dedicated CSV profiler that provides rich statistical insights, descriptive metrics, distributions, missing-value reports, outlier detection, and schema summaries. These insights give the agent the precise context it needs to choose the most effective training strategy.

3. Flexible Model Trainer: BlaxML includes a dedicated model trainer that supports various models like `random forests`, `SVMs`, and `linear models`. Agents can specify training parameters such as `task type`, `target column`, `features`, `scalers`, `outlier handling`, `missing-value strategies`, `test size`, and `random seeds`.

4. Automatic Model Deployment: BlaxML doesn’t just train models; it deploys them on demand. When the agent requests a deployment, BlaxML provisions a fresh, isolated Blaxel sandbox, transfers all model artifacts into it, launches an inference server, and creates a preview URL that is returned to the user.

5. Powerful Gradio Inference UI: BlaxML’s inference UI is fully type aware and schema driven. When a model is trained, BlaxML saves metadata for every feature's data types, valid categorical values, numeric ranges, and smart imputed defaults. This allows the deployed Gradio interface to automatically adapt; for example, if your CSV had a gender column with values male and female, the UI automatically renders a dropdown instead of a text box. Numeric fields display their observed training ranges, ideal for production use. The result is a self-adapting, intelligent inference UI with no manual setup required.

6. Optimized for Speed: You might think creating sandboxes, transferring files, profiling a CSV, training a model, and spinning up an inference server would take a lot of time, but it doesn’t. Blaxel sandboxes are lightweight, Docker-based environments that start up extremely fast, and both the profiler and trainer are built to run quickly even in non-GPU environments. The whole BlaxML workflow feels surprisingly snappy from start to finish.
