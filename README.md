# Atlas ⚡

> **An open-source distributed runtime for machine learning and data engineering.**

![C++](https://img.shields.io/badge/C++-20-blue.svg)
![Python](https://img.shields.io/badge/Python-3.10+-yellow.svg)
![Architecture](https://img.shields.io/badge/Architecture-Distributed_Systems-success.svg)
![Status](https://img.shields.io/badge/Status-Active_Development-orange.svg)

## 📌 What is Atlas?

**Atlas** simplifies distributed training, ETL pipelines, and large-scale compute by providing scheduling, execution, fault tolerance, and observability in a single modular platform. 

Today, scaling a machine learning pipeline or big data workload requires stitching together a brittle stack of disparate frameworks (Ray, Celery, Kubernetes, Redis, Spark). Atlas solves this by providing a unified, high-performance execution engine written entirely in modern C++20, controlled via a seamless and intuitive Python SDK.

**The philosophy is simple:** Performance is won in C++ memory layouts and cache locality. Developer adoption is won through a clean Python API.

```bash
# How Atlas is designed to work:
pip install atlas-runtime

atlas run train.py --cluster=local

```

## 🏗️ System Architecture

Atlas draws heavy architectural inspiration from systems like PyTorch, Apache Arrow, and Databricks Photon. The codebase maintains a strict boundary: **Python handles the API layer; C++ owns all execution, memory, and networking.**

```text
               [ CLI / Python SDK ] 
            (atlas run, atlas metrics)
                      │
                      ▼
 ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
                      ▼
             [ C++ Scheduler & DAG ]
           (Dependency Parsing, Retries)
                      │
 ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
                      ▼
            [ C++ Execution Engine ]
                      │
      ┌───────────────┼───────────────┬───────────────┐
      ▼               ▼               ▼               ▼
[ Thread Pool ]   [ Memory ]     [ Network ]     [ Storage ]
 (Work Stealing)   (Arena/Slab)    (gRPC/TCP)     (Dataset API)
                      │
      ┌───────────────┴───────────────┐
      ▼                               ▼
[ PyTorch Plugin ]              [ Custom C++ Tasks ]

```

## ⚙️ Core Engineering Primitives

Atlas strictly avoids "magic" code. Core infrastructure components are built from first principles to explore the depths of performance engineering, leveraging the C++ standard library only where it provides robust abstractions.

* **Concurrency:** Custom work-stealing thread pool supporting task dependency graphs and asynchronous futures.
* **Memory Management:** Custom allocators (Arena, Slab, Object Pools) designed to bypass standard `malloc()` overhead during high-throughput data processing.
* **Execution Engine:** Dependency-aware DAG executor handling complex dataflow topologies.
* **Networking & RPC:** High-throughput TCP/gRPC layer utilizing binary serialization for zero-copy data transfers between nodes.
* **Distributed Consensus:** Heartbeat monitoring, node discovery, task queues, and automated failure recovery.

## 🗄️ Project Structure

This project is maintained with the engineering rigor of a production infrastructure product.

```bash
[github.com/TechieSamosa/atlas](https://github.com/TechieSamosa/atlas)
├── runtime/                 # Modern C++ execution engine (allocators, thread pools)
├── scheduler/               # Distributed scheduling logic and DAG parsing
├── networking/              # RPC, transport layer, and binary serialization
├── storage/                 # Distributed dataset abstractions
├── python-sdk/              # User-facing Python API and SDK logic
├── cli/                     # Command Line Interface (atlas run, cluster up)
├── benchmarks/              # Microbenchmarks (latency, throughput, cache misses)
├── docs/
│   ├── ADRs/                # Architecture Decision Records (e.g., ADR-0001-Why-CPP20.md)
│   ├── HLD/                 # High-Level Design specs
│   └── LLD/                 # Low-Level Design specs (e.g., lock-free queues)
├── research/                # Engineering papers, profiling reports, and design notes
└── papers-we-implemented/   # Reference implementations of relevant CS research

```

## 🗺️ Milestone Roadmap (v1.0 target)

Atlas is currently under active development. The goal is not to build a bloated orchestration platform, but to deliver a focused, highly performant `v1.0` runtime.

**Phase 1: The Engine Primitive**

* [ ] Implement C++20 foundation with CMake build system.
* [ ] Build custom work-stealing thread pool and synchronization primitives.
* [ ] Develop arena/slab allocators for task execution.

**Phase 2: The Local Runtime**

* [ ] Build the local DAG Executor for parsing task dependencies.
* [ ] Implement asynchronous futures and task state management.
* [ ] Develop the initial Python SDK bindings (via Pybind11).

**Phase 3: The Cluster Network**

* [ ] Implement TCP/gRPC worker nodes and binary serialization.
* [ ] Establish heartbeat monitoring, node discovery, and automatic fault recovery.
* [ ] Finalize the `atlas` CLI for cluster deployment and observability.

## 🧪 Code Quality & Engineering Standards

Atlas enforces three strict engineering rules before any code is merged:

1. **No Magic:** Every critical subsystem is documented deeply.
2. **Artifact-First:** Every PR must include the implementation, unit tests, and an updated Architecture Decision Record (ADR) justifying design tradeoffs.
3. **Continuous Benchmarking:** All custom abstractions (e.g., arena allocator) are benchmarked against standard library equivalents using rigorous telemetry [1].

## 👨‍💻 Author

**Aditya Khamitkar**
[GitHub Profile](https://github.com/TechieSamosa) | [Portfolio](https://adityakhamitkar.vercel.app)
