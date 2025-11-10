# Research Agent Setup Guide

This guide will help you set up the research agent environment.

## Prerequisites

- Poetry environment `deepagents-p1D-gHwd-py3.12` already installed
- Python 3.12

## Step 1: Activate Poetry Environment

Activate your poetry environment:

```bash
poetry shell
```

Or use poetry run for individual commands:

```bash
poetry run <command>
```

## Step 2: Navigate to Research Directory

```bash
cd examples/research
```

## Step 3: Install Dependencies

Install the required dependencies from `requirements.txt`:

```bash
poetry run pip install -r requirements.txt
```

This will install:
- `deepagents` (editable install from parent directory)
- `langgraph-cli[inmem]` (for running LangGraph agents)
- `tavily-python` (for web search functionality)

## Step 4: Create .env File

Create a `.env` file in the `examples/research/` directory with the following variables:

```bash
# Tavily API Key (required for web search)
TAVILY_API_KEY=your_tavily_api_key_here

# Anthropic API Configuration (for Claude model)
# For standard Anthropic API:
ANTHROPIC_API_KEY=your_anthropic_api_key_here
ANTHROPIC_MODEL=claude-sonnet-4-5-20250929

# For 3rd party Claude provider, add:
# ANTHROPIC_API_KEY=your_api_key
# ANTHROPIC_API_URL=https://your-custom-endpoint.com/v1
#   OR
# ANTHROPIC_BASE_URL=https://your-custom-endpoint.com/v1
# ANTHROPIC_MODEL=your-model-name
```

**Note:** The research agent has been updated to automatically load the model from these environment variables. If `ANTHROPIC_API_KEY` is set, it will use that configuration. Otherwise, it will use the default model.

### Getting API Keys

1. **Tavily API Key**: Get one from [https://www.tavily.com/](https://www.tavily.com/)
2. **Anthropic API Key**: Get one from [https://console.anthropic.com/](https://console.anthropic.com/) or your 3rd party provider

## Step 5: Verify Setup

Verify that your environment variables are loaded:

```bash
poetry run python -c "import os; from dotenv import load_dotenv; load_dotenv(); print('TAVILY_API_KEY:', 'SET' if os.getenv('TAVILY_API_KEY') else 'NOT SET'); print('ANTHROPIC_API_KEY:', 'SET' if os.getenv('ANTHROPIC_API_KEY') else 'NOT SET')"
```

## Step 6: Run the Research Agent

You can run the research agent using LangGraph CLI:

```bash
poetry run langgraph dev
```

This will start the LangGraph development server. The `langgraph.json` file is configured to:
- Load environment variables from `.env`
- Expose the research agent graph at `./research_agent.py:agent`

## Troubleshooting

### Model Configuration Issues

If you're using a 3rd party Claude provider, you may need to modify `research_agent.py` to load the model from environment variables. See the code comments in the file for instructions.

### Missing Dependencies

If you encounter import errors, make sure you've activated the poetry environment and installed all dependencies:

```bash
poetry shell
poetry run pip install -r requirements.txt
```

### Environment Variables Not Loading

Make sure your `.env` file is in the `examples/research/` directory and that `langgraph.json` has `"env": ".env"` configured (which it does by default).

