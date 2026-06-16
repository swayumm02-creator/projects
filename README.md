# Aegis Health - Advanced Clinical Intelligence Matrix

Aegis Health is an enterprise-grade AI-powered healthcare platform engineered to track patient health trajectories, analyze complex symptoms, and project systemic medical risk profiles. By combining an asynchronous, high-performance API gateway with Deep Learning, Natural Language Processing, and Explainable AI (XAI) frameworks, the platform turns unstructured clinical interactions and complex biometric streams into actionable, transparent diagnostic insights.

---

## 🏗️ System Architecture

The application relies on a decoupled, containerized network structure to isolate client requests, backend orchestration logic, and dedicated computational nodes.

```mermaid
graph TD
    classDef client fill:#3498db,stroke:#2980b9,color:#fff,stroke-width:2px;
    classDef server fill:#2ecc71,stroke:#27ae60,color:#fff,stroke-width:2px;
    classDef node fill:#eaedf1,stroke:#cbd5e1,color:#2c3e50,stroke-width:1px;

    subgraph Client_Side [Client-Side Browser]
        ReactUI["React UI Dashboard <br> (http://localhost:3000)"]:::client
    end

    subgraph Server_Side [Server-Side Docker Node]
        FastAPI["FastAPI Engine (Uvicorn) <br> ([http://127.0.0.1:8000](http://127.0.0.1:8000))"]:::server
        Router1["APIRouter <br> (Metrics)"]:::node
        Router2["APIRouter <br> (Tracker)"]:::node
    end

    subgraph Operations [Target Engine Nodes]
        ML["ML Pipeline Node <br> (Inference/Simulation)"]:::node
        DB["Data Storage Node <br> (Observation Logging)"]:::node
    end

    ReactUI -->|Axios HTTP POST /predict/timeseries| FastAPI
    ReactUI -->|Axios HTTP POST /symptoms/log| FastAPI
    FastAPI --> Router1
    FastAPI --> Router2
    Router1 --> ML
    Router2 --> DB
