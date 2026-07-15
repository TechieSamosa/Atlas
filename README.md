# Atlas ⚡

> **A distributed compute runtime for machine learning and data processing.**

![C++](https://img.shields.io/badge/C++-20-blue.svg)
![Python](https://img.shields.io/badge/Python-3.10+-yellow.svg)
![Architecture](https://img.shields.io/badge/Architecture-Distributed_Systems-success.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen)

## 📌 Philosophy

Atlas is built around a simple philosophy: **distributed systems should be understandable, inspectable, and extensible.** 

Instead of hiding scheduling, execution, memory, and networking behind opaque frameworks, Atlas exposes these mechanisms as first-class components. It is designed to be studied, modified, and benchmarked. Atlas implements performance-critical infrastructure components—such as task scheduling, memory allocation, and execution graphs—from first principles to explore their design trade-offs, while leveraging the C++ standard library where it provides robust, well-tested abstractions.

It executes workloads across clusters by exposing the core primitives behind production systems like Ray, Spark, Dask, Borg, and Photon.

## 🧭 Design Principles

*   **Performance First:** Performance-critical paths are implemented in modern C++20 with custom allocators, minimal dynamic allocation, and cache-friendly data structures.
*   **Explicit over Implicit:** Scheduling, execution, networking, and storage remain visible and highly configurable to the user.
*   **Modular Architecture:** Every subsystem (scheduler, allocator, executor) is decoupled and can be replaced independently.
*   **Scalability:** The exact same runtime topology should execute locally on a single laptop or distributed across a hundred nodes.
*   **Observability:** Every important decision made by Atlas (scheduling latency, queue depths, cache hits) is exposed as measurable telemetry.

## 🏗️ System Architecture

Atlas flips the traditional Python-heavy ML stack. Python serves strictly as the control plane and SDK glue, while the high-performance C++ runtime owns all execution, memory, networking, and scheduling.

```text
              [ CLI ] (atlas submit, atlas status, atlas metrics)
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

```

## 🗄️ Project Structure

Atlas is maintained with the engineering rigor of a production infrastructure product. Documentation, architecture decision records (ADRs), and research are treated as first-class citizens alongside the codebase.

```bash
[github.com/TechieSamosa/atlas](https://github.com/TechieSamosa/atlas)
├── runtime/                 # Modern C++ execution engine (allocators, thread pools)
├── scheduler/               # Distributed scheduling logic and DAG parsing
├── networking/              # RPC, transport layer, and binary serialization
├── storage/                 # Dataset abstractions (AtlasFS - Planned)
├── python-sdk/              # User-facing Python API and control plane glue
├── cli/                     # Command Line Interface (atlas submit, logs, metrics)
├── benchmarks/              # Throughput, latency, and scalability tests
├── docs/
│   ├── ADRs/                # Architecture Decision Records (e.g., ADR-0001-Why-CPP20.md)
│   ├── HLD/                 # High-Level Design specs
│   └── LLD/                 # Low-Level Design specs (e.g., lock-free queues)
├── research/                # Engineering papers, profiling reports, and design notes
└── papers-we-implemented/   # Reference implementations of CS research papers

```

## 🗺️ Project Status & Roadmap

Atlas is currently in active development, scaling towards a stable `v1.0` release by 2027. We strictly avoid overpromising features; the list below reflects the actual state of the codebase.

**Current Focus (In Active Development)**

* [ ] **Runtime Core:** C++20 foundation, work-stealing thread pool, and arena allocators.
* [ ] **Local Executor:** Dependency-aware DAG execution engine and asynchronous futures.
* [ ] **Benchmarking Suite:** Microbenchmarks for memory allocators and task scheduling latency.

**Planned (Future Architecture)**

* [ ] **Distributed Networking:** TCP/gRPC layer, binary serialization, zero-copy transfers.
* [ ] **Consensus & Discovery:** Worker registration, heartbeats, and leader election.
* [ ] **AtlasFS:** A lightweight distributed object store abstraction supporting chunking and checksums.
* [ ] **Plugin Ecosystem:** PyTorch/XGBoost executors and CUDA runtime support.

## 📊 Benchmarks & Observability

Atlas does not rely on web dashboards. It is built for infrastructure engineers, relying on CLI tooling and raw telemetry. We benchmark continuously against standard implementations:

* *Sample Benchmark Focus:* Custom Arena Allocator vs. standard `malloc()` over 10 million allocations (measuring latency, throughput, and cache misses).
* *Telemetry:* Exposes metrics on CPU/Memory utilization, task latency, scheduling latency, queue lengths, and worker uptime.


## 👨‍💻 Author

**Aditya Khamitkar**
[Portfolio](https://adityakhamitkar.vercel.app) | Systems Engineering & Distributed Compute
