# Document Intelligence Pipeline on Azure

> **AI Coding Lab demo** — a structured vibe prompt for a complex, real-world Azure use case, packaged as an [APM](https://github.com/microsoft/apm) (Agent Package Manager) package.

---

## What this is

This repo contains a complete, opinionated AI prompt that instructs a coding assistant to build a **Document Intelligence Pipeline on Azure**: users upload PDFs, the system extracts text, tables, and figures using Azure AI Document Intelligence, interprets visuals with GPT-5, chunks and indexes everything into Azure AI Search, and exposes a query interface — all within minutes of deployment.

The prompt is intentionally detailed and spec-first: it asks the AI to produce a manifesto, architecture diagram, threat model, and development plan *before writing a single line of code*.

---

## Is this safe to be public?

Yes. This repository contains:

- A generic, technology-level prompt with no credentials, API keys, secrets, or subscription IDs
- No organization-specific data, internal service names, or personally identifiable information
- No proprietary architecture — only well-documented, publicly available Azure services

If you fork this and add environment-specific details (resource group names, tenant IDs, connection strings), keep those in a `.env` file and add it to `.gitignore` before pushing.

---

## Use as an APM package

This repo is a valid [APM](https://github.com/microsoft/apm) package. APM (Agent Package Manager) lets you declare AI agent instructions as versioned, reproducible dependencies — like `npm install` but for prompts and agent context.

### Install APM

```bash
# macOS
brew install microsoft/apm/apm

# Python (cross-platform)
pip install apm-cli
```

### Install this prompt into your project

```bash
apm install M2LabOrg/document-intelligence-pipeline@v1.0.0
```

Or declare it as a dependency in your project's `apm.yml`:

```yaml
packages:
  - M2LabOrg/document-intelligence-pipeline@v1.0.0
```

Then run:

```bash
apm install
```

APM will pull `.instructions.md` into your agent's context. Your coding assistant (Claude Code, GitHub Copilot, Cursor, Windsurf, etc.) will have the full pipeline prompt available without any manual copy-pasting.

### Package structure

```
.
├── apm.yml              # APM manifest — declares this as a versioned package
├── .instructions.md     # The prompt in APM-standard instructions format
├── initial-prompt.md    # Same prompt, standalone human-readable version
└── README.md
```

---

## Use without APM (direct copy-paste)

1. Open [`initial-prompt.md`](./initial-prompt.md)
2. Copy the full contents
3. Paste into your AI coding assistant (Claude, Copilot, Cursor, Gemini, etc.)
4. The assistant will produce a full spec document — review it, then ask it to generate the code and Bicep infrastructure files

---

## What the prompt demonstrates

| Technique | How it appears in the prompt |
|---|---|
| **Vibe prompting** | Intent and tone set clearly; the AI infers sensible defaults |
| **Structured specificity** | Exact JSON schema, file size limits, Bicep folder layout |
| **Spec-first thinking** | Explicit instruction to produce manifesto + threat model before code |
| **Deferral as a tool** | Out-of-scope items named explicitly so the AI doesn't over-engineer |
| **Constraint clarity** | PDF-only, 20 MB limit, 30-minute deployment target |

---

## Azure services used

| Service | Role |
|---|---|
| Azure AI Document Intelligence | Extract text, tables, reading order, figures from PDFs |
| Azure AI Foundry + GPT-5 | Interpret figures and infographics; power the query layer |
| Azure Blob Storage | Store raw PDFs and intermediate JSON chunks |
| Azure AI Search | Index all chunks for fast, grounded retrieval |
| Azure App Services | Host the Flask or React front end |

---

## Contributing

Pull requests welcome — especially improvements to the prompt itself (better constraints, additional deferral items, alternative chunking strategies). Open an issue if you adapt this for a different cloud provider.

---

## License

MIT
