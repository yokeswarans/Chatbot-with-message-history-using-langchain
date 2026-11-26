# Chatbot with Message History using LangChain

An LLM-powered chatbot that **remembers the conversation**, built step by step in a Jupyter notebook with **LangChain** and **Groq**. It covers session-based memory, prompt templates and trimming the history so it fits the model's context window.

![Chatbot with Message History infographic](assets/chatbot-infographic.png)

## 💡 The key idea

LLMs are **stateless**: they don't remember anything between calls. To build a chatbot, the previous messages have to be sent again with every new question. LangChain's `RunnableWithMessageHistory` stores the history for each session and sends it to the model automatically.

## 📚 What the notebook covers

| # | Topic | Main LangChain pieces |
|---|---|---|
| 1 | Load the API key from `.env` | `load_dotenv()` |
| 2 | Connect to the LLM | `ChatGroq(model="openai/gpt-oss-20b")` |
| 3 | Chat with messages, passing history by hand | `HumanMessage`, `AIMessage` |
| 4 | Automatic message history with sessions | `ChatMessageHistory`, `RunnableWithMessageHistory`, `session_id` |
| 5 | Prompt templates | `ChatPromptTemplate`, `MessagesPlaceholder`, `prompt \| model` |
| 6 | Extra prompt variables | `{language}` in the system prompt |
| 7 | Memory with multiple input keys | `input_messages_key="messages"` |
| 8 | Trimming the conversation history | `trim_messages`, `RunnablePassthrough.assign`, `itemgetter` |

## 🔄 How one chat turn works

1. **User message** comes in with a `session_id`.
2. **Load history**: `RunnableWithMessageHistory` fetches the past messages for that session from the store.
3. **Trim**: `trim_messages` keeps only the newest messages that fit in the token budget (`max_tokens=45`, `strategy="last"`), and always keeps the system message.
4. **Build prompt**: `ChatPromptTemplate` combines the system instructions, `{language}` and the chat messages.
5. **Generate reply**: `ChatGroq` returns an `AIMessage`.
6. **Save back**: the question and the reply are added to the session history for the next turn.

Each `session_id` has its own history, so different chats never share memory.

## 🛠️ Tech stack

- Python
- [LangChain](https://python.langchain.com/) (`langchain-core`, `langchain-community`)
- [Groq](https://groq.com/) via `langchain-groq`
- `python-dotenv`
- Jupyter Notebook

## 🚀 Getting started

1. **Clone the repo**

   ```bash
   git clone <your-repo-url>
   cd Chatbot-with-message-history-using-langchain
   ```

2. **Install the packages**

   ```bash
   pip install langchain-core langchain-community langchain-groq python-dotenv ipykernel
   ```

3. **Add your Groq API key.** Create a `.env` file in the project folder:

   ```env
   GROQ_API_KEY=your_groq_api_key_here
   ```

   You can get a free key from the [Groq Console](https://console.groq.com/keys). `.env` is already listed in `.gitignore`, so the key won't be committed.

4. **Run the notebook.** Open `chatbots.ipynb` in Jupyter or VS Code and run the cells from top to bottom.

## 📁 Project structure

```
├── chatbots.ipynb              # Main notebook with explanations and comments
├── assets/
│   └── chatbot-infographic.png # Infographic used in this README
├── .env                        # Your GROQ_API_KEY (not committed)
├── .gitignore
└── README.md
```
