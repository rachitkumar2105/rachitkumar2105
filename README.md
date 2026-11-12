<div align="center">

# Rachit Kumar Singh

<img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=500&size=18&duration=3500&pause=1200&color=7AA2F7&center=true&vCenter=true&width=620&lines=Distributed+systems+in+Java+21;Vector+search+from+first+principles;Backend+APIs+with+FastAPI+%2B+PostgreSQL;Measured+under+failure%2C+reported+honestly" alt="Distributed systems in Java 21, vector search from first principles, backend APIs" />

**B.Tech CSE '27 @ KIIT (8.31 CGPA) • Ex-SWE Intern @ Accenture • Distributed Systems, Backend & AI Engineering • Seeking Jan–Jun 2027 SDE Internships**

[![Portfolio](https://img.shields.io/badge/Portfolio-rachitkumarsingh.online-0A0A0A?style=for-the-badge&logo=googlechrome&logoColor=white)](https://rachitkumarsingh.online)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rachit%20Kumar%20Singh-0A66C2?style=for-the-badge)](https://linkedin.com/in/rachit-kumar-singh-79429827b)
[![Email](https://img.shields.io/badge/Email-rachitkumar2105%40gmail.com-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:rachitkumar2105@gmail.com)

</div>

---

I build backend and distributed systems from first principles, then prove them by breaking them: killing processes, partitioning networks, and publishing only the numbers I measured.
My two flagship builds are a Kafka-style commit log in Java 21 and a vector database in Python, each benchmarked against the real thing (Apache Kafka, FAISS) with the gaps stated honestly.
I care about correctness under failure, test coverage that means something, and systems I can defend line by line in an interview.

**Accenture, Associate Software Engineer Intern (May–Jul 2026).** Built a FastAPI RAG chatbot for L1 ticket triage over a knowledge base of resolved tickets, with confidence-scored root-cause analysis; defined 13 evaluation criteria and 15 process rules in JIRA across 4 teams. Selected among the top 5 interns to present before a visiting Japanese delegation at an Agentic AI conference.

## Flagship Systems & Verified Benchmarks

| Project | What it is | Verified results | Stack | Links |
|:--|:--|:--|:--|:--|
| **LogForge** | Kafka-style distributed commit log, ~11K lines of Java 21 on the JDK alone | **0** acknowledged records lost across 10 SIGKILL leader failovers (1.48M records acked at `acks=all`)<br>Fixed a blocking socket close (~7 s heartbeat stall): false failovers **22/22 → 0/22**<br>5 Docker Compose chaos scenarios + 100 crash-recovery kills<br>126 JUnit 5 tests in GitHub Actions CI | Java 21, Java NIO, CRC-32C, leader-follower replication, ISR, Docker, Next.js | [![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/rachitkumar2105/logforge) [![Live](https://img.shields.io/badge/Live%20Demo-000000?style=flat-square&logo=vercel&logoColor=white)](https://logforge-five.vercel.app) |
| **VectorForge** | Vector database from first principles, no ANN library in the engine | HNSW: **976 queries/s at 0.954 recall@10** on 200k SIFT1M vectors (**42.8x** faster than exact search)<br>int8 scalar quantization: **4x** storage cut at 0.9766 recall@10<br>100/100 crash-recovery kills survived via CRC32 WAL<br>Benchmarked against FAISS: 9.5x gap measured and explained<br>206 engine tests (95.36% coverage) + 30 frontend tests | Python, NumPy, FastAPI, Next.js, React, TypeScript | [![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/rachitkumar2105/vectorforge) [![Live](https://img.shields.io/badge/Live%20Demo-000000?style=flat-square&logo=vercel&logoColor=white)](https://vectorforge-olive.vercel.app) |
| **APIWatch** | Production-style task-management REST API with observability | Eliminated **2 race conditions** (duplicate idempotent writes, lost updates) via Idempotency-Key checks and optimistic concurrency<br>**51 tests at 98.34% coverage** behind an 85% CI gate<br>Prometheus + Grafana dashboard, liveness/readiness probes | Python, FastAPI, SQLAlchemy, PostgreSQL 16, SQLite, Prometheus, Grafana | [![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/rachitkumar2105/APIWatch) |
| **AI Resume Screening Assistant** | Full-stack resume classifier and JD matcher | 2-layer PyTorch NN over 2,362 real resumes (1,890/472 split)<br>Test accuracy **62.9% → 67.4%** (macro F1 66.7%)<br>Confidence-calibrated "UNCERTAIN" abstention on ambiguous input | Python, PyTorch, Scikit-learn, spaCy NER, TF-IDF, FastAPI, React | [![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/rachitkumar2105/AI-Resume-Screening-Assistant) |

### How LogForge acknowledges a write (`acks=all`)

```mermaid
flowchart LR
    P[Producer] -->|ProduceRequest| L[Leader broker<br/>append batch + CRC-32C]
    F1[Follower 1<br/>append fetched batch] -->|FetchRequest| L
    F2[Follower 2<br/>append fetched batch] -->|FetchRequest| L
    L -->|all ISR caught up:<br/>advance high watermark| A[Ack to producer]
    C[Controller<br/>heartbeats + fencing] -.->|leader fails:<br/>elect from ISR| F1
```

<details>
<summary><b>Engineering notes: what these systems deliberately do not do (yet)</b></summary>

<br>

- **LogForge:** single fixed controller (no Raft), no TLS on the binary protocol, no zero-copy `sendfile`, compression or transactions. Benchmarks vs Apache Kafka 3.9: faster in 2 configurations, slower in 2, the rest within run-to-run noise.
- **VectorForge:** 9.5x slower than FAISS at matched recall on SIFT1M (Python loops vs optimised C++). Crash tests use process kills, not power loss.
- **APIWatch:** single instance; load test was sequential, not concurrent.

Knowing where a system stops is part of building it.

</details>

## Tech Stack

**Languages**

![Java 21](https://img.shields.io/badge/Java%2021-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-336791?style=for-the-badge)

**Backend & Systems**

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Java NIO](https://img.shields.io/badge/Java%20NIO-5382A1?style=for-the-badge&logo=openjdk&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white)
![REST APIs](https://img.shields.io/badge/REST%20APIs-6BA539?style=for-the-badge&logo=openapiinitiative&logoColor=white)

**Frontend**

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)

**DevOps & Observability**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)
![Microsoft Azure](https://img.shields.io/badge/Microsoft%20Azure-0078D4?style=for-the-badge)

**AI / ML**

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![FAISS](https://img.shields.io/badge/FAISS-0467DF?style=for-the-badge)
![spaCy](https://img.shields.io/badge/spaCy-09A3D5?style=for-the-badge&logo=spacy&logoColor=white)

## Where My Commits Go

<p align="center">
  <img height="180" src="https://github-profile-summary-cards.vercel.app/api/cards/most-commit-language?username=rachitkumar2105&theme=tokyonight" alt="Top languages by commit" />
</p>

## Problem Solving

![LeetCode](https://img.shields.io/badge/LeetCode-300%2B%20DSA%20%26%20SQL%20problems-FFA116?style=for-the-badge&logo=leetcode&logoColor=black)
![IMO](https://img.shields.io/badge/SOF%20IMO-5x%20Medalist%20%283%20Gold%2C%202%20Silver%29-B8860B?style=for-the-badge)
