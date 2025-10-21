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
Response
json
Copy code
{
  "id": "a9e5c123",
  "ok": true,
  "data": {
    "address": "123 Main St, Dallas TX",
    "price": 625000
  },
  "error": null
}
Core Message Types
Type	Purpose	Example
tool	Executes a defined tool function	sum_numbers, get_client
resource	Returns static or dynamic metadata	/version, /config
ping	Health/latency check	{ "type": "ping" }
event	Asynchronous status updates	build_complete, error_report

Transport
Stdio (default)
Line-delimited JSON (\n)

Must print a startup message { "ok": true, "ready": true }

All responses must flush immediately.

HTTP (optional)
RESTful endpoints mirror stdio schema.

Useful for debugging and CI tests.

Error Handling
Every response must include:

Field	Type	Description
ok	boolean	Indicates success
error	string or null	Human-readable error message
details	object (optional)	Stack, trace, or context

Example:

json
Copy code
{
  "id": "b17",
  "ok": false,
  "error": "missing parameter: property_id"
}
Correlation
Each request includes a unique id.
Responses must echo the same id for deterministic matching in async environments.

Timeout & Retries
Default timeout: 15 s

Retryable errors: UPSTREAM_ERROR, TIMEOUT, TRANSIENT_NETWORK

Retries use exponential backoff (2× delay, up to 3 tries)

Versioning
All MCP servers expose a resource: version

json
Copy code
{ "name": "mcp-basic-server", "version": "0.1.0", "language": "python" }
Security
Input validated using JSON Schema or typed models (Pydantic, Zod)

Never execute raw code from input

Sanitize logs (remove tokens, PII)

📎 See also:

mcp-tools-and-resources.md

auth.md

yaml
Copy code

---

### ⚙️ **File 2 — `mcp-tools-and-resources.md`**

```markdown
# ⚙️ MCP Tools and Resources

## Purpose
Defines how tools and resources are declared, discovered, and invoked by Claude MCP servers.

---

## 1️⃣ Tools

### Definition
A **tool** is an executable function that performs an action.

Example (Python):
```python
@tool("sum_numbers")
def sum_numbers(numbers: list[int]) -> dict:
    return {"total": sum(numbers)}
Example (TypeScript):

ts
Copy code
tool("sum_numbers", (args) => ({
  total: (args.numbers as number[]).reduce((a,b)=>a+b,0)
}));
Tool Metadata
Each tool must define:

Field	Type	Required	Description
name	string	✅	Unique ID
description	string	✅	What the tool does
args_schema	object	✅	JSON Schema for input
returns_schema	object	✅	JSON Schema for output
examples	array	❌	Example invocations

Example manifest:

json
Copy code
{
  "name": "sum_numbers",
  "description": "Adds numbers in an array.",
  "args_schema": { "type": "object", "properties": { "numbers": { "type": "array", "items": { "type": "number" } } } },
  "returns_schema": { "type": "object", "properties": { "total": { "type": "number" } } }
}
2️⃣ Resources
Definition
A resource is a read-only or computed data endpoint.

Examples:

/version — metadata

/config — server settings

/health — uptime, latency metrics

Example response:

json
Copy code
{
  "ok": true,
  "data": {
    "name": "mcp-basic-server",
    "version": "0.1.0",
    "uptime_seconds": 5421
  }
}
3️⃣ Registration & Discovery
Each MCP server must register its tools/resources during startup.

Python:

python
Copy code
self.tools = {"sum_numbers": self.sum_numbers}
self.resources = {"version": lambda: {...}}
TypeScript:

ts
Copy code
this.tools = { "sum_numbers": this.sumNumbers };
this.resources = { "version": () => ({ ... }) };
Listing Endpoint
Optional meta-tool for discovery:

json
Copy code
{ "type": "resource", "name": "list_tools" }
Returns:

json
Copy code
{
  "ok": true,
  "data": ["ping", "sum_numbers", "normalize_text"]
}
4️⃣ Error Semantics
All tool invocations follow MCP’s error schema.

Error	Cause	Recoverable
INVALID_ARGUMENT	schema validation failed	❌
NOT_FOUND	resource missing	✅
TIMEOUT	downstream latency	✅
INTERNAL_ERROR	unhandled exception	❌

5️⃣ Best Practices
Keep tools idempotent and side-effect free.

Prefer structured responses over free text.

Document input/output with JSON Schemas.

Always return ok and error fields.

Include example invocations in your README.

📎 See also:

mcp-protocol.md

auth.md

yaml
Copy code

---

### 🔒 **File 3 — `auth.md`**

```markdown
# 🔒 Authentication and Authorization

## Overview
All Claude MCP servers must implement consistent authentication and authorization to protect sensitive APIs and data.

---

## 1️⃣ Supported Methods

| Method | Description | Typical Use |
|---------|--------------|--------------|
| **API Key** | Static key in header or env | Local development, internal tools |
| **OAuth 2.0** | Token exchange via client credentials or device flow | SaaS or multi-user integrations |
| **JWT** | Signed JSON Web Tokens with claims | Microservices, stateless deployments |

---

## 2️⃣ API Key Authentication

### Configuration
Environment variable or `.env`:
API_KEY=your-secret-key

shell
Copy code

### Request Header
Authorization: Bearer your-secret-key

pgsql
Copy code

Server-side check (Python):
```python
if headers.get("Authorization") != f"Bearer {os.getenv('API_KEY')}":
    return {"ok": False, "error": "UNAUTHORIZED"}
3️⃣ OAuth 2.0 (Client Credentials)
Flow
Client posts to /oauth/token

Receives access_token

Sends with each MCP request:

makefile
Copy code
Authorization: Bearer <access_token>
Server validates via introspection endpoint or local verification.

Example (Python + FastAPI)
python
Copy code
from fastapi import Security, HTTPException
from fastapi.security import HTTPBearer
auth = HTTPBearer()

def verify_token(credentials=Security(auth)):
    token = credentials.credentials
    if not verify_jwt(token):
        raise HTTPException(status_code=403, detail="Invalid token")
4️⃣ JWT Authentication
Use for internal trusted services.

Structure
json
Copy code
{
  "sub": "service:mcp-server",
  "iss": "auth.claude.ai",
  "exp": 1720000000,
  "roles": ["read:tools", "invoke:skills"]
}
Server validates signature & claims before tool execution.

5️⃣ Role-Based Access Control (RBAC)
Role	Permissions
viewer	Read resources
operator	Invoke tools
admin	Register/unregister tools/resources

Enforce via simple role map:

python
Copy code
if "invoke:skills" not in claims["roles"]:
    raise PermissionError("Missing permission")
6️⃣ Audit Logging
Each request must include metadata for traceability:

Timestamp

Request ID

Auth type

User/service ID

Tool or resource invoked

Result (ok / error)

Output to JSON log:

json
Copy code
{
  "time": "2025-10-21T20:00:00Z",
  "auth_type": "JWT",
  "subject": "service:mcp-server",
  "action": "invoke_tool",
  "tool": "sum_numbers",
  "ok": true
}
7️⃣ Security Best Practices
Always use HTTPS or TLS if HTTP transport is enabled.

Never log full tokens.

Rotate keys periodically.

Validate token expiry and issuer.

Return 401 Unauthorized instead of generic errors.

Include WWW-Authenticate header on failures.

📎 See also:

mcp-protocol.md

mcp-tools-and-resources.md

yaml
Copy code

---

Would you like me to generate matching **`errors.md`** and **`versioning.md`** files next to complete the API reference set?






