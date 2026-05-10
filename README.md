# FlowAI Studio

FlowAI Studio is an advanced full-stack visual low-code orchestration platform for AI applications. It is designed to lower the barrier to AI app development, enabling developers and business users to quickly build, test, and deploy complex AI workflows through intuitive drag-and-drop interactions.

## Highlights

### 1. Deep Visual Workflow Orchestration
- Interactive editor: Built with React Flow, supporting free node dragging, zooming, and automatic layout.
- Rich node types:
    - Start/end nodes: Define workflow boundaries.
    - LLM nodes: Support custom prompts and model parameter configuration, such as temperature and max tokens.
    - RAG retrieval nodes: Seamlessly integrate knowledge bases for retrieval-augmented generation.
    - Conditional branch nodes: Support logical decisions for complex workflow paths.
    - Skill/tool nodes: Invoke external APIs or built-in capabilities.
- Real-time state synchronization: Canvas operations are tightly synchronized with global state management through Zustand, ensuring consistent orchestration data.

### 2. Powerful RAG Knowledge Base Management
- Full lifecycle management: Supports document upload, chunking, vectorization, storage, and retrieval.
- Multi-format support: Handles common document formats to provide precise context for AI.
- Debugging tools: Includes built-in retrieval testing so you can validate knowledge base recall before orchestration.

### 3. Flexible Plugin and Tool System
- MCP integration: Follows the Model Context Protocol, making it easy to extend AI with external perception and action capabilities.
- Built-in tool library: Provides common tools such as web search, calculator, and code execution.
- Custom skills: Allows developers to define custom tool interfaces and use them as workflow nodes.

### 4. Enterprise-Grade Backend Architecture
- Modular design: The backend is built with NestJS and follows a strict modular architecture, keeping business logic clear and easy to extend.
- Robust data layer: Uses Prisma ORM with SQLite/PostgreSQL and supports complex relational queries and transactions.
- Unified API standards: Includes global exception filters, response interceptors, and request validation pipes to keep API calls safe and consistent.

### 5. Production-Ready Application Management
- Application status control: Supports draft, published, archived, and other application states.
- User authentication: JWT-based authentication and authorization protect private user data and workflow assets.
- Responsive UI: Adapts to different screen sizes and provides a smooth desktop experience.

## Tech Stack

### Frontend
- Core framework: React 18 (Vite)
- State management: Zustand
- UI component library: Ant Design (AntD)
- Flowchart engine: React Flow
- Styling: Tailwind CSS / CSS Modules
- Routing: React Router v6

### Backend
- Core framework: NestJS
- Database layer: Prisma ORM + SQLite
- Authentication and security: JWT (JSON Web Token) + Passport
- Validation tools: Zod / Class-validator
- Async communication: Axios / Server-Sent Events (SSE)

## Quick Start

### 1. Prerequisites
Make sure your development environment has the following software installed:
- Node.js (v18.0.0 or later)
- npm (v9.0.0 or later)

### 2. Backend Setup and Startup
```bash
cd flowai-studio-backend
# Install dependencies
npm install
# Configure environment variables (see .env.example)
cp .env.example .env
# Sync the database schema
npx prisma db push
# Seed default demo data (default account, knowledge base, and document)
npx prisma db seed
# Start the backend development server
npm run start:dev
```
*Note: Make sure to add a valid QWEN_API_KEY in the .env file to enable AI node functionality.*

### 3. Frontend Setup and Startup
```bash
cd flowai-studio-frontend
# Install dependencies
npm install
# Start the frontend development server
npm run dev
```
After startup, open http://localhost:5173 in your browser to begin building AI workflows.

## Default Account (Demo)
- Username: admin
- Password: admin123

## How to Verify RAG (Shortest Path)
1. Log in with the default account.
2. Open "Knowledge Base Management" and confirm that the "Default Knowledge Base" exists and contains the document "FlowAI Studio Feature Introduction.md".
3. Open "Debug Center" -> "AI Chat" and select the "Default Knowledge Base" from the "Associated Knowledge Base" dropdown.
4. Send the question: `What are the core features of FlowAI Studio?`
5. Check the "Reference Documents" section below the response. You should see matched document chunks and similarity scores.

## Verify RAG via API (Optional)
```bash
# 1) Log in to get a token
curl -s -X POST http://localhost:3000/api/users/login \
  -H 'Content-Type: application/json' \
  -d '{"username":"admin","password":"admin123"}'

# 2) Get the knowledge base list with the token (replace YOUR_TOKEN with the token returned above)
curl -s http://localhost:3000/api/rag/knowledge-bases \
  -H "Authorization: Bearer YOUR_TOKEN"

# 3) Retrieve documents (replace KB_ID with the knowledge base id)
curl -s -X POST http://localhost:3000/api/rag/retrieve \
  -H 'Content-Type: application/json' \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -d '{"query":"What are the core features of FlowAI Studio?","knowledgeBaseId":"KB_ID","topK":3}'
```

## Project Structure

```text
├── flowai-studio-frontend   # Frontend project
│   ├── src/components       # Shared components and workflow node components
│   ├── src/pages            # Business page views
│   ├── src/store            # Global state management slices
│   ├── src/router           # Routing configuration
│   └── src/types            # TypeScript type definitions
└── flowai-studio-backend    # Backend project
    ├── src/modules          # Business modules (AI, Workflow, RAG, User, etc.)
    ├── src/common           # Shared middleware, decorators, and interceptors
    ├── src/config           # Environment variables and global configuration
    └── prisma               # Database schema definitions
```
