# RAG-KG-chatbot

## Overview
**RAG-KG-chatbot** is a project that integrates Neo4j knowledge graphs and language models for retrieval-augmented generation (RAG). The goal is to use LangChain to facilitate a chatbot that retrieves knowledge from a graph database and leverages local language models (LLMs) via Ollama for natural language responses.

## Project Structure
```bash
RAG-KG-chatbot/
│
├── api/                      # API-related code for interacting with the chatbot and handling queries
│   ├── app.py                # Main FastAPI application to serve the API endpoints (manages request/response cycle, setup, and routes)
│   ├── template_examples.txt # Contains example Cypher queries for querying Neo4j
│   ├── app.py                # Main FastAPI application to serve the API endpoints (manages request/response cycle, setup, and routes)
│   └── query_handler.py      # Defines logic for handling incoming queries, interfacing with the Neo4j database, and returning responses
│
├── data/                     # Contains dataset files
│   └── kg_file.nq.gz         # Compressed N-Quads file representing the knowledge graph (to be loaded into Neo4j)
│
├── neo4j_files/              # Neo4j-specific files for running and managing the database
│   ├── .env                  # Environment variables required for Neo4j configuration (username, password, etc.)
│   ├── example.env           # Example environment configuration file to be used as a template
│   ├── connection.py         # Handles the connection logic to Neo4j, establishing communication between the API and the database
│   ├── cypher_query.cql      # Contains Cypher queries used for interacting with the Neo4j graph database
│   ├── docker-compose.yml    # Docker Compose configuration to set up the Neo4j database service, with ports, plugins, and volumes configured
│   ├── Dockerfile            # Dockerfile to build the Neo4j instance, defining the base image and necessary configurations
│   └── wrapper.sh            # Script for project-related automation tasks (e.g., starting Neo4j, loading data)
│
├── ui/                       # Frontend UI for the chatbot interface
│   ├── index.html            # HTML structure for the chatbot's web interface
│   ├── script.js             # JavaScript to handle frontend logic, including user interaction and API requests
│   └── style.css             # CSS file for styling the chatbot's interface
│
├── src/                      # Source code for the chatbot's logic and handlers
│   ├── __init__.py           # Marks the folder as a Python package (required for package imports)
│   └── llm_handler.py        # Manages interactions with the language model (LLM), processing user inputs and generating responses
│
├── .gitignore                # Specifies files and directories to be ignored by Git (e.g., venv, data files)
├── README.md                 # Project overview, setup instructions, and usage guidelines
└── requirements.txt          # List of Python dependencies required for the project (used for setting up the virtual environment)
```

## Setup

### 1. Create an env file
Create an env file inside the neo4j_files folder.
You can use the example.env file as a template and rename it to .env. Make sure to set the appropriate environment variables, such as NEO4J_USER, NEO4J_PASS, and others related to Neo4j authentication and configuration.

### 2. Run Ollama Model
**Install Ollama**

First, install Ollama by following the instructions on their website. Once installed, you’ll be able to use local language models.

**Pull the model**

Run the following command to pull the specific model you’ll be using in the project:
```bash
ollama pull deepseek-coder-v2:16b
```

**Serve the model**
Once the model is pulled, start the Ollama model serving process:
```bash
ollama serve
```

### 3. Create and activate a virtual environment
```bash
python3 -m venv venv
source venv/bin/activate
```

### 4. Install dependencies
```bash
pip install -r requirements.txt
```

### 5. Run Neo4J

Ensure you have Docker and docker-compose installed. To start Neo4j and configure it automatically:

Run the following command from the root of the project:
```bash
docker-compose -f neo4j_files/docker-compose.yml up
```
This will:

* Start a Neo4j instance with the appropriate plugins (apoc, n10s).
* Expose ports 7474 (web interface) and 7687 (Bolt protocol).
* Import your knowledge graph data into Neo4j.

Once Neo4j is up, you can access the Neo4j browser at http://localhost:7474 and interact with the graph using your configured database.

*Optional: Use Example Query for Fine-Tuning and Enhancing LLM Response*

The `template_examples.txt` file in the `api/` folder, which should be renamed to `examples.txt`, contains adaptable Cypher queries for working with graph data in Neo4j. These queries can be used to optimize both Neo4j interactions and how the language model generates responses to specific questions. By fine-tuning or customizing these queries, you can improve the model’s ability to provide more precise and relevant answers, leading to better overall performance in both querying and response accuracy.

Additionally, users can also include a help reply in the same file, separating it from the examples with the %%% delimiter.

### 6. Serve Static Files

To serve static files, run a local HTTP server using Python:
```bash
python -m http.server
```

### 7. Run the API Server

To start the API server using Uvicorn, run the following command:
```bash
uvicorn api.app:app --reload --port 8080
```
Once the API server is running, you can access the static files at http://localhost:8080/static/index.html.


## Technologies

* **Neo4j**: Graph database for storing and retrieving knowledge.
* **LangChain**: Framework for building LLM-powered applications with integrated retrieval.
* **Ollama**: Provides local LLM inference and model serving.
* **FastAPI**: A high-performance web framework for building APIs.
* **Uvicorn**: ASGI server to serve the FastAPI-based backend.
* **Docker**: Containerization platform to run Neo4j and other services.
* **Cypher**: Query language for interacting with Neo4j graph databases.

## License

This project is licensed under the MIT License.