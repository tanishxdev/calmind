# Calmind - AI Mental Wellness Platform

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Next.js](https://img.shields.io/badge/Next.js-15-black)](https://nextjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-20-green)](https://nodejs.org/)
[![MongoDB](https://img.shields.io/badge/MongoDB-8.0-brightgreen)](https://www.mongodb.com/)

## Overview

Calmind is a full-stack AI-powered mental wellness platform that helps users manage their emotional health through conversational therapy, mood tracking, and activity-based insights. The system combines real-time AI interaction with structured user data to provide personalized recommendations and continuous mental health support.

Unlike basic chatbots, Calmind maintains conversational context across sessions and uses historical user data—mood trends, activity logs, and chat history—to influence future AI responses, creating a truly personalized and evolving support system.

## Problem Statement

Current mental health solutions face three critical gaps:

- **Accessibility:** Traditional therapy is expensive and not always available
- **Personalization:** Most digital apps provide generic content without adapting to individual users
- **Fragmentation:** No system connects mood, activities, and conversations into a unified experience

## Features

| Feature                             | Description                                              |
| ----------------------------------- | -------------------------------------------------------- |
| 🤖 **AI Therapy Chat**              | Session-based conversations with context-aware responses |
| 📊 **Mood Tracking**                | Log emotional state with scores (0-100) and notes        |
| 🏃 **Activity Logging**             | Track meditation, exercise, journaling, and more         |
| 🎯 **Personalized Recommendations** | AI-driven suggestions based on user behavior             |
| 📜 **Session History**              | Complete conversation and data history                   |
| 🔐 **Secure Authentication**        | JWT + session-based auth with expiry                     |

## Tech Stack

### Frontend

- **Next.js 15** (App Router) - React framework
- **TypeScript** - Type safety
- **Tailwind CSS** - Styling
- **React Context** - State management

### Backend

- **Node.js** - Runtime
- **Express** - API framework
- **MongoDB + Mongoose** - Database & ODM
- **JWT + bcrypt** - Authentication
- **Inngest** - Async job processing

### AI & Integrations

- **OpenAI API / LangChain** - LLM integration
- **Vercel** - Frontend deployment
- **Render / VPS** - Backend hosting
- **MongoDB Atlas** - Cloud database

## Architecture

```

┌─────────────────────────────────────────────────────────────┐
│ Client (Browser) │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Next.js Frontend (Vercel) │
│ Pages → Components → API Layer → State │
└─────────────────────────────────────────────────────────────┘
                            │
                            REST API
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Express Backend (Node.js) │
│ Routes → Middleware → Controllers → Services → Models │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ MongoDB Atlas │
│ User | Session | ChatSession | Mood | Activity | Recs │
└─────────────────────────────────────────────────────────────┘

```

### Data Flow Example (Chat)

```

User sends message
↓
React Component captures input
↓
API Layer sends POST request
↓
Auth Middleware verifies JWT
↓
Controller extracts message & session
↓
AI Service calls LLM (with history)
↓
Response + metadata stored in ChatSession
↓
Response returned to frontend
↓
UI updates with AI response

```

## Data Models

```typescript
// Core models centered around User
User {
  name, email, password (hashed)
}

Session {
  userId, token, expiresAt (TTL), lastActive, deviceInfo
}

ChatSession {
  sessionId, userId, messages[], status, startTime
  // messages: { role, content, timestamp, metadata }
  // metadata: { emotionalState, riskLevel, currentGoal }
}

Mood {
  userId, score (0-100), note, context, timestamp
}

Activity {
  userId, type, name, duration, timestamp
}

Recommendation {
  userId, items[]
}
```

## API Endpoints

| Method | Endpoint                          | Description              |
| ------ | --------------------------------- | ------------------------ |
| `POST` | `/api/auth/register`              | User registration        |
| `POST` | `/api/auth/login`                 | User login (returns JWT) |
| `GET`  | `/api/auth/me`                    | Get current user         |
| `POST` | `/api/chat/sessions`              | Create chat session      |
| `POST` | `/api/chat/sessions/:id/messages` | Send message             |
| `GET`  | `/api/chat/sessions/:id/history`  | Get session history      |
| `POST` | `/api/mood`                       | Log mood                 |
| `GET`  | `/api/mood`                       | Get mood history         |
| `POST` | `/api/activity`                   | Log activity             |
| `GET`  | `/api/activity`                   | Get activity history     |

## Getting Started

### Prerequisites

- Node.js 20+
- MongoDB (local or Atlas)
- OpenAI API key

### Installation

```bash
# Clone repository
git clone https://github.com/yourusername/calmind.git
cd calmind

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env.local

# Configure .env.local
MONGODB_URI=your_mongodb_uri
JWT_SECRET=your_jwt_secret
OPENAI_API_KEY=your_openai_key

# Run development server
npm run dev
```

### Environment Variables

```env
# Database
MONGODB_URI=mongodb://localhost:27017/calmind

# Authentication
JWT_SECRET=your_super_secret_key
JWT_EXPIRES_IN=7d

# AI Services
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4-turbo

# App
NEXT_PUBLIC_API_URL=http://localhost:3000/api
```

## Key Technical Decisions

| Decision                            | Why                                                        |
| ----------------------------------- | ---------------------------------------------------------- |
| **Session-based chat**              | Maintains context across messages for meaningful responses |
| **Hybrid auth (JWT + DB sessions)** | JWT for scalability, DB for control & revocation           |
| **MongoDB**                         | Flexible schema for evolving AI metadata                   |
| **Service layer**                   | Separates AI/recommendation logic from controllers         |
| **Async AI processing**             | Prevents blocking main request cycle                       |

## Security

- ✅ JWT-based authentication with database sessions
- ✅ bcrypt password hashing
- ✅ Input validation & sanitization
- ✅ Protected routes with auth middleware
- ✅ Session expiry via TTL indexes
- ✅ Data isolation (userId-scoped queries)

## Deployment

| Component          | Platform             |
| ------------------ | -------------------- |
| Frontend (Next.js) | Vercel               |
| Backend (Node.js)  | Render / VPS         |
| Database           | MongoDB Atlas        |
| CI/CD              | GitHub + Auto-deploy |

## Future Improvements

- [ ] Real-time chat using WebSockets
- [ ] Enhanced emotion detection (NLP sentiment analysis)
- [ ] Mobile application (React Native)
- [ ] Advanced analytics dashboard
- [ ] Redis caching layer
- [ ] Microservices architecture

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing`)
5. Open a Pull Request

## License

MIT © Calmind

---

## Contact

**Project Link:** [https://github.com/tanishxdev/calmind](https://github.com/tanishxdev/calmind)

---

_Built with ❤️ for mental wellness accessibility_
