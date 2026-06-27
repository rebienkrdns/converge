# LLMs Pack

Activates the LLMs Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that involve integrating, prompting, fine-tuning, or operating large language models as components in a production system.

## Activate When

The decision involves selecting an LLM provider or model, designing a prompting strategy, implementing tool use or function calling, building a multi-agent system, fine-tuning a model, evaluating model output quality, or setting cost and latency budgets for inference.

## Added Concerns

**Architectural concerns:**
- Is the prompting strategy designed to be maintainable? Prompt strings embedded directly in code are difficult to version, test, and update. Prompts used in production systems should be treated as versioned artifacts with their own change management.
- Is the system designed to tolerate LLM non-determinism? The same prompt can produce different outputs on different calls. Systems that depend on structurally stable outputs must validate and parse responses defensively.
- Is there a human review or escalation path for outputs that will affect high-stakes decisions, financial transactions, or irreversible actions?

**Cost concerns:**
- Is the token consumption of the system modeled under the expected usage volume? LLM inference costs scale with token count. A prompt that is 2000 tokens sent 100,000 times per day is a significant cost item that must be budgeted.
- Are smaller, cheaper models evaluated for tasks where the full capability of a frontier model is not required? Model selection should be based on evaluated fitness for the task, not on availability alone.
- Is caching used for prompts with stable inputs (system prompts, few-shot examples) to reduce redundant token processing?

**Security and safety concerns:**
- Is prompt injection a risk surface for this system? Any system that includes user-controlled content in a prompt must treat the LLM's output as potentially adversarial.
- Are the outputs of the LLM sanitized before rendering to the user or executing as actions? LLM-generated content is untrusted and must be treated accordingly.
- Is there a content moderation layer for user-facing outputs in systems where harmful content generation is a risk?

**Quality concerns:**
- Is the system evaluated against a structured test set with expected outputs, not just demonstrated on cherry-picked examples?
- Is the evaluation methodology consistent with the production use case? An evaluation that tests summarization but deploys the model for classification is not predictive of production quality.

**Observability concerns:**
- Are per-request token counts, latency, error rates, and cost tracked in production?
- Are outputs sampled for quality review at a rate sufficient to detect degradation before users report it?

## Council Interactions

The LLMs Specialist defers to the Security Architect on prompt injection and output sanitization, to the AI Specialist on evaluation methodology and model selection, to the Performance Engineer on latency budgets, and to the RAG Specialist when the LLM is paired with a retrieval system.
