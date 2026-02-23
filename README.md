# 🦜 LangChain + Crazyrouter

> Use LangChain with 300+ AI models through Crazyrouter. Save 45%.

## ⚡ Python Setup
```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="claude-sonnet-4-20250514",
    api_key="sk-your-crazyrouter-key",
    base_url="https://crazyrouter.com/v1"
)
response = llm.invoke("What is an API gateway?")
```

## 🔗 [Crazyrouter](https://crazyrouter.com?ref=github) | [Telegram](https://t.me/crzrouter)
## 📄 License: MIT
