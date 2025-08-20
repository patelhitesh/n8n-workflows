# ChefGPT 🍳 - Your AI Sous-Chef

Ever looked at the bottle gourd growing in your garden and wondered what to make beyond the usual curry? **ChefGPT** is here to unlock the full potential of your home garden produce!

This project is an intelligent recipe assistant built with a Retrieval-Augmented Generation (RAG) architecture. It ingests the *"Nutrient Rich Recipes from Home Garden Produce"* handbook and lets you chat with it via Telegram. Get instant, context-aware answers and discover nutritious, simple recipes straight from the source.

## ✨ Features

- **Ask Anything**: Chat with the recipe book in natural language.
- **Ingredient-Based Search**: "I have bottle gourd (lauki) and tomatoes. What can I make?"
- **Nutritional Guidance**: "What are the health benefits of pumpkin?"
- **Smart & Scalable**: Easily add other recipe books just by dropping them in a folder.
- **Lightning Fast**: Get answers in seconds, thanks to the magic of vector search.

## 🧠 The Secret Sauce: How It Works

ChefGPT isn't just a simple chatbot. It's a sophisticated culinary expert powered by two distinct, automated **n8n** workflows. Think of it as a well-organized kitchen with a prep station and a service counter.

### The Prep Kitchen (Document Ingestion)

This is where we prepare our ingredients (the knowledge). This workflow runs automatically whenever a new recipe document is added.

- **Fresh Delivery (Google Drive Trigger)**: The "Recipes Book.pdf" is dropped in our designated Google Drive folder, kicking things off.
- **Mise en Place (Text Extraction & Chunking)**: The workflow pulls the text from the document and slices it into perfectly-sized, digestible chunks.
- **The Flavor Infusion (Generate Embeddings)**: Each chunk is sent to the OpenAI API to be transformed into a rich, numerical "flavor profile" (a vector embedding).
- **Stocking the Pantry (Store in Pinecone)**: These vectors are neatly stored in our Pinecone vector database, ready for immediate recall.

### The Service Counter (Live Querying)

This is where the magic happens for the end-user. This workflow fires up whenever you send a message to the bot.

- **Order Up! (Telegram Trigger)**: You send a message to our Telegram bot with a burning culinary question.
- **Finding the Right Ingredients (Pinecone Query)**: We take your question, convert it into a vector, and ask Pinecone to instantly find the most relevant recipe chunks from our pantry.
- **The Master Chef (OpenAI Chat)**: The relevant chunks are passed to the OpenAI Chat model as context. This is key—the model isn't just guessing; it's crafting an answer based on the exact information from the recipe book.
- **Bon Appétit! (Telegram Send Message)**: The beautifully crafted, accurate answer is served back to you in the Telegram chat. Voilà!

## 🛠️ What's in the Toolbox?

This project was cooked up using a handful of powerful, modern tools.

## 🔥 Fire up the Ovens! - Getting Started

Ready to get your own AI Sous-Chef running? Follow these steps.

### Prerequisites

- An active **n8n** instance (Cloud or self-hosted).
- API keys for **OpenAI** and **Pinecone**.
- A **Telegram Bot Token**.
- A **Google Account** for the Drive trigger.

### Installation

1. **Clone the Repo**:
   
2. **Set up Your n8n Credentials**:
   Inside your n8n instance, go to **Credentials** and add new credentials for:
   - OpenAI
   - Pinecone
   - Telegram Bot
   - Google (using OAuth2)

3. **Import the Workflows**:
   Import the two workflow JSON files (`ingestion_workflow.json` and `query_workflow.json`) from this repository into your n8n workspace.

4. **Configure the Nodes**:
   Open each workflow and ensure the nodes are linked to the credentials you just created. You'll also need to set:
   - Your **Pinecone index name**.
   - The specific **Google Drive folder ID** to monitor.

5. **Activate the Workflows**:
   Toggle the workflows to "Active" in the n8n dashboard. Drop the "Recipes Book.pdf" in your Google Drive folder to test the ingestion, then send a message to your Telegram bot to test the query!

## 🧑‍🍳 Chat with the Chef

Once active, simply open a chat with your bot in Telegram and start asking questions based on the *"Nutrient Rich Recipes"* book!

- "How do I make 'Lauki Ka Kofta'?"
- "Suggest a simple soup recipe from the book."
- "What ingredients do I need for Palak Paneer?"
- "Tell me a recipe that uses pumpkin."

Enjoy your meal! 🍴
