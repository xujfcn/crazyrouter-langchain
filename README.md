# 🦜 LangChain + Crazyrouter Setup Guide

> Use LangChain with available AI models through Crazyrouter.

[Crazyrouter](https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community) — One API key for compatible models.

## Current prices

[Current model prices](https://crazyrouter.com/en/pricing). Rates depend on the model, account group and billing unit; there is no universal discount. Check input, output, cache and task charges before use.

## ⚡ Quick Start

### Installation
```bash
pip install langchain langchain-openai langchain-community faiss-cpu
```

### Basic Chat
```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="claude-sonnet-4-6",
    api_key="sk-your-crazyrouter-key",
    base_url="https://crazyrouter.com/v1"
)

response = llm.invoke("What is an API gateway?")
print(response.content)
```

### Streaming
```python
for chunk in llm.stream("Explain microservices architecture"):
    print(chunk.content, end="", flush=True)
```

### Chain Example
```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful coding assistant."),
    ("user", "{input}")
])

chain = prompt | llm
response = chain.invoke({"input": "Write a Python function to parse JSON"})
print(response.content)
```

### RAG Pipeline
```python
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import FAISS

embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    api_key="sk-your-crazyrouter-key",
    base_url="https://crazyrouter.com/v1"
)
vectorstore = FAISS.from_texts(["Our support hours are Monday to Friday, 09:00–17:00 UTC."], embeddings)
question = "When is support available?"
docs = vectorstore.similarity_search(question, k=1)
context = "\n".join(doc.page_content for doc in docs)
print(llm.invoke(f"Answer using this context: {context}\nQuestion: {question}").content)
```

### Agent with a local tool (LangChain 1.x)
```python
from langchain.agents import create_agent
from langchain_core.tools import tool

@tool
def support_hours() -> str:
    """Return the support team's operating hours."""
    return "Monday to Friday, 09:00–17:00 UTC"

agent = create_agent(model=llm, tools=[support_hours], system_prompt="Use the tool for support hours.")
result = agent.invoke({"messages": [{"role": "user", "content": "When is support available?"}]})
print(result["messages"][-1].content)
```

## 🎯 Switch Models Easily

```python
# GPT-4o
gpt = ChatOpenAI(model="gpt-4o", api_key="sk-key", base_url="https://crazyrouter.com/v1")

# Claude Opus
claude = ChatOpenAI(model="claude-opus-4-8", api_key="sk-key", base_url="https://crazyrouter.com/v1")

# Gemini
gemini = ChatOpenAI(model="gemini-2.5-pro", api_key="sk-key", base_url="https://crazyrouter.com/v1")

# Same key, same base_url, different models!
```

## 🔧 LangChain.js (TypeScript)

```typescript
import { ChatOpenAI } from "@langchain/openai";

const llm = new ChatOpenAI({
  model: "claude-sonnet-4-6",
  apiKey: "sk-your-crazyrouter-key",
  configuration: { baseURL: "https://crazyrouter.com/v1" }
});

const response = await llm.invoke("Hello!");
```

## ❓ FAQ

**Q: Does LangChain work with Crazyrouter?**
A: Yes, use `langchain-openai` with Crazyrouter base URL.

**Q: Do agents and tools work?**
A: Select a model and channel supporting tool calling, and validate your tool workflow before deployment.

**Q: Can I use embeddings?**
A: Yes, `text-embedding-3-small` and other embedding models are available.

## 🔗 Links
- 🌐 [Crazyrouter](https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community)
- 🦜 [LangChain](https://github.com/langchain-ai/langchain)
- 💬 [Telegram](https://t.me/crzrouter)

## 📄 License
MIT
