# Atlas Core 🌍

> **A lightweight, distributed machine learning execution platform built from scratch.**

![C++](https://img.shields.io/badge/C++-17-blue.svg)
![Python](https://img.shields.io/badge/Python-3.10+-yellow.svg)
![Architecture](https://img.shields.io/badge/Architecture-Distributed-success.svg)
![Status](https://img.shields.io/badge/Status-In_Development-orange.svg)

## 📌 The Vision

Running ML at scale today requires stitching together a complex web of tools (Kubernetes, Ray, Spark, Airflow, MLflow). Small teams and independent developers often struggle with this massive infrastructure overhead just to train and deploy models.

**Atlas** is an open-source, distributed AI platform designed to simplify this. It provides a unified engine for job scheduling, distributed execution, and experiment tracking—essentially serving as a lightweight "Kubernetes + Ray + MLflow" for single developers or small teams.

It is built to demonstrate deep engineering fundamentals across **C++ runtime execution, Python orchestration, Data Engineering, and Distributed Systems.**

## 🏗️ High-Level Architecture

Atlas is divided into two primary layers:

1. **The Control Plane (The Brain - Python/PostgreSQL):** Manages the REST/gRPC API, schedules jobs, tracks metadata, and orchestrates the worker nodes.
2. **The Execution Runtime (The Engine - C++):** High-performance worker nodes that handle memory management, thread pooling, and execution of local or distributed ML tasks (PyTorch/TensorFlow).

```text
                    Web Dashboard / CLI
                             │
                    REST / gRPC API
                             │
                 Control Plane (Python)
                             │
         ┌───────────────────┴───────────────────┐
         │                                       │
     Scheduler                            Metadata Service
         │                                       │
   Worker Node A                           Worker Node B
         │                                       │
 C++ Execution Runtime                   C++ Execution Runtime
         │                                       │
    Python Workers                       PyTorch / Spark
```

## 🛠️ Tech Stack

* **Runtime Engine:** C++ (Thread pools, memory management, serialization)
* **Control Plane API:** Python (FastAPI, gRPC)
* **Data & State Management:** PostgreSQL, Redis (Metadata, leader election, task queues)
* **Machine Learning:** PyTorch, XGBoost
* **Data Engineering:** Apache Spark, Parquet, S3/MinIO
* **Infrastructure & MLOps:** Docker, Prometheus, Grafana

## 🗺️ Development Roadmap

This project is actively being developed in phased milestones:

* [ ] **Phase 1: The Engine (Local Execution)**
* Core C++ worker runtime.
* Custom thread pool implementation.
* Basic task execution and status reporting.

* [ ] **Phase 2: The Brain (Control Plane)**
* Python API development (FastAPI).
* PostgreSQL schema for tracking jobs, users, and datasets.
* CLI for submitting basic jobs.

* [ ] **Phase 3: The Network (Distributed Systems)**
* Worker node registration and discovery.
* Heartbeat mechanisms and fault tolerance.
* Distributed task queues and retry logic.

* [ ] **Phase 4: The Payload (AI & Data)**
* PyTorch integration for distributed model training.
* S3/MinIO integration for pulling datasets.
* Spark pipelines for data preprocessing.

* [ ] **Phase 5: The Polish (MLOps)**
* Experiment tracking (metrics, loss, weights).
* Prometheus/Grafana monitoring dashboards.
* Dockerized one-click deployment.

## 🚀 Getting Started

*(Instructions for building the C++ runtime and spinning up the Python control plane will be added here as Phase 1 stabilizes.)*

## 🤝 Contributing

Currently, this is a solo flagship project, but feedback, architectural reviews, and discussions on distributed systems are always welcome!