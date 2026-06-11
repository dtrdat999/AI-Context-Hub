# AGENTS.md — V-FENG Compass Project Instructions

## Project identity

You are the coding agent for V-FENG Compass.

V-FENG Compass is not a general feng shui chatbot. It is a decision-support web app that helps apartment buyers create a shortlist of 1–3 suitable units based on their personal feng shui profile, structured apartment data, transparent rule-based scoring, and AI explanations.

Core user journey:
Buyer enters basic personal info → system creates User Feng Profile → buyer browses apartment/project listings → system shows Feng Fit Score → buyer understands why a unit fits or does not fit → buyer compares 2–3 units → buyer saves unit / submits lead / books consultation → sales receives contextual insight.

The product must help the user answer:
“Which apartment should I prioritize, and why?”

## Product principles

1. Every feature must help users choose apartments better. If it does not, deprioritize it.
2. Rule Engine is the source of truth.
3. LLM must not invent scores, rules, apartment data, or feng shui conclusions.
4. AI is only used for explanation, summary, comparison, and next-step guidance.
5. RAG may enrich explanations but must not replace scoring logic.
6. Do not make absolute claims about wealth, health, destiny, or guaranteed outcomes.
7. Always include the idea that feng shui results are advisory and must be considered alongside legal, financial, location, and lifestyle factors.

## MVP scope

In scope:
- Auth and basic roles: Admin, Sales, Buyer
- Buyer onboarding
- User Feng Profile
- Cung Phi / Gua calculation
- East/West group classification
- Apartment/project listing
- Apartment detail
- Feng Fit Score badge
- Bat Trach 64-case Rule Engine
- Score breakdown
- AI explanation
- Compare 2–3 apartments
- Lead capture
- Basic Sales dashboard
- Admin CRUD for projects, apartments, and rules
- RAG only if the core flow already works
- Responsive UI / PWA only if it does not delay the core demo

Out of scope for MVP:
- General feng shui chatbot
- Zi Wei, destiny, romance, career prediction
- Ba Zi / Four Pillars requiring birth hour
- Flying Star Feng Shui
- Image/floorplan analysis
- Deep interior design feng shui
- Numerology as a core score factor
- Real estate marketplace
- Payment / deposit
- Full CRM
- Complex multi-agent workflows

## Architecture rules

The system has three layers:

Layer 1 — Rule Engine:
- Core scoring layer.
- Must be deterministic and testable.
- Uses Bat Trach 8 gua/cung × 8 directions = 64 cases.
- Returns star_name, score, compatibility_level, short explanation, and recommendation.
- Must work independently from any LLM.

Layer 2 — RAG:
- Retrieves reference knowledge only.
- Used to enrich explanation.
- Must never decide the score.

Layer 3 — LLM:
- Receives rule result + user profile + apartment data + optional RAG context.
- Explains the result in natural language.
- Must not override rule result.
- Must not hallucinate missing apartment data.
- If data is missing, say that more data is needed.

## Scoring philosophy

Always keep score transparent.

Preferred output format:
- Total Feng Fit Score: x/10
- Bat Trach: x/10 — reason
- Loan Dau / environment: x/10 — reason or risk
- Supplementary Ngu Hanh / Nap Am: x/10 — reason
- Practical Fit: x/10 — reason
- Recommendation level: Prioritize / Consider / Do not prioritize / Need more data
- Confidence: High / Medium / Low

Bat Trach is the core layer.
Loan Dau / environment can cap or penalize score.
Ngu Hanh / Nap Am is supplementary only.
Practical Fit includes budget, area, room count, legal status, availability, and buyer purpose.

## Required data model direction

User Feng Profile should include:
- birth_year
- gender
- cung_phi / gua
- east_west_group
- nap_am_element if available
- good_directions
- bad_directions
- budget_range
- preferred_area
- buying_purpose
- priority_weights

Project data should include:
- project_id
- project_name
- district
- city
- segment
- developer
- legal_status
- amenities
- description

Block data should include:
- block_id
- project_id
- block_name
- floors
- units_per_floor
- elevators
- block_orientation

Apartment data should include:
- apartment_id
- project_id
- block_id
- unit_code
- floor
- area_m2
- bedrooms
- bathrooms
- price_total
- price_per_m2
- main_door_direction
- balcony_direction
- view_type
- legal_status
- availability_status
- advantages
- limitations

## Engineering rules

Before coding:
- Inspect the current repo structure first.
- Do not rewrite the whole project unless explicitly asked.
- Prefer small, targeted changes.
- State which files you will modify.
- Keep Rule Engine separate from UI.
- Keep scoring logic separate from LLM explanation.
- Keep data schema explicit and easy to inspect.

When coding:
- Prioritize working demo over over-engineering.
- Use simple, readable code.
- Add comments only where they clarify business logic.
- Avoid adding new dependencies unless necessary.
- Ask before changing architecture, database provider, auth provider, or framework.
- Do not remove existing project files unless explicitly requested.

When implementing AI:
- Use low-cost models by default.
- Keep prompts short and grounded.
- Pass structured rule results into the LLM.
- Do not ask the LLM to calculate Bat Trach score.
- The LLM should explain, compare, summarize, and suggest next steps only.

When implementing RAG:
- Use RAG only for explanation/reference.
- Keep source chunks traceable.
- Do not let RAG replace the Rule Engine.
- If retrieved context conflicts with the rule table, follow the rule table and flag the conflict.

## UI/UX priorities

Aha moment:
The buyer should see which apartments are more suitable directly in the listing, before chatting.

Important screens:
1. Landing
2. Login / role selection
3. Buyer onboarding
4. Apartment/project listing with Feng Score Badge
5. Apartment detail with score breakdown
6. Compare 2–3 units
7. AI explanation panel
8. Lead capture
9. Sales dashboard
10. Admin CRUD

UI principle:
Less chatbot, more decision-support interface.

## Definition of done

A task is done only when:
- The app still runs.
- The feature supports the core apartment-choice flow.
- Rule/scoring logic is traceable.
- AI does not invent scores or data.
- The user can understand why one apartment is better than another.
- Relevant tests/checks are run when available.
- The final response summarizes changed files, what was implemented, and how to verify.

## Communication style

Respond in Vietnamese unless the user asks otherwise.
Be direct, practical, and implementation-focused.
When there are trade-offs, explain them clearly.
If the requested feature is likely to dilute MVP scope, push back constructively.
Prefer:
working → demoable → debuggable → explainable → scalable.
