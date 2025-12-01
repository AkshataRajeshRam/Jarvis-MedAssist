# Google Calendar Integration for ADK Voice Assistant



-----

# 🎙️ Jarvis: The ADK Voice Calendar Assistant

Welcome to **Jarvis**, your personal AI voice assistant\! This project isn't just a chatbot; it is a fully functional "agent" that can listen to your voice, understand what you need, and actually reach out to change your Google Calendar in real-time.

Think of this project as building a robot secretary that lives inside your computer. You talk to it, and it manages your schedule for you.

[Image of AI voice assistant conceptual diagram]

-----

## 🧩 How It Works (The Big Picture)

This project uses the **ADK (Agent Development Kit)** and the **Gemini 2.0 Flash** model. It works in a loop like this:

1.  **Input:** You speak to the website (or type text).
2.  **Transmission:** Your voice is sent instantly to the AI agent using a "WebSocket" (imagine a telephone line that stays open).
3.  **Thinking (The Agent):** The AI (Gemini 2.0) listens to the audio. It decides if it needs to check your calendar, add an event, or just chat.
4.  **Action (The Tools):** If you asked to schedule a meeting, the Agent uses its "hands" (Tools) to talk to Google Calendar.
5.  **Response:** The Agent speaks back to you with the confirmation.

[Image of client server AI architecture diagram]

-----

## 🛠️ What Can Jarvis Do? (The Capabilities)

Jarvis has been given specific **Tools** (Python functions) that allow it to interact with the real world. It doesn't just guess; it actually executes code.

  * **📅 List Events:** It can see what is on your schedule for today, tomorrow, or next week.
  * **➕ Create Events:** You can say "Schedule date night tomorrow at 5 PM," and it will add it.
  * **✏️ Edit Events:** Made a mistake? Tell Jarvis to move the meeting, and it will update the time.
  * **🗑️ Delete Events:** If plans change, Jarvis can remove events from your calendar.

-----

## 📂 Project Structure (The Files)

Here is how the code is organized. Think of the folder like a human body:

### 1\. `app/agent.py` (The Brain) 🧠

This file contains the **Instructions** and the **Model** definition.

  * It tells the AI: "You are Jarvis. You are helpful. You manage calendars."
  * It defines the model: **Gemini 2.0 Flash Experimental** (chosen because it is super fast and understands audio nativey).

### 2\. `app/tools.py` (The Hands) ✋

The brain needs hands to do work. This file contains the code that connects to Google.

  * It handles the **Authentication** (logging into Google).
  * It defines the functions: `list_events`, `create_events`, etc.
  * It formats the data so the AI can understand it easily (e.g., converting computer time to human time).

### 3\. `app/main.py` (The Nervous System) ⚡

This is the connection layer using **FastAPI**.

  * It runs the website server.
  * It manages the **WebSockets** (the real-time connection between your browser and the Python code).
  * It handles the audio streaming so the conversation feels instant.

-----

## 🚀 How to Run This Project

### Prerequisites

  * **Python 3.10+**
  * **Google Cloud Account** (To get Calendar API access).
  * **Gemini API Key** (To power the brain).

### Step-by-Step Setup

1.  **Download Credentials:**
    You need to create a project in Google Cloud, enable the Calendar API, and download your `credentials.json` file. This is like giving Jarvis the key to your office.

2.  **Install Dependencies:**
    We use `uv` or `pip` to install the requirements found in `pyproject.toml`.

    ```bash
    pip install -r requirements.txt
    ```

3.  **Authenticate Locally:**
    Run the setup script to log in to Google on your machine.

    ```bash
    python setup_calendar.py
    ```

4.  **Run the App (Phase 1 - Testing):**
    You can test quickly using the ADK web interface.

    ```bash
    adk web
    ```

5.  **Run the Full App (Phase 2 - Production):**
    To see the custom website with the voice interface:

    ```bash
    uvicorn app.main:app --reload
    ```

-----

## ⚠️ Important Notes

  * **The Model:** We use `Gemini 2.0 Flash Experimental`. Why? Because standard models are sometimes too slow for real-time voice, or they don't support "Native Audio" (listening to raw sound files).
  * **Safety:** The agent is instructed to be careful. For example, if you ask to delete an event, it looks for the specific ID of that event first to make sure it doesn't delete the wrong thing.

[Image of Python WebSocket real-time communication diagram]

-----

## 🎓 What You Are Learning

By building this, you are learning:

1.  **Multimodal AI:** How AI handles text and audio at the same time.
2.  **Tool Calling:** How to make AI trigger real coding functions.
3.  **WebSockets:** How to build real-time apps that don't need to refresh the page.
4.  **API Integration:** How to connect Python to Google Services.

This document explains how to set up and use the Google Calendar integration with your ADK Voice Assistant.

## Setup Instructions

### 1. Install Dependencies

First, create a virtual environment:

```bash
# Create a virtual environment
python -m venv .venv

uv venv --python 3.12

.venv\Scripts\activate

#cmd- .venv\Scripts\activate.bat

#apple- source .venv/bin/activate 
```

Activate the virtual environment:

On Windows:
```bash
# Activate virtual environment on Windows
.venv\Scripts\activate
```


python --version - check instalation

On macOS/Linux:
```bash
# Activate virtual environment on macOS/Linux
source .venv/bin/activate
```

Then, install all required Python packages using pip:

```bash
# Install all dependencies
pip install -r requirements.txt
```

### 2. Set Up Gemini API Key

1. Create or use an existing [Google AI Studio](https://aistudio.google.com/) account
2. Get your Gemini API key from the [API Keys section](https://aistudio.google.com/app/apikeys)
3. Set the API key as an environment variable:

Create a `.env` file in the project root with:

```
GOOGLE_API_KEY=your_api_key_here
```

### 3. Create a Google Cloud Project

1. Go to the [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select an existing one
3. Enable the Google Calendar API for your project:
   - In the sidebar, navigate to "APIs & Services" > "Library"
   - Search for "Google Calendar API" and enable it

### 4. Create OAuth 2.0 Credentials

1. In the Google Cloud Console, navigate to "APIs & Services" > "Credentials"
2. Click "Create Credentials" and select "OAuth client ID"
3. For application type, select "Desktop application"
4. Name your OAuth client (e.g., "ADK Voice Calendar Integration")
5. Click "Create"
6. Download the credentials JSON file
7. Save the file as `credentials.json` in the root directory of this project

### 5. Run the Setup Script

Run the setup script to authenticate with Google Calendar:

```bash
python setup_calendar_auth.py
```

This will:
1. Start the OAuth 2.0 authorization flow
2. Open your browser to authorize the application
3. Save the access token securely for future use
4. Test the connection to your Google Calendar

## Working with Multiple Calendars

The Google Calendar integration supports working with multiple calendars. The OAuth flow will grant access to all calendars associated with your Google account. You can:

1. List all available calendars using the voice command "What calendars do I have access to?"
2. Specify which calendar to use for operations by name or ID
3. Use your primary calendar by default if no calendar is specified

Examples:
- "Show me all my calendars"
- "Create a meeting in my Work calendar" 
- "What's on my Family calendar this weekend?"

## Using the Calendar Integration

Once set up, you can interact with your Google Calendar through the voice assistant:

### Examples:

- "What's on my calendar today?"
- "Show me my schedule for next week"
- "Create a meeting with John tomorrow at 2 PM"
- "Schedule a doctor's appointment for next Friday at 10 AM"
- "Find a free time slot for a 30-minute meeting tomorrow"
- "Delete my 3 PM meeting today"
- "Reschedule my meeting with Sarah to Thursday at 11 AM"
- "Change the title of my dentist appointment to 'Dental Cleaning'"

## Running the Application

After completing the setup, you can run the application using the following command:

```bash
# Start the ADK Voice Assistant with hot-reloading enabled
uvicorn app.main:app --reload
```

This will start the application server, and you can interact with your voice assistant through the provided interface.

## Troubleshooting

### Token Errors

If you encounter authentication errors:

1. Delete the token file at `~/.credentials/calendar_token.json`
2. Run the setup script again

### Permission Issues

If you need additional calendar permissions:

1. Delete the token file at `~/.credentials/calendar_token.json`
2. Edit the `SCOPES` variable in `app/jarvis/tools/calendar_utils.py`
3. Run the setup script again

### API Quota

Google Calendar API has usage quotas. If you hit quota limits:

1. Check your [Google Cloud Console](https://console.cloud.google.com/)
2. Navigate to "APIs & Services" > "Dashboard"
3. Select "Google Calendar API"
4. View your quota usage and consider upgrading if necessary

### Package Installation Issues

If you encounter issues installing the required packages:

1. Make sure you're using Python 3.8 or newer
2. Try upgrading pip: `pip install --upgrade pip`
3. Install packages individually if a specific package is causing problems

## Security Considerations

- The OAuth token is stored securely in your user directory
- Never share your `credentials.json` file or the generated token
- The application only requests the minimum permissions needed for calendar operations


