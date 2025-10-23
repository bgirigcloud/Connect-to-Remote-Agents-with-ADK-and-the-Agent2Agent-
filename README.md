# Connect to Remote Agents with ADK and the Agent2Agent (A2A) SDK

This project demonstrates how to connect two agents using the Agent Development Kit (ADK) and the Agent2Agent (A2A) SDK. The project consists of two agents: a `slide_content_agent` and an `illustration_agent`.

## Project Overview

The `slide_content_agent` is the main agent that a user interacts with. It's designed to take an idea from a user, create a headline and body text for a presentation slide, and then delegate the task of creating an accompanying illustration to the `illustration_agent`.

The `illustration_agent` is a remote agent that receives a text prompt from the `slide_content_agent`. It uses a generative AI model to create an image based on the prompt, uploads the image to a Google Cloud Storage bucket, and returns a public URL for the image.

This project showcases a simple, yet powerful, multi-agent workflow where one agent orchestrates a task by delegating a specialized sub-task to another agent.

## Agents

### Slide Content Agent

-   **Role:** Main agent for user interaction.
-   **Functionality:**
    1.  Receives a topic or idea from the user.
    2.  Generates a concise headline and body text for a slide.
    3.  Invokes the `illustration_agent` to create a relevant image.
-   **File:** `slide_content_agent/agent.py`

### Illustration Agent

-   **Role:** Specialized agent for generating images.
-   **Functionality:**
    1.  Receives a text prompt for an illustration.
    2.  Uses a generative AI model to create an image.
    3.  Uploads the generated image to Google Cloud Storage.
    4.  Returns the public URL of the image.
-   **Files:** `illustration_agent/agent.py`, `illustration_agent/agent.json`

## How it Works

1.  The user provides a prompt to the `slide_content_agent`.
2.  The `slide_content_agent` processes the prompt and generates a headline and body text.
3.  The `slide_content_agent` then calls the `illustration_agent` using the A2A SDK, passing a new prompt for an image.
4.  The `illustration_agent` generates the image and returns the URL to the `slide_content_agent`.
5.  The `slide_content_agent` can then present the complete slide content (text and image URL) to the user.

## Setup and Installation

To run this project, you will need to set up both agents.

### Prerequisites

-   Python 3.10 or later
-   Google Cloud SDK
-   A Google Cloud Project with the Vertex AI API enabled.
-   A Google Cloud Storage bucket.

### 1. Clone the repository

```bash
git clone https://github.com/your-username/adk_and_a2a.git
cd adk_and_a2a
```

### 2. Configure Environment Variables

Each agent has a `.env` file that you need to configure.

**`illustration_agent/.env`**

```
GOOGLE_CLOUD_PROJECT="your-gcp-project-id"
GOOGLE_CLOUD_LOCATION="your-gcp-region"
MODEL="gemini-1.0-pro"
IMAGE_MODEL="gemini-1.0-pro-vision"
```

**`slide_content_agent/.env`**

```
MODEL="gemini-1.0-pro"
```

### 3. Install Dependencies

Install the required Python libraries for each agent.

**For the `illustration_agent`:**

```bash
pip install -r illustration_agent/requirements.txt
```

**For the `slide_content_agent`:**

The `slide_content_agent` uses the `google-adk` and `a2a-sdk` libraries. Make sure you have them installed.

```bash
pip install google-adk a2a-sdk
```

### 4. Run the Agents

You will need to run both agents. The `illustration_agent` needs to be deployed to a public endpoint so that the `slide_content_agent` can communicate with it. The `illustration-agent-card.json` contains a placeholder URL that you will need to replace with your deployed agent's URL.

**Deploy the `illustration_agent`:**

You can deploy the `illustration_agent` as a Cloud Function or a Cloud Run service. Once deployed, update the `url` field in `illustration-agent-card.json` with the public URL of your deployed agent.

**Run the `slide_content_agent`:**

You can run the `slide_content_agent` locally.

```bash
# Navigate to the slide_content_agent directory
cd slide_content_agent

# Run the agent (the specific command will depend on how you have set up your agent to run)
python agent.py
```

## Usage

Once both agents are running, you can interact with the `slide_content_agent`. For example, you could give it a prompt like:

"Create a slide about the importance of teamwork in a fast-paced environment."

The agent will then generate the text content for the slide and an illustration to go with it.