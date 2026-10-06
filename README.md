<div align="center">

# 🛡️ Aegis-Vector AI — Enterprise Hybrid Agent Platform
### In-Database Relational-Vector Joins (Oracle 23ai), Autonomous Commerce & Context-Aware Guardrails

[![Python Version](https://img.shields.io/badge/Python-3.11%2B-blue?logo=python&logoColor=white)](https://python.org)
[![Oracle Database 23ai](https://img.shields.io/badge/Database-Oracle_23ai_AI_Vector_Search-F80000?logo=oracle&logoColor=white)](https://www.oracle.com/database/23ai/)
[![Multi-Agent SDK](https://img.shields.io/badge/Orchestration-OpenAI_Agents_SDK_%7C_LangGraph-412991)](#)
[![Security Guardrails](https://img.shields.io/badge/Guardrails-Context--Aware_Input_Filtering-00C7B7)](#)
[![Autonomous Action](https://img.shields.io/badge/Action-Oracle_DB_Insert_%7C_PDF_Invoice-22C55E)](#)
[![Dual Interface](https://img.shields.io/badge/UI-Gradio_%7C_Streamlit-FF4B4B)](#)

<p align="center">
  <a href="#-executive-summary">Executive Summary</a> •
  <a href="#-hybrid-relational-vector-sql-architecture">In-Database Hybrid SQL</a> •
  <a href="#-agentic-orchestration--autonomous-action">Agentic Action Engine</a> •
  <a href="#-context-aware-security-guardrails">Input Guardrails</a>
</p>

</div>

---

## 📌 Executive Summary

Enterprise AI deployments commonly hit two fundamental architecture barriers:
1. **The Retrieval-Latency Bottleneck:** Developers query vector stores separately, serialize dense embeddings over the network, and reconcile tabular relational constraints (e.g., pricing, inventory, dosage thresholds) in application code.
2. **Passive Chatbot Syndrome:** Agents offer conversational advice but lack secure execution privileges to lock database transactions and issue legally valid financial artifacts.

**Aegis-Vector AI** bridges transactional enterprise operations and generative reasoning. Powered by **Oracle Database 23ai AI Vector Search**, the platform executes **hybrid relational-vector SQL joins** directly inside the database kernel—evaluating relational thresholds against unstructured regulatory directives (e.g., EU Chemical & Dietary Regulations) in a single unified SQL query.

Furthermore, the platform deploys an **Autonomous Action Engine**: agents execute semantic inventory lookups, maintain context-aware security guardrails, record transactional orders in Oracle tables, and synthesize branded PDF tax invoices on the fly.

---

## 🏛️ In-Database Hybrid Relational-Vector SQL

The hallmark capability of Aegis-Vector AI is shifting analytical heavy-lifting from Python application memory into **Oracle Database 23ai's native vector computation engine**:

```mermaid
graph TD
    UserQuery([Client Inquiry / Regulatory Audit Request]) --> Gateway{Security Guardrail<br/>Context-Aware Input Verifier}
    
    Gateway -->|Approved| AgentRouter[Enterprise Agent Dispatcher<br/>Catalog / Compliance / Triage]
    
    subgraph OracleKernel ["⚡ Oracle Database 23ai Kernel Layer (Zero Python Logic)"]
        AgentRouter --> SQLQuery["Unified Hybrid SQL Execution<br/>Relational Join + VECTOR_DISTANCE()"]
        
        RelationalData[(Relational Schemas<br/>product_recipes / internal_formulas)] --> SQLQuery
        VectorEmbeddings[(Vector Tablespace<br/>compliance_rules / regulatory_chunks)] --> SQLQuery
        
        SQLQuery --> HardEvaluation{"In-Database Evaluation<br/>dosage_mg > max_allowed_mg"}
    end
    
    HardEvaluation -->|Threshold Breach| AuditResult[Deterministic Compliance Report<br/>Statutory Clause Linked via COSINE]
    HardEvaluation -->|Catalog Match| ActionTrigger[Autonomous Order & Invoice Tool<br/>finalize_purchase]
    
    ActionTrigger --> DBCommit[(Oracle 23ai orders Commit)]
    ActionTrigger --> PDFEngine[ReportLab Invoice Compiler<br/>PDF Generated in /Rechnungen]

### The Hybrid SQL Breakthrough
Rather than pulling thousands of vector chunks into application memory for manual cross-referencing, the system issues a single native Oracle 23ai SQL query executing cross-joins, relational filtering, and vector distance ranking simultaneously:

```sql
WITH matched_violations AS (
    SELECT 
        f.product_name,
        f.ingredient,
        f.dosage_mg,
        l.max_allowed_mg,
        c.doc_name,
        c.page_num,
        c.paragraph,
        ROW_NUMBER() OVER (
            PARTITION BY f.product_name, f.ingredient 
            ORDER BY VECTOR_DISTANCE(c.embedding, TO_VECTOR(:1), COSINE)
        ) as rn
    FROM internal_formulas f
    JOIN substance_limits l ON f.ingredient = l.substance_name
    CROSS JOIN compliance_rules c
    WHERE f.dosage_mg > l.max_allowed_mg
      AND LOWER(c.paragraph) LIKE '%' || LOWER(f.ingredient) || '%'
)
SELECT product_name, ingredient, dosage_mg, max_allowed_mg, doc_name, page_num, paragraph
FROM matched_violations
WHERE rn = 1;
```
* **Performance Gain:** Sub-20ms evaluation across complex regulatory corpuses.
* **Guaranteed Determinism:** Mathematical relational constraints (`>` or `<`) cannot be hallucinated by an LLM.

---

## ⚡ Agentic Orchestration & Autonomous Action

The multi-agent network operates via specialized functional tools, taking requests from initial discovery to finalized business transactions:

### 1. The Autonomous Commerce Engine (`finalize_purchase`)
When interacting with customer requests, the `Product_Catalog_Agent` does not merely suggest links:
* **Semantic Vector Discovery:** Executes `search_product_catalog` against Oracle tables using local `all-MiniLM-L6-v2` dense embeddings (384 dimensions) combined with strict maximum price thresholds (`WHERE price <= :max_price`).
* **Autonomous Order Placement:** Automatically invokes `execute_order_placement`, reserving inventory and committing an immutable order record to Oracle (`orders` table).
* **Automated Fiscal Invoice Synthesis:** Invokes `create_pdf_invoice`, compiling a branded A4 PDF tax invoice with net subtotal and VAT calculations (19% German USt) via ReportLab.

### 2. Multi-Agent Triage & Handoff Pipeline
```text
[User Request] 
      │
      ▼
Customer_Support_Triage (Frontline Gateway)
      │
      ├─► Order_Status_Agent   (Tool: lookup_order against ORDERS_DB)
      ├─► Refund_Agent         (Tool: process_refund with status rules)
      └─► Product_Catalog_Agent(Tools: search_product_catalog + finalize_purchase)
```

---

## 🛡️ Context-Aware Security Guardrails

Conventional input guardrails suffer from a severe flaw: they evaluate short user inputs in isolation and frequently reject benign follow-ups (e.g., classifying a simple *"yes"*, *"continue"*, or *"buy"* as ambiguous or off-topic).

Aegis-Vector AI implements **State-Tracking Dynamic Guardrails** (`is_benign_follow_up`):
1. **Adversarial Pattern Scanning:** Regular expression evaluation detecting prompt injection tokens (`hack`, `exploit`, `bypass`, `threat`, `fraud`).
2. **Context Horizon Inspection:** Dynamically inspects the preceding interaction turns (`chat_history[-3:]`).
3. **Conversational Continuation Exemption:** If prior context indicates an active product search or support negotiation, affirmative responses (`"yes"`, `"kaufen"`, `"weiter"`, `"ich nehme"`) pass the guardrail with zero latency overhead.

---

## 🛠️ Enterprise Tech Stack Matrix

| Domain | Technology | Implementation Details & Architectural Purpose |
| :--- | :--- | :--- |
| **Enterprise Database** | **Oracle Database 23ai** | Native `VECTOR(384, FLOAT32)` storage in `USERS` tablespace with in-database cosine joins |
| **Database Connectivity**| **python-oracledb (Thin)** | Zero-overhead Thin-Mode client connecting directly without Oracle Instant Client binaries |
| **Multi-Agent Runtime** | **OpenAI Agents SDK & LangGraph** | Declarative stateful agents, handoffs, and dual-tier reflection workflows |
| **Dense Embeddings** | **all-MiniLM-L6-v2 & OpenAI** | Local 384-dimensional dense encoding (`sentence-transformers`) and cloud 1536d fallback |
| **LLM Inference** | **GPT-4o-mini & Ollama** | Hybrid inference: local `qwen2.5-coder:14b` pre-auditing paired with cloud decision agents |
| **Document Processing** | **PyPDF & ReportLab** | High-throughput extraction of 850+ page regulatory PDFs and dynamic fiscal invoice generation |
| **User Interfaces** | **Gradio & Streamlit** | Multi-channel presentation: responsive Gradio chat interfaces and Streamlit telemetry cockpits |
| **API Gateway** | **FastAPI** | REST microservice dispatching semantic catalog lookups via structured function calling |

---

## 📂 Repository Topology

```text
aegis-vector-ai/
├── tools/                                # Autonomous Action Tools
│   ├── db_writer.py                      # Immutable transactional order booking in Oracle 23ai
│   └── invoice_generator.py              # Automated A4 PDF tax invoice compiler (ReportLab)
│
├── Product_Catalog_Agent.py              # Autonomous sales & inventory orchestrator with action tools
├── 08_customer_support.py                # Multi-agent support triage system with guardrail verification
├── compliance_auditor.py                 # Pure in-database relational-vector SQL join engine
├── setup_compliance_db.py                # Oracle schema provisioning (internal_formulas & compliance_rules)
├── ingest_lexicon.py                     # High-volume PDF ingestion engine (850+ page medical corpuses)
├── vector_final.py                       # Products table setup with native VECTOR(384) columns
├── product_catalog_server.py             # FastAPI REST gateway for semantic catalog queries
├── interface.py                          # Production Gradio ChatInterface with security interceptors
├── interface2.py                         # Streamlit corporate on-premise search UI (sub-20ms lookup)
├── app.py                                # Streamlit compliance auditing dashboard with LangGraph
├── db.py                                 # Enterprise 1536-dimensional vector schema and SQL join runner
├── test_db.py                            # Oracle 23ai Thin-mode connectivity test script
└── db_data_delete.py                     # Database maintenance and table cleanup utilities
```

---

## ⚡ Quickstart & Execution

### Prerequisites
* **Python 3.11+**
* **Oracle Database 23ai** running locally (Docker container or OCI Free Tier on `localhost:1521/FREEPDB1`)
* OpenAI API Key *(or a local Ollama instance)*

---

### 1. Installation

```bash
# Clone repository
git clone https://github.com/your-username/aegis-vector-ai.git
cd aegis-vector-ai

# Initialize virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install oracledb sentence-transformers pypdf reportlab gradio streamlit fastapi uvicorn openai langchain-openai langgraph python-dotenv
```

---

### 2. Provision Oracle 23ai Database Schemas

Initialize tables with native vector columns and relational schemas:

```bash
# Set up internal formulas and regulatory vector tables
python setup_compliance_db.py

# Set up product catalog table with native VECTOR(384, FLOAT32) column
python vector_final.py
```

---

### 3. Ingest High-Volume Regulatory Corpuses (Optional)

The ingestion pipeline (`ingest_lexicon.py`) processes dense, large-scale regulatory documentation (e.g., 850+ page medical/chemical lexicons) into vectorized database rows:

```bash
python ingest_lexicon.py
```
> Extracts text page-by-page, generates 384-dimensional dense vectors, and executes batch inserts with progress reporting.

---

### 4. Launch Interfaces & Simulation Modes

#### Mode 1: Interactive Multi-Agent Web Gateway (Gradio)
Launches the conversational portal with built-in guardrails and example customer queries:
```bash
python interface.py
```
> Access interface at `http://localhost:7860`.

#### Mode 2: Autonomous Commerce Simulation (CLI)
Executes conversational discovery, budget filtering, order confirmation, and automated invoice PDF generation:
```bash
python Product_Catalog_Agent.py
```

#### Mode 3: Pure In-Database SQL Audit
Runs the relational-vector hybrid SQL join directly inside the Oracle 23ai kernel:
```bash
python compliance_auditor.py
```

#### Mode 4: Enterprise Compliance Dashboard (Streamlit)
Launches the interactive audit cockpit with side-by-side regulatory comparisons:
```bash
streamlit run app.py
```

---

## 📈 Scalability Benchmark: 850+ Page Ingestion

The ingestion architecture (`ingest_lexicon.py`) demonstrates high-throughput document processing:
* **Batch Processing:** Seamlessly digests dense 850+ page regulatory PDFs (`medizin_lexikon_icd10.pdf`).
* **Chunk Normalization:** Truncates and sanitizes text blocks to respect Oracle `VARCHAR2(4000)` column boundaries.
* **Vector Indexing:** Commits progress checkpoints every 50 pages, ensuring atomic commits and predictable memory profiles.

---

## 👨‍💻 Engineering & Systems Architecture

Architected by **Elvijs Landmans** ([landmansIT](https://landmansit.de)).

* **In-Database Intelligence:** Eliminating external vector databases by computing high-dimensional cosine similarity directly within relational tables (**Oracle 23ai**).
* **Actionable Agent Swarms:** Empowering generative agents to execute business transactions (reserving inventory, generating fiscal invoices) under strict schema guardrails.
* **Contextual Security Defense:** Pioneering state-aware input guardrails that prevent adversarial prompt injection while preserving natural, human-like dialogue flow.

---

## 📄 License

Proprietary Software. All Rights Reserved. Developed for enterprise commerce, automated regulatory compliance, and industrial supply chain operations.
