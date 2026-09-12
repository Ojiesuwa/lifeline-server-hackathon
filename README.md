# Lifeline Server

Lifeline Server is a Node.js emergency coordination backend that listens for incident reports over WebSocket, finds nearby contacts and emergency services, and calls them one by one through an AI-powered response flow.

## What it does

When a client sends an emergency report, the server:

- analyzes the situation using the Strands AI agent,
- finds the closest responders and contacts by location,
- calls each one sequentially,
- tracks whether they picked up, declined, or failed,
- returns a final incident summary to the client.

The application is built for rapid emergency-response workflows and is designed to work with voice-based follow-up actions.

## Tech stack

- Node.js
- Express
- WebSocket server (`ws`)
- Strands Agents SDK
- OpenAI-compatible Groq model
- ElevenLabs voice integration

## Project structure

```text
lifeline-server/
├── src/
│   ├── agent.js
│   ├── server.js
│   ├── data/
│   │   └── data.js
│   ├── tools/
│   │   ├── call.js
│   │   └── contact.js
│   └── utils/
│       └── getElevenLabsSignedUrl.js
├── .env
├── .gitignore
├── package.json
├── README.md
└── package-lock.json
```

## Setup

1. Install dependencies:

```bash
npm install
```

2. Add your environment variables in a `.env` file:

```env
GROQ_API_KEY=your_groq_api_key
ELEVENLABS_API_KEY=your_elevenlabs_api_key
ELEVENLABS_AGENT_ID=your_elevenlabs_agent_id
```

3. Start the server:

```bash
npm run dev
```

The app runs on port `4000`.

## WebSocket usage

Connect to the server and send a message like:

```json
{
  "type": "START_INCIDENT",
  "emergency": "A fire broke out near Marina Road and people may be trapped."
}
```

The backend responds with live status events and a final `OPERATION_COMPLETE` summary.

## Main logic

- [src/server.js](src/server.js) — WebSocket server and incident trigger
- [src/agent.js](src/agent.js) — emergency coordination agent
- [src/tools/contact.js](src/tools/contact.js) — closest contact lookup by distance
- [src/tools/call.js](src/tools/call.js) — outbound call flow and result tracking
- [src/data/data.js](src/data/data.js) — nearby emergency service and contact dataset

## License

ISC License
