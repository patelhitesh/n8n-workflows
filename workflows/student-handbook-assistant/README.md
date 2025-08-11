# Student Handbook Assistant
This is an implementation of n8n workflow designed to act as an automated assistant for educational institutions. The system is configured to answer student queries related to a specific corpus of documents, such as a student handbook, academic policies, or a syllabus. By leveraging a Retrieval-Augmented Generation (RAG) architecture, the bot provides accurate, context-aware responses to questions about topics like exam policies, grade requirements, or specific curriculum details. The system is designed to provide immediate, 24/7 support while offloading repetitive inquiries from human staff.

---
## **Workflow Design**: 
This project is built on two primary workflows orchestrated by n8n. 

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

## **System Prompt**: 
"You are a helpful assistant that only answers questions based on the uploaded student handbook and academic documents. Do not respond to unrelated or personal queries. If the question falls outside your document scope, respond politely." 

---

## **Guardrails**:
* **Off-topic queries**: The bot is configured to respond with: "I can only assist with questions related to the uploaded documents." 
* **No relevant chunks found**: If the search fails to find relevant information, the bot will respond with: "I couldn’t find information related to your query in the documents." 

---

## **Test Scenarios**:

| Test Category | Test Prompt | Expected Outcome | Rationale |
|---------------|-------------|------------------|-----------|
| Positive Case | "Tell me about yourself." | A professional, purpose-driven self-introduction focused on assisting with the handbook, without mentioning limitations. | Tests the specific instruction for the "About Me" query. |
| Positive Case | "What are the academic regulations regarding course enrollment?" | A direct and concise summary of the handbook's section on course enrollment policies. | Tests ability to retrieve and summarize specific, on-topic information. |
| Positive Case | "Describe the student conduct policy." | A clear, professional summary of the student code of conduct as outlined in the handbook. | Tests ability to handle broader, multi-faceted topics within the handbook. |
| Positive Case | "What happens in a disciplinary hearing?" | A detailed and factual description of the disciplinary process as described in the handbook. | Tests retrieval of procedural information. |
| Positive Case | "Where can I find resources for mental health?" | A list of mental health resources as mentioned in the handbook, without adding external context. | Tests retrieval of specific resource information. |
| Negative Case | "What are the requirements for applying to Harvard College?" | A polite denial stating the query is outside the scope of the handbook. | Tests ability to deny an off-topic query related to admissions. |
| Negative Case | "When is the next Harvard-Yale football game?" | A polite denial, explaining the assistant's purpose is limited to the handbook. | Tests ability to deny a non-academic, event-related query. |
| Negative Case | "Can you help me with my Python programming homework?" | A polite denial, clearly stating the query is not related to the handbook's content. | Tests ability to deny a completely unrelated, technical query. |
| Negative Case | "What is the history of Harvard University?" | A polite denial, as this information is not typically found in a student handbook. | Tests ability to distinguish between general Harvard knowledge and handbook content. |
| Boundary Case | "What is the policy on student organizations and when are the club fairs?" | The agent should answer the part about **student organizations** from the handbook and politely decline to answer about **club fairs**. | Tests the ability to handle a mixed query and correctly identify the on-topic and off-topic parts. |
| Boundary Case | "Where is the registrar's office and what is the policy on leave of absence?" | The agent should answer the part about **leave of absence policy** and politely decline to answer about the **physical location of an office**. | Tests the ability to separate procedural information from physical location queries, which are often not in the handbook. |

---

## **Demo Video**: