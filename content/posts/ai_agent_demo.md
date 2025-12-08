---
title: "Building AI Agents with mcp-use + fastapi + ollama"
date: 2025-12-08T04:42:23-07:00
draft: false
categories:
    - demo
tags:
    - ollama
    - fastapi
    - python
    - AI 
    - LLM 
    - MCP
---

One of the more exciting capabilities to come out of the AI craze of 2025
is [Model Context Protocol](https://en.wikipedia.org/wiki/Model_Context_Protocol),
a standardized way for LLM models to interact with software tools, such as filesystems,
databases or HTTP servers.

While getting MCP servers setup and configured with things like `Claude` and `Copilot` via `VSCode` is possible, I ultimately found this clunky, and not condusive to sharing across teams or organizations.

Enter [mcp-use](https://mcp-use.com/), a cool new tool that does the heavy lifting of standardizing how you can hook up multiple MCP servers with a given LLM model to create an AI Agent that can be deployed and utilized.

This blog post showcases how easy it is to set up a simple MCP tool wrapped around a `FastAPI` server, and connect it to a locally running LLM.

## Scenario

For this demo/tutorial, I will be making an AI Agent for a School Schedule, where the agent
can answer quetions regarding students, enrollment and classes.

## Repo

[schedule_agent](https://github.com/willemmirkovich/schedule_agent)

## Result

![Schedule Agent Chat](/img/schedule_chat.png)

## Code

While this is not all the code required to run this app, it contains the major components and logic snippets

### `docker compose`

Nice separation of the different services that are working in unison


```yml
services:

  agent: 
    build:
      dockerfile: agent.Dockerfile
    ports: 
      - '8501:8501'
    depends_on:
      - llm
      - server

  server:
    build:
      dockerfile: server.Dockerfile 
    volumes:
      - ./server:/server
    ports: 
      - '8000:8000'

  llm:
    build:
      dockerfile: llm.Dockerfile
    ports:
      - '11434:11434'
    volumes: 
      # NOTE: mount to save downloaded models locally
      - ./ollama:/root/.ollama
    entrypoint: [ "/bin/bash", "ollama_entrypoint.sh" ]
```

### Schedule MCP Tool

Setting up an MCP tool accessible to `mcp-use` is easy with `fastapi-mcp`

```python

from fastapi import FastAPI, Depends
from fastapi_mcp import FastApiMCP

app = FastAPI()


"""
app endpoints....
"""


# NOTE: mcp setup
mcp = FastApiMCP(
    app,
    describe_all_responses=True,
    describe_full_response_schema=True,
)
mcp.mount_http()
```

This makes this API accessible via `http://localhost:8000/mcp`


### Agent

The actual agent is code is pretty simple when using `mcp-use`:

```python
from langchain_ollama import ChatOllama
from mcp_use import MCPAgent, MCPClient


# NOTE: setup of MCP servers
mcp_config = {
    "mcpServers": {
        "school_schedule": {
            "url": "http://server:8000/mcp"
        }
    }
}
client = MCPClient.from_dict(mcp_config)

# NOTE: local running llm via ollama
llm = ChatOllama(
    model="llama3.1:8b",
    base_url="http://llm:11434",
    temperature=0.7,
)

agent = MCPAgent(
    llm=llm, 
    client=client,
    memory_enabled=True,
    disallowed_tools=["file_system", "network", "shell"],
    system_prompt="For all class, student and enrollment information, please use the `school_schedule` mcp server",
)
```

### Chat App

`streamlit` made it easy to setup a chat app that can send prompts from the user to the agent

```python
import asyncio
import streamlit as st

"""
agent setup...
"""


st.title("School Schedule Agent")


async def main():
    # Initialize chat history
    if "messages" not in st.session_state:
        st.session_state.messages = []

    # Display chat messages from history
    for message in st.session_state.messages:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])

    # Accept user input
    if prompt := st.chat_input("Input"):
        # Display user message in chat message container
        with st.chat_message("user"):
            st.markdown(prompt)
        
        # Add user message to chat history
        st.session_state.messages.append({"role": "user", "content": prompt})

        # Request response from local llm w/ mcp servers
        response = await agent.run(prompt)
        # Display assistant response in chat message container
        with st.chat_message("assistant"):
            st.markdown(response)
        # Add assistant response to chat history
        st.session_state.messages.append({"role": "assistant", "content": response})

if __name__ == "__main__":
    asyncio.run(main())
```

## Summary

Setting up MCP tools and connecting them to an LLM to create an AI Agent has become pretty simple. There are plenty of other tools for building MCP connections based on `openapi` specs, rolling your own, or out of the box MCP tools for popular software.

I see huge potential here for filling in gaps for internal teams that need questions regarding APIs answered quickly, whether on documentation on how to quickly set up a HTTP request for a internal tool, or MCP tools around client configuration for large scale software systems.
