# agent-smith

## 🤖 Bot-in-the-Loop Engineering

**From Coding to Steering: An Intent-Driven Development Paradigm**

Modern software projects are shifting from *Human-in-the-Loop* coding to a new paradigm: **Bot-in-the-Loop**. In this model, the "Developer" evolves into an "Architect," operating at a higher level of abstraction—expressing intent through GitHub Issues—while autonomous agents execute the implementation lifecycle.

**Agent-Smith** is an autonomous DevOps agent designed for Kubernetes Homelabs. It doesn't just write code; it owns the delivery pipeline—from commit to deployment verification.

### Core Philosophy

In the `agent-smith` workflow, humans no longer execute line-by-line implementation.
Instead, the workflow follows this cycle:

1.  **Intent:** You define the *What* (GitHub Issue).
2.  **Execution:** The Agent determines the *How* (Code, Test, Refactor).
3.  **Delegation:** The Agent leverages specialized bots (e.g., `restack-ai`) for safe GitOps delivery.
4.  **Verification:** The Agent closes the loop by validating infrastructure health (Argo CD).

-----

## 🏗 Architecture & Features

This system integrates **Generative AI** with **Cloud-Native Observability** to create a self-correcting feedback loop.

### 1\. Hybrid Intelligence (The Brain)

  * **Tiered Routing:** Uses a cost-effective router (Local LLM/Ollama) to triage issues into `Feature`, `Bug`, or `Docs`.
  * **Escalation Strategy:** Routes complex logic to SOTA models (OpenAI) while handling routine tasks locally.

### 2\. Isolated Execution (The Hands)

  * **DevContainer Native:** All code changes are verified within consistent, containerized environments (`.devcontainer`).
  * **Sandbox Testing:** Runs unit/integration tests in ephemeral environments before pushing.

### 3\. GitOps Delegation (The Release Manager)

  * **Collaboration with `restack-ai`:** Instead of touching K8s manifests directly, Agent-Smith focuses on application code (`src/`) and relies on `restack-ai[bot]` to handle image tag updates and GitOps triggers.
  * **Deterministic Deployment:** Ensures that infrastructure changes are handled by specialized, rule-based automation.

### 4\. Self-Healing Feedback Loop (The Eyes)

  * **Argo CD Integration:** The agent doesn't stop at "git push." It monitors Argo CD application status.
  * **Log-Driven Debugging:** If a deployment fails or a pod crashes, the agent retrieves logs, analyzes the root cause, and triggers a self-correction cycle until the system is healthy.

-----

## 🔄 Workflow: The Life of an Issue

```mermaid
graph TD
    User(User) -->|Creates Issue| GitHub[GitHub Issue]
    GitHub -->|Trigger| Router{LLM Router}
    
    subgraph "Agent-Smith Workflow"
        Router -->|Analysis| Plan[Planning & Coding]
        Plan -->|Execution| Sandbox[DevContainer / Test]
        Sandbox -->|Push Code| Repo[App Repository]
    end
    
    subgraph "CI/CD Pipeline"
        Repo -->|Build & Push| CI[GitHub Actions]
        CI -->|Trigger| Restack[restack-ai bot]
        Restack -->|Update Tag| Manifest[K8s Manifest Repo]
    end
    
    subgraph "Verification Loop"
        Manifest -->|Sync| ArgoCD[Argo CD]
        ArgoCD -->|Status Feedback| Observer[Agent Observer]
        Observer -- Healthy --> Done(Close Issue)
        Observer -- "Failed (Logs & Context)" --> Plan
    end
```
