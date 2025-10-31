# Applied LLM & Generative AI Projects using Google Gemini

This repository serves as a portfolio of hands-on projects built using Google's Gemini API. The goal is to demonstrate the practical application of large language models (LLMs) and generative AI to create engaging, intelligent, and useful applications.

Each project is a standalone Streamlit web application showcasing a different capability of the Gemini models, from creative content generation to tool-based reasoning and Retrieval-Augmented Generation (RAG).

## 🚀 Projects Overview

1.  **[AI Story Generator](#1-🤖-ai-story-generator):** A creative tool that writes and narrates unique stories based on user-uploaded images and a chosen genre.
2.  **[Morning Buddy](#2-☀️-morning-buddy):** A personalized, agent-like dashboard that provides weather, news summaries, and a "smart planner" by using Gemini with external API tools.
3.  **[Youtube RAG (VidSynth AI)](#3-🎬-youtube-rag-vidsynth-ai):** A powerful RAG pipeline that "learns" the content of any YouTube video, allowing you to generate notes or chat with it to ask specific questions.

---

## 1. 🤖 AI Story Generator

This application demonstrates the creative power of multimodal LLMs. Users upload 1-10 images, select a genre (e.g., Mystery, Morale), and the Gemini API generates a single, cohesive story that connects all the images in order. It then uses Google's Text-to-Speech (gTTS) to provide an audio narration.

### File Breakdown

* **`AI-Story-Generator/app.py`**: The main Streamlit frontend. It handles the user interface, including the image uploader, style-selection dropdown, and the "Generate" button. It displays the generated story and the audio player.
* **`AI-Story-Generator/story_generator.py`**: The backend logic.
    * `generate_story_from_images()`: Builds a detailed prompt and calls the `gemini-2.5-flash-lite` model with the images and text prompt.
    * `narrate_story()`: Takes the generated story text and uses `gTTS` to create an in-memory audio file.
* **`AI-Story-Generator/requirements.txt`**: Contains all necessary Python packages, such as `streamlit`, `google-generativeai`, and `gtts`.

---

## 2. ☀️ Morning Buddy

This project showcases Gemini's ability to act as a reasoning "agent" by using external tools. It's a multi-page dashboard to help a user start their day. It can fetch a human-readable weather report, summarize top news articles based on an interest, and generate a full day's itinerary by combining weather forecasts and local event data.

### File Breakdown

* **`Morning-Buddy/app.py`**: The Streamlit frontend. It uses `st.sidebar` to create a multi-page application (Home, Weather, News, Planner) and manages the UI and state for each page.
* **`Morning-Buddy/applications.py`**: The backend "agent" logic.
    * Defines functions that act as **tools** for the LLM (e.g., `get_weather()`, `get_news()`, `find_local_events()`) which call external APIs (OpenWeatherMap, NewsAPI, SerpApi).
    * Defines **agent** functions (`temperature_of_city()`, `smart_plan()`) that call Gemini, providing it with the tools and a prompt, allowing the LLM to decide which tool to call to fulfill the user's request.
* **`Morning-Buddy/requirements.txt`**: Lists all dependencies, including `streamlit`, `google-genai`, `requests`, and `python-dotenv`.

---

## 3. 🎬 Youtube RAG (VidSynth AI)

This is a complete Retrieval-Augmented Generation (RAG) pipeline. A user provides a YouTube URL, and the application does the rest:
1.  **Extracts** the video's transcript.
2.  **Splits** the transcript into small, manageable chunks.
3.  **Embeds** these chunks into a vector database (ChromaDB) using Google's embedding model.
4.  **Retrieves** relevant chunks based on a user's question and feeds them to Gemini as context.

This allows the user to ask detailed questions and get answers based *only* on the video's content.

### File Breakdown

* **`Youtube-RAG/app.py`**: The Streamlit frontend. It manages the user input (URL, language) and the chat interface. Critically, it uses `st.session_state` to store the chat history and the persistent vector store, allowing for a continuous conversation.
* **`Youtube-RAG/supporting_functions.py`**: The complete backend RAG pipeline.
    * `get_transcript()` / `translate_transcript()`: Fetches and (if needed) translates the video transcript.
    * `create_chunks()` / `create_vector_store()`: Uses LangChain's text splitters and ChromaDB to build the vector index.
    * `rag_answer()`: The core RAG function. It finds relevant documents in the vector store and passes them, along with the user's question, to a Gemini prompt-chain to generate a factual, in-context answer.
* **`Youtube-RAG/requirements.txt`**: Lists all dependencies for this advanced pipeline, including `streamlit`, `langchain`, `langchain-chroma`, `youtube-transcript-api`, and `google-generativeai`.

