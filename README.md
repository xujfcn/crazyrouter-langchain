# 🦜 LangChain + Crazyrouter Setup Guide

> Use LangChain with 300+ AI models through Crazyrouter — Save 45% on API costs.

[Crazyrouter](https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community?ref=github) — One API key for all AI models.

## 💰 Price Comparison

| Model | Official (In/Out per 1M tokens) | Crazyrouter | Savings |
|-------|--------------------------------|-------------|---------|
| Claude Opus 4 | $15 / $75 | $8.25 / $41.25 | **45%** |
| GPT-4o | $2.50 / $10 | $1.38 / $5.50 | **45%** |
| Gemini 2.5 Pro | $1.25 / $10 | $0.69 / $5.50 | **45%** |

## ⚡ Quick Start

### Installation
```bash
pip install langchain langchain-openai
```

### Basic Chat
```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="claude-sonnet-4-20250514",
    api_key="sk-your-crazyrouter-key",
    base_url="https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1"
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
from langchain.chains import RetrievalQA

# Embeddings via Crazyrouter
embeddings = OpenAIEmbeddings(
    model="text-embedding-3-small",
    api_key="sk-your-crazyrouter-key",
    base_url="https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1"
)

# Create vector store
vectorstore = FAISS.from_texts(["your documents here"], embeddings)

# RAG chain
qa = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=vectorstore.as_retriever()
)
result = qa.invoke("What does the document say about pricing?")
```

### Agent with Tools
```python
from langchain.agents import create_openai_tools_agent, AgentExecutor
from langchain_core.tools import tool

@tool
def search_web(query: str) -> str:
    """Search the web for information."""
    return f"Results for: {query}"

agent = create_openai_tools_agent(llm, [search_web], prompt)
executor = AgentExecutor(agent=agent, tools=[search_web])
result = executor.invoke({"input": "Search for AI API pricing"})
```

## 🎯 Switch Models Easily

```python
# GPT-4o
gpt = ChatOpenAI(model="gpt-4o", api_key="sk-key", base_url="https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1")

# Claude Opus
claude = ChatOpenAI(model="claude-opus-4-20250514", api_key="sk-key", base_url="https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1")

# Gemini
gemini = ChatOpenAI(model="gemini-2.5-pro", api_key="sk-key", base_url="https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1")

# Same key, same base_url, different models!
```

## 🔧 LangChain.js (TypeScript)

```typescript
import { ChatOpenAI } from "@langchain/openai";

const llm = new ChatOpenAI({
  modelName: "claude-sonnet-4-20250514",
  openAIApiKey: "sk-your-crazyrouter-key",
  configuration: { baseURL: "https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community/v1" }
});

const response = await llm.invoke("Hello!");
```

## ❓ FAQ

**Q: Does LangChain work with Crazyrouter?**
A: Yes, use `langchain-openai` with Crazyrouter base URL.

**Q: Do agents and tools work?**
A: Yes, function calling and tool use work perfectly.

**Q: Can I use embeddings?**
A: Yes, `text-embedding-3-small` and other embedding models are available.

## 🔗 Links
- 🌐 [Crazyrouter](https://crazyrouter.com?utm_source=github&utm_medium=github&utm_campaign=dev_community?ref=github)
- 🦜 [LangChain](https://github.com/langchain-ai/langchain)
- 💬 [Telegram](https://t.me/crzrouter)

## 📄 License
MIT

