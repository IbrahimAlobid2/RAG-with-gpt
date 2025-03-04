# **Project Overview**

This project is built using **FastAPI** and integrates multiple technologies to manage, process, and retrieve text-based documents efficiently. It provides capabilities such as:

1. **Project Management** - Users can create and manage projects within MongoDB.
2. **File Upload & Validation** - Allows uploading and storing documents in a structured manner.
3. **Text Chunking & Storage** - Splits documents into smaller text chunks and stores them in MongoDB.
4. **Vector Indexing** - Converts text chunks into vector embeddings and stores them in a vector database (e.g., **Qdrant**) for semantic search.
5. **Semantic Search** - Retrieves the most relevant text chunks based on user queries.
6. **Retrieval-Augmented Generation (RAG)** - Enhances the retrieval process by passing relevant chunks to an LLM (Large Language Model) for generating informative responses.

This project follows a **layered architecture**, ensuring modularity and scalability. It consists of several components:
- **Routes** - Defines FastAPI endpoints.
- **Models** - Represents data structures stored in MongoDB.
- **Controllers** - Handles business logic for file processing, chunking, and retrieval.
- **Stores** - Manages interactions with LLM providers and vector databases.
- **Settings** - Configures application settings through `.env` files.
- **Docker Integration** - Provides a `docker-compose.yml` file for containerized deployment of MongoDB.

---

## **Database Structure**

### **MongoDB (NoSQL)**
- Stores metadata related to **projects**, **assets (files)**, and **text chunks**.
- Uses indexes for fast lookups.

### **Vector Database (e.g., Qdrant)**
- Stores vector embeddings of text chunks for **semantic search**.
- Supports similarity-based retrieval using cosine similarity or dot product.

---

## **Docker Setup**

The project includes a `docker-compose.yml` file to manage MongoDB as a containerized service. This configuration ensures a consistent development and production environment.

### **MongoDB Container Configuration**
```yaml
services:
  mongodb:
    image: mongo:7-jammy
    container_name: mongodb
    ports:
      - "27007:27017"
    volumes:
      - mongodata:/data/db
    environment:
      - MONGO_INITDB_ROOT_USERNAME=${MONGO_INITDB_ROOT_USERNAME}
      - MONGO_INITDB_ROOT_PASSWORD=${MONGO_INITDB_ROOT_PASSWORD}
    networks:
      - backend
    restart: always

networks:
  backend:

volumes:
  mongodata:
```

### **Key Features of the Docker Setup:**
- Uses the **MongoDB 7 Jammy** image for stability.
- Exposes MongoDB on **port 27007** to avoid conflicts with local installations.
- Uses **environment variables** for authentication, configurable via `.env`.
- Stores data persistently using **Docker volumes**.
- Configures **network isolation** for backend services.

### **How to Run MongoDB Using Docker**
1. Create a `.env` file with MongoDB credentials:
   ```env
   MONGO_INITDB_ROOT_USERNAME=admin
   MONGO_INITDB_ROOT_PASSWORD=securepassword
   ```

2. Start MongoDB with Docker Compose:
   ```sh
   docker-compose up -d
   ```

3. Verify MongoDB is running:
   ```sh
   docker ps
   ```

4. Access MongoDB shell:
   ```sh
   docker exec -it mongodb mongosh -u admin -p securepassword
   ```

5. Stop the service:
   ```sh
   docker-compose down
   ```

---

## **Data Models (db_schemes)**

### **1. Project**
- Fields: `_id`, `project_id`
- Indexed by `project_id` (alphanumeric, unique).

### **2. Asset (File Metadata)**
- Fields: `_id`, `asset_project_id`, `asset_name`, `asset_type`, `asset_size`, `asset_pushed_at`
- Indexed by `(asset_project_id, asset_name)` to prevent duplicates.

### **3. DataChunk (Text Segments)**
- Fields: `_id`, `chunk_text`, `chunk_metadata`, `chunk_order`, `chunk_project_id`, `chunk_asset_id`
- Indexed by `chunk_project_id` for efficient retrieval.

### **4. RetrievedDocument**
- Used for returning search results.
- Fields: `text`, `score`

---

## **Usage Workflow**

1. **Start the application**
   ```sh
   uvicorn main:app --reload
   ```
2. **Upload a file** (`POST /api/v1/data/upload/{project_id}`)
3. **Process the file** (`POST /api/v1/data/process/{project_id}`)
4. **Index text chunks** (`POST /api/v1/nlp/index/push/{project_id}`)
5. **Perform semantic search** (`POST /api/v1/nlp/index/search/{project_id}`)
6. **Use RAG for answering queries** (`POST /api/v1/nlp/index/answer/{project_id}`)

---

## **Conclusion**

This project integrates document management, text chunking, vector search, and LLMs into a single FastAPI-based system. The modular design allows easy adaptation to different LLMs and vector DBs, making it a scalable and flexible solution for text-based AI applications.

With **Docker support**, the deployment process is streamlined, ensuring a reproducible environment for development and production.

