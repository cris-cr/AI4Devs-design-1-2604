# LTI - ATS System Design

## Table of Contents
1. [Software Description & Business Model](#1-software-description--business-model)
2. [Main Use Cases](#2-main-use-cases)
3. [Data Model](#3-data-model)
4. [High-Level System Design](#4-high-level-system-design)
5. [C4 Diagram](#5-c4-diagram)

---

## 1. Software Description & Business Model

### Brief Description

**LTI (Let's Track It)** is a next-generation Applicant Tracking System (ATS) built for the modern hiring landscape. LTI combines intelligent automation, real-time collaboration, and AI-powered insights to help HR departments and hiring managers attract, evaluate, and hire the best talent — faster and smarter than ever before.

### Added Value & Competitive Advantages

- **AI-assisted screening:** Automatically ranks and filters candidates based on job requirements, reducing time-to-shortlist by up to 70%.
- **Real-time collaboration:** Recruiters and hiring managers can comment, rate, and make decisions on candidates simultaneously, eliminating email chains and delayed feedback.
- **Automated workflows:** Configurable pipeline stages with automated notifications, scheduling, and follow-up actions reduce manual administrative work.
- **Bias reduction tools:** AI flags potentially biased language in job descriptions and anonymizes CV reviews to promote fair hiring.
- **Predictive analytics:** Dashboards surfacing hiring funnel metrics, time-to-hire, source effectiveness, and candidate drop-off insights.
- **Seamless integrations:** Native connectors to LinkedIn, job boards, HRIS platforms (Workday, BambooHR), and calendar systems (Google, Outlook).
- **Candidate experience portal:** A branded, mobile-friendly portal where candidates can track their application status, schedule interviews, and communicate directly with the team.

### Main Features

1. **Job Posting Management** – Create, publish, and manage job openings across multiple channels from a single interface.
2. **Candidate Pipeline** – Visual Kanban-style board to manage candidates across hiring stages (Applied → Screening → Interview → Offer → Hired/Rejected).
3. **AI Resume Parsing & Scoring** – Automatically extract structured data from CVs and score candidates against the job requirements.
4. **Interview Scheduling** – Integrated calendar tool allowing one-click interview scheduling with automatic notifications to all parties.
5. **Collaborative Evaluation** – Scorecards, notes, and star ratings shared in real time among the hiring team.
6. **Offer Management** – Generate, send, and track digital offer letters with e-signature capabilities.
7. **Analytics & Reporting** – Customizable dashboards with KPIs: time-to-hire, cost-per-hire, source quality, diversity metrics.
8. **Compliance & GDPR Tools** – Automated data retention policies, candidate consent management, and audit trails.

---

### Lean Canvas

```mermaid
block-beta
  columns 3

  block:problem["**PROBLEM**\n\n- Fragmented tools (email, spreadsheets, disconnected ATS)\n- Slow screening processes\n- Poor collaboration between HR & managers\n- Bad candidate experience"]:1
  block:solution["**SOLUTION**\n\n- Unified AI-powered ATS\n- Automated screening & scoring\n- Real-time collaboration workspace\n- Candidate self-service portal"]:1
  block:uvp["**UNIQUE VALUE PROPOSITION**\n\nThe ATS that lets recruiters and managers hire smarter together — with AI doing the heavy lifting"]:1

  block:unfair["**UNFAIR ADVANTAGE**\n\n- Proprietary AI scoring model trained on millions of hiring outcomes\n- Real-time collab engine (no page refresh needed)\n- Deep HRIS integrations out of the box"]:1
  block:customer_segments["**CUSTOMER SEGMENTS**\n\n- Mid-size to large companies (50–5000 employees)\n- High-growth startups scaling fast\n- Staffing & recruitment agencies"]:1
  block:channels["**CHANNELS**\n\n- Direct sales (B2B SaaS)\n- LinkedIn & HR events\n- Integration marketplace\n- Referral program"]:1

  block:metrics["**KEY METRICS**\n\n- Time-to-hire reduction\n- Monthly Active Recruiters\n- Candidate NPS\n- Pipeline conversion rate"]:1
  block:cost["**COST STRUCTURE**\n\n- Cloud infrastructure (AWS/GCP)\n- AI model training & inference\n- Sales & marketing\n- Customer success team"]:1
  block:revenue["**REVENUE STREAMS**\n\n- Monthly SaaS subscription (per seat)\n- Enterprise annual contracts\n- Premium add-ons (AI insights, white-label)"]:1
```

> *Note: The Lean Canvas above is rendered as a block diagram. For a traditional Lean Canvas table view, see below.*

| | | |
|---|---|---|
| **Problem** | **Solution** | **Unique Value Proposition** |
| Fragmented tools, slow screening, poor HR-manager collaboration, bad candidate experience | Unified AI-powered ATS, automated screening & scoring, real-time collaboration, candidate self-service portal | "The ATS that lets recruiters and managers hire smarter together — with AI doing the heavy lifting" |
| **Unfair Advantage** | **Customer Segments** | **Channels** |
| Proprietary AI scoring model, real-time collab engine, deep HRIS integrations | Mid-size to large companies, high-growth startups, staffing agencies | Direct B2B sales, LinkedIn, HR events, integration marketplace, referrals |
| **Key Metrics** | **Cost Structure** | **Revenue Streams** |
| Time-to-hire reduction, Monthly Active Recruiters, Candidate NPS, pipeline conversion rate | Cloud infra, AI training/inference, sales & marketing, CS team | Per-seat SaaS subscription, enterprise annual contracts, premium AI add-ons |

---

## 2. Main Use Cases

### Use Case 1: AI-Powered Candidate Screening

**Description:** A recruiter creates a job posting and the system automatically screens incoming applications using AI, scoring and ranking candidates based on skills, experience, and cultural fit indicators — surfacing the top candidates for human review.

**Actors:** Recruiter, AI Screening Engine, Candidate

```mermaid
flowchart TD
    A([Candidate]) -->|Submits application| B[Application Received]
    B --> C[AI Resume Parser]
    C --> D[Extract structured data\nfrom CV]
    D --> E[AI Scoring Engine]
    E --> F{Score >= threshold?}
    F -->|Yes| G[Move to Shortlist]
    F -->|No| H[Auto-reject with\npolite notification]
    G --> I([Recruiter])
    I --> J[Review ranked shortlist]
    J --> K{Decision}
    K -->|Advance| L[Move to Phone Screen stage]
    K -->|Reject| H
    H --> M([Candidate notified\nvia email/portal])
    L --> M
```

**Preconditions:**
- A Job Opening has been published in the system.
- The candidate has submitted their application with a CV.

**Main Flow:**
1. Candidate submits an application through the careers portal or job board integration.
2. The AI Resume Parser extracts structured data (skills, experience, education, contact info).
3. The AI Scoring Engine compares the candidate profile against the job requirements and generates a match score (0–100).
4. Candidates above the configured threshold are moved to the "Shortlist" stage; those below receive an automated rejection.
5. The recruiter reviews the ranked shortlist and makes advancement decisions.

**Postconditions:**
- Candidate status is updated in the pipeline.
- Candidate receives an automated notification about their application status.

---

### Use Case 2: Real-Time Collaborative Interview Evaluation

**Description:** After a candidate completes an interview, multiple stakeholders (recruiter, hiring manager, technical interviewer) independently submit structured scorecards. The system aggregates scores and facilitates a team decision in real time.

**Actors:** Recruiter, Hiring Manager, Technical Interviewer, System

```mermaid
sequenceDiagram
    participant R as Recruiter
    participant S as LTI System
    participant HM as Hiring Manager
    participant TI as Technical Interviewer
    participant C as Candidate

    R->>S: Schedule interview & assign evaluators
    S->>HM: Send interview invitation + scorecard link
    S->>TI: Send interview invitation + scorecard link
    S->>C: Send interview confirmation & calendar invite

    Note over C,S: Interview takes place

    HM->>S: Submit completed scorecard (ratings + notes)
    TI->>S: Submit completed scorecard (ratings + notes)
    R->>S: Submit completed scorecard (ratings + notes)

    S->>S: Aggregate scores & detect consensus/conflict
    S->>R: Notify: all scorecards received
    R->>S: Open collaborative decision panel
    S->>HM: Real-time panel notification
    S->>TI: Real-time panel notification

    HM->>S: Vote: Advance to Offer
    TI->>S: Vote: Advance to Offer
    R->>S: Confirm decision: Advance to Offer

    S->>C: Notify: Moving to next stage
    S->>R: Update pipeline stage → Offer
```

**Preconditions:**
- Candidate is in the "Interview" stage.
- At least one interview has been completed.

**Main Flow:**
1. Recruiter schedules the interview and assigns evaluators via the system.
2. All parties receive calendar invites and evaluation scorecards.
3. After the interview, each evaluator independently submits their scorecard (ratings on competencies + free-text notes).
4. The system aggregates scores and notifies the recruiter when all scorecards are submitted.
5. The team convenes in a real-time decision panel to discuss and vote.
6. Once consensus is reached, the pipeline is updated and the candidate is notified.

**Postconditions:**
- Candidate's pipeline stage is updated.
- All scorecards are stored and auditable.
- Candidate is notified of next steps.

---

### Use Case 3: Offer Generation & Digital Signature

**Description:** Once a candidate is approved for hire, the recruiter generates a formal offer letter, customizes the terms, and sends it for digital signature. The system tracks the offer status in real time.

**Actors:** Recruiter, Hiring Manager, Candidate, E-signature Service

```mermaid
stateDiagram-v2
    [*] --> CandidateApproved : Team decides to hire

    CandidateApproved --> OfferDraft : Recruiter initiates offer

    OfferDraft --> OfferReview : Recruiter fills in terms\n(salary, start date, role)

    OfferReview --> OfferApproved : Hiring Manager approves
    OfferReview --> OfferDraft : Hiring Manager requests changes

    OfferApproved --> OfferSent : System sends offer\nvia email + portal

    OfferSent --> OfferViewed : Candidate opens offer

    OfferViewed --> OfferSigned : Candidate signs digitally
    OfferViewed --> OfferNegotiation : Candidate requests changes
    OfferViewed --> OfferDeclined : Candidate declines

    OfferNegotiation --> OfferDraft : Recruiter revises offer

    OfferSigned --> Hired : System updates pipeline\n& notifies HR
    OfferDeclined --> [*] : Candidate marked as\nDeclined / Re-open role

    Hired --> [*]
```

**Preconditions:**
- Candidate has reached the "Offer" stage after team approval.
- Offer template has been configured for the relevant job role.

**Main Flow:**
1. Recruiter selects the candidate and initiates offer creation, choosing a pre-built template.
2. Recruiter customizes compensation, start date, and other terms.
3. Hiring Manager reviews and approves the offer within the system.
4. System sends the offer letter to the candidate via email and through the candidate portal.
5. Candidate opens, reviews, and digitally signs the offer.
6. System notifies the recruiter and HR team; pipeline is updated to "Hired".

**Postconditions:**
- Signed offer letter is stored and linked to the candidate record.
- Candidate status is set to "Hired".
- Onboarding workflow can be triggered.

---

## 3. Data Model

### Entity-Relationship Diagram

```mermaid
erDiagram
    COMPANY {
        uuid id PK
        string name
        string industry
        string size
        string website
        string logo_url
        timestamp created_at
    }

    USER {
        uuid id PK
        uuid company_id FK
        string email
        string full_name
        string role
        string avatar_url
        boolean is_active
        timestamp created_at
        timestamp last_login
    }

    JOB_OPENING {
        uuid id PK
        uuid company_id FK
        uuid created_by FK
        string title
        string department
        string location
        string employment_type
        string status
        text description
        text requirements
        integer salary_min
        integer salary_max
        string currency
        date open_date
        date close_date
        timestamp created_at
    }

    CANDIDATE {
        uuid id PK
        string email
        string full_name
        string phone
        string linkedin_url
        string location
        string current_title
        string source
        boolean gdpr_consent
        timestamp gdpr_consent_date
        timestamp created_at
    }

    APPLICATION {
        uuid id PK
        uuid job_opening_id FK
        uuid candidate_id FK
        string stage
        integer ai_score
        string ai_recommendation
        string status
        timestamp applied_at
        timestamp updated_at
    }

    CV {
        uuid id PK
        uuid candidate_id FK
        string file_url
        string file_name
        text parsed_text
        json structured_data
        timestamp uploaded_at
    }

    INTERVIEW {
        uuid id PK
        uuid application_id FK
        uuid scheduled_by FK
        string interview_type
        datetime scheduled_at
        integer duration_minutes
        string location_or_link
        string status
        timestamp created_at
    }

    INTERVIEW_PARTICIPANT {
        uuid id PK
        uuid interview_id FK
        uuid user_id FK
        string role
    }

    SCORECARD {
        uuid id PK
        uuid interview_id FK
        uuid evaluator_id FK
        integer overall_score
        string recommendation
        text notes
        json competency_scores
        timestamp submitted_at
    }

    OFFER {
        uuid id PK
        uuid application_id FK
        uuid created_by FK
        string status
        integer salary
        string currency
        string benefits
        date start_date
        string contract_type
        string document_url
        timestamp sent_at
        timestamp signed_at
        timestamp expires_at
    }

    PIPELINE_STAGE {
        uuid id PK
        uuid job_opening_id FK
        string name
        integer order_index
        boolean auto_reject_below_score
        integer rejection_threshold
    }

    TAG {
        uuid id PK
        uuid company_id FK
        string name
        string color
    }

    CANDIDATE_TAG {
        uuid candidate_id FK
        uuid tag_id FK
    }

    ACTIVITY_LOG {
        uuid id PK
        uuid company_id FK
        uuid user_id FK
        uuid entity_id
        string entity_type
        string action
        json metadata
        timestamp occurred_at
    }

    COMPANY ||--o{ USER : "has"
    COMPANY ||--o{ JOB_OPENING : "creates"
    COMPANY ||--o{ TAG : "defines"
    USER ||--o{ JOB_OPENING : "creates"
    JOB_OPENING ||--o{ APPLICATION : "receives"
    JOB_OPENING ||--o{ PIPELINE_STAGE : "has"
    CANDIDATE ||--o{ APPLICATION : "submits"
    CANDIDATE ||--o{ CV : "has"
    CANDIDATE ||--o{ CANDIDATE_TAG : "labeled_with"
    TAG ||--o{ CANDIDATE_TAG : "applied_to"
    APPLICATION ||--o{ INTERVIEW : "generates"
    APPLICATION ||--|{ OFFER : "results_in"
    INTERVIEW ||--o{ INTERVIEW_PARTICIPANT : "includes"
    INTERVIEW ||--o{ SCORECARD : "produces"
    USER ||--o{ SCORECARD : "submits"
    USER ||--o{ INTERVIEW_PARTICIPANT : "participates_in"
    USER ||--o{ OFFER : "creates"
    USER ||--o{ ACTIVITY_LOG : "generates"
```

### Key Entities Summary

| Entity | Description |
|---|---|
| **Company** | Tenant organization using LTI |
| **User** | Recruiter, Hiring Manager, or Admin within a company |
| **Job Opening** | A position to be filled |
| **Candidate** | A person applying for one or more positions |
| **Application** | Link between a candidate and a job opening, with pipeline stage |
| **CV** | Resume file + parsed/structured data |
| **Interview** | A scheduled interview session tied to an application |
| **Interview Participant** | User assigned to attend and evaluate an interview |
| **Scorecard** | Evaluation submitted by an interviewer after an interview |
| **Offer** | Formal job offer tied to an application, with e-signature tracking |
| **Pipeline Stage** | Custom stages defined per job opening |
| **Tag** | Labels for organizing and filtering candidates |
| **Activity Log** | Audit trail of all system actions |

---

## 4. High-Level System Design

### Architecture Overview

LTI is built as a **cloud-native, multi-tenant SaaS application** using a **microservices architecture** deployed on AWS. The system is designed for high availability, horizontal scalability, and data isolation between tenant organizations.

The architecture is organized into the following layers:

**Frontend Layer:** A React-based Single Page Application (SPA) delivered via a CDN (CloudFront). The candidate-facing portal is a separate, lightweight Next.js application to optimize public performance and SEO. Both clients communicate with the backend exclusively through a versioned REST/GraphQL API.

**API Gateway Layer:** AWS API Gateway handles authentication (JWT validation via Cognito), rate limiting, request routing, and SSL termination. All traffic flows through this single entry point.

**Backend Services Layer:** The core business logic is decomposed into independent microservices, each owning its own database:
- **Jobs Service** – manages job openings, pipeline configuration, and multi-board publishing.
- **Applications Service** – tracks application lifecycle and pipeline stage transitions.
- **Candidates Service** – stores and manages candidate profiles and CV data.
- **AI Service** – handles resume parsing, candidate scoring, and bias detection using ML models.
- **Interview Service** – manages scheduling, calendar integrations, and real-time scorecard collaboration.
- **Offers Service** – generates offer documents and orchestrates the e-signature flow.
- **Notifications Service** – sends email, in-app, and SMS notifications via event-driven triggers.
- **Analytics Service** – aggregates data from other services to power reporting dashboards.

**Data Layer:** Each service uses a dedicated PostgreSQL instance on AWS RDS. A Redis cluster provides caching for session data and real-time collaboration features. An S3 bucket stores all binary assets (CVs, offer PDFs, profile images). An Elasticsearch cluster powers advanced candidate and job search.

**Event Bus:** AWS EventBridge decouples services via asynchronous events (e.g., `ApplicationStatusChanged`, `InterviewScheduled`, `OfferSigned`), ensuring resilience and loose coupling.

**External Integrations:** The system integrates with LinkedIn, Indeed, and other job boards via their APIs; with Google Calendar and Outlook for scheduling; with DocuSign/HelloSign for e-signatures; and with BambooHR/Workday for HRIS sync.

### High-Level Architecture Diagram

```mermaid
graph TB
    subgraph Clients
        REC[Recruiter Web App\nReact SPA]
        CAND[Candidate Portal\nNext.js]
        MOB[Mobile App\nReact Native]
    end

    subgraph CDN["AWS CloudFront CDN"]
        CF[Static Assets &\nFrontend Delivery]
    end

    subgraph Gateway["API Gateway Layer"]
        APIGW[AWS API Gateway\nAuth · Rate Limiting · Routing]
        AUTH[Auth Service\nAWS Cognito]
    end

    subgraph Services["Backend Microservices (ECS / Fargate)"]
        JOBS[Jobs Service]
        APPS[Applications Service]
        CANDS[Candidates Service]
        AI[AI Service\nML Models]
        INT[Interview Service]
        OFFERS[Offers Service]
        NOTIF[Notifications Service]
        ANALYTICS[Analytics Service]
    end

    subgraph EventBus["Event Bus"]
        EB[AWS EventBridge]
    end

    subgraph DataLayer["Data Layer"]
        PG[(PostgreSQL RDS\nPer-service DBs)]
        REDIS[(Redis Cluster\nCache & Real-time)]
        S3[(AWS S3\nFiles & Assets)]
        ES[(Elasticsearch\nSearch Index)]
    end

    subgraph Integrations["External Integrations"]
        LI[LinkedIn API]
        IND[Indeed / Job Boards]
        CAL[Google / Outlook Calendar]
        ESIGN[DocuSign / HelloSign]
        HRIS[BambooHR / Workday]
        EMAIL[SendGrid / SES]
    end

    REC --> CF
    CAND --> CF
    MOB --> APIGW
    CF --> APIGW
    APIGW --> AUTH
    APIGW --> JOBS
    APIGW --> APPS
    APIGW --> CANDS
    APIGW --> AI
    APIGW --> INT
    APIGW --> OFFERS
    APIGW --> ANALYTICS

    JOBS --> EB
    APPS --> EB
    INT --> EB
    OFFERS --> EB
    EB --> NOTIF
    EB --> ANALYTICS
    EB --> AI

    JOBS --> PG
    APPS --> PG
    CANDS --> PG
    INT --> PG
    OFFERS --> PG
    AI --> PG
    ANALYTICS --> PG

    CANDS --> S3
    OFFERS --> S3
    AI --> S3
    APPS --> REDIS
    INT --> REDIS
    CANDS --> ES
    JOBS --> ES

    JOBS --> LI
    JOBS --> IND
    INT --> CAL
    OFFERS --> ESIGN
    CANDS --> HRIS
    NOTIF --> EMAIL
```

---

## 5. C4 Diagram

The C4 diagrams below zoom into the **AI Service**, which is the most differentiating component of LTI. We go from the System Context level down to the Component level.

### Level 1 – System Context

```mermaid
C4Context
    title System Context — LTI ATS Platform

    Person(recruiter, "Recruiter", "HR professional managing job openings and candidate pipelines")
    Person(manager, "Hiring Manager", "Reviews candidates and makes hiring decisions")
    Person(candidate, "Candidate", "Applies to job openings and tracks application status")

    System(lti, "LTI ATS Platform", "AI-powered applicant tracking system for end-to-end recruitment management")

    System_Ext(jobBoards, "Job Boards", "LinkedIn, Indeed, Glassdoor — job posting distribution")
    System_Ext(calendar, "Calendar Systems", "Google Calendar, Microsoft Outlook — interview scheduling")
    System_Ext(esign, "E-Signature Service", "DocuSign / HelloSign — digital offer signing")
    System_Ext(hris, "HRIS Platforms", "BambooHR, Workday — employee data sync")
    System_Ext(email, "Email Service", "SendGrid / AWS SES — transactional emails")

    Rel(recruiter, lti, "Creates jobs, reviews candidates, schedules interviews")
    Rel(manager, lti, "Reviews scorecards, approves offers, makes decisions")
    Rel(candidate, lti, "Submits applications, signs offers, tracks status")

    Rel(lti, jobBoards, "Publishes job openings", "REST API")
    Rel(lti, calendar, "Creates interview events", "REST API / CalDAV")
    Rel(lti, esign, "Sends & tracks offer documents", "REST API")
    Rel(lti, hris, "Syncs hired employee data", "REST API")
    Rel(lti, email, "Sends notifications", "SMTP / REST API")
```

### Level 2 – Container Diagram (LTI Platform)

```mermaid
C4Container
    title Container Diagram — LTI ATS Platform

    Person(recruiter, "Recruiter")
    Person(candidate, "Candidate")

    System_Boundary(lti, "LTI ATS Platform") {
        Container(spa, "Recruiter Web App", "React SPA", "HR team interface for pipeline management")
        Container(portal, "Candidate Portal", "Next.js", "Candidate-facing application & status tracking")
        Container(api, "API Gateway", "AWS API Gateway", "Single entry point, auth, rate limiting, routing")
        Container(jobsSvc, "Jobs Service", "Node.js / Express", "Manages job openings and pipeline stages")
        Container(appsSvc, "Applications Service", "Node.js / Express", "Tracks application lifecycle")
        Container(candsSvc, "Candidates Service", "Node.js / Express", "Manages candidate profiles and CVs")
        Container(aiSvc, "AI Service", "Python / FastAPI", "Resume parsing, scoring, bias detection")
        Container(intSvc, "Interview Service", "Node.js / Express", "Scheduling and real-time scorecards")
        Container(offerSvc, "Offers Service", "Node.js / Express", "Offer generation and e-signature tracking")
        Container(notifSvc, "Notifications Service", "Node.js", "Event-driven notifications dispatcher")
        ContainerDb(db, "Relational Databases", "PostgreSQL (per service)", "Persistent application data")
        ContainerDb(cache, "Cache / Real-time", "Redis", "Session data and live collaboration state")
        ContainerDb(search, "Search Index", "Elasticsearch", "Full-text search for candidates and jobs")
        ContainerDb(storage, "File Storage", "AWS S3", "CVs, offer PDFs, profile images")
        Container(eventBus, "Event Bus", "AWS EventBridge", "Async inter-service communication")
    }

    Rel(recruiter, spa, "Uses", "HTTPS")
    Rel(candidate, portal, "Uses", "HTTPS")
    Rel(spa, api, "API calls", "REST / GraphQL")
    Rel(portal, api, "API calls", "REST")
    Rel(api, jobsSvc, "Routes to")
    Rel(api, appsSvc, "Routes to")
    Rel(api, candsSvc, "Routes to")
    Rel(api, aiSvc, "Routes to")
    Rel(api, intSvc, "Routes to")
    Rel(api, offerSvc, "Routes to")
    Rel(jobsSvc, eventBus, "Publishes events")
    Rel(appsSvc, eventBus, "Publishes events")
    Rel(intSvc, eventBus, "Publishes events")
    Rel(offerSvc, eventBus, "Publishes events")
    Rel(eventBus, notifSvc, "Triggers notifications")
    Rel(eventBus, aiSvc, "Triggers AI processing")
    Rel(jobsSvc, db, "Reads/writes")
    Rel(appsSvc, db, "Reads/writes")
    Rel(candsSvc, db, "Reads/writes")
    Rel(aiSvc, db, "Reads/writes")
    Rel(intSvc, cache, "Real-time state")
    Rel(candsSvc, storage, "Stores CVs")
    Rel(candsSvc, search, "Indexes candidates")
    Rel(jobsSvc, search, "Indexes jobs")
```

### Level 3 – Component Diagram (AI Service)

```mermaid
C4Component
    title Component Diagram — AI Service

    Container_Boundary(aiSvc, "AI Service (Python / FastAPI)") {

        Component(apiHandler, "API Handler", "FastAPI Router", "Exposes REST endpoints for internal service calls: /parse, /score, /analyze-bias, /recommend")

        Component(resumeParser, "Resume Parser", "Python Component\n(pdfminer, spaCy NLP)", "Extracts raw text from PDF/DOCX CVs and applies NLP to identify skills, experience blocks, education sections, and contact data")

        Component(structuredExtractor, "Structured Data Extractor", "Python Component\n(LLM-based, GPT-4o)", "Transforms raw parsed text into a normalized JSON schema: skills[], experience[], education[], certifications[]")

        Component(scoringEngine, "Candidate Scoring Engine", "Python Component\n(scikit-learn / custom model)", "Compares structured candidate profile against job requirements using a trained ML model. Returns a match score 0–100 and a breakdown by dimension")

        Component(biasDetector, "Bias Detection Module", "Python Component\n(fine-tuned classifier)", "Scans job description text for gender-coded, age-discriminatory, or exclusionary language and returns flagged phrases with suggested alternatives")

        Component(rankingEngine, "Shortlist Ranking Engine", "Python Component", "Sorts a pool of scored candidates using multi-factor ranking (score, recency, source quality) and applies configurable threshold logic")

        Component(modelRegistry, "Model Registry Client", "Python Component\n(MLflow)", "Manages versioned ML model artifacts, handles model loading, A/B testing of model versions, and performance tracking")

        Component(featureStore, "Feature Store", "Python Component\n(Redis-backed)", "Caches computed candidate feature vectors for fast re-scoring and incremental updates without full re-parsing")

        Component(eventPublisher, "Event Publisher", "Python Component\n(boto3)", "Publishes scoring and parsing completion events to AWS EventBridge for downstream services to consume")

    }

    ContainerDb(cvStorage, "CV File Storage", "AWS S3")
    ContainerDb(aiDb, "AI Service DB", "PostgreSQL", "Stores parsed profiles, scores, model run logs")
    ContainerDb(featureCache, "Feature Cache", "Redis")
    ContainerDb(modelStore, "Model Artifact Store", "AWS S3 / MLflow")
    Container(eventBus, "Event Bus", "AWS EventBridge")
    Container(appsSvc, "Applications Service", "Node.js")
    Container(candsSvc, "Candidates Service", "Node.js")

    Rel(appsSvc, apiHandler, "POST /parse & /score", "REST")
    Rel(candsSvc, apiHandler, "POST /analyze-bias", "REST")
    Rel(apiHandler, resumeParser, "Calls")
    Rel(apiHandler, scoringEngine, "Calls")
    Rel(apiHandler, biasDetector, "Calls")
    Rel(apiHandler, rankingEngine, "Calls")
    Rel(resumeParser, cvStorage, "Fetches CV file", "S3 SDK")
    Rel(resumeParser, structuredExtractor, "Passes raw text")
    Rel(structuredExtractor, featureStore, "Caches feature vector")
    Rel(scoringEngine, featureStore, "Reads features")
    Rel(scoringEngine, modelRegistry, "Loads model")
    Rel(rankingEngine, scoringEngine, "Uses scores")
    Rel(modelRegistry, modelStore, "Loads artifacts")
    Rel(featureStore, featureCache, "Redis read/write")
    Rel(apiHandler, aiDb, "Stores results")
    Rel(apiHandler, eventPublisher, "Triggers after scoring")
    Rel(eventPublisher, eventBus, "Publishes events")
```

---

## Appendix: Prompts Used

See `prompts.md` in this folder for the complete list of AI prompts used to generate and refine this document.