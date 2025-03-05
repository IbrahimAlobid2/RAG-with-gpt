# **AI-Powered Document Processing & Retrieval**

**A cutting-edge FastAPI-based solution** that revolutionizes how organizations store, process, and leverage text-based documents. By integrating **MongoDB**, **Qdrant** (or similar vector databases), and advanced **LLM** capabilities, this project delivers lightning-fast semantic search, sophisticated text chunking, and Retrieval-Augmented Generation (RAG) for superior insights.

---

## **Executive Summary**

Today’s decision makers need **quick, accurate, and context-rich** access to information. This project harnesses modern **AI** techniques and best-in-class infrastructure to ensure that critical data is always at your fingertips:

- **Ultra-Fast Search**: Vector embeddings make it possible to retrieve precisely what you need, not just keyword matches.  
- **Context-Rich Answers**: Retrieval-Augmented Generation synthesizes relevant text chunks to produce **intelligent, human-like responses**.  
- **Flexible & Extensible**: Easily integrates with additional LLM providers or vector databases, adapting to evolving business needs.

---

## **Key Features**

1. **Project Management & Metadata**  
   - Organize all files under dedicated projects, each with unique IDs stored in MongoDB.  
   - Prevent name collisions and ensure consistent data integrity.

2. **File Upload & Validation**  
   - Streamlined FastAPI routes to securely upload documents.  
   - Built-in checks for file size and type, minimizing the risk of invalid or malicious files.

3. **Text Chunking & Structured Storage**  
   - Automatic text segmentation (chunking) for more **granular and precise** indexing.  
   - Chunks stored in MongoDB with robust metadata and relationships.

4. **Semantic Vector Indexing**  
   - Each text chunk is converted into embeddings (via your choice of LLM model) and indexed in **Qdrant** (or any pluggable vector DB).  
   - Empowers similarity-based lookup, enabling advanced **semantic search**.

5. **Retrieval-Augmented Generation (RAG)**  
   - Dynamically merges semantic search results with your LLM, producing answers that **deeply reference** domain-specific data.  
   - Accelerates research, knowledge discovery, and informed decision-making.

---

## **Architecture & Workflow**

1. **Upload** – Store file metadata in MongoDB and save the file on the server.  
2. **Process** – Split documents into chunks, each chunk carrying essential metadata (page info, source file, etc.).  
3. **Index** – Convert chunks into embeddings and store them in a vector database for high-speed retrieval.  
4. **Search** – Perform **semantic lookups** to quickly locate relevant chunks.  
5. **RAG** – Pass retrieved chunks into an LLM for context-aware, **intelligent** answer generation.

---

## **Technology Stack**

- **FastAPI**: High-performance Python web framework for building **scalable** REST APIs.  
- **MongoDB**: NoSQL database managing **project** and **file** metadata, chunk information, and other structured data.  
- **Qdrant**: (Optional) Vector database providing advanced similarity search; can be replaced by any **VectorDBInterface** implementation.  
- **LLM Providers**: (OpenAI, Together, Cohere, etc.) offering text-embedding and text-generation services, encapsulated in a flexible provider architecture.  
- **Docker**: Simplifies deployment and ensures consistent runtime environments.  

---

## **Deployment with Docker**

To streamline infrastructure setup, a **Docker Compose** configuration is provided for **MongoDB**.  

1. **Configure `.env`**  
   ```env
   MONGO_INITDB_ROOT_USERNAME=admin
   MONGO_INITDB_ROOT_PASSWORD=supersecret
   ```
2. **Start Services**  
   ```sh
   docker-compose up -d
   ```
   - Launches MongoDB in a container named `mongodb` on port **27007**.

3. **Verify**  
   ```sh
   docker ps
   ```
   - Confirm that the MongoDB container is up and running.  

By containerizing MongoDB, you ensure **consistency** across development, testing, and production environments, with **persistent data storage** maintained by Docker volumes.

---

## **Installation & Setup**

1. **Install Dependencies**  
   ```sh
   pip install -r requirements.txt
   ```
2. **Start the FastAPI Server**  
   ```sh
   uvicorn main:app --reload
   ```
   - Launches the application on the default port (e.g., `http://127.0.0.1:8000`).

3. **Configure Environment Variables**  
   - Adjust `.env` to align with your environment (e.g., `MONGODB_URL`, `GENERATION_BACKEND`, etc.).

4. **Run Additional Services**  
   - Start or connect to your preferred vector DB (Qdrant, FAISS, Pinecone) as configured.

---

## **Usage Guide**

1. **Upload a File**  
   - Endpoint: `POST /api/v1/data/upload/{project_id}`  
   - Body: File to upload.  
   - Result: File is stored, and an **Asset** record is created in MongoDB.

2. **Process a File into Chunks**  
   - Endpoint: `POST /api/v1/data/process/{project_id}`  
   - Body: `{ "file_id": "your_file_name.pdf", ... }`  
   - Result: Creates **DataChunk** records in MongoDB.

3. **Index Chunks**  
   - Endpoint: `POST /api/v1/nlp/index/push/{project_id}`  
   - Body: `{ "do_reset": 1 }` (optional)  
   - Result: Embeddings are generated, and chunks are uploaded to the vector DB.

4. **Semantic Search**  
   - Endpoint: `POST /api/v1/nlp/index/search/{project_id}`  
   - Body: `{ "text": "Your query", "limit": 5 }`  
   - Result: Returns the top-matching chunk results sorted by relevance.

5. **RAG Answering**  
   - Endpoint: `POST /api/v1/nlp/index/answer/{project_id}`  
   - Body: `{ "text": "Your question", "limit": 5 }`  
   - Result: LLM-based answer with references to the most relevant chunks.

---

## **Advantages & Business Value**

- **Enhanced Decision Making**: Quickly extract critical knowledge from large document repositories.  
- **Reduced Turnaround Time**: Fast semantic search means less waiting for vital data, boosting productivity.  
- **Future-Proof Design**: Pluggable architecture allows easy switching of vector DBs or LLM providers.  
- **Enterprise-Ready**: Docker-based setup, environment-based settings, and role-based access in MongoDB ensure **reliability** and **security**.  
- **High Scalability**: Scales horizontally by replicating the FastAPI service and the vector DB nodes as needed.

---

## **Conclusion**

In an era where **data-driven insights** shape an organization’s competitive edge, this AI-Powered Document Processing & Retrieval system stands out for its **speed, intelligence, and versatility**. By seamlessly combining advanced text chunking, vector semantic search, and RAG-based AI responses, it delivers **unparalleled performance** in managing and exploiting textual data.

**Ready to transform your document workflows**? Deploy this solution today and leverage the power of **cutting-edge AI** to stay ahead in the knowledge economy.

---

> **Questions or contributions?**  
> Feel free to open an issue or submit a pull request. We’re always looking to **improve and evolve** this project further.
_______________
### **Aouther** : Ibrahim Alobaid

