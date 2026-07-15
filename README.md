# Atlas ⚡

> **A distributed compute runtime for machine learning and data processing.**

![C++](https://img.shields.io/badge/C++-20-blue.svg)
![Python](https://img.shields.io/badge/Python-3.10+-yellow.svg)
![Architecture](https://img.shields.io/badge/Architecture-Distributed_Systems-success.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen)

## 📌 The Vision

**Atlas** is a distributed execution runtime built from scratch in modern C++ with a Python control plane. It executes machine learning, data engineering, and general compute workloads across clusters while exposing the core primitives behind production systems like Ray, Spark, Borg, Photon, and Kubernetes—not by wrapping them, but by implementing their underlying mechanisms.

Atlas is designed as a foundational infrastructure project, prioritizing low-level performance, custom memory management, concurrent execution, and distributed fault tolerance.

## 🏗️ System Architecture

Atlas flips the traditional Python-heavy ML stack. Python serves strictly as the control plane and SDK glue, while the high-performance C++ runtime owns all execution, memory, networking, and scheduling.

```text
              [ CLI ] (atlas submit, atlas status, atlas logs)
                 │
                 ▼
       [ Job Submission API ]
                 │
                 ▼
      [ Scheduler Service ] (Python / Control Plane)
                 │
 ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
                 │
        [ Worker Discovery ]
        [ Leader Election ]
        [ Task Queue & DAG Engine ]
        [ Resource Manager ]
                 │
 ┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
                 ▼
       [ C++ Runtime Engine ] (The Core)
                 │
      ┌──────────┼──────────┬──────────┬──────────┐
      ▼          ▼          ▼          ▼          ▼
[ Thread Pool] [Memory] [Executor] [Network]  [Storage]
 (Stealing)    (Arena)    (DAG)     (gRPC)    (AtlasFS)
                 │
      ┌──────────┴──────────┐
      ▼                     ▼
[ Python Plugin ]     [ CUDA Plugin ]

```

## ⚙️ Core Primitives (Implemented from Scratch)

Atlas avoids standard library crutches in favor of custom-built, highly optimized systems:

* **Concurrency:** Custom work-stealing thread pool with priority queues, futures, and task dependency graphs.
* **Memory Management:** Custom allocators (Arena, Slab, Object Pools) to eliminate `malloc` bottlenecks during high-throughput data processing.
* **Execution Engine:** Dependency-aware DAG execution engine supporting complex task topologies (A → B → C ↘ D).
* **Networking & RPC:** Custom TCP/gRPC layer for binary serialization, message framing, and zero-copy transfers.
* **Distributed Consensus:** Worker registration, heartbeat monitoring, leader election, and automatic failure detection/rescheduling.
* **Storage (AtlasFS):** A lightweight distributed object store abstraction supporting chunking, checksums, and dataset primitives (Parquet, Arrow, S3/MinIO).

## 🗄️ Project Structure

The codebase is organized with rigorous separation of concerns, mirroring production infrastructure standards:

```bash
[github.com/TechieSamosa/atlas](https://github.com/TechieSamosa/atlas)
├── runtime/           # Modern C++ execution engine (allocators, thread pools)
├── scheduler/         # Distributed scheduling logic and DAG parsing
├── storage/           # Dataset and object storage abstractions (AtlasFS)
├── networking/        # RPC, transport layer, and binary serialization
├── python-sdk/        # User-facing Python API and control plane glue
├── cli/               # Command Line Interface (atlas submit, logs, metrics)
├── benchmarks/        # Throughput, latency, and scalability tests
├── docs/
│   ├── architecture/  # System diagrams and topology
│   ├── ADRs/          # Architecture Decision Records
│   ├── HLD.md         # High-Level Design specs
│   └── LLD.md         # Low-Level Design specs (e.g., lock-free queues)
└── examples/          # Sample DAGs, ML training scripts, and ETL jobs

```

## 🗺️ Development Roadmap (12–18 Months)

* [ ] **Phase 1: Runtime Core** - Modern C++ foundation: work-stealing thread pool, task scheduler, arena memory pool, binary serialization, and logging.
* [ ] **Phase 2: Local Execution Engine** - Dependency-aware DAG execution, asynchronous futures, retries, and local profiling.
* [ ] **Phase 3: Distributed Runtime** - TCP/gRPC networking, worker registration, heartbeats, leader election, and distributed task queues.
* [ ] **Phase 4: Storage & Data Layer** - Parquet/Arrow readers, S3/MinIO support, dataset abstraction, and AtlasFS chunked storage.
* [ ] **Phase 5: ML & Data Processing Plugins** - PyTorch/XGBoost executors, Spark integration, and distributed inference capabilities.
* [ ] **Phase 6: Performance & Production** - Lock-free data structures, SIMD optimization, Prometheus metrics telemetry, CLI deployment, and comprehensive benchmarking.

## 📊 Observability & Metrics

Atlas does not rely on flashy web dashboards. It exposes raw, actionable telemetry for infrastructure engineers:

* `CPU/Memory Utilization` (per worker and per task)
* `Task Latency & Scheduling Latency`
* `Queue Depths & Throughput`
* `Network Bandwidth & Zero-copy transfer rates`
* `Worker Uptime, Retries, and Failure Rates`

## 👨‍💻 Author

**Aditya Khamitkar**
[Portfolio](https://adityakhamitkar.vercel.app) | Systems Engineering & Distributed Compute
