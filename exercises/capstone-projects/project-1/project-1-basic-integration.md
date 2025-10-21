# 🧩 Capstone Project 1 — Building a Claude MCP Server

## 🎯 Objective
Design, build, and test a **Claude-compatible MCP Server** in either **Python** or **TypeScript** that exposes at least two tools and one resource. This project validates your understanding of the Model Context Protocol, server–client communication, and tool/resource architecture.

---

## 🧠 Learning Outcomes
By completing this project, you will:
- Implement the MCP server structure and lifecycle.
- Register, expose, and validate multiple tools.
- Integrate with at least one mock external API or dataset.
- Return structured JSON responses and handle errors gracefully.
- Write automated tests (pytest or Jest) that validate each endpoint.

---

## 🧱 Project Structure
Your project folder should follow this structure:
basic_mcp_server/
├── src/
│ ├── server.py OR index.ts
│ ├── tools/
│ │ ├── ping.py
│ │ ├── data_query.py
│ └── resources/
│ └── version.json
├── tests/
│ ├── test_ping.py
│ └── test_data_query.py
└── README.md

yaml
Copy code

---

## 🧩 Requirements
1. **Tools**
   - `ping`: returns a simple response `{ "pong": true }`
   - `sum_numbers`: accepts an array of numbers and returns their sum
   - One additional tool of your choice (e.g., `normalize_text`, `fetch_weather`, or `generate_report`)

2. **Resources**
   - Must include a `/version` resource returning metadata (server name, version, author)

3. **Validation**
   - Use Pydantic (Python) or Zod (TypeScript) schemas
   - Validate arguments and handle edge cases

4. **Tests**
   - Include at least 3 unit tests per tool
   - Run all tests successfully via `pytest` or `npm test`

---

## 🧮 Evaluation Rubric
| Criteria | Points |
|-----------|--------|
| Functional MCP Server (runs via stdio) | 30 |
| Schema validation & error handling | 20 |
| At least 3 working tools | 20 |
| Resource endpoints implemented | 10 |
| Unit testing coverage | 10 |
| Code readability & documentation | 10 |
| **Total** | **100** |

---

## 💡 Bonus Challenges
- Add OAuth authentication to your MCP server.
- Implement logging and metrics output.
- Create a Dockerfile to containerize your server.

---

## 📤 Submission
1. Push your project to your `claude-enterprise-integrations` repo under `/exercises/capstone-projects/`
2. Include a README with:
   - Setup instructions
   - Example request/response payloads
   - Testing instructions

---

## 🔗 References
- Module 3 — *Building MCP Servers*
- Module 8 — *Advanced Patterns*
- Templates: `/templates/mcp-servers/python` or `/templates/mcp-servers/node`
