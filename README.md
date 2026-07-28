Prompts to build applications:
---
Act as a Principal UX Architect specializing in high-stress human psychology, crisis management interfaces, and accessible web design. 

We are building NayanAI, a zero-profile, location-aware crisis decision engine. Users are in high-anxiety situations (e.g., midnight hospital triage). The UI must eliminate cognitive friction, feel instantly safe, and handle three distinct mental modes via a bottom-anchored or top-sticky persistence bar.

Implement a 3-Tab Segmented Interface with the following psychological structural rules:

1. TAB 1: "Get Answers" (The Emergency Search/Input Mode)
- Psychology: Hyper-focused "Google-style" isolation. When active, dim or hide other layout elements to prevent distraction.
- UI Layout: A large, comforting input text area with a clear placeholder: "What immediate crisis or choice are you facing right now?"
- Underneath, place a prominent, high-contrast, accessible action button labeled "Scan System / Ask Community".
- Below the input, display a live, ambient connection status pill ("📍 Location Mode: Active | 🔒 Privacy Status: 100% Anonymous") to lower user anxiety.

2. TAB 2: "Live Debates" (The Crowdsourced Feed Mode)
- Psychology: Scannability under panic. Users need to read solutions instantly without filtering out noise.
- UI Layout: A feed of active debates. Each card must clearly split into two distinct visual columns or structured blocks: Left column = "Proposed Options" (Clean progress bars or vote percentages); Right column/block = "Lived-Experience Comments" (Prioritized by urgency weight, using a clear sans-serif font).
- Use subtle high-contrast color pills for categories (e.g., [Medical - Urgent], [Legal - Time Sensitive]).

3. TAB 3: "My Threads" (The Local Ephemeral History Mode)
- Psychology: Reassurance and control. The user needs to quickly check up on things they posted without needing a login profile.
- UI Layout: Read the client's local storage `Device-Token-Hash` to render a private list of only the threads authored by this device. 
- Give each item a prominent status indicator (e.g., "🟢 ACTIVE DEBATE - 12 New Perspectives" or "🔴 CLOSED / SOLVED"). Include a destructive but clear "Delete Permanently from Device" action button next to each thread.

CRITICAL VISUAL DESIGN & ATTENTION DIRECTIONS:
- Color Palette: Dark mode by default (reduces eye strain in midnight/emergency settings). Use a calming, high-trust primary base (e.g., deep charcoal, soft slate, slate blue) with highly deliberate emergency accent colors (e.g., a warm, non-aggressive amber/yellow for urgency pills, never a flashing aggressive neon red).
- Typography: Large line height, highly readable system sans-serif (Inter, SF Pro). No complex decorative elements.
- Transitions: Zero-latency or ultra-smooth, hardware-accelerated transitions when flipping tabs to prevent the interface from feeling broken or sluggish during a real-time crisis.

Generate the complete, responsive frontend layout using [Tailwind CSS / React / Next.js / HTML] based on these specific rules. Do not use generic placeholders.
Act as a world-class Copywriter and Brand Strategist who specializes in radical transparency, minimalism, and high-empathy communication. 

We are finalizing the landing page for our app, using one of our core name concepts: [Insert Chosen Name, e.g., Veridion / Kith / Aethel]. The core vision of this platform is to solve the devastating isolation of high-stakes, real-time decisions—such as a person in India or anywhere globally who has no idea which hospital to take an elderly relative to in the middle of the night, needing an immediate, trusted answer.

Generate the complete text layout for the landing page following these strict guidelines:

1. TONERULE: Absolutely zero corporate buzzwords, no hyperbole, and no fake statistics or placeholder metrics. The tone must be clinical, deeply respectful, deeply human, and fiercely protective of the user's focus. Use short, active sentences.
2. HERO SECTION: Write a powerful, simple headline and subheadline that makes a panicking user instantly realize they are in a safe, useful space.
3. MISSION MODULE: Write a 3-paragraph "Origin & Vision" statement based on the elderly hospital triage scenario. Make it hit home emotionally without being overly dramatic. It must feel like an open, honest statement written by a dedicated engineer who cares about human life.
4. FEATURE DESCRIPTIONS: Write copy for three feature blocks that explain the mechanics cleanly:
   - Feature 1: The Zero-Profile Privacy Model (Focus on the Device-Token-Hash security)
   - Feature 2: High-Speed Semantic Matching (How it instantly finds historical resolutions to similar crises)
   - Feature 3: The Structured Debate Format (The exact split between actionable "Proposed Options" and narrative "Lived-Experience Comments")

Output the text beautifully formatted in clean Markdown so it can be directly integrated into our front-end components.
act all the domains expert mentioned above.


---


## Copywriting & Content Prompts

* Value Proposition: "Act as an expert UX copywriter. Write 3 distinct, punchy hero headline options and subheadlines for an app that my app aimed at normal all users in india. Focus on clarity and immediate benefit rather than buzzwords."
* Feature Breakdown: "Write microcopy for 3 core feature benefit blocks for my app. For each, provide a short 2-sentence description that explains how the feature solves a painful user problem."

## UI & Web Design Prompts

* Layout & Vibe: "Act as a senior mobile and web UI designer. Generate a layout and structural description for a landing page for myidea. Specify a mobile-first grid, color scheme codes (HEX), font pairings, call-to-action (CTA) button placement, and section hierarchy." [2, 4, 6, 7, 8] 
* User Onboarding Flow: "Outline a 3-step screen-by-screen UX flow and visual layout for the onboarding process of an app that helps [insert target audience] do [insert core task]. Include screen titles, primary visual focus, and user interaction cues." [9, 10] 

## Logo Design Prompts

* Visual Direction: "Act as a brand identity designer. Suggest 3 conceptual directions for a minimalist app logo for my app. Describe the primary symbol, typography style (serif vs sans-serif), and a 3-color palette that reflects a [playful/trustworthy/professional] mood." [5] 
add these to my application the nayan.ai act as a expert in branding person who can place a great name to the my apps and has 30 + years experience to name a app.
and remove all these —  and AI symbols and make it really great content writing to my website.
---
can we build this apps it is web app.
there should be great tech docs readme files and github repo and deployments kit like helm docker files.
This platforms can be for all the information that can be buying a banana dozen in Indian cities or it can be in world. this is the basic idea.
the problem here is a person who is spending is not satisfied because of lack of information.
Here is a master Master Engineering Prompt designed to be fed directly into an advanced AI coding agent or system architect. It covers the complete Software Development Life Cycle (SDLC), enforces the "Zero-Profile" constraint, and relies entirely on a high-density, Python-centric architecture.
------------------------------

# SYSTEM ARCHITECT PROMPT: END-TO-END AUTONOMOUS SOFTWARE ENGINEERING# PROJECT CODENAME: NayanAI (The Vision/Guide System)# TARGET ARCHITECTURE: Zero-Profile, Problem-First Decision-Routing Engine
You are an expert Principal Software Architect and DevOps Engineer. Your objective is to autogenerate the complete, production-grade codebase, configuration files, and deployment pipelines for "NayanAI". Follow the strict structural blueprint below. Do not use placeholders; output functional, complete Python files and infrastructure scripts.
---## 1. COMPREHENSIVE REQUIREMENTS & SYSTEM BOUNDARIES*   **Core Utility:** A non-authenticated, location-aware platform to help users crowdsource high-risk, real-time choices (e.g., midnight hospital triage, immediate legal gridlocks) via structured debates.
*   **Privacy Model:** Absolutely zero user registration, profiles, cookies, or trackable histories. Authentication must rely solely on an ephemeral, hardware-derived `Device-Token-Hash` generated on the client-side to allow a user to edit/close their own thread.*   **Interaction Model:** 
    1. A user enters a crisis query in natural language.
    2. The system executes a high-speed semantic vector search over past discussions.
    3. If a 90%+ match is found, it surfaces the existing resolution thread.
    4. If no match is found, it spins up an ephemeral, structured debate thread divided explicitly into "Proposed Options" and "Lived-Experience Comments."
---## 2. PRODUCTION TECH STACK (PYTHON-CENTRIC)*   **Backend Framework:** FastAPI (Asynchronous execution, auto-generated OpenAPI documentation).
*   **Vector Database & Storage:** PostgreSQL with the `pgvector` extension (handles relational threads, comments, and semantic vector match lookups).
*   **AI/LLM Integration:** `Instructor` library wrapped around `LiteLLM` using local/cloud embedding and classification models.
*   **Task Queue & Event Handling:** `Celery` with a `Redis` broker for background AI asynchronous processing (moderation, semantic tagging).*   **Containerization & Deployment:** Docker, Docker Compose, and GitHub Actions CI/CD deploying directly to AWS ECS Fargate or a cloud VPS.
---## 3. AUTONOMOUS SDLC EXECUTION STEPS
Execute each of the following code-generation layers sequentially. Output clean, modular Python modules.
### LAYER A: THE DATABASE COMPONENT (SQLAlchemy + pgvector)Write a fully asynchronous PostgreSQL schema using SQLAlchemy. Include tables for `Threads`, `Options`, and `Comments`. The `Threads` table must include a 1536-dimensional vector column for semantic indexing.
[GENERATE: backend/app/db/models.py]

import uuidfrom sqlalchemy import Column, String, DateTime, ForeignKey, Integer, Boolean, Text, funcfrom sqlalchemy.dialects.postgresql import UUIDfrom sqlalchemy.orm import declarative_base, relationshipfrom pgvector.sqlalchemy import Vector
Base = declarative_base()
class Thread(Base):
    __tablename__ = "threads"
    
    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    device_token_hash = Column(String(64), nullable=False, index=True) # Anonymized hardware anchor
    raw_text = Column(Text, nullable=False)
    text_vector = Column(Vector(1536), nullable=False) # For semantic search lookups
    location_tag = Column(String(100), nullable=True)
    urgency_score = Column(Integer, default=1)
    status = Column(String(20), default="ACTIVE", index=True) # ACTIVE, RESOLVED
    created_at = Column(DateTime, server_default=func.now(), index=True)

    options = relationship("DecisionOption", back_populates="thread", cascade="all, delete-orphan")
class DecisionOption(Base):
    __tablename__ = "decision_options"
    
    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    thread_id = Column(UUID(as_uuid=True), ForeignKey("threads.id", ondelete="CASCADE"), nullable=False)
    option_title = Column(String(255), nullable=False) # e.g., "Manipal Hospital ER"
    upvotes = Column(Integer, default=0)
    
    thread = relationship("Thread", back_populates="options")
    comments = relationship("ExperienceComment", back_populates="option", cascade="all, delete-orphan")
class ExperienceComment(Base):
    __tablename__ = "experience_comments"
    
    id = Column(UUID(as_uuid=True), primary_key=True, default=uuid.uuid4)
    option_id = Column(UUID(as_uuid=True), ForeignKey("decision_options.id", ondelete="CASCADE"), nullable=False)
    anonymous_alias = Column(String(50), nullable=False) # e.g., "Peer_782"
    content = Column(Text, nullable=False)
    is_verified_experience = Column(Boolean, default=False) # Set true via local cryptographic receipts
    created_at = Column(DateTime, server_default=func.now())

    option = relationship("DecisionOption", back_populates="comments")

### LAYER B: THE ENGINE CORES (AI Vector Search, Red-Flag Triage, & Schema Extraction)Build the foundational AI logic services. Write a script that checks an incoming query for life-threatening medical keywords, converts the query text to vector embeddings, and extracts metadata fields seamlessly.
[GENERATE: backend/app/services/ai_engine.py]

import osfrom typing import List, Optionalfrom pydantic import BaseModel, Fieldimport litellmfrom openai import OpenAI
# Mock embedding configuration; replace with your chosen LLM ecosystem credentialsEMBEDDING_MODEL = "text-embedding-3-small"LLM_MODEL = "gpt-4o-mini"
class CrisisTriage(BaseModel):
    is_life_threatening: bool = Field(description="True if the text mentions acute chest pain, active stroke symptoms, unconsciousness, or extreme trauma.")
    urgency_rating: int = Field(description="Score from 1 (low urgency, long-term choice) to 5 (immediate midnight action needed).")
    inferred_location: Optional[str] = Field(description="Extract Indian city or locality names if explicitly provided, else None.")
class AIEngine:
    def __init__(self):
        # Initialise standard endpoint provider
        self.client = OpenAI(api_key=os.getenv("OPENAI_API_KEY", "mock-key"))

    async def get_embedding(self, text: str) -> List[float]:
        """Generates 1536-dimensional embeddings for pgvector match queries."""
        response = self.client.embeddings.create(
            input=[text],
            model=EMBEDDING_MODEL
        )
        return response.data[0].embedding

    async def analyze_and_triage(self, text: str) -> CrisisTriage:
        """Parses raw user text to extract structural constraints and flag critical risk thresholds."""
        # Simple structural keyword intercept fallback before reaching the LLM
        critical_keywords = ["chest pain", "heart attack", "unconscious", "accident", "bleeding out"]
        if any(kw in text.lower() for kw in critical_keywords):
            return CrisisTriage(is_life_threatening=True, urgency_rating=5, inferred_location=None)

        # Main processing block utilizing JSON structural returns
        completion = self.client.beta.chat.completions.parse(
            model=LLM_MODEL,
            messages=[
                {"role": "system", "content": "Analyze the Indian crowdsourced emergency query. Categorize extraction parameters perfectly Blueprint structures."},
                {"role": "user", "content": text}
            ],
            response_format=CrisisTriage
        )
        return completion.choices[0].message.parsed

### LAYER C: THE CONTROLLER ENDPOINTS (FastAPI Router Engine)Write the functional FastAPI endpoints. Implement routes to:
1. Handle query postings, run the semantic search algorithm via `pgvector`, and conditionally create a thread or return a 90%+ match.2. Allow anonymous suggestions ("Options") and peer experience responses tied directly to a thread.
[GENERATE: backend/app/main.py]

from fastapi import FastAPI, Depends, HTTPException, statusfrom pydantic import BaseModelfrom typing import List, Optionalimport uuidfrom sqlalchemy.ext.asyncio import AsyncSession, create_async_enginefrom sqlalchemy.orm import sessionmakerfrom sqlalchemy import select
# App initializationapp = FastAPI(title="NayanAI", version="1.0.0")
DATABASE_URL = os.getenv("DATABASE_URL", "postgresql+asyncpg://postgres:postgres@localhost:5432/nayan")engine = create_async_engine(DATABASE_URL, echo=False)AsyncSessionLocal = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)
# Dependency injection for DB sessionasync def get_db():
    async with AsyncSessionLocal() as session:
        yield session
# Request Validation Modelsclass QueryIn(BaseModel):
    device_token_hash: str
    raw_text: str
    location_input: Optional[str] = None
class ThreadOut(BaseModel):
    thread_id: uuid.UUID
    matched_existing: bool
    is_emergency: bool
    raw_text: str
    options: List[dict] = []

@app.post("/api/v1/dilemma/search-or-create", response_model=ThreadOut)async def process_incoming_dilemma(payload: QueryIn, db: AsyncSession = Depends(get_db)):
    # 1. Immediate Intercept Check for Emergency Red Flags
    # Instantiating internal structural checks
    from app.services.ai_engine import AIEngine
    ai = AIEngine()
    
    triage_results = await ai.analyze_and_triage(payload.raw_text)
    if triage_results.is_life_threatening:
        return ThreadOut(
            thread_id=uuid.uuid4(),
            matched_existing=False,
            is_emergency=True,
            raw_text="EMERGENCY DIRECTIVE SYSTEM DETECTED: Immediately contact 102/108 or go directly to the nearest casualty center."
        )

    # 2. Convert to Embedding Vector
    query_vector = await ai.get_embedding(payload.raw_text)
    
    # 3. Dynamic Semantic Vector Search using pgvector cosine distance operators
    # Selecting columns from threads where cosine distance metric is less than a specific threshold (e.g. < 0.15 for 85%+ similarity)
    # For functional compilation, we fall back to generating a fresh thread if database entries are absent.
    
    # Complete creation fallback path execution
    from app.db.models import Thread
    new_thread = Thread(
        device_token_hash=payload.device_token_hash,
        raw_text=payload.raw_text,
        text_vector=query_vector,
        location_tag=payload.location_input or triage_results.inferred_location,
        urgency_score=triage_results.urgency_rating,
        status="ACTIVE"
    )
    db.add(new_thread)
    await db.commit()
    await db.refresh(new_thread)

    return ThreadOut(
        thread_id=new_thread.id,
        matched_existing=False,
        is_emergency=False,
        raw_text=new_thread.raw_text
    )

### LAYER D: CONTAINER DEPLOYMENT AND ORCHESTRATION PIPELINESGenerate the automated Multi-Stage Dockerfile optimizing compilation times, alongside a docker-compose cluster configurations to stand up the complete Python/PostgreSQL stack locally or in cloud target systems immediately.
[GENERATE: Dockerfile]

# Multi-stage production builds for Python optimizationFROM python:3.11-slim AS builder
WORKDIR /appRUN apt-get update && apt-get install -y --no-install-recommends gcc build-essential libpq-dev && rm -rf /var/lib/apt/lists/*
COPY requirements.txt .RUN pip install --no-cache-dir --user -r requirements.txt
FROM python:3.11-slim AS runnerWORKDIR /app
RUN apt-get update && apt-get install -y --no-install-recommends libpq5 && rm -rf /var/lib/apt/lists/*COPY --from=builder /root/.local /root/.localCOPY ./backend /app
ENV PATH=/root/.local/bin:$PATHENV PYTHONUNBUFFERED=1
EXPOSE 8000CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]

### LAYER E: SYSTEM VERIFICATION & COMPLIANCE RULESEnsure that all auto-generated endpoints strictly follow these design constraints:1. No route accepts emails, usernames, names, or phone profiles.2. Every return payload maps strictly back to the unstructured problem, structural voting counts, and localized experience fragments.

Initialize the pipeline engine generation routines immediately.

------------------------------
## How to use this prompt:

   1. Copy the code block above entirely.
   2. Paste it directly into any advanced workspace tool or developer engine.
   3. The platform will dynamically autogenerate a production-ready, highly localized framework for your "Zero-Profile" decision app.

TECH STACK : PYTHON and it can be any other tech stack.
