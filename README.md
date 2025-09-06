#  APPLE AI-Financial Assistant (n8n + OpenAI + Pinecone)

An end-to-end **AI chat assistant** that answers Apple-related finance questions using
OpenAI for reasoning, **Pinecone** for vector search, and **OneDrive** as a document source—
all orchestrated in **n8n**.

- **Chat workflow**: public chat, AI Agent, OpenAI chat model, short-term memory, and Pinecone retrieval as a tool. :contentReference[oaicite:0]{index=0}
- **Ingestion workflow**: OneDrive **Search → Download**, chunk/encode, and upsert to Pinecone (namespace `apple-data`). :contentReference[oaicite:1]{index=1}
- **Sample data**: Apple FY23 Q4 Consolidated Financial Statements (PDF) for grounding. :contentReference[oaicite:2]{index=2}

---

##  Features

- **Chat UI Trigger**: public chat endpoint preloaded with a friendly system greeting. :contentReference[oaicite:3]{index=3}  
- **AI Agent with Tools**: uses Pinecone as a retrieval tool (namespace `apple-data`) to ground answers. :contentReference[oaicite:4]{index=4}  
- **OpenAI Models**: GPT-4o-mini for chat; OpenAI embeddings for indexing & search.   
- **Vector Store**: Pinecone index `ai-agent2-d1536`, namespace `apple-data`.   
- **OneDrive Pipeline**: robust **Search → Download** pattern using the returned `id` (no manual URL parsing). :contentReference[oaicite:7]{index=7}

---

##  Repository Structure

/workflows
├── APPLE AI-Agent Chat.json
└── APPLE AI-Agent Vector database flow.json
/data
└── FY23_Q4_Consolidated_Financial_Statements.pdf
/docs
├── chat-screenshot.png
├── agent-graph.png
└── ingestion-graph.png
README.md
LICENSE

markdown
Copy code

> Replace the images with your screenshots (the chat view and both workflow graphs).

---

##  Quick Start

1. **Import workflows**
   - In n8n, **Import**:
     - `workflows/APPLE AI-Agent Chat.json` (chat agent) :contentReference[oaicite:8]{index=8}
     - `workflows/APPLE AI-Agent Vector database flow.json` (ingestion) :contentReference[oaicite:9]{index=9}

2. **Configure credentials**
   - **OpenAI API** (chat + embeddings)   
   - **Pinecone** (index: `ai-agent2-d1536`, namespace: `apple-data`)   
   - **Microsoft OneDrive OAuth2** (read access to your documents) :contentReference[oaicite:12]{index=12}

3. **Load documents (ingestion workflow)**
   - Open **“APPLE AI-Agent Vector database flow”**.
   - Node **Search a file**: query your PDF name (e.g., `FY23_Q4_Consolidated_Financial_Statements.pdf`).  
   - Node **Download a file**: `File ID = {{ $json.id }}` (wired from Search). :contentReference[oaicite:13]{index=13}
   - Run the workflow to upsert chunks/embeddings into Pinecone. :contentReference[oaicite:14]{index=14}

4. **Start chatting (chat workflow)**
   - Open **“APPLE AI-Agent Chat”**, click **Open chat**, ask questions like:
     - “Summarize Apple’s FY23 Q4 net sales and margins.”
     - “How did Services perform year-over-year?”  
   - Answers are grounded in your indexed PDF. :contentReference[oaicite:15]{index=15}

---

##  How It Works

**Chat path**  
`Chat Trigger → AI Agent → OpenAI Chat Model + Memory → (Tool) Pinecone Vector Store`  
When a question arrives, the agent retrieves relevant chunks from Pinecone and composes a concise answer. :contentReference[oaicite:16]{index=16}

**Ingestion path**  
`Manual Trigger → OneDrive Search → OneDrive Download → Pinecone Vector Store (insert) → OpenAI Embeddings → Default Data Loader`  
This pipeline searches OneDrive, downloads the PDF using the **driveItem id**, chunks/embeds, and upserts to Pinecone (namespace `apple-data`). :contentReference[oaicite:17]{index=17}

---

##  Example Grounding Data (Apple FY23 Q4)

The bundled PDF includes Apple’s FY23 Q4 results (e.g., **Total net sales $89,498M** for the quarter), balance sheet, and cash flows—perfect for Q&A and summaries. :contentReference[oaicite:18]{index=18}

---

##  Notes & Tips

- Prefer **Search → Download** in OneDrive; don’t paste web URLs with `/personal/...`—use the returned `id`. :contentReference[oaicite:19]{index=19}  
- Keep your Pinecone namespace consistent (`apple-data`) across ingestion and chat.   
- You can add more PDFs (10-Ks, press releases) and re-run the ingestion workflow to expand knowledge.

---

##  License

MIT (or your preferred license)


#n8n #OpenAI #Pinecone #RAG #LLM #OneDrive #AIagents #FinanceTech
