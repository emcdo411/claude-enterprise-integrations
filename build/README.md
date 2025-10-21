# 🧱 build/ (Ephemeral Workspace)

This folder is used for **temporary or intermediate build outputs** generated during development or CI/CD runs.  
Final, versioned deliverables should go into **`dist/`**.

---

## 📂 Recommended Subfolders

| Folder | Purpose |
|---------|----------|
| **mcp-python/** | Python MCP server builds, virtualenvs, or temporary wheels |
| **mcp-node/** | Raw TypeScript/Node.js transpilation output, source maps, cache |
| **skills/** | Assembled skill bundles before publishing |
| **docs-preview/** | Locally rendered docs, PDFs, or static site previews |
| **assets/** | Compiled diagrams or generated images (Mermaid → PNG/SVG) |
| **k8s/** | Rendered Helm/Kustomize manifests for local smoke testing |
| **packages/** | Zipped course or module bundles prior to promotion to `/dist/` |

---

## ⚙️ Usage Guidelines
- Treated as **scratch space**: contents can be deleted safely.
- Use `.gitkeep` to preserve folder structure.
- Do **not** store secrets or production configs here.
- CI pipelines can freely clean or rebuild this directory.

---

**Final, release-ready artifacts → `/dist/`**  
**Logs, coverage, test results → `/logs/` and `/coverage/`**

---

📍 _Maintained by Claude Enterprise Integrations Build System_
