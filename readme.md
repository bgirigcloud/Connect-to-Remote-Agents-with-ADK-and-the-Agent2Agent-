Enable another ADK agent to call this agent remotely
In this task, you will provide a second ADK agent the ability to identify your illustration agent's capabilities and call it remotely. This second agent will be an agent tasked with creating contents for slides. It will write a headline and a couple of sentences of body text, then transfer to the illustration agent to generate an image to illustrate that text.

In the Cloud Shell Terminal, run the following command to copy the Agent Card JSON file to your adk_and_a2a directory and change its name to indicate that it represents the illustration_agent.

cp illustration_agent/agent.json illustration-agent-card.json
Copied!
In the Cloud Shell Editor's file explorer pane, navigate to the adk_and_a2a/slide_content_agent and open the agent.py file.

Review this agent's instruction to see it will take a user's suggestion for a slide and write a headline & body text, then transfer to your A2A agent to illustrate the slide.

Paste the following code under the # Agents header to add the remote agent using the RemoteA2aAgent class from ADK:

illustration_agent = RemoteA2aAgent(
    name="illustration_agent",
    description="Agent that generates illustrations.",
    agent_card=(
        "illustration-agent-card.json"
    ),
)
Copied!
Add the illustration_agent as a sub-agent of the root_agent by adding the following parameter to the root_agent:

sub_agents=[illustration_agent]
Copied!
Save the file.

Launch the UI from the Cloud Shell Terminal with:

cd ~/adk_and_a2a
adk web
