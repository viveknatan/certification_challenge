
# Medicare RAG Application: Project Overview & Code Review

## 🔍 Problem Statement
Navigating Medicare is complex. Users struggle with dense documents and unclear eligibility or enrollment details. This application solves that by providing a natural language interface to Medicare knowledge based on official documents.

Seniors living in the USA are covered by Medicare. The application process starts around the time one turns 65 and there are several steps. Seniors and their trusted advisors (i.e.children, etc) spend a lot of time understanding the landscape of Medicare and will need to spend time evaluating if the enrollee needs additional coverage through Medicare Advantage by looking at their current medical needs. There is a lot of searches, phone calls and discussions with insurance agents to identify what is the best product to enrol under. This app in its final form is designed to act as the one trusted advisor who can walk a person through their concerns and questions before enrolling in Medicare.

## 👥 Target Users
- **Medicare Beneficiaries & Caregivers**: Want simple answers about plans, costs, and enrollment.
- **Customer Service Agents**: Need fast, reliable Medicare info during calls.

## 💡 Solution Summary
A Retrieval-Augmented Generation (RAG) app built with Chainlit and LangGraph. It processes user queries, retrieves relevant content from Medicare PDFs, and generates accurate, jargon-free answers using GPT-4o.

## 🧱 Architecture Overview
- **LLM**: GPT-4o (via OpenAI API) - great general purpose model that is being used for understanding the question, retrieving the data and summarizing it.
- **Embeddings**: Hugging Face (`vivnatan/snowflake-arctic-embed-l-medicare`) - Medicare data fine tuned version of Snowflake Arctic Embed
- **Orchestration**: LangGraph (retrieval + generation flow)
- **Vector Database**: Qdrant (in-memory)
- **Monitoring**: Langsmith
- **Evaluation**: RAGAS
- **Frontend**: Chainlit (conversational UI)
- **Document Ingestion**: PyMuPDFLoader
- **Text Splitting**: RecursiveCharacterTextSplitter with `chunk_size=750`, `overlap=100`, `length_function=tiktoken_len`
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
| Guardrails & Validation  | Add confidence thresholds and source checks  | Prevents hallucination, improves trust     |
| Personalization          | Accept ZIP code, age, etc.                   | Customized answers (e.g., regional plans)  |
