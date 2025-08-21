# AI-Powered Travel Guide Chatbot

This project is an AI-powered travel guide chatbot designed to assist tourists with itinerary planning and providing detailed information about attractions. It leverages a Retrieval-Augmented Generation (RAG) architecture built with a no-code orchestration platform to create a seamless and efficient user experience.

## ✨ Key Features

- **Automated Document Ingestion**: A robust pipeline that automatically processes and prepares new tourism documents for the AI model.
- **Context-Aware Responses**: Utilizes a vector database to ensure that all generated responses are grounded in the provided tourism documents, preventing hallucinations.
- **Multi-platform Interface**: Integrates with Telegram to provide a familiar and accessible chat interface for users.
- **Scalable Architecture**: Built on a flexible no-code platform that allows for easy expansion and integration with other services.

## 🚀 How It Works

The project operates on two distinct, automated workflows orchestrated by n8n. These pipelines handle everything from data preparation to live user interactions.

### 1. Document Ingestion Pipeline

This workflow is responsible for keeping the chatbot's knowledge base up-to-date.

- **Google Drive Trigger**: The process begins when a new travel or tourism document (PDF, DOCX, etc.) is uploaded to a designated Google Drive folder.
- **Text Extraction & Chunking**: The content is automatically extracted from the document and then segmented into smaller, logical chunks. This is a crucial step for efficient retrieval, as it allows the model to find specific, relevant information without processing the entire document.
- **Generate Embeddings**: Each text chunk is converted into a high-dimensional numerical vector using the OpenAI Embeddings API. These vectors mathematically represent the semantic meaning of the text.
- **Store in Pinecone**: The generated vectors are stored in a Pinecone vector database, which is optimized for fast similarity searches. This ensures that the relevant information can be retrieved quickly during a live query. Metadata, such as the document title or source URL, is also attached to each vector for context.

### 2. Live Query Pipeline

This workflow handles real-time user queries, providing instant, accurate responses.

- **Telegram Trigger**: An incoming message from a user in a Telegram chat triggers this workflow.
- **Pinecone Query**: The user's message is transformed into a vector, which is then used to perform a similarity search in Pinecone. This retrieves the top relevant document chunks—the "context"—from the tourism knowledge base.
- **OpenAI Chat**: The retrieved chunks are used to augment a prompt sent to the OpenAI Chat API. The model is instructed to generate a response only based on the provided context, effectively grounding its answer and preventing the generation of incorrect or irrelevant information.
- **Telegram Send Message**: The final, AI-generated answer is sent back to the user via the Telegram Bot API.

## 🛠️ Technology Stack

| Tool/API           | Purpose                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------|
| **n8n**            | The central no-code automation platform that orchestrates and connects all the workflows. |
| **OpenAI API**     | Provides the power of language understanding through its Embeddings API (for vectorization) and Chat API (for response generation). |
| **Pinecone**       | The specialized vector database that enables efficient and scalable similarity search for information retrieval. |
| **Telegram Bot API**| The messaging interface that connects the chatbot to the end-users.                        |
| **Google Drive API**| Facilitates the automated ingestion of new documents into the system.                      |
