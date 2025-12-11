#ADK Voice Assistant
🚀 Jarvis: Your AI Voice Calendar & Wellness Assistant

Powered by Google ADK + Gemini 2.0 Flash Experimental

Jarvis is your personal AI voice assistant. You talk to it, and it not only manages your Google Calendar but also gives general wellness suggestions based on your symptoms — just like a friendly assistant who cares about your schedule and your well-being.

Jarvis listens, thinks, and acts — all in real time.

<img width="1313" height="804" alt="image" src="https://github.com/user-attachments/assets/8791cc03-72bd-4c07-b8ad-6e49a375c0fb" />


-----

🧠 How It Works (The Big Picture)

This project uses:

ADK (Agent Development Kit) → Handles the agent logic and WebSocket communication.

Gemini 2.0 Flash Experimental → Listens, understands your intent, and generates responses.

Google Calendar API → Creates, updates, and deletes calendar events.

Real-Time Audio → Jarvis hears you and responds instantly.

Flow

1.You speak through the web interface.

2.Audio is streamed to the Agent using a WebSocket.

3.The Agent transcribes your speech → Gemini understands the intent.

4.Jarvis decides whether you want to:

5.Create a meeting

6.Check your schedule

7.Edit or delete an event, OR

8.Ask about symptoms such as headaches, stomach pain, fever, etc.

9.If it's a calendar task, Jarvis uses Google Calendar API to perform the action.

10.Jarvis speaks back with a natural voice output.

-----
🧩 What Jarvis Can Do (Features)
🗓 1. Calendar Management

Jarvis understands natural language commands like:

“Schedule a meeting tomorrow at 5 PM.”

“Show me my meetings for next week.”

“Move my 3 PM meeting to 4 PM.”

“Delete the event on Monday.”

🤒 2. Basic Wellness Suggestions

When you say things like:

“I have a headache.”

“My stomach is hurting.”

“I feel dizzy.”

Jarvis gives general, non-medical suggestions such as:

“You might be dehydrated; try drinking water.”

“Stomach pain may be caused by acidity or gastritis.”

“If symptoms worsen, consider consulting a doctor.”

(This is NOT a medical assistant — only general guidance.)

🎧 3. Real-Time Speech

Live listening

Instant responses

Conversational memory during the session
-----
📁 Project Structure
/app
 ├── agent.py       # The brain: conversations + model instructions
 ├── tools.py       # The hands: calendar API + scheduling logic
 └── main.py        # The nervous system: WebSocket server + audio streaming

static/              # Frontend (HTML + JS)
templates/           # Web interface for the voice chat
requirements.txt     # Dependencies

-----
🔍 Breakdown
1. agent.py (The Brain)

Contains the main instructions to Gemini.

Defines the assistant’s personality:

Manages your calendar

Gives wellness suggestions

Maintains polite, conversational behavior

2. tools.py (The Hands)

Handles:

Authentication with Google

Creating, reading, updating, deleting calendar events

Converting natural language → timestamps

Safety checks (e.g., confirming event details)

3. main.py (The Nervous System)

WebSocket connection using FastAPI

Handles real-time audio streaming

Sends your voice to the Agent

Outputs text and audio responses
-----

🧳 Step-by-Step Setup
### 1. Install Python dependencies
   pip install -r requirements.txt


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

### ⚠️ Important Notes

The model used is Gemini 2.0 Flash Experimental because it supports real-time audio and more advanced reasoning.

Jarvis gives general wellness suggestions, not medical advice.

Calendar actions always require clear date/time to avoid mistakes.

The assistant intentionally double-checks event details for safety.

###🎓 What You Learn From This Project

How to build an AI voice assistant with Google ADK

Real-time audio streaming and WebSockets

Tool calling with Python functions

Integrating Gemini models with Google Calendar

Structuring a multi-file AI agent project

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


