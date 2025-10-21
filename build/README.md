# build/ (ephemeral)
Scratch space for local/CI builds. Intermediate artifacts live here and are **not** versioned.
- mcp-python/: wheel/sdist staging, generated schemas
- mcp-node/: raw TS build, maps, caches
- skills/: assembled skill bundles (pre-release)
- docs-preview/: static site/PDF previews
- assets/: rendered diagrams/images
- k8s/: rendered manifests for local tests
- packages/: course ZIPs before promoting to dist/
Final, publishable artifacts go to ../dist/.
