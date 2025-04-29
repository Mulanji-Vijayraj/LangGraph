# LangGraph - Short Notes

A graph-based framework for building complex, stateful LLM workflows.

---

## 🚀 Overview

LangGraph aids users to design AI agents and workflows as **graphs**, where each node represents a computation step (like an LLM call or tool), and edges define the flow of execution.

### Key features:
- 🔄 Loops, branching, and dynamic control
- 🧠 Multi-agent orchestration
- ⚙️ Stateful memory management
- 🛠️ Built on NetworkX + LangChain

---

## 📦 Packages and Classes

Below are the core packages and classes used when working with LangGraph, along with their basic functions and syntax:

### `langgraph.graph`
- `StateGraph` – Core class for building logic graphs.
- `END` – Marks the end node of a graph.

```python
from langgraph.graph import StateGraph, END
```

---

### `langchain.chat_models`
- `ChatOpenAI` – Interface to OpenAI’s chat models (GPT-3.5, GPT-4).

```python
from langchain.chat_models import ChatOpenAI
```

---

### `langchain.tools`
- `Tool` – Define callable tools for agent use.

```python
from langchain.tools import Tool
```

---

### `langchain.agents`
- `initialize_agent` – Builds an agent with LLM and tools.
- `AgentType` – Enum for agent behavior styles.

```python
from langchain.agents import initialize_agent, AgentType
```

---

### `networkx`
- `DiGraph` – Helps visualize or debug the graph structure.

```python
import networkx as nx
```

---

## 📥 Installation

1. Download the `requirements.txt` from the repository.
2. Change the directory to the path containing the `requirements.txt`
3. Use the command:

```python
pip install -r requirements.txt
```

## Sample Code:

```python
# LangGraph Quick Example: Two-step LLM workflow with looping

from langgraph.graph import StateGraph
from langchain.chat_models import ChatOpenAI

# Define your state (simple dictionary-based state)
def add_message(state):
    state["messages"].append({"role": "user", "content": "What is LangGraph?"})
    return state

# Define an LLM step (LangChain model)
llm = ChatOpenAI(model="gpt-3.5-turbo")

def call_llm(state):
    response = llm.predict_messages(state["messages"])
    state["messages"].append(response)
    return state

# Build the graph
builder = StateGraph()

builder.add_node("add_message", add_message)
builder.add_node("llm_step", call_llm)

# Connect nodes
builder.set_entry_point("add_message")
builder.add_edge("add_message", "llm_step")

# Optional: create loop or condition
builder.add_edge("llm_step", "add_message")  # infinite loop example

# Compile and run
graph = builder.compile()
initial_state = {"messages": []}
graph.invoke(initial_state)
```

## 📝 Output:

 - This setup creates a looping agent that keeps asking the LLM the same question.
 - In practice, you'd add exit conditions, multiple nodes, or tool use.

---

# Thank you for reading!
