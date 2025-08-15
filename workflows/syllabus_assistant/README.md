# Syllabus Assistant
Meet the Syllabus Assistant, a specialized AI providing instant, accurate answers grounded exclusively in official course documents using a Retrieval-Augmented Generation (RAG) framework. This ensures every response is a verifiable source of truth.

For a seamless user experience, the assistant proactively introduces itself upon greeting and uses conversational history to understand follow-up questions. Its core feature is precision: it is programmed to only state what is explicitly written, never inferring or making assumptions. It intelligently reports when a specific detail is missing from a relevant topic and politely redirects any query that falls outside its syllabus-defined scope. This creates a reliable, 24/7 self-service tool for students and faculty, eliminating ambiguity by providing direct access to course information.

---
## **Workflow Design**: 
This is built on two primary workflows orchestrated by n8n. 

1.  **Document Ingestion Pipeline**: 
    * **Google Drive Trigger**: The workflow is initiated when a new syllabus document (PDF, DOCX, etc.) is uploaded to a designated Google Drive folder. 
    * **Text Extraction & Chunking**: The document's content is programmatically extracted, then segmented into logical chunks to prepare it for vectorization. 
    * **Generate Embeddings**: Each text chunk is converted into a numerical vector using the OpenAI Embeddings API. 
    * **Store in Pinecone**: The vectors are stored in a Pinecone vector database, with metadata attached to each chunk for context. 

2.  **Live Query Pipeline**: 
    * **Telegram Trigger**: This workflow is triggered by an incoming message from a user in a Telegram chat. 
    * **Pinecone Query**: The user's message is used to perform a vector search in Pinecone, retrieving the top relevant document chunks from the handbook. 
    * **OpenAI Chat**: The retrieved chunks are used to augment a prompt sent to the OpenAI Chat API, enabling the model to generate a specific, informed response. 
    * **Telegram Send Message**: The final answer is sent back to the student. 

---

## **Tools and API's used**: 
1.  **n8n**: The core no-code automation platform for workflow orchestration. 
2.  **OpenAI API**: For both text embeddings and the chat model. 
3.  **Pinecone**: The vector database for efficient similarity search. 
4.  **Telegram Bot API**: The messaging interface for the chatbot. 
5.  **Google Drive API**: Used for document ingestion. 

---