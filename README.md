# Document Intelligence Pipeline on Azure

This repository is used in our **AI Coding Lab** demo session to showcase how to write a high-quality prompt for a complex, real-world use case using **vibe prompting / vibe coding**.

## What this demo covers

The `initial-prompt.md` file in this repo contains a fully structured prompt that describes a complete Document Intelligence Pipeline built on Azure. It is intentionally detailed and opinionated — covering architecture, data flow, infrastructure as code, constraints, and success criteria — to demonstrate how a well-crafted prompt can serve as the single source of truth for an AI coding assistant building a non-trivial system.

## Key ideas illustrated

- **Vibe prompting**: expressing intent, tone, and constraints clearly so the AI can infer sensible decisions without being micro-managed
- **Structured specificity**: combining high-level goals with concrete technical requirements (service choices, JSON schemas, file size limits, Bicep layout)
- **Spec-first thinking**: asking the AI to produce a manifesto, architecture diagram, threat model, and development sequence *before* writing any code
- **Deferral as a design tool**: explicitly naming what is out of scope for the first build so the AI does not over-engineer

## How to use this in the lab

1. Open `initial-prompt.md`
2. Paste its contents into your AI coding assistant of choice (Claude, Copilot, Cursor, etc.)
3. Watch the assistant produce a full spec document, then follow up to generate the actual code and Bicep infrastructure files
4. Discuss as a group: what made the prompt effective? What would you change?
