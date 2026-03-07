# Strategicadvisor – Your Digital AI Strategic Advisor

Strategicadvisor is a production-style AI assistant designed to act as a **digital strategic advisor for technology and delivery leaders**.  
It combines a clean, modern web experience with a robust FastAPI backend and the OpenAI API to show how you can take an AI concept from idea to a running product.

This project is intentionally opinionated: it is built to demonstrate how I think about **strategy, delivery, and real-world AI deployment** – the same way I would approach an internal tool or a customer-facing product in your organization.

---

## The story: why build a Digital Strategic Advisor?

Technology and delivery leaders rarely have time to step back and think deeply about:

- How to **prioritize strategic initiatives**
- Where **delivery bottlenecks** are really coming from
- How to **introduce AI into existing products and teams** without derailing roadmaps

Strategicadvisor explores a simple idea:

> What if a manager could open a browser and have an always‑available, consistent, opinionated AI that speaks the language of strategy, agile delivery, and AI deployment?

This project turns that idea into a working prototype that:

- Encodes a **persistent “advisor personality”** via the backend’s `ed.txt` prompt
- Accepts natural language questions about **Agile, delivery, and AI in production**
- Responds with tailored guidance suitable for leaders, not just engineers

---

## High-level architecture

At a glance:

- **Frontend** – `frontend/`
  - Next.js application (App Router) with a single-page experience
  - `app/page.tsx` renders the main layout and hosts the chat component
  - `components/strategicai.tsx` implements the chat experience:
    - Message history (user and assistant)
    - Loading states and inline error feedback
    - Keyboard shortcuts (Enter to send)
    - A polished, card-based UI built with utility CSS classes

- **Backend** – `backend/`
  - `server.py` is a FastAPI application exposing:
    - `GET /` – simple welcome endpoint
    - `GET /health` – health check for monitoring/uptime
    - `POST /chat` – main AI interaction endpoint
  - Uses `pydantic` models to validate input/output
  - Uses `python-dotenv` to load environment variables
  - Reads the advisor “personality” from `ed.txt` and injects it as a system prompt
  - Wraps calls to the OpenAI client (`gpt-4o-mini` model)

- **AI layer**
  - Stateless per-request interaction (no server-side chat history)
  - Frontend sends a `session_id` which can be used for future extensions (e.g., adding memory or analytics)
  - System prompt in `ed.txt` is where the advisor’s tone, expertise, and guardrails live

---

## Key user scenarios

A manager can use Strategicadvisor to:

- **Stress‑test a strategy**  
  “We’re planning to roll out a new platform – what risks am I not seeing?”

- **Diagnose delivery issues**  
  “Our teams keep missing sprint goals. What should I look at first?”

- **Explore AI in production**  
  “How would you phase an AI feature rollout to minimize risk and maximize learning?”

- **Prepare for stakeholder conversations**  
  “Help me explain the value of investing in observability to non-technical executives.”

These scenarios are the backbone of how I think about AI: not as a novelty, but as a **lever for better conversations and better decisions**.

---

## Running the project locally

You can run the backend and frontend separately on your machine.

### 1. Backend (FastAPI)

From `backend/`:

```bash
pip install -r requirements.txt  # or use your preferred env manager

# Set your OpenAI key in .env
echo "OPENAI_API_KEY=your_api_key_here" > .env

# Optionally configure CORS origins (default: http://localhost:3000)
echo "CORS_ORIGINS=http://localhost:3000" >> .env

# Run the API
python server.py
```

This will start the FastAPI server on `http://localhost:8000`.

Useful endpoints:

- `GET /` → basic API info
- `GET /health` → returns `{ "status": "healthy" }`
- `POST /chat` → main chat endpoint used by the frontend

### 2. Frontend (Next.js)

From `frontend/`:

```bash
npm install
npm run dev
```

Then open `http://localhost:3000` in your browser.

The frontend is configured (in `components/strategicai.tsx`) to call the backend at `http://localhost:8000/chat` by default.

---

## Implementation details worth noting

- **Typed request/response contracts**  
  `ChatRequest` and `ChatResponse` models keep the API clear and explicit, making it easy to extend with things like:
  - Conversation topics
  - Organization IDs
  - User roles or permissions

- **Personality as a first‑class artifact**  
  Instead of burying the system prompt inside the code, it’s stored in `ed.txt`, making it:
  - Easy to iterate on
  - Easy to version control
  - Easy to review in code review as a core part of behavior

- **Error handling and UX**  
  The frontend:
  - Shows a loading indicator while waiting for responses
  - Displays a friendly error message if the backend fails
  - Maintains a chat-like timeline with timestamps for each message

---

## How a hiring manager can read this repo

If you’re looking at this as part of a recruiting process, a few suggested entry points:

- **Product sense**  
  Look at how the UI copy, layout, and “Digital Strategic Advisor” concept are framed in `app/page.tsx` and the chat header/empty state.

- **AI integration mindset**  
  Check `backend/server.py` and `ed.txt` to see how the personality and constraints are encoded. This reflects how I would work with your domain SMEs to shape an AI assistant.

- **Engineering practices**  
  Observe:
  - How concerns are separated between frontend and backend
  - How configuration and secrets are handled using environment variables
  - How endpoints, models, and components are named and structured

---

## Possible next steps and extensions

To grow this prototype into a more production-grade internal tool, I would explore:

- **Persistent conversations and context** (session-based memory, history)
- **Integration with delivery tools** (e.g., Jira, Azure DevOps, GitHub) to ground advice in real data
- **Authentication and authorization** for team-specific advisors
- **Analytics and insight dashboards** to show how leaders are using the advisor
- **Deployment pipeline** (containerization, CI/CD, observability, and logging)

These are deliberate next steps: the current version is a clean, minimal core you can reason about quickly, designed to show how I approach building and shaping AI-powered products.

