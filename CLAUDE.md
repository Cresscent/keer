# CLAUDE.md - AI Assistant Guide for Keer Repository

## Project Overview

**Keer** is a collection of web applications. Currently contains the **Voice Task Manager** - a browser-based voice-to-text task management application with AI-powered task extraction and calendar integration.

## Repository Structure

```
keer/
├── CLAUDE.md                    # This file - AI assistant guidance
└── voice-task-manager/          # Voice Task Manager application
    ├── index.html               # Single-file React application (all-in-one)
    ├── README.md                # Application documentation
    └── .gitignore               # Git ignore patterns
```

## Voice Task Manager Application

### Architecture

The Voice Task Manager is a **single-file React application** with no build step required:

- **Frontend Framework**: React 18 via CDN with Babel transpilation
- **Styling**: Tailwind CSS via CDN
- **Voice Recognition**: Web Speech API (webkit/standard)
- **AI Processing**: Anthropic Claude API (claude-sonnet-4-20250514)
- **No Backend**: Everything runs client-side in the browser

### Key Components (`voice-task-manager/index.html`)

| Component | Lines | Description |
|-----------|-------|-------------|
| `App` | 606-781 | Main app component, state management, API calls |
| `VoiceInputScreen` | 118-325 | Voice recording with Web Speech API |
| `TaskManagementScreen` | 423-603 | Task list, scheduling, and export |
| `TaskCard` | 328-420 | Individual task display with priority colors |
| `LoadingSpinner` | 109-115 | Loading state during AI processing |
| Icon Components | 48-106 | SVG icons as React components |

### State Management

The app uses React hooks for state:
- `screen`: Current view ('voice' or 'tasks')
- `transcript`: Voice input text
- `tasks`: Array of extracted tasks
- `apiKey`: Anthropic API key (persisted in localStorage)
- `isLoading`, `error`: UI states

### Task Data Structure

```javascript
{
  id: number,              // Unique identifier
  title: string,           // Task name
  description: string,     // Brief description
  estimatedDuration: number, // Minutes
  priority: 'high' | 'medium' | 'low',
  suggestedTime: 'morning' | 'afternoon' | 'evening',
  completed: boolean,
  scheduledDate: string,   // YYYY-MM-DD
  scheduledTime: string    // HH:MM
}
```

### Priority Color Scheme

- **High**: Red (`bg-red-50 border-red-200`, badge `bg-red-500`)
- **Medium**: Yellow (`bg-yellow-50 border-yellow-200`, badge `bg-yellow-500`)
- **Low**: Blue (`bg-blue-50 border-blue-200`, badge `bg-blue-500`)
- **Scheduled**: Green ring indicator

## Development Workflow

### Running the Application

No build step required - simply open in a browser:
```bash
# Open directly in browser
open voice-task-manager/index.html

# Or serve with any static file server
npx serve voice-task-manager
python -m http.server 8000 --directory voice-task-manager
```

### Browser Requirements

- Chrome, Edge, or Safari recommended (full Web Speech API support)
- Firefox: Text input only (no voice recognition)
- Requires microphone access for voice features

### API Key Handling

The app requires an Anthropic API key:
- Stored in `localStorage` under key `anthropic_api_key`
- Never sent to third-party servers
- Prompted on first use if not set

## Code Conventions

### React Patterns

1. **Functional Components Only**: All components use function syntax with hooks
2. **Hooks Used**: `useState`, `useEffect`, `useRef`, `useCallback`
3. **State Lifting**: Parent components own shared state, pass via props
4. **Inline Event Handlers**: Arrow functions for simple handlers

### Styling Conventions

1. **Tailwind CSS Classes**: All styling via Tailwind utility classes
2. **Custom CSS**: Only for animations (in `<style>` block)
3. **Gradient Theme**: Purple/indigo gradient (`gradient-bg`, `gradient-bg-dark`)
4. **Responsive**: Mobile-first with `md:` breakpoint prefixes

### JavaScript/JSX Style

1. **ES6+ Syntax**: Arrow functions, destructuring, template literals
2. **Async/Await**: For API calls and async operations
3. **Error Handling**: Try/catch with user-friendly error messages
4. **No TypeScript**: Plain JavaScript with JSX

## Common Operations

### Adding New Features

When adding features to the Voice Task Manager:
1. Add new components before the `App` component
2. Import any new icons as SVG components in the icons section
3. Follow the existing component structure pattern
4. Use Tailwind classes for styling

### Modifying AI Task Extraction

The AI prompt is in the `generateTasks` function (~lines 642-653):
```javascript
content: `Analyze this conversation and extract actionable tasks...`
```

To modify task extraction:
1. Update the prompt in the `generateTasks` function
2. Adjust the expected JSON structure if needed
3. Update the task processing in `tasksWithIds` mapping

### Adding New Task Properties

1. Update the task structure in `generateTasks` prompt
2. Modify the `tasksWithIds` mapping to include new fields
3. Update `TaskCard` component to display new properties
4. Adjust ICS export if the property should be included

### Calendar Export (ICS)

The `exportToCalendar` function generates ICS files:
- Location: `TaskManagementScreen` component (~lines 485-535)
- Generates standard VCALENDAR/VEVENT format
- Compatible with Google Calendar, Outlook, Apple Calendar

## Testing Considerations

Since this is a single-file browser app:

1. **Manual Testing**: Open in browser, test voice input and task flow
2. **Browser Console**: Check for JavaScript errors
3. **Network Tab**: Verify API calls to Anthropic
4. **Voice Testing**: Test in Chrome/Edge for full speech API support

### Test Scenarios

- Voice recording start/stop
- Manual text input fallback
- API key prompt and storage
- Task generation from transcript
- Auto-scheduling algorithm
- ICS file export and import
- Priority color display
- Task completion toggle
- Task deletion

## API Integration

### Anthropic API Call

```javascript
fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': apiKey,
    'anthropic-version': '2023-06-01',
    'anthropic-dangerous-direct-browser-access': 'true'
  },
  body: JSON.stringify({
    model: 'claude-sonnet-4-20250514',
    max_tokens: 2048,
    messages: [{ role: 'user', content: prompt }]
  })
});
```

Note: Uses `anthropic-dangerous-direct-browser-access` header for client-side API calls.

## Security Notes

1. **API Key Storage**: Stored in localStorage (client-side only)
2. **No Backend**: No server-side code to maintain
3. **Voice Data**: Processed locally via Web Speech API
4. **CORS**: Requires Anthropic API to allow browser requests

## Files to Never Commit

Per `.gitignore`:
- `.env` files (API keys)
- `node_modules/`
- IDE configuration (`.idea/`, `.vscode/`)
- OS files (`.DS_Store`, `Thumbs.db`)
- Build outputs (`dist/`, `build/`)
