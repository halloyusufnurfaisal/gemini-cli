# Agent Army

## Introduction

The Gemini CLI introduces the concept of an "Agent Army," allowing users to define specialized AI personas for various development tasks. This feature enhances the CLI's capabilities by enabling tailored responses and behaviors from the Gemini model, optimizing its output for specific roles like backend development, frontend design, security analysis, or product management.

## How to Use

To utilize a specific agent, use the `--agent` (or `-ag`) flag followed by the agent's name:

```bash
gemini --agent <agent_name> -p "Your prompt here."
```

**Example:**

```bash
gemini --agent backend -p "Write a TypeScript function to fetch data from a REST API."
```

## Agent Configuration

Agents are defined in a JSON file named `agents.json`. This file contains a collection of agent definitions, each with a unique name and specific properties.

### Configuration Hierarchy

The `agents.json` file can be placed in two locations, with local configurations taking precedence over global ones:

1.  **Global Agents (Lower Priority):**
    *   Location: `~/.gemini/agents.json`
    *   Purpose: Defines your standard set of agents available across all projects.

2.  **Local Agents (Higher Priority):**
    *   Location: `<your_project_root>/.gemini/agents.json`
    *   Purpose: Allows you to override or define project-specific agents. If an agent with the same name exists globally and locally, the local definition will be used.

### Agent Definition Structure

Each agent in `agents.json` is an object with the following properties:

```json
{
  "<agent_name>": {
    "description": "A brief description of the agent's role.",
    "temperature": 0.0, // Optional: Model temperature (0.0 - 1.0)
    "persona": "The system prompt that defines the agent's personality and expertise."
  }
}
```

*   `description`: A short, human-readable explanation of the agent's purpose.
*   `temperature`: (Optional) Controls the randomness of the model's output. Lower values (e.0-0.2) are suitable for precise, factual, or code-related tasks. Higher values (e.g., 0.5-0.9) encourage more creative or diverse responses. If not specified, the CLI's default temperature (0) or any explicitly provided `--temperature` flag will be used.
*   `persona`: A crucial system prompt that instructs the Gemini model on how to behave, what role to adopt, and what kind of output to generate. This is where you define the agent's expertise and communication style.

## Example Agents

Here are some example agents you can define:

```json
{
  "backend": {
    "description": "An expert in server-side logic, databases, and APIs. Provides clean and efficient code.",
    "temperature": 0.1,
    "persona": "You are a senior backend software engineer. Your primary focus is on performance, security, and scalability. Be concise, provide code examples where relevant, and explain the trade-offs of your proposed solutions."
  },
  "frontend": {
    "description": "A specialist in UI/UX, React, and CSS. Creates intuitive and appealing interfaces.",
    "temperature": 0.2,
    "persona": "You are a frontend developer with a keen eye for design and user experience. Prioritize accessibility and reusable components. Use TypeScript and React best practices in all code examples."
  },
  "security": {
    "description": "Analyzes code for vulnerabilities and recommends security best practices.",
    "temperature": 0.0,
    "persona": "You are a cybersecurity analyst. Your task is to review code and architecture for potential vulnerabilities. Provide clear explanations of risks and offer concrete mitigation steps. Make no assumptions about security."
  },
  "product-manager": {
    "description": "Helps define features, write user stories, and analyze user needs.",
    "temperature": 0.7,
    "persona": "You are a user-focused product manager. Think about the 'why' behind every feature. Help me clarify requirements, write clear user stories, and consider the business impact of each decision."
  }
}
```

## Agent Context Awareness

It's important to note that agents, despite their specialized personas, still have full access to the project context (file structure, `GEMINI.md` content, etc.) provided by the Gemini CLI. The agent's persona acts as an additional layer of instruction, guiding how the model interprets and responds to the context and your prompts, rather than limiting its access to information.
