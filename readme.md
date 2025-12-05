### JobFinder Chatbot — AI Assistant with Long-Term & Short-Term Memory

This project is an AI-powered chatbot for the JobFinder platform.
It supports:

✅ Long-term memory using ChromaDB (knowledge base: product FAQ, features, docs)

✅ Short-term conversation memory using SQLite (per-user chat history)

✅ LLM responses using Groq API (Llama 3.1 models)

✅ Multi-user conversations

✅ Persistent storage on disk

-> Perfect foundation for your future startup / job search tool

🚀 Features
📚 1. Long-Term Memory (Knowledge Base)

All product documentation, FAQs, and feature descriptions are stored in ChromaDB.
This allows the bot to:

Understand JobFinder features

Answer questions based on your real product text

Avoid hallucinating

Retrieve the most relevant text chunk for each question

The knowledge base is stored locally in:

/chroma_store

💬 2. Short-Term Memory (Conversation History)

Conversation history is saved in SQLite, allowing the chatbot to remember:

Previous questions

Context of the conversation

Responses per specific user

Stored in:

chat_history.db


Each user has their own memory, separated by user_id.

🤖 3. LLM Response Generation (Groq)

The chatbot uses:

llama-3.1-70b-versatile

or llama-3.1-8b-instant

via Groq API for fast, low-latency answers.

🗂️ Project Structure


JobFinder/
│── chatbot.ipynb        # Main notebook with bot logic
│── ProductBacklog_v1.txt      # Knowledge base file (loaded into Chroma)
│── chat_history.db      # SQLite database for conversations
│── chroma_store/        # Folder created by ChromaDB for long-term memory
│── README.md            # You are reading this 🙂
│── venv/                # Python virtual environment

🔧 Installation
1️⃣ Clone repository
git clone <your-repo>
cd JobFinder

2️⃣ Create and activate virtual environment
python -m venv venv
source venv/bin/activate     # Mac/Linux
venv\Scripts\activate        # Windows

3️⃣ Install dependencies
pip install -r requirements.txt

4️⃣ Create .env file

Inside your project folder, create:

GROQ_API_KEY=your_key_here

🧠 How It Works (Simple Explanation)
1️⃣ Knowledge Base (Chroma → Long-Term Memory)

Your file product.txt is embedded and stored in Chroma:

emb = HuggingFaceEmbeddings(...)
vectorstore = Chroma.from_documents(docs, embedding=emb)
retriever = vectorstore.as_retriever(k=1)


Every time a user asks something:

relevant_docs = retriever.invoke(user_question)


This finds the most relevant piece of product information.

2️⃣ Conversation Memory (SQLite → Short-Term)

All conversation messages are stored:

save_message(user_id, "user", content)
save_message(user_id, "assistant", reply)


History is loaded using:

history_rows = load_history(user_id)


This ensures the bot remembers the flow of the conversation.

3️⃣ Building the Prompt

Each LLM call receives:

System instructions

Full conversation history

Retrieved knowledge

User’s current question

combined_input = f"Context:\n{context_text}\n\nUser question: {user_question}"
result = llm.invoke(messages)

▶️ Example Usage
print(ask_bot("user1", "Hi, what is JobFinder?"))
print(ask_bot("user1", "Do you support jobs outside the US?"))

print(ask_bot("user2", "Hello, can you help me?"))
print(ask_bot("user2", "What does JobFinder do?"))


Each user gets their own chat history.

🧱 Technologies Used
Component	Technology
LLM Provider	Groq API
Model	Llama-3.1-70b or 8b
Long-Term Memory	ChromaDB
Short-Term Memory	SQLite
Embeddings	SentenceTransformers (MiniLM)
Interface	Jupyter Notebook / Python
🧩 Future Enhancements

Web UI (FastAPI / Streamlit)

Resume analyzer using vector search

Job matching engine

Multi-document knowledge base

User authentication and dashboards

❤️ Author

Project built by Vyshnavi (JobFinder startup).
