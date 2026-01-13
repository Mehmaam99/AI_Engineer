# LangChain Tool-Based Agent (Calculator Bot)

**Overview:**
This is a CLI-based assistant that uses Gemini via LangChain and includes a custom arithmetic calculator tool that can be invoked during conversations.

**Technologies Used:**
- LangChain (tools, messages, React agent)
- Gemini (ChatGoogleGenerativeAI)
- dotenv for API key

**Flow:**
1. Gemini model is initialized.
2. A custom calculator function is registered as a tool.
3. React agent is created using LangGraph's `create_react_agent()`.
4. User inputs text via CLI.
5. Agent either chats or calls the calculator tool.

**Purpose:**
To demonstrate how to integrate tools in an AI assistant using LangChain's React-style agent architecture.

## How to Run

### Prerequisites
- Python 3.12+
- Docker (optional)
- A Google Cloud API Key for Gemini

### Environment Setup
1. Copy the example environment file:
   ```bash
   cp env_example .env
   ```
2. Open `.env` and add your `GOOGLE_API_KEY`.

### Running Locally
1. Install dependencies using uv (recommended) or pip:
   ```bash
   # Using uv
   uv sync
   
   # Using pip (requires pip >= 21.3 for pyproject.toml support)
   pip install .
   ```

2. Run the application:
   ```bash
   # Using uv
   uv run main.py
   
   # Using python directly
   python main.py
   ```

### Running with Docker

1. **Build the Docker image:**
   ```bash
   docker build -t basic-chatbot .
   ```

2. **Run the container:**
   The `-it` flags are required to interact with the chatbot in the terminal.
   ```bash
   docker run -it --env-file .env basic-chatbot
   ```

### Running with Docker Compose

To run interactively using Docker Compose:
```bash
docker compose run --rm basic-chatbot
```