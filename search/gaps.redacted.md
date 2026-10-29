# search/gaps.md — Delta: Attested Record vs. Target Role Requirements

Public GitHub submission copy — project/employer names genericized to
Project A / Project B / "prior internship" to match
search/resume.redacted.json. Full-detail version stays local in
search/gaps.md (gitignored).

Agent-drafted from real postings/JD-template evidence; student killed one
false-positive row and rewrote the Python row in their own words (2026-10-29).

## How to read this table
- **Evidence column**: cites a posting, O*NET requirement, or pattern across ≥3 postings — not a feeling.
- **Plan column**: describes an *output* a stranger can verify, not an activity ("take a course" is not a plan).

---

| Gap | Evidence the target demands it | What I have | Plan to close it |
|-----|-------------------------------|-------------|-----------------|
| Named production Python experience *(rewritten by student, see below)* | 2026 AI Engineer JD template requires "3+ years professional Python development... async patterns, type hints, packaging for production" ([korebpo.com](https://korebpo.com/ai-engineers-job-description-template/)); same pattern in a bank new-grad posting and a telecom posting. **Personal framing:** my target is AI-application dev — model APIs, RAG, eval — which lives in the Python ecosystem; Java solves "how does the service stay up," Python is "how is the AI layer built." Also not optional: this term's coursework project is in Python. | Scripts yes, full Java-grade engineering no: never managed dependencies/packaging properly, no real test suite, code runs for me but isn't "clone and run." No async, no type hints. A full tier below my Java. | Not a separate toy project — using this term's coursework project as the vehicle, built properly end-to-end: directory structure, `pyproject.toml`, module separation, pytest suite, README, pushed to GitHub so a stranger can run it in one command. Scheduled a fixed few evenings a week for it, not tutorial-watching. If time remains after: port one core piece of a Java project (Project A's code-search feature) to Python and write up where the two implementations actually differ. |
| Cloud platform deployment experience (AWS/GCP/Azure, managed, not local) | korebpo.com template names "AWS Bedrock, GCP Vertex AI, or Azure AI Foundry" as the deployment environment; Capital One senior AI-platform posting: "building agentic LLM systems on AWS/Kubernetes"; an OpenAI backend posting: "production backend services, APIs, databases and cloud/deployment experience" | Project B ships with Docker Compose only (local/self-hosted); no cloud provider named anywhere on the resume | Deploy Project B to a managed Kubernetes service (EKS or GKE) with Terraform-defined infra instead of local docker-compose; document the architecture and any autoscaling/observability setup. Closing condition: public repo shows Terraform/K8s manifests plus a reachable deployed URL or a documented demo, not just a local compose file. |
| Named vector-database product (Pinecone / Weaviate / Qdrant / Chroma / pgvector) | korebpo.com template: "practical experience with at least one vector store (Pinecone, Weaviate, Qdrant, Chroma, or pgvector) in a production context" | Two custom vector-storage builds exist (search-index-based embedding search at a prior internship; local-database-based vector storage in Project A's code-search feature) but neither is one of the named mainstream vector-DB products | Add a pgvector (or Qdrant) backend option to Project A's code-search feature, benchmark retrieval quality/latency against the current backend. Closing condition: repo shows a working pgvector/Qdrant integration plus a benchmark table in the README. |

## Python gap — in my own words (unedited)

> **为什么最要紧:** 我的目标是 AI 应用开发岗。这类岗位调模型 API、做 RAG、搭评测,基本都在 Python 生态里。Java 后端我有底子,但它解决的是"服务怎么稳",Python 补的是"AI 这一层怎么搭"。而且这学期的课程项目就是用 Python 做,这是眼前躲不掉的事。
>
> **现在什么水平:** 能写脚本、能看懂别人的代码,本科做机器学习项目、实习做推荐算法时都用过。但我没用 Python 做过一个完整的工程:没认真管过依赖和打包,没写过像样的测试,代码是"我自己能跑",不是"别人 clone 下来就能用"。异步、类型标注这些也不熟。跟我的 Java 比,差一个档次。
>
> **真打算怎么补:** 不另开一个练习项目,就拿这学期的课程项目当载体。用 Python 从头做完整,包括目录结构、pyproject.toml、拆模块、pytest 测试、README,最后推到 GitHub,让别人一条命令就能跑起来。每周固定留几个晚上写它,不看教程刷题。做完之后,如果还有精力,再把 Java 项目里一小块核心逻辑用 Python 重写一遍,对比两边的写法差在哪。

## Killed rows
- **Multi-agent / agentic workflow design experience** — killed 2026-10-29. Reason: this isn't actually a gap — Project A's multi-agent architecture (dependency-aware scheduling, bounded auto-retry) and Project B's workflow engine (topological sort, cycle detection) already demonstrate it directly; the agent flagged it from the posting-template keyword "agentic workflow development" without checking it against what's already in resume.json.

## Closed gaps (move entries here when resume.json is updated with closing evidence)
