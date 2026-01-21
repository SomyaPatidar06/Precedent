# System Architecture

## High-Level Data Flow

```mermaid
graph TD
    User[User] -->|Uploads Doc| Front[Frontend (React + Vite)]
    User -->|Queries 'Why AWS?'| Front
    
    Front -->|POST /ingest| API[Backend API (FastAPI)]
    Front -->|POST /search| API
    
    subgraph "Ingestion Pipeline"
        API -->|1. Extract Text| Parser[PDF/Text Parser]
        Parser -->|2. Raw Text| LLM_Ingest[Groq LLM (Llama 3)]
        LLM_Ingest -->|3. Extract Decisions (JSON)| Struct[Structured Data]
        Struct -->|4. Generate Embedding| Embed[Sentence Transformer (Local)]
        Embed -->|5. Store Vector + Metadata| VectorDB[(Qdrant Vector DB)]
    end
    
    subgraph "Retrieval Pipeline"
        API -->|1. Receive Query| Embed_Query[Sentence Transformer]
        Embed_Query -->|2. Vector Search| VectorDB
        VectorDB -->|3. Return Top Matches| API
        API -->|4. Format JSON Response| Front
    end
```

## Component Description

1.  **Frontend**: Built with React and TypeScript. Replaced standard search with a "Decision Card" UI to display rich context (Rationale, Alternatives).
2.  **Backend**: Python FastAPI. Handles file processing and search requests.
3.  **LLM Layer (Groq)**: Used strictly for *Extraction*, not generation. We pass raw documents to Llama-3-70b-8192 via Groq to convert unstructured text into structured JSON (Decisions, Rationales, Companies).
4.  **Vector Store (Qdrant)**: Stores semantic embeddings of the "Decision Rationale". This allows users to search by *concept* (e.g., "cost saving") rather than just keywords.
5.  **Local Embeddings**: We use `all-MiniLM-L6-v2` locally to ensure fast, free, and private embedding generation without external API calls for this step.
