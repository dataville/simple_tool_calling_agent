# 🤖 Tool-Calling AI Agent with Semantic Evaluation

**A complete educational implementation showing how to build an AI agent with function calling capabilities and LLM-based evaluation - built with LangChain, LangGraph, and vLLM.**

## 🎯 What This Demonstrates

This tutorial notebook shows the complete pipeline for building agents that make **actual structured function calls** rather than just describing tool usage in text or faking tool calls. Includes:

- ✅ **Structured Tool Calling**: Agent executes real functions with validated parameters, not text descriptions or fake calls
- ✅ **Smart Routing**: LangGraph workflow that decides when to call tools vs. return final answers
- ✅ **Pydantic Validation**: Type-safe tool schemas that guide LLM behavior
- ✅ **Semantic Evaluation**: LLM-based assessment that avoids brittle keyword matching
- ✅ **GPU-Efficient Architecture**: Single model instance serving dual roles (agent + evaluator)
- ✅ **Production Patterns**: Proper error handling, state management, and tool composition

## 🛠️ Technologies

- **LangChain** - Tool abstraction and LLM integration framework
- **LangGraph** - Workflow orchestration with state graphs
- **vLLM** - High-performance local model serving with function calling support
- **Hermes-3-Llama-3.1-8B** - Function-calling capable open-source model
- **Pydantic** - Schema validation and structured outputs
- **Python 3.11+** - Core implementation

## 🏗️ Architecture

**Single LLM, Dual Roles:**
- **Agent Mode**: `llm_with_tools` - generates structured tool calls
- **Evaluator Mode**: `llm` (raw) - performs semantic assessment

This hybrid approach runs on a 24GB GPU by using one model instance for both agent execution and evaluation, avoiding the memory overhead of loading two separate models.

## 📋 What You'll Learn

### Core Concepts
1. **Why model selection matters** - Function calling requires specifically trained models
2. **Tool binding workflow** - How to properly attach tools to LLMs (commonly skipped by coding agents)
3. **Pydantic schema design** - Creating tool definitions that guide LLM behavior
4. **Docstring importance** - How comprehensive docstrings determine agent reliability
5. **State graph patterns** - Building agent workflows with conditional routing

### Implementation Details
- Setting up vLLM with tool-calling flags for Hermes models
- Defining tools with proper validation and error handling
- Building state graphs with the current LangGraph API
- Implementing semantic evaluation vs. brittle keyword matching
- Managing conversation state and context across turns

## 🚀 Quick Start

### Prerequisites

- Python 3.11 or higher
- CUDA-capable GPU with 24GB VRAM (for local deployment)
- Jupyter Notebook or JupyterLab

### Installation

```bash
# Install dependencies
pip install langchain langchain-openai langgraph pydantic

# Install vLLM (requires CUDA)
pip install vllm
```

### Start vLLM Server

In a separate terminal, start the vLLM server with function-calling support:

```bash
vllm serve NousResearch/Hermes-3-Llama-3.1-8B \
  --port 8082 \
  --enable-auto-tool-choice \
  --tool-call-parser hermes \
  --max-model-len 8192
```

**Flag explanations:**
- `--enable-auto-tool-choice`: Enables automatic tool calling
- `--tool-call-parser hermes`: Uses Hermes-specific parser for structured calls
- `--max-model-len 8192`: Limits context window to fit in 24GB GPU memory

Wait for "Application startup complete" message before running the notebook.

### Run the Notebook

```bash
jupyter notebook simple_tool_agent.ipynb
```

Execute cells sequentially. The notebook includes:
1. Connection setup
2. Tool definitions (weather, calculator)
3. Agent graph construction
4. Combined testing with message flow and evaluation output

## 🔧 Implemented Tools

### Weather Tool
- Returns mock weather data for any location
- Demonstrates single-parameter tool with string validation
- Shows proper Pydantic schema with length constraints

### Calculator Tool
- Performs basic arithmetic (add, subtract, multiply, divide)
- Demonstrates multi-parameter tool with type enforcement
- Includes error handling for invalid operations and division by zero

Both tools follow production patterns:
- Complete docstrings with parameter descriptions
- Pydantic validation schemas
- Natural language return values (not just raw numbers)
- Proper error messages

## 📊 Evaluation System

The notebook includes a complete evaluation framework that assesses:

1. **Tool Selection**: Did the agent choose appropriate tools?
2. **Response Quality**: Is the final answer clear and complete?
3. **Overall Success**: Does the response address the user's question?

**Why LLM-based evaluation?**
- Understands semantic variations ("multiply" vs "multiplied" vs "times")
- Avoids brittle keyword matching that breaks on rephrasing
- Provides reasoned assessment with explanations
- Scales to complex tool compositions

## ⚠️ Important Notes

### Model Selection is Critical

**This will NOT work with general instruction models.** Function calling requires models specifically trained for structured tool use:

✅ **Function-calling capable:**
- Hermes-3-Llama-3.1-8B (used here)
- GPT-4, GPT-3.5-turbo
- Claude 3+ models
- Mistral-Large, Mixtral-8x7B-Instruct

❌ **Lacks function calling:**
- Mistral-7B-Instruct
- Base Llama models
- Most general chat models

Wrong model = text descriptions instead of actual function calls.

### AI Coding Assistants Generate Outdated Code

**Warning**: Many LLM-based coding assistants (Claude, ChatGPT, etc.) generate LangGraph code using the **legacy API**:

```python
# Legacy (outdated)
graph_builder.set_entry_point("agent")
lambda x: "tools" if x else "__end__"
```

This notebook uses the **current API**:

```python
# Current (correct)
graph_builder.add_edge(START, "agent")
lambda x: "tools" if x else END
```

If copying code from AI assistants, verify and update to current syntax.

## 🔄 From Tutorial to Production

This notebook provides patterns for building agents that interact with:

- 🌐 **REST APIs** - Web services, external data sources
- 🗄️ **Databases** - SQL queries, data retrieval
- 📊 **Analytics Tools** - Data processing, visualization
- 🔧 **System Operations** - File management, command execution
- 💼 **Business Systems** - CRM, ERP, ticketing systems

Key extension points:
- Add more tools by following the Pydantic + docstring pattern
- Implement authentication for external APIs
- Add retry logic and rate limiting
- Extend evaluation criteria for domain-specific requirements
- Scale to multi-agent systems with specialized tool sets

## 📁 Repository Structure

```
.
├── simple_tool_agent.ipynb  # Main tutorial notebook
├── README.md                # This file
└── LICENSE                  # MIT License
```

## 🎓 Learning Path

**Recommended order:**
1. Read through the complete notebook markdown (don't execute yet)
2. Understand the architecture overview and why each component exists
3. Start vLLM server and verify it loads successfully
4. Execute notebook cells one by one, reading output carefully
5. Experiment by modifying test questions
6. Try adding a new tool using the established patterns

## 📝 License

MIT License - See LICENSE file for details.

## 🏷️ Keywords

`AI agents` `LangChain` `LangGraph` `function calling` `tool calling` `structured outputs` `Pydantic validation` `LLM evaluation` `Hermes-3` `vLLM` `semantic evaluation` `agent workflows` `state graphs`

---
