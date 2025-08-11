# Student Handbook Assistant
This is an implementation of n8n workflow designed to act as an automated assistant for educational institutions. The system is configured to answer student queries related to a specific corpus of documents, such as a student handbook, academic policies, or a syllabus. By leveraging a Retrieval-Augmented Generation (RAG) architecture, the bot provides accurate, context-aware responses to questions about topics like exam policies, grade requirements, or specific curriculum details. The system is designed to provide immediate, 24/7 support while offloading repetitive inquiries from human staff.

---
## **Workflow Design**: 
This is built on two primary workflows orchestrated by n8n. 

1.  **Document Ingestion Pipeline**: 
    * **Google Drive Trigger**: The workflow is initiated when a new student handbook or syllabus document (PDF, DOCX, etc.) is uploaded to a designated Google Drive folder. 
    * **Text Extraction & Chunking**: The document's content is programmatically extracted, then segmented into logical chunks to prepare it for vectorization. 
    * **Generate Embeddings**: Each text chunk is converted into a numerical vector using the OpenAI Embeddings API. 
    * **Store in Pinecone**: The vectors are stored in a Pinecone vector database, with metadata attached to each chunk for context. 

2.  **Live Query Pipeline**: 
    * **Telegram Trigger**: This workflow is triggered by an incoming message from a student in a Telegram chat. 
    * **Pinecone Query**: The student's message is used to perform a vector search in Pinecone, retrieving the top relevant document chunks from the handbook. 
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