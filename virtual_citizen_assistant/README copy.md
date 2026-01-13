# Virtual Citizen Assistant (Foundry Edition)

> **A Low-Code / No-Code implementation of the Virtual Citizen Assistant using Microsoft Azure AI Foundry.**

## 📋 Overview
This repository demonstrates how to build a **Virtual Citizen Assistant** using the **Azure AI Foundry Agent Service**. 

Unlike traditional "Code-First" approaches (using Semantic Kernel or LangChain), this project uses a "Service-First" architecture. We replace manual Python orchestration, custom RAG pipelines, and code-based plugins with Foundry's native **Knowledge** and **Actions** features.

### 🎯 Goals
* **Zero Infrastructure Management:** No need to manage conversation state or hosting servers.
* **Native RAG:** Replace custom Python search code with Foundry "Knowledge" connections.
* **Declarative Actions:** Replace code-based plugins (`@kernel_function`) with OpenAPI specifications.

## 📐 Architecture

![Virtual Citizen Assistant Architecture](./images/architecture_diagram.png)

The solution leverages a **Service-First** architecture designed for simplicity and scale:

1.  **User Interface:** Users interact via a standard chat interface (Web App or Microsoft Teams).
2.  **Orchestration (The Brain):** The **Azure AI Agent Service** manages the conversation history, reasoning loop, and tool selection.
3.  **Knowledge (RAG):** The agent natively queries **Azure AI Search** to retrieve answers from official city documents (PDF/Markdown) stored in **Azure Blob Storage**.
4.  **Actions (Plugins):** For transactional lookups (e.g., permit status), the agent executes HTTP calls to external APIs defined via **OpenAPI specifications**.

## 🏗️ Approach Comparison

| Feature | Code-First Approach (Original) | Foundry Approach (This Repo) |
| :--- | :--- | :--- |
| **Orchestration** | Python/C# Loop | Azure AI Agent Service (Managed) |
| **Retrieval (RAG)** | `SearchClient` code | Native **Knowledge** integration |
| **Plugins** | Python Classes | **Actions** (OpenAPI / Swagger) |
| **Hosting** | App Service / Container | Serverless / Cloud-Hosted |

## 🚀 Key Features
1.  **Citizen Q&A**: Answers questions about city services using official documents.
2.  **Service Lookup**: Queries external APIs (simulated via OpenAPI) to check permit statuses or schedules.
3.  **Persona Management**: Uses system instructions to maintain a polite, helpful civic persona.

## 🛠️ Prerequisites
* An **Azure Subscription**.
* Access to **Azure AI Foundry** (formerly Azure AI Studio).
* (Optional) Azure AI Search resource (created automatically during setup).

## 📚 Documentation
* Follow the [Step-by-Step Guide](step_by_step.md) to rebuild this agent from scratch in the UI.

---
*This project is an alternative implementation of the [Microsoft AI Hackathon Use Cases](https://github.com/msftsean/ai-hackathon-use-cases).*
