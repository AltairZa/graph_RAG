# Graph RAG Architecture

## Overview

This document describes the architecture of the **Graph RAG (Retrieval-Augmented Generation)** application that combines **OpenAI GPT** with **GraphDB knowledge graphs** for intelligent jaguar conservation queries. The application demonstrates how to build a conversational AI using **Microsoft Agent Framework DevUI** that can query structured data using SPARQL and provide natural language responses.

## Graph RAG High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                Microsoft Agent Framework                    │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                    DevUI Server                    │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │   │
│  │  │   Auto-     │  │ Interactive │  │   Built-in  │ │   │
│  │  │   opening   │  │    Chat     │  │  Debugging  │ │   │
│  │  │   Browser   │  │ Interface   │  │   Tools     │ │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘ │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────┬───────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────┐
│                Microsoft Agent Framework                    │
│  ┌─────────────────────────────────────────────────────┐   │
│  │            Jaguar Query Agent                       │   │
│  │  ┌─────────────────────────────────────────────┐   │   │
│  │  │            System Prompt                    │   │   │
│  │  │  - Graph RAG Instructions                   │   │   │
│  │  │  - SPARQL Guidelines                        │   │   │
│  │  │  - Response Formatting                      │   │   │
│  │  └─────────────────────────────────────────────┘   │   │
│  │                                                   │   │
│  │  ┌─────────────────────────────────────────────┐   │   │
│  │  │            OpenAI Client                    │   │   │
│  │  │  - GPT-4 Integration                       │   │   │
│  │  │  - Function Calling                        │   │   │
│  │  │  - Thread Management                       │   │   │
│  │  └─────────────────────────────────────────────┘   │   │
│  │                                                   │   │
│  │  ┌─────────────────────────────────────────────┐   │   │
│  │  │            GraphDB Tool                     │   │   │
│  │  │  - SPARQL Query Execution                   │   │   │
│  │  │  - Query Validation                        │   │   │
│  │  │  - Result Processing                       │   │   │
│  │  └─────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────┬───────────────────────────────────┘
                          │
┌─────────────────────────▼───────────────────────────────────┐
│                    External Systems                         │
│  ┌─────────────────┐              ┌─────────────────────┐   │
│  │   OpenAI API    │              │      GraphDB        │   │
│  │  - GPT-4        │              │  - RDF Triple Store │   │
│  │  - Responses    │              │  - SPARQL Engine    │   │
│  │  - Function     │              │  - Jaguar Ontology  │   │
│  │    Calling      │              │  - Conservation     │   │
│  └─────────────────┘              │    Data             │   │
│                                   └─────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

## Graph RAG Component Descriptions

### Microsoft Agent Framework DevUI
- **DevUI Server**: Built-in development web interface
- **Auto-opening Browser**: Automatically launches at `http://localhost:8000`
- **Interactive Chat**: Real-time conversation interface
- **Built-in Debugging**: Visual inspection of agent behavior
- **Zero Configuration**: No frontend code required

### Microsoft Agent Framework
- **Jaguar Query Agent**: Graph RAG specialist agent
- **System Prompt**: Graph RAG instructions and SPARQL guidelines
- **OpenAI Client**: GPT-4 integration with function calling
- **Thread Management**: Conversation context preservation

### Graph RAG Tools
- **GraphDB Tool**: Core Graph RAG component
  - Executes SPARQL queries against knowledge graph
  - Validates query syntax and ontology compliance
  - Processes and formats query results

### External Systems
- **OpenAI API**: GPT-4 model for natural language processing
- **GraphDB**: RDF triple store with jaguar conservation ontology

## Graph RAG Data Flow

### User Query Processing

1. **DevUI Launch**: `python3 main.py` starts DevUI server
2. **User Interaction**: User types queries in DevUI interface
3. **Agent Processing**:
   - **LLM Analysis**: OpenAI GPT analyzes natural language query
   - **SPARQL Generation**: AI generates appropriate SPARQL query
   - **Tool Call Detection**: Agent Framework detects need for GraphDB tool
4. **Knowledge Graph Query**:
   - **SPARQL Execution**: `query_jaguar_database` tool executes query
   - **Result Processing**: Raw SPARQL results processed and formatted
5. **Response Generation**:
   - **LLM Interpretation**: OpenAI GPT interprets GraphDB results
   - **Natural Language Response**: Generates human-readable response
   - **Markdown Formatting**: Applies formatting with code blocks
6. **DevUI Display**:
   - **Response Delivery**: Shows formatted response in DevUI
   - **Context Preservation**: Conversation history maintained automatically

## Graph RAG Design Patterns

### 1. DevUI Pattern
DevUI provides automatic state management:
```python
# DevUI handles all state management automatically
serve(entities=[query_agent], auto_open=True)
```

### 2. Agent Framework Pattern
Microsoft Agent Framework handles conversation management:
```python
# Agent Framework manages threads and context automatically
response = asyncio.run(agent.run(user_message, thread=thread, store=True))
```

### 3. Graph RAG Pattern
Combines LLM with knowledge graph queries:
```python
# LLM generates SPARQL, tool executes against GraphDB
tools=[query_jaguar_database]
tool_choice="auto"
```

## Graph RAG State Management

### DevUI State Management
```python
# DevUI handles all state management automatically
# No manual state configuration required
serve(entities=[query_agent], auto_open=True)
```

### Thread Management
- **Agent Framework**: Microsoft Agent Framework manages conversation context
- **OpenAI Responses API**: Server-side thread persistence
- **DevUI Interface**: Built-in conversation history and state management

## Graph RAG Configuration

### Environment Variables
```bash
# OpenAI Configuration
OPENAI_API_KEY=your_openai_api_key
OPENAI_RESPONSES_MODEL_ID=gpt-4

# GraphDB Configuration
GRAPHDB_URL=http://localhost:7200
GRAPHDB_REPOSITORY=your_repo_name
```

### Agent Settings
```python
# Hardcoded in create_jaguar_query_agent() function
settings = OpenAISettings(
    api_key=os.getenv("OPENAI_API_KEY", ""),
    model_id=os.getenv("OPENAI_RESPONSES_MODEL_ID", "gpt-4")
)
```

## Graph RAG Security

### API Security
1. **API Keys**: Stored in `.env`, never committed to repository
2. **Input Validation**: User inputs validated before processing
3. **Error Handling**: Errors logged but sanitized for user display
4. **SPARQL Injection Prevention**: Tool validates SPARQL syntax

### Data Privacy
- **Read-Only Access**: Agent only reads from GraphDB
- **No Data Storage**: No user data persisted beyond conversation
- **Secure APIs**: Use HTTPS for all external communications

## Graph RAG Scalability

### Current POC State
- Microsoft Agent Framework DevUI
- Automatic state management
- Single-user design
- OpenAI API rate limits

### Future Enhancements
1. **Multi-User Support**: Session-based user management
2. **Distributed State**: Redis or database backend
3. **Horizontal Scaling**: Multiple agent instances
4. **Query Caching**: Cache frequent SPARQL patterns
5. **Async Processing**: Non-blocking GraphDB queries

## Graph RAG Monitoring

### Logging
- DevUI request/response logging
- Agent Framework conversation logging
- GraphDB query execution logging

### Performance Metrics
- **Response Times**: Graph RAG query processing time
- **Query Success Rate**: SPARQL execution success rate
- **Tool Usage**: GraphDB tool call frequency

## Graph RAG Testing

### Manual Testing
1. **DevUI Interface**: Test queries through the DevUI
2. **SPARQL Validation**: Verify generated queries in GraphDB
3. **Response Quality**: Check natural language response accuracy


## Graph RAG Dependencies

### Core Dependencies
- **Microsoft Agent Framework**: Agent management and DevUI
- **OpenAI**: GPT API client
- **Requests**: GraphDB HTTP communication
- **python-dotenv**: Environment variable management

### Graph RAG Specific
- **GraphDB**: RDF triple store
- **SPARQL**: Query language for knowledge graphs
- **RDF/Turtle**: Ontology format

## Graph RAG Project Structure

```
graph_RAG/
├── main.py                  # DevUI entry point
├── src/
│   └── agents/
│       ├── jaguar_query_agent.py  # Agent creation with DevUI integration
│       └── jaguar_tool.py         # GraphDB tool implementation
├── data/
│   ├── jaguar_ontology.ttl        # Basic jaguar ontology
│   ├── jaguar_ontology_rich.ttl    # Extended ontology with more classes
│   └── jaguars.ttl               # Jaguar instance data
├── docs/
│   ├── agent_design.md      # Agent design documentation
│   └── architecture.md      # Architecture documentation
├── .env                     # Environment variables
├── requirements.txt         # Python dependencies
└── README.md               # Project documentation
```

## Graph RAG Benefits

### Technical Benefits
- **Hybrid Intelligence**: Combines LLM reasoning with structured data
- **Real-time Queries**: Live data from knowledge graphs
- **Context Awareness**: Maintains conversation context
- **Extensible**: Easy to add new tools and capabilities

### Conservation Benefits
- **Data-Driven Insights**: Access to structured conservation data
- **Natural Language Interface**: Easy querying of complex data
- **Educational Tool**: Demonstrates Graph RAG capabilities
- **Research Support**: Facilitates conservation research queries

