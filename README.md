# Agent-to-Agent MCP Demo

This project implements an Agent-to-Agent (A2A) communication system using the MCP (Multi-Agent Communication Protocol) framework. It provides a robust server implementation for handling agent-to-agent communications with support for task management, push notifications, and streaming responses.

## System Architecture and Components

### Core Components

1. **MCP Server (`mcp_app.py`)**

   - Central server that manages the Multi-Agent Communication Protocol
   - Exposes a suite of tools (functions/endpoints) for agent consumption
   - Handles request routing and response management
   - Manages authentication and authorization
   - Provides API documentation via Swagger and ReDoc

2. **Agent B (`agentpartner.py`)**

   - Secondary agent that acts as a tool consumer
   - Registers itself with the MCP server
   - Listens for incoming requests from Agent A
   - Processes requests by utilizing MCP server tools
   - Implements error handling and request validation
   - Maintains session state and authentication

3. **Agent A (`host_agent.py`)**
   - Primary agent that initiates communication
   - Implements the A2A protocol for agent communication
   - Sends structured requests to Agent B
   - Handles response processing and error management
   - Manages task lifecycle and state

## Prerequisites

- Python 3.8+
- Virtual environment (recommended)
- OpenAI API key

## Installation

1. Clone the repository:

```bash
git clone [repository-url]
cd a2a-mcp-demo
```

2. Create and activate a virtual environment (recommended):

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Set up environment variables:

Create a `.env` file in the root directory:

```bash
touch .env  # On Windows: type nul > .env
```

Add the following configuration to your `.env` file:

```plaintext
# OpenAI API Configuration
OPENAI_API_KEY=your_openai_api_key_here

# Optional: Server Configuration
HOST=0.0.0.0
PORT=5000

# Optional: Logging Configuration
LOG_LEVEL=INFO

# Optional: Other API Keys and Configuration
# GOOGLE_CLOUD_PROJECT=your_project_id
# GOOGLE_APPLICATION_CREDENTIALS=path/to/credentials.json
```

Replace `your_openai_api_key_here` with your actual OpenAI API key. You can get your API key from the [OpenAI dashboard](https://platform.openai.com/api-keys).



## Detailed Execution Flow

### 1. Starting the MCP Server

```bash
python mcp_app.py
```

This command initializes the MCP server, which:

- Loads configuration and environment variables
- Sets up the FastAPI application
- Initializes the task manager
- Starts listening for incoming connections
- Exposes tool endpoints for agent consumption
- Makes API documentation available at `/docs` and `/redoc`

### 2. Launching Agent B (Tool Consumer)

```bash
python agentpartner.py
```

When Agent B starts, it:

- Registers itself with the MCP server
- Initializes its authentication credentials
- Sets up event listeners for incoming requests
- Prepares its tool consumption capabilities
- Enters a waiting state for instructions from Agent A

### 3. Starting Agent A (Host Agent)

```bash
python host_agent.py
```

Agent A initialization includes:

- Setting up A2A protocol handlers
- Establishing connection with Agent B
- Preparing task requests and workflows
- Initializing error handling mechanisms

### 4. Communication Flow

1. **Request Initiation**

   - Agent A formulates a task request
   - Request is serialized into A2A protocol format
   - Authentication headers are added

2. **Request Processing**

   - Agent B receives the request
   - Validates request format and authentication
   - Maps request to appropriate MCP tool
   - Calls tool via MCP server

3. **Tool Execution**

   - MCP server processes tool request
   - Executes requested operation
   - Prepares response data

4. **Response Flow**
   - Results returned to Agent B
   - Agent B formats response for A2A protocol
   - Response sent back to Agent A
   - Agent A processes and handles the result

### Quick Start Guide

1. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

2. **Configure Environment**

   - Create and set up `.env` file as described in the installation section
   - Ensure OpenAI API key is properly configured

3. **Start the Components (in separate terminal windows)**

   Terminal 1 - Start MCP Server:

   ```bash
   python mcp_app.py
   ```

   This will start the MCP server that exposes tool endpoints.

   Terminal 2 - Start Agent B:

   ```bash
   python agentpartner.py
   ```

   Agent B will register itself and wait for instructions from Agent A.

   Terminal 3 - Start Agent A:

   ```bash
   python host_agent.py
   ```

   Agent A will initiate communication with Agent B using the A2A protocol.

### Expected Results

When all components are running correctly, you should see:

- MCP Server running and listening for connections
- Agent B successfully registered with the MCP server
- Agent A establishing connection with Agent B
- Successful message exchange between agents
- Tool invocations being processed and returning results



