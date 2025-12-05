# Objective
Add durable persistence, a human approval gate, and a focused agentic RAG subgraph that grounds the system’s recommendations in policy documents.

---

# Required

## Persistence
- Compile the graph with a Postgres checkpointer.  
- Demonstrate stopping the process mid-run and resuming the same thread.

## Approval
- The `propose_remedy` node calls `payments.refund_preview`, then pauses for human approval.  
- On resume, call `payments.refund_commit`.

## Agentic RAG Subgraph
- Add a small knowledge base of **8 to 12 short policy Markdown files** stored locally in the repository.  
- Provide an indexing CLI `kb_index` that chunks and embeds the documents into a vector store. Options include **pgvector** or **Chroma**.  
- Build a `kb_orchestrator` node that:
  - Plans retrieval  
  - Selects a retriever  
  - Runs top-k retrieval  
  - Optionally invokes a reranker or LLM-based re-scorer  
  - Returns citations  
- Integrate RAG into `policy_evaluator` so proposed actions must cite relevant policy sections.

## Observability
- Display the run tree and node metadata in **LangSmith** or **Langfuse**, including:
  - Retrieved document IDs  
  - Citation spans  

---

# Live Demo

- Start a run, stop the server, restart, and resume to completion.  
- Show an interrupted run that resumes after human approval.  
- Show RAG citations in the proposed remedy and the final reply.
