# AI Pack

Activates the AI Specialist from the Adaptive Council and extends the permanent council's evaluation criteria for decisions that integrate machine learning models, AI-generated outputs, or AI-assisted workflows into production systems.

## Activate When

The decision involves deploying a machine learning model to production, integrating a third-party AI API, using AI to generate content that will be shown to users, automating a decision with AI output, or evaluating the fitness of an AI system for a production use case.

## Added Concerns

**Architectural concerns:**
- Is the AI component's output used as a hard decision or as a recommendation that a human reviews? Decisions with irreversible consequences must not be fully automated on AI output without explicit justification and fallback design.
- Is the model's behavior under distribution shift understood? A model trained on historical data may perform well on training data and poorly on inputs that differ from it in ways that are not immediately obvious.
- Is there a fallback path for when the AI component is unavailable, slow, or returns low-confidence output?

**Quality and accuracy concerns:**
- What is the measured accuracy of the model on a held-out evaluation set that represents the production distribution? Accuracy on training data is not an acceptable substitute.
- Are false positive and false negative rates evaluated separately? An aggregate accuracy metric can hide an asymmetric failure mode that is unacceptable for the use case.
- Is there a mechanism for detecting model drift after deployment? Model performance degrades as the world changes and the training distribution diverges from the live distribution.

**Security concerns:**
- Is the model exposed to adversarial inputs? Inputs crafted to manipulate model output (prompt injection for LLMs, adversarial examples for classifiers) are a real attack vector for user-facing AI systems.
- Is the output of the AI model sanitized before it is rendered or executed? AI-generated content can contain XSS payloads, malicious URLs, or other injections if the output is not treated as untrusted.

**Ethical and compliance concerns:**
- Does the model produce outputs that could discriminate against protected groups? Bias evaluation must be part of the pre-deployment review, not a post-launch observation.
- Are there regulatory requirements (GDPR, EU AI Act, CCPA) that apply to the use of automated decision-making for this use case?

**Observability concerns:**
- Are model inference latency, error rates, and output confidence scores monitored in production?
- Is a human-review pipeline in place for low-confidence outputs or flagged outputs?

## Council Interactions

The AI Specialist defers to the Security Architect on adversarial input and output sanitization, to the QA Lead on evaluation methodology and acceptance criteria, to the Software Architect on fallback design, and to the Legal or Compliance function on regulatory requirements when applicable.
