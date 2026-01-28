# MCP Phishing Response Demo

Demo mocking an SOC agent that triages a phishing report and triggers containment via an MCP tool backed by an Orkes Conductor workflow.

## What’s included
- `mcp-workbench-orkes-demo.ipynb` — end-to-end notebook:
  - registers a Conductor workflow
  - defines an input JSON schema
  - shows MCP `tools/list` + `tools/call`
  - includes MCP Workbench validation steps

## Prereqs
- Orkes Conductor Cloud account
- MCP Workbench (locally hosted)
- An MCP Gateway service + route in Orkes

## Quick start
1. **Set Orkes env vars** (use your own values):
   ```bash
   export ORKES_KEY_ID="..."
   export ORKES_KEY_SECRET="..."
   export ORKES_SERVER_URL="https://developer.orkescloud.com/api"
   export ORKES_APPLICATION_NAME="mcp-workbench-gateway-demo"
   ```

2. **Open the notebook** and run the workflow registration cells.

3. **Create the schema** in Orkes UI (Definitions → Schema) using the `phishing_schema` cell.

4. **Create the MCP Gateway service + route**
   - Service: MCP Enabled = ON
   - Route: `/phishing-response` → workflow `phishing_response_wf`
   - Attach input schema `PhishingResponseSchema`

5. **Set MCP endpoint + auth**
   ```bash
   export MCP_TOOL_ENDPOINT="https://<your-mcp-endpoint>/mcp"
   export MCP_AUTH_HEADER="x-api-key"   # or "authorization"
   export MCP_AUTH_VALUE="<your-value>"
   ```

6. **Run MCP calls** from the notebook to list tools and call the phishing tool.

## MCP Workbench validation
- Add connection → **Streamable HTTP (Stateless)**
- Paste MCP Tool Remote Endpoint
- Add the same auth header/value
- List tools → run phishing response tool with the sample input

## Notes
- The notebook uses only stdlib HTTP calls
- Output includes: `caseId`, `status`, `severity`, `ioc`, `summary`.
