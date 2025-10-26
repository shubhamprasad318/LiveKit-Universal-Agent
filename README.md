# LiveKit Universal Agent with RAG Integration

A comprehensive voice AI agent system that supports both browser sessions and SIP calling, with Retrieval-Augmented Generation (RAG) capabilities powered by Pinecone.

## Features

- **Dual Session Support**: Browser-based voice sessions and SIP phone calls
- **RAG Integration**: Knowledge base search with Pinecone vector database
- **Multiple LLM Backends**: OpenAI and Google Gemini support
- **Agent Configuration**: Customizable voice, personality, and behavior
- **Real-time Communication**: LiveKit-powered audio streaming
- **Modern UI**: React/Next.js frontend with Tailwind CSS

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   FastAPI       │    │   LiveKit       │    │   External      │
│   (Next.js)     │◄──►│   Backend       │◄──►│   Worker        │◄──►│   Services      │
│                 │    │   (api.py)      │    │   (main.py)     │    │                 │
│ • Agent Config  │    │ • Agent Mgmt    │    │ • Voice Agent   │    │ • LiveKit       │
│ • Knowledge UI  │    │ • RAG Processing│    │ • RAG Queries   │    │ • Pinecone      │
│ • SIP Interface │    │ • Call Dispatch │    │ • SIP Support   │    │ • OpenAI/Gemini │
│ • Voice Session │    │ • Document Mgmt │    │ • Multi-LLM     │    │ • SIP Provider  │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Quick Start

### 1. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv ai
source ai/bin/activate  # On Windows: ai\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Copy environment file
cp env.example .env

# Edit .env with your configuration
# Required: LIVEKIT_URL, LIVEKIT_API_KEY, LIVEKIT_API_SECRET
# Required: OPENAI_API_KEY, PINECONE_API_KEY
# Optional: SIP_OUTBOUND_TRUNK_ID for SIP calling

# Start the FastAPI server (for agent management)
python api.py

# In another terminal, start the LiveKit agent worker
python main.py
```

### 2. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Copy environment file
cp env.example .env.local

# Edit .env.local with your configuration
# Required: LIVEKIT_URL, LIVEKIT_API_KEY, LIVEKIT_API_SECRET

# Start the development server
npm run dev
```

### 3. Start the Application

```bash
# Terminal 1 - FastAPI Backend
cd backend
source ai/bin/activate  # or ai\Scripts\activate.bat on Windows
python api.py

# Terminal 2 - LiveKit Worker
cd backend
source ai/bin/activate  # or ai\Scripts\activate.bat on Windows
python main.py

# Terminal 3 - Frontend
cd frontend
npm run dev
```

### 4. Access the Application

Open [http://localhost:3000](http://localhost:3000) in your browser.

## Configuration

### Backend Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `LIVEKIT_URL` | LiveKit server URL | Yes |
| `LIVEKIT_API_KEY` | LiveKit API key | Yes |
| `LIVEKIT_API_SECRET` | LiveKit API secret | Yes |
| `OPENAI_API_KEY` | OpenAI API key for embeddings | Yes |
| `PINECONE_API_KEY` | Pinecone API key | Yes |
| `PINECONE_INDEX_NAME` | Pinecone index name | Yes |
| `SIP_OUTBOUND_TRUNK_ID` | SIP trunk ID for outbound calls | No |

### Frontend Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `LIVEKIT_URL` | LiveKit server URL | Yes |
| `LIVEKIT_API_KEY` | LiveKit API key | Yes |
| `LIVEKIT_API_SECRET` | LiveKit API secret | Yes |
| `NEXT_PUBLIC_ENABLE_SIP_CALLS` | Enable SIP calling UI | No |
| `NEXT_PUBLIC_ENABLE_KNOWLEDGE_BASE` | Enable knowledge base UI | No |

## Usage

### 1. Agent Configuration

Configure your AI agent's:
- **Name & Persona**: Customize the agent's identity and behavior
- **Voice Settings**: Choose from OpenAI or Google voices
- **LLM Preferences**: Select between OpenAI and Gemini
- **Temperature**: Control response creativity (0.0-1.0)
- **Session Type**: Browser or SIP calling

### 2. Knowledge Base Search

- Search the agent's knowledge base for relevant information
- View search results with source attribution and confidence scores
- The agent automatically uses knowledge base during conversations

### 3. SIP Calling

- Enter a phone number in international format
- Configure agent settings for the call
- Initiate outbound SIP calls with your AI agent
- Real-time voice processing with RAG capabilities

### 4. Voice Sessions

- Start browser-based voice conversations
- Real-time audio streaming with LiveKit
- Automatic knowledge base integration
- Multi-turn conversations with context

## API Endpoints

### Frontend API Routes

- `POST /api/connection-details` - Create LiveKit session with agent config
- `POST /api/agents` - Configure and manage agents
- `POST /api/knowledge` - Search knowledge base

### Backend Agent Worker

The Python backend runs as a LiveKit agent worker that:
- Handles both browser and SIP sessions
- Performs RAG queries against Pinecone
- Manages agent configuration via metadata
- Provides real-time voice processing

## Development

### Project Structure

```
├── backend/
│   ├── main.py              # LiveKit agent worker
│   ├── requirements.txt     # Python dependencies
│   └── env.example         # Environment template
├── frontend/
│   ├── app/                # Next.js app directory
│   │   ├── api/            # API routes
│   │   └── components/     # React components
│   ├── components/         # Shared components
│   │   ├── agent-config.tsx
│   │   ├── knowledge-base.tsx
│   │   └── sip-caller.tsx
│   ├── lib/               # Utilities and types
│   └── env.example        # Environment template
└── README.md
```

### Adding New Features

1. **Backend**: Modify `main.py` to add new agent capabilities
2. **Frontend**: Create new components in `components/` directory
3. **API**: Add new routes in `app/api/` directory
4. **Configuration**: Update `lib/types.ts` and `app-config.ts`

## Troubleshooting

### Common Issues

1. **Connection Failed**: Check LiveKit credentials and network connectivity
2. **No Audio**: Ensure microphone permissions are granted
3. **SIP Calls Fail**: Verify SIP trunk configuration and phone number format
4. **Knowledge Base Empty**: Check Pinecone configuration and index setup

### Debug Mode

Enable debug logging by setting:
```bash
# Backend
export LOG_LEVEL=DEBUG

# Frontend
NEXT_PUBLIC_DEBUG=true
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support and questions:
- Check the [LiveKit documentation](https://docs.livekit.io/)
- Review the [Pinecone documentation](https://docs.pinecone.io/)
- Open an issue on GitHub
