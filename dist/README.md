# 🚚 dist/ — Release Artifacts

**Purpose:** Publish-ready bundles for users and CI releases.  
Artifacts here are **versioned**, reproducible, and safe to attach to GitHub Releases.

## Layout
- `mcp-python/` — Python wheels/sdists
- `mcp-node/` — Node/TS tarballs or bundles
- `skills/` — Claude skill bundles (skill.md + schema.json + examples)
- `examples/` — Demo packs with data & run scripts
- `docs/` — Exported PDFs or static site
- `manifests/` — K8s/Compose release manifests
- `checksums/` — `SHA256SUMS.txt` and detached signatures
- `release-notes/` — Version-scoped notes

## Conventions
- Filenames: `name-<semver>.<ext>` (e.g., `mcp-basic-server-0.1.0.tgz`)
- Include a checksum entry for each binary
- Optional: `SBOM-<semver>.spdx.json` per package

## Typical Generation (CI)
- Python: `python -m build` → `dist/mcp-python/*.whl|*.tar.gz`
- Node:   `npm pack` → `dist/mcp-node/*.tgz`
- Skills: `zip -r dist/skills/<skill>-<ver>.zip templates/skills/<skill>/`
- Docs:   `pandoc` or site exporter → `dist/docs/*.pdf`
- Manifests: render from `templates/deployment` → `dist/manifests/...`

> **Note:** `build/` is for intermediate files. Only **final** artifacts belong here.
