
# Medicare RAG Application: Project Overview & Code Review

## 🔍 Problem Statement
Navigating Medicare is complex. Users struggle with dense documents and unclear eligibility or enrollment details. This application solves that by providing a natural language interface to Medicare knowledge based on official documents.

## 👥 Target Users
- **Medicare Beneficiaries & Caregivers**: Want simple answers about plans, costs, and enrollment.
- **Customer Service Agents**: Need fast, reliable Medicare info during calls.

## 💡 Solution Summary
A Retrieval-Augmented Generation (RAG) app built with Chainlit and LangGraph. It processes user queries, retrieves relevant content from Medicare PDFs, and generates accurate, jargon-free answers using GPT-4o.

## 🧱 Architecture Overview
- **Frontend**: Chainlit (conversational UI)
- **Orchestration**: LangGraph (retrieval + generation flow)
- **Document Ingestion**: PyMuPDFLoader
- **Text Splitting**: RecursiveCharacterTextSplitter with `chunk_size=750`, `overlap=100`, `length_function=tiktoken_len`
- **Embeddings**: Hugging Face (`vivnatan/snowflake-arctic-embed-l-medicare`)
- **Vector Store**: Qdrant (in-memory)
- **LLM**: GPT-4o (via OpenAI API)
- **Agent Layer**: LangGraph agent with conditional tool invocation

## 🧠 Agentic Reasoning
Used to:
- Decide when to invoke the document-based RAG tool
- Fall back when no relevant context is found
- Enable multi-turn interactions and extensibility with new tools (e.g., search APIs)

## 📚 Data Sources
- `10050-medicare-and-you.pdf` from Medicare.gov
- Embeddings via HuggingFace model
- Optionally: TavilySearchResults and ArxivQueryRun for dynamic content

## 📐 Chunking Strategy
- RecursiveCharacterTextSplitter with 750-character chunks and 100-character overlap.
- Token-aware (`tiktoken_len`) for GPT-4 compatibility.
- Chosen for balance between precision and contextual integrity.

## 📊 Evaluation: RAGAS Framework
| Metric               | Fine-tuned Model | Baseline Model | Improvement |
|----------------------|------------------|----------------|-------------|
| Faithfulness         | 0.89             | 0.74           | +0.15       |
| Response Relevance  | 0.86             | 0.71           | +0.15       |
| Context Precision   | 0.77             | 0.61           | +0.16       |
| Context Recall      | 0.84             | 0.69           | +0.15       |

## 🔮 Planned Enhancements
| Area                     | Enhancement                                  | Outcome                                    |
|--------------------------|----------------------------------------------|--------------------------------------------|
| Conversational Memory    | Use LangChain memory                         | Handles follow-up questions                |
| Multi-doc Corpus         | Ingest more PDFs, forms                      | Greater coverage and accuracy              |
| Real-time Tools          | Re-enable Tavily or SerpAPI                  | Live updates from Medicare.gov             |
| Guardrails & Validation  | Add confidence thresholds and source checks  | Prevents hallucination, improves trust     |
| Usage Metrics            | Integrate LangSmith or custom logging        | Visibility into success/failure patterns   |
| Personalization          | Accept ZIP code, age, etc.                   | Customized answers (e.g., regional plans)  |

---

This Markdown summary documents the code, architecture, purpose, and improvement plan for the Medicare RAG application. Ideal for inclusion in a GitHub repo or Hugging Face Space.
