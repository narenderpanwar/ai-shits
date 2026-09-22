# Building an AI Agent: Zero to Hero — Episode 1

**Series:** 10-video, project-driven series building a **Kubernetes Investigation Agent** end-to-end.
**Platform:** Gemini Enterprise Agent Platform + Google Cloud Platform (GCP)

---

## Series Structure

The agent lifecycle is split into four pieces, mapped to the video order:

1. **Building** the agent
2. **Deploying and scaling** the agent
3. **Governing** the agent
4. **Observing** the agent in production

Hands-on building starts in the next video ("Day 2"). This episode covers **Day 1 vs. Day 2 operations** — the difference between a prototype and an enterprise-grade agent.

---

## Day 1: Building the Prototype

Any coding agent (Claude, Cursor, GitHub Copilot) can build a working agent quickly:
- Prompt it to create a Kubernetes troubleshooting/investigation agent
- Connect it to Kubernetes access — via **MCP tools** or **RAG**
- Ask: *"What is the issue with my Kubernetes cluster?"*
- The agent inspects **pods, deployments, and logs**, and returns a detailed root-cause analysis for why a service is failing

This works well in testing — a good **Day 1 prototype**. But taking this as-is into production is **not viable**.

### Day 2 questions raised
- Can we trust the agent's answers?
- Where will the agent run, and how does it scale?
- Can it remember previous incidents?
- Who is this agent, and what is it allowed to access?
- Can we see what it did?
- What does it cost to run?
- Who is accountable for it?

These are **Day 2 operations** — addressed via specific Gemini Enterprise Agent Platform features.

---

## Day 2: The Five Reasons a Prototype Isn't Production-Ready

### 1. Trust — *Agent Evaluation*

- Can't blindly trust agent answers in production.
- **Example:** Agent says a service failed due to a "memory issue." Two possibilities:
  1. It genuinely checked logs, events, and recent cluster changes, and correctly found a memory shortage.
  2. It found nothing conclusive and the underlying model just **guessed** "memory issue."
- **Solution:** **Agent Evaluation** feature — validates agent responses instead of blindly trusting them.

### 2. Runtime & Scaling — *Agent Runtime*

- Questions: How to handle multiple concurrent users? Multiple simultaneous incidents? Scaling? Crash recovery — who restarts it?
- **Solution:** **Agent Runtime** feature — provides a production environment for deployment and scaling of agents.

### 3. Memory — *Memory Bank*

- **Example:** An incident last week was root-caused to a config change. A similar incident recurs weeks later — the agent should recall the prior investigation instead of starting from zero.
- **Solution:** **Memory Bank** — persistent memory for agents, maintaining context beyond a single session/interaction, so historical context informs future investigations.

### 4. Identity — *Agent Identity + SPIFFE*

- Every agent needs its own identity with scoped access.
- **Example:** A Kubernetes Investigation Agent may only need **read** access (logs, deployment/service status). An agent that changes infrastructure (creating/upgrading clusters) needs **read + write**.
- **Solution:** **Agent Identity** feature + **SPIFFE** (explained in a later video).

### 5. Observability & Cost — *Google Cloud Observability + Cost Management + Model Garden*

- **Observability:** What actions the agent performed, when, what went wrong, and success rate — via **Google Cloud's observability capabilities**.
- **Cost:** What infrastructure was used, where cost is high, where to optimize — via **Google Cloud's cost management capabilities**.
- **Reducing cost via model choice:** Use cheaper/lighter models for low-complexity tasks via **Model Garden**, which hosts Google and select third-party models.

---

## Summary Table

| # | Challenge | Feature/Capability |
|---|---|---|
| 1 | Trust | Agent Evaluation |
| 2 | Runtime & Scaling | Agent Runtime |
| 3 | Memory | Memory Bank |
| 4 | Identity | Agent Identity + SPIFFE |
| 5 | Observability | Google Cloud Observability |
| 5 | Cost | Google Cloud Cost Management + Model Garden |

## Key Takeaways

- A Day 1 prototype is not sufficient for production.
- Day 2 readiness requires: trust, runtime/scaling, memory, identity, observability, and cost — each mapped to a Gemini Enterprise Agent Platform or Google Cloud feature.
- The series builds a single running example, the Kubernetes Investigation Agent, to demonstrate all of these.
- Hands-on building begins in the next episode ("Day 2").
