# ✂️ Backlog Item & Epic Decomposer (AI Agent Skill)

[![GitHub Repo](https://img.shields.io/badge/GitHub-nadymovds%2Fbacklog--item--decomposer-181717?logo=github)](https://github.com/nadymovds/backlog-item-decomposer)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Format: Agent Skill](https://img.shields.io/badge/Format-Agent%20Skill-green.svg)](SKILL.md)
[![Compatible with Claude, Cursor, Antigravity & ChatGPT](https://img.shields.io/badge/Compatible-Claude%20%7C%20Cursor%20%7C%20Antigravity%20%7C%20GPTs-purple.svg)](#-quick-start)

> An open-source, battle-tested AI Agent Skill that automatically decomposes oversized Backlog Items, Epics, and Work Packages into small, INVEST-compliant vertical slices using 9 proven agile decomposition patterns.

Based on the article: **["Stop Overstuffing Sprints: 9 Battle-Tested Patterns for Slicing Backlog Items & Epics"](https://www.linkedin.com/)** by Denis Nadymov.

---

## 💡 Why This Exists

Over 70% of sprint carryovers and estimation stalls happen not because teams lack engineering capability, but because backlog items are too large to fit cleanly into a single sprint. 

When a work package is that bulky, it almost certainly conceals hidden pitfalls and unconsidered edge cases that remain invisible during refinement. If the work cannot be finished and made ready for inspection at sprint review, risks don't just add up—**the potential costs of those risks start multiplying exponentially.**

This skill gives any AI agent (or PM using LLMs) the exact mental models, heuristics, and diagnostic rules of a Staff Product Manager to split bulky epics into clean, sprint-ready backlog items with Gherkin acceptance criteria (`Given-When-Then`).

---

## 📐 The 9 Decomposition Patterns

This skill systematically applies the following 9 patterns depending on feature topology:

| # | Pattern | Core Mechanism | Key Diagnostic Question |
|---|---|---|---|
| **1** | **Simple to Complex** | Launch minimal sorting before dynamic faceted catalog search. | *"What is the 20% simplest mechanic that solves 60% of the core need?"* |
| **2** | **Business Rule Variations** | Isolate simple standard cutoffs before dynamic pricing/rules (e.g. EV spot-price charging). | *"Can we launch with one standard rule before adding dynamic calculations?"* |
| **3** | **Input/Output Variations** | Deliver a lightweight responsive list view before building interactive map clusters. | *"Which single visual modality provides the highest immediate utility?"* |
| **4** | **Data Variations** | Support manual form entry before building batch CSV imports and automated ERP webhooks. | *"Can we support manual entry or a single file type first before batch/API automation?"* |
| **5** | **Operations (CRUD)** | Deconstruct ambiguous "manage" items into distinct Read, Update, and Wallet actions. | *"What distinct actions (View, Edit, Delete, Share) make up this feature?"* |
| **6** | **Workflow Steps** | Release the shortest happy-path checkout before delivery slot scheduling and substitution logic. | *"What is the absolute shortest happy path that delivers an end-to-end outcome?"* |
| **7** | **Major Effort First** | Absorb the heavy gateway plumbing (Stripe/Adyen) in Item 1; add Apple Pay & Klarna incrementally. | *"Where does 80% of the foundational architecture live?"* |
| **8** | **Deferring Performance** | Ship working asynchronous report compilation today; optimize with realtime caching tomorrow. | *"Can an asynchronous job or simple progress indicator deliver value right now?"* |
| **9** | **The Spike (Research vs Dev)** | Run a timeboxed 2-day prototype to eliminate unknowns before sizing production code. | *"What specific unknown prevents us from estimating this with confidence?"* |

---

## 🚀 Quick Start

### 1. In Claude Projects / Claude.ai
1. Open your Project in [Claude.ai](https://claude.ai).
2. Upload [`SKILL.md`](SKILL.md) to **Project Knowledge**.
3. In Project Instructions, add:
   > *"Whenever I paste a Backlog Item, Epic, Jira ticket, or feature requirement, act as the Backlog Decomposer skill defined in your knowledge base and slice it vertically using the 9 patterns."*

### 2. In Cursor (`.cursorrules` or Agent Rules)
Copy the contents of [`SKILL.md`](SKILL.md) into your `.cursor/rules/backlog-decomposition.md` or `.cursorrules` file.

### 3. In Antigravity / Claude Code
Place the `skill/` folder into your `.agents/skills/backlog-decomposer/` or custom skills root directory.

### 4. In ChatGPT (Custom GPT)
1. Create a new Custom GPT in [chatgpt.com](https://chatgpt.com).
2. Paste the contents of [`SKILL.md`](SKILL.md) into the **Instructions** configuration box.

---

## 📝 Real-World Examples

Check the [`examples/`](examples/) directory for end-to-end decomposition blueprints:

1. **[B2B SaaS Data Onboarding](examples/b2b_saas_data_onboarding.md)**: Slicing an enterprise vendor import epic using **Pattern 4 (Data Variations)** + **Pattern 8 (Deferring Performance)**.
2. **[On-Demand Grocery Delivery Checkout](examples/grocery_delivery_checkout.md)**: Slicing a complex checkout flow using **Pattern 6 (Workflow Steps)** + **Pattern 2 (Business Rules)**.
3. **[E-Commerce Multi-Provider Payment Rails](examples/ecommerce_payment_methods.md)**: Front-loading credit card compliance and adding Apple Pay & Klarna using **Pattern 7 (Major Effort First)**.

---

## 🤝 Contributing & Feedback

Have another decomposition pattern, heuristic, or real-world case study? Contributions, PRs, and suggestions are welcome!

- **Author:** Denis Nadymov — [LinkedIn](https://www.linkedin.com/)
- **Repository:** [github.com/nadymovds/backlog-item-decomposer](https://github.com/nadymovds/backlog-item-decomposer)
- **License:** [MIT](LICENSE)
