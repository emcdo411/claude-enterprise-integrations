# 🔌 Model Context Protocol (MCP)

## Overview
The **Model Context Protocol (MCP)** defines how Claude communicates with external systems (MCP servers) via structured I/O.  
It standardizes **tool invocation**, **resource retrieval**, and **skill orchestration** across all languages.

---

## Message Format

### Request
Each message must be valid UTF-8 JSON, newline-delimited.

```json
{
  "id": "a9e5c123",
  "type": "tool",
  "name": "get_property_details",
  "args": {
    "property_id": "TX-90124"
  }
}
