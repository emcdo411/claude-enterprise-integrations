# 🚀 Capstone Project 2 — Multi-System Skill Orchestration

## 🎯 Objective
Design a **multi-system integration** using multiple Claude MCP servers and one orchestrating Skill. Your system should demonstrate how Claude can query several data sources, combine results, and return unified insights.

---

## 🧠 Learning Outcomes
By completing this project, you will:
- Implement **multiple MCP servers** that represent real systems (CRM, Analytics, or Documents)
- Build an **Orchestrator Skill** that queries and merges responses
- Apply **prompt engineering** logic for reasoning between systems
- Create a full workflow demonstrating Claude’s MCP orchestration capabilities

---

## 🧱 Architecture Overview
Your solution should resemble this diagram:

Claude Skill (Orchestrator)
│
├── CRM MCP Server → returns client data
├── Analytics MCP Server → returns performance metrics
├── Document MCP Server → returns reports/templates
└── Real Estate MCP Server → returns property listings

yaml
Copy code

---

## ⚙️ Technical Requirements
1. **Servers**
   - At least **three MCP servers** with independent logic
   - Each must support two tools (e.g., `get_client`, `get_properties`, `get_report`)
   - Include validation and standardized JSON outputs

2. **Skill**
   - Skill file in `/templates/skills/multi_system_skill/`
   - Must perform orchestration logic (call → merge → summarize)
   - Use guardrails: partial failure handling, evidence tracking, schema validation

3. **Testing**
   - Include test cases for:
     - Successful multi-server orchestration
     - API failure and fallback handling
     - Schema compliance

4. **Deployment**
   - Optional: containerize your servers and orchestrator using Docker Compose
   - Optional: simulate real responses with mock JSON data

---

## 🧮 Evaluation Rubric
| Criteria | Points |
|-----------|--------|
| 3+ functional MCP servers | 25 |
| Orchestrator skill integrates responses | 25 |
| Schema validation + error handling | 15 |
| Tests for success/failure conditions | 15 |
| Documentation & diagrams | 10 |
| Bonus (Dockerized or live demo) | 10 |
| **Total** | **100** |

---

## 🧰 Suggested Tech Stack
| Component | Language | Framework |
|------------|-----------|------------|
| MCP Servers | Python / TypeScript | FastAPI / Express |
| Skill | Markdown + JSON Schema | Claude MCP Skill spec |
| Testing | pytest / jest | Coverage ≥ 80% |
| Deployment | Docker / Compose | Optional but encouraged |

---

## 📘 Deliverables
- `/examples/real-estate/` or `/examples/construction/` working demo
- `/skills/orchestrator/skill.md` with logic summary
- Architecture diagram (Mermaid or Draw.io)
- README explaining your orchestration pipeline

---

## 📤 Submission
1. Push your project under `/exercises/capstone-projects/`
2. Add a video (optional) or Markdown walkthrough of your orchestration flow
3. Tag your submission: `#MCP-Orchestration-Capstone`

---

## 🔗 References
- Module 4 — *Creating Skills*
- Module 5–7 — *Industry Examples*
- Module 9 — *Security & Compliance*
- Module 10 — *Deployment & Operations*
