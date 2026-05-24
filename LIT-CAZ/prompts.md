# Prompts Used — LTI ATS Design

## Prompt 1 – Initial Brainstorming
> "You are a senior product manager. I need to design an ATS (Applicant Tracking System) called LTI for a startup. Generate a brief description, key added value, competitive advantages over existing tools like Workday, Greenhouse, and Lever, and a Lean Canvas model. Focus on AI-powered features and real-time collaboration."

## Prompt 2 – Use Cases
> "Based on the LTI ATS described above, define the 3 most important use cases. For each one provide: actors, preconditions, main flow, and postconditions. Then generate a Mermaid diagram (flowchart, sequence diagram, or state diagram as appropriate) for each use case."

## Prompt 3 – Data Model
> "Design a complete relational data model for the LTI ATS system covering: Companies, Users, Job Openings, Candidates, Applications, CVs, Interviews, Scorecards, Offers, Pipeline Stages, Tags, and Activity Logs. Define all entities with their attributes (name and type) and relationships. Output as a Mermaid ER diagram."

## Prompt 4 – High-Level Architecture
> "Design a high-level cloud-native microservices architecture for LTI ATS on AWS. Describe each layer (frontend, API gateway, backend services, data layer, event bus, integrations) in prose, then generate a Mermaid graph diagram showing all components and their connections."

## Prompt 5 – C4 Diagram (AI Service)
> "Using the C4 model, create three levels of diagrams (System Context, Container, and Component) for the LTI ATS platform, zooming in to the AI Service at the component level. The AI Service should include: resume parser, structured data extractor (LLM-based), scoring engine (ML model), bias detector, ranking engine, model registry, feature store, and event publisher. Generate all three as Mermaid C4 diagrams."