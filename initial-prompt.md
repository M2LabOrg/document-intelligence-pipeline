# Document Intelligence Pipeline on Azure

## Goal

Build a system where users upload complex PDFs containing tables, infographics and figures, have them processed automatically by AI, and query the results immediately. The output must be visible on screen within minutes of deployment.

## Core Services

* Azure AI Foundry as the orchestration backbone
* Azure AI Document Intelligence to parse tables, figures and document structure
* Azure Blob Storage to store raw uploaded files and intermediate JSON outputs
* Azure AI Search to index chunked content for retrieval by any LLM or agent
* Azure App Services to host and serve the front end
* GPT-5 via Azure AI Foundry to interpret figures, infographics and visual content, and to power the query layer

## Document Processing Pipeline

Process each uploaded PDF through the following stages in sequence:

**Stage 1 -- Extraction**
Run the document through Azure AI Document Intelligence to extract text, tables, reading order and layout structure. For any figure or infographic detected, crop the image and pass it to GPT-5 with the prompt: "Describe the key information in this figure clearly and concisely for use in a retrieval system."

**Stage 2 -- Chunking**
Split all extracted content, including figure descriptions, into overlapping chunks of around 512 tokens with roughly 10% overlap. Treat tables and figure captions as discrete chunks and never split them mid-content.

**Stage 3 -- JSON Conversion**
Convert the full chunked output into a structured JSON file with the following shape per chunk:

```json
{
  "id": "unique_chunk_id",
  "source_file": "filename.pdf",
  "page_number": 1,
  "chunk_type": "text | table | figure",
  "content": "extracted or described content here",
  "token_count": 512
}
```

Store this JSON file in Azure Blob Storage alongside the original PDF. This file serves as the auditable intermediate artifact between extraction and indexing.

**Stage 4 -- Indexing**
Push each JSON chunk as a document into Azure AI Search. Map the fields directly to the search index schema so all chunks are immediately queryable.

## Front End

Build a simple Flask or React app hosted on Azure App Services. The UI must include:

* A drag-and-drop or file selector for PDF uploads
* Real-time status feedback during processing, showing which stage is active
* A results panel that displays the extracted and chunked JSON content on screen immediately after processing
* A query box where users can type a question and receive a grounded answer retrieved from the Azure AI Search index, powered by GPT-5

## Constraints

* PDF files only
* Maximum file size of 20MB per upload
* No exotic or uncommon dependencies
* Must be fully deployed and usable within 30 minutes using standard Azure services

## Specs to Generate Before Any Code

Produce the following as a single document before writing any code:

1. **Manifesto** -- one paragraph on what this system is and why it exists
2. **Auditor Description** -- plain English explanation of what data moves where and why, with no jargon
3. **Architecture Overview** -- ASCII or text diagram showing the full data flow from upload through extraction, JSON conversion and into the search index
4. **Development Steps** -- ordered sequence of what to build and in what order
5. **Threat Model** -- key risks covering data in transit, storage access, API exposure and untrusted file uploads, each with a mitigation

## Infrastructure as Code

Use Bicep files to define and track every Azure resource provisioned during the build. Each service -- Blob Storage, Document Intelligence, Azure AI Search, App Services and AI Foundry -- must have a corresponding Bicep definition. This serves two purposes: it makes the deployment fully repeatable, and it gives auditors a clear record of exactly what was provisioned and how.

Organise the Bicep files as follows:

```
infra/
  main.bicep                          -- entry point, ties all modules together
  modules/
    storage.bicep                     -- Blob Storage account and containers
    document-intelligence.bicep       -- Document Intelligence resource
    ai-search.bicep                   -- Azure AI Search index and configuration
    app-service.bicep                 -- App Service plan and web app
    ai-foundry.bicep                  -- Azure AI Foundry and GPT-5 deployment
```

## Deferred to Later Stages

The following are acknowledged and must appear in the spec, but should not block the initial build:

* **Authentication** -- Azure Entra ID integration, pending IT collaboration
* **Error Handling** -- graceful failure on malformed files, retry logic with backoff for Document Intelligence timeouts
* **Monitoring** -- Azure Application Insights and a processing audit trail
* **Cost Governance** -- tier selection and budget alerts

## Success Criteria

A user uploads a PDF, the system extracts and interprets all content including figures, converts the output to a structured JSON file stored in Blob Storage, indexes all chunks into Azure AI Search, displays the results on screen, and returns a grounded answer to a typed question. All of this works within minutes of deployment.
