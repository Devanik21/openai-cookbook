# Openai Cookbook

![Language](https://img.shields.io/badge/Language-MDX-555555?style=flat-square) ![Stars](https://img.shields.io/github/stars/Devanik21/openai-cookbook?style=flat-square&color=yellow) ![Forks](https://img.shields.io/github/forks/Devanik21/openai-cookbook?style=flat-square&color=blue) ![Author](https://img.shields.io/badge/Author-Devanik21-black?style=flat-square&logo=github) ![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=flat-square)

> A practical collection of OpenAI API recipes, patterns, and production-ready implementations for real-world LLM applications.

---

**Topics:** `api` · `cookbook` · `deep-learning` · `embeddings` · `function-calling` · `generative-ai` · `gpt-4` · `large-language-models` · `openai` · `prompt-engineering`

## Overview

This repository is a curated collection of practical OpenAI API implementations — going beyond
the official documentation examples to provide production-oriented patterns, architectural templates,
and reusable code for building reliable LLM applications. Each recipe is a complete, runnable
implementation of a specific pattern that addresses a real challenge in LLM application development.

The collection is organised around functional categories rather than API features: RAG (Retrieval
Augmented Generation) patterns for grounding LLMs in private knowledge bases, agent architectures
for multi-step autonomous task completion, structured output extraction for converting unstructured
text into typed data, prompt caching strategies for cost reduction, streaming implementations for
responsive UX, function calling patterns for tool use, and fine-tuning pipelines for domain adaptation.

A key design philosophy of this cookbook is that each recipe is opinionated and production-ready,
not just a minimal working example. It includes error handling (rate limit retry with exponential
backoff, timeout handling, graceful degradation), observability (token counting, cost tracking,
latency measurement), and testing (prompt regression tests, output validation) — the pieces that
are almost always missing from tutorial code but essential for anything running in production.

---

## Motivation

The official OpenAI documentation and cookbook cover the API surface well but leave a significant
gap between 'this is how the API works' and 'this is how to build a production application with it'.
This repository was assembled to fill that gap — patterns learned from building real LLM applications,
distilled into reusable, well-documented recipes that save the next developer the debugging time
that went into writing them the first time.

---

## Architecture

```
Cookbook Repository Structure
        │
  ┌─────────────────────────────────────────────────────────┐
  │  /rag_patterns/           (RAG architectures)          │
  │  /agent_patterns/         (tool-use agents)            │
  │  /structured_extraction/  (JSON/Pydantic output)       │
  │  /streaming/              (token streaming patterns)   │
  │  /function_calling/       (tool definitions + use)     │
  │  /cost_optimisation/      (prompt caching, batching)   │
  │  /fine_tuning/            (training data + JSONL)      │
  │  /evaluation/             (LLM-as-judge, benchmarking) │
  │  /production_patterns/    (retry, rate limits, logging)│
  └─────────────────────────────────────────────────────────┘
```

---

## Features

### RAG Pattern Library
Five RAG variants from simple (naive RAG: embed + retrieve + generate) to advanced (HyDE, parent-child chunking, contextual compression, multi-query retrieval) — each with benchmark comparison on a shared evaluation set.

### Agent Architecture Templates
ReAct agent, Plan-and-Execute agent, and multi-agent pipeline templates — with tool definition schemas, action parsing, observation injection, and error recovery patterns.

### Structured Output Recipes
JSON mode, function calling, and Pydantic model validation patterns for reliable structured data extraction — with validation, retry on schema violation, and partial extraction handling.

### Production-Grade Error Handling
Rate limit retry with exponential backoff and jitter, token count validation before API call, streaming timeout handling, and graceful degradation patterns for API unavailability.

### Cost Tracking and Optimisation
Per-call token cost logging, prompt caching implementation, batch API integration for asynchronous workloads, and prompt compression techniques (AutoCompressor, selective truncation).

### LLM Evaluation Framework
LLM-as-judge evaluation harness with 1–10 rubric scoring, reference-free and reference-based evaluation modes, inter-rater agreement measurement, and batch evaluation pipeline.

### Fine-Tuning Pipeline
End-to-end fine-tuning workflow: training data preparation (JSONL format, data quality checks), job submission, monitoring, evaluation of fine-tuned vs base model, and deployment switching.

### Streaming Implementation Patterns
Server-Sent Events (SSE) streaming for web applications, token-by-token display in Streamlit, streaming with function call detection, and progress indication during long generations.

---

## Tech Stack

| Library / Tool | Role | Why This Choice |
|---|---|---|
| **OpenAI Python SDK** | Core API client | GPT-4o, embeddings, fine-tuning, batch API |
| **FAISS / Chroma** | Vector store (RAG) | Embedding storage and similarity retrieval |
| **Pydantic** | Structured outputs | Schema validation for JSON mode and function calling |
| **LangChain (optional)** | Agent framework | ReAct agent and tool use abstraction |
| **tiktoken** | Token counting | Pre-call token budget validation and cost estimation |
| **python-dotenv** | Config | API key management |
| **pytest** | Testing | Prompt regression tests and output validation |

---

## Getting Started

### Prerequisites

- Python 3.9+ (or Node.js 18+ for TypeScript/JavaScript projects)
- A virtual environment manager (`venv`, `conda`, or equivalent)
- API keys as listed in the Configuration section

### Installation

```bash
git clone https://github.com/Devanik21/openai-cookbook.git
cd openai-cookbook
python -m venv venv && source venv/bin/activate
pip install openai faiss-cpu pydantic tiktoken python-dotenv pytest langchain
echo 'OPENAI_API_KEY=sk-...' > .env
# Run a specific recipe
python rag_patterns/naive_rag.py --docs ./sample_docs/ --query 'What is the refund policy?'
```

---

## Usage

```bash
# Naive RAG
python rag_patterns/naive_rag.py --docs docs/ --query 'Your question here'

# ReAct agent with tools
python agent_patterns/react_agent.py --task 'Find the current price of AAPL and compute its P/E ratio'

# Structured extraction
python structured_extraction/extract_invoice.py --image invoice.pdf

# Fine-tuning pipeline
python fine_tuning/prepare_data.py --raw data.jsonl --output training.jsonl
python fine_tuning/submit_job.py --training training.jsonl --model gpt-4o-mini

# Evaluate with LLM-as-judge
python evaluation/llm_judge.py --predictions outputs.jsonl --rubric rubric.json
```

---

## Configuration

| Variable | Default | Description |
|---|---|---|
| `OPENAI_API_KEY` | `(required)` | OpenAI API key |
| `DEFAULT_MODEL` | `gpt-4o-mini` | Default completion model |
| `EMBEDDING_MODEL` | `text-embedding-3-small` | Embedding model for RAG |
| `MAX_RETRIES` | `3` | Maximum retry attempts on rate limit |
| `BACKOFF_FACTOR` | `2.0` | Exponential backoff multiplier for retries |

> Copy `.env.example` to `.env` and populate required values before running.

---

## Project Structure

```
openai-cookbook/
├── README.md
├── requirements.txt
├── Dockerfile
├── .github/scripts/check_notebooks.py
├── examples/api_request_parallel_processor.py
├── examples/chatgpt/rag-quickstart/azure/function_app.py
├── examples/chatgpt/rag-quickstart/gcp/main.py
├── examples/Assistants_API_overview_python.ipynb
├── examples/Chat_finetuning_data_prep.ipynb
├── examples/Classification_using_embeddings.ipynb
├── .github/authors_schema.json
├── .github/registry_schema.json
├── examples/chatgpt/rag-quickstart/azure/.vscode/extensions.json
└── ...
```

---

## Roadmap

- [ ] Multimodal recipes: vision + text patterns for GPT-4o with image inputs
- [ ] Real-time voice agent: Realtime API integration for low-latency voice conversation
- [ ] Context window management: sliding window, summarisation, and selective retention strategies
- [ ] Observability integration: LangSmith, Weights & Biases, and Datadog tracing setup
- [ ] Cost optimisation benchmark: measure cost vs quality trade-off across model sizes for common tasks

---

## Contributing

Contributions, issues, and suggestions are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-idea`
3. Commit your changes: `git commit -m 'feat: add your idea'`
4. Push to your branch: `git push origin feature/your-idea`
5. Open a Pull Request with a clear description

Please follow conventional commit messages and add documentation for new features.

---

## Notes

All API calls in this cookbook use the official OpenAI Python SDK. API usage incurs costs — review the OpenAI pricing page before running large batches. Fine-tuning and batch API jobs run asynchronously and may take minutes to hours depending on data size and queue length.

---

## Author

**Devanik Debnath**  
B.Tech, Electronics & Communication Engineering  
National Institute of Technology Agartala

[![GitHub](https://img.shields.io/badge/GitHub-Devanik21-black?style=flat-square&logo=github)](https://github.com/Devanik21)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-devanik-blue?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/devanik/)

---

## License

This project is open source and available under the [MIT License](LICENSE).

---

*Built with curiosity, depth, and care — because good projects deserve good documentation.*
