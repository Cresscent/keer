# Voice Task Manager

A voice-to-text task management application with AI-powered task extraction and calendar integration.

## Features

- **Voice Input**: Continuous voice recording using Web Speech API with real-time transcription
- **AI Task Extraction**: Uses Anthropic's Claude API to intelligently extract tasks from natural conversation
- **Smart Task Cards**: Color-coded by priority (high/medium/low) with duration and time-of-day suggestions
- **Auto-Scheduling**: Automatically blocks time for all tasks starting at 9 AM with smart scheduling
- **Calendar Export**: ICS file export compatible with Google Calendar, Outlook, and Apple Calendar
- **Manual Scheduling**: Set custom date/time for individual tasks
- **Modern UI**: Purple/indigo gradient theme with smooth animations and responsive design

## Quick Start

1. Open `index.html` in a modern web browser (Chrome, Edge, or Safari recommended)
2. Enter your Anthropic API key when prompted (stored locally in your browser)
3. Click the microphone button to start recording
4. Speak naturally about your tasks and to-dos
5. Click "Generate Task List" to extract tasks with AI
6. Review, schedule, and export your tasks to your calendar

## Requirements

- Modern web browser with Web Speech API support (Chrome, Edge, Safari)
- Anthropic API key ([Get one here](https://console.anthropic.com/))
- Microphone access for voice input

## How It Works

### Voice Input Screen
- Tap the microphone to start/stop recording
- Real-time transcription displays as you speak
- Manual text input available if voice not supported
- Purple gradient design with pulsing animation when recording

### Task Management Screen
- Tasks extracted by AI with:
  - Title and description
  - Estimated duration in minutes
  - Priority level (high, medium, low)
  - Suggested time of day (morning, afternoon, evening)
- Priority color coding:
  - **Red**: High priority
  - **Yellow**: Medium priority
  - **Blue**: Low priority
- Green border indicates scheduled tasks

### Scheduling Features
- **Auto-Schedule All**:
  - Starts at 9 AM
  - Adds 15-minute buffer between tasks
  - Moves to next day after 6 PM
  - Skips already scheduled tasks
- **Manual Scheduling**: Date and time inputs for each task
- **ICS Export**: Download calendar file for import into any calendar app

## Technical Details

- **Frontend**: React 18 via CDN with Babel transpilation
- **Styling**: Tailwind CSS via CDN
- **Voice Recognition**: Web Speech API (webkit/standard)
- **AI Processing**: Anthropic Claude claude-sonnet-4-20250514 API
- **No Backend Required**: Everything runs in the browser
- **API Key Storage**: Saved in localStorage (never sent to third-party servers)

## Browser Compatibility

| Browser | Voice Input | Full Support |
|---------|-------------|--------------|
| Chrome  | ✅          | ✅           |
| Edge    | ✅          | ✅           |
| Safari  | ✅          | ✅           |
| Firefox | ❌          | ⚠️ Text only |

## Privacy

- Your API key is stored only in your browser's localStorage
- Voice data is processed locally via Web Speech API
- Conversations are sent only to Anthropic's API for task extraction
- No data is stored on external servers

## Files

```
voice-task-manager/
├── index.html    # Main app (all-in-one)
├── README.md     # This file
└── .gitignore    # Git ignore file
```

## License

MIT License - Feel free to use, modify, and distribute.
