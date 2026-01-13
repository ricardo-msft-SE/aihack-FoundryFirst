# Step-by-Step Guide: Building the Agent in Foundry

This guide walks you through creating the Virtual Citizen Assistant using the Azure AI Foundry portal.

## Phase 1: Create the Project
1.  Navigate to [Azure AI Foundry](https://ai.azure.com).
2.  Click **+ Create Project**.
3.  Name the project `Virtual-Citizen-Assistant`.
4.  Select your hub and resource group, then click **Create**.

---

## Phase 2: The "Brain" (Create the Agent)
1.  In the left navigation bar, select **Agents** (under *Build*).
2.  Click **+ Create agent**.
3.  **Name**: `CityServicesBot`
4.  **Model Deployment**: Select `gpt-4o` or `gpt-4-turbo`.
5.  **Instructions**: Paste the following system prompt:
    > "You are a helpful Virtual Citizen Assistant for the City of Exampleville. You help citizens find information about public services, permits, and schedules. Always be polite, concise, and prioritize official city data found in your Knowledge base. If you cannot find the answer, direct them to call 311."
6.  Click **Create**.

---

## Phase 3: The "Knowledge" (No-Code RAG)
*Replaces: `document_retrieval_plugin.py`*

Instead of writing Python code to query Azure Search, we will attach the data directly.

1.  **Prepare Data**: Ensure you have your city documents (PDFs, DOCX) ready.
2.  In your Agent's overview page, locate the **Knowledge** section.
3.  Click **+ Add knowledge**.
4.  Select **Create a new search index**.
5.  **Upload Data**: Upload your sample files (e.g., `recycling_schedule.pdf`, `permit_guide.docx`).
6.  **Configure Search**:
    * Select your Azure AI Search service (or create a new free tier one).
    * Keep the default vectorization settings.
7.  Click **Next** and finish the wizard.
    * *Note: Foundry will now automatically chunk, embed, and index these files.*

---

## Phase 4: The "Actions" (No-Code Plugins)
*Replaces: Python Functions and Semantic Kernel Plugins*

We will add a "Tool" that lets the agent check a permit status. Since we don't have a real city API, we will use a **Mock OpenAPI definition**.

### Step 4a: Create the OpenAPI Spec
Save the following JSON as `permit-api.json` on your computer:

```json
{
  "openapi": "3.0.0",
  "info": {
    "title": "City Permit API",
    "version": "1.0.0",
    "description": "API to check status of city permits"
  },
  "paths": {
    "/permits/{permitId}": {
      "get": {
        "operationId": "GetPermitStatus",
        "summary": "Gets the status of a specific permit",
        "description": "Retrieves current status (Approved, Pending, Denied) of a building permit.",
        "parameters": [
          {
            "name": "permitId",
            "in": "path",
            "required": true,
            "schema": { "type": "string" },
            "description": "The unique permit ID (e.g., PER-123)"
          }
        ],
        "responses": {
          "200": {
            "description": "Successful response",
            "content": {
              "application/json": {
                "schema": {
                  "type": "object",
                  "properties": {
                    "status": { "type": "string" },
                    "estimated_completion": { "type": "string" }
                  }
                }
              }
            }
          }
        }
      }
    }
  },
  "servers": [
    { "url": "[https://example-city-api.com/api](https://example-city-api.com/api)" }
  ]
}
