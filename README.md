# Distributed Orchestrated Data Analytics Pipeline (Argo Workflows)

A production-style distributed ETL pipeline built using **Kubernetes + Argo Workflows**.
The system generates a transactional dataset, processes it across parallel workers, validates & cleans the data, and produces a final analytics report — simulating real-world data engineering platforms.

---

## Architecture

```
Generate CSV
     ↓
Split into 5 chunks
     ↓
Parallel Validation Workers (Fan-Out)
     ↓
Merge Results (Fan-In)
     ↓
Clean Data
     ↓
Analytics Engine
     ↓
Report Output
     ↓
Exit Cleanup
```

---

## Key Concepts Demonstrated

* DAG workflow orchestration
* Parallel distributed processing
* Retry & failure recovery (exponential backoff)
* Artifact passing between workflow steps
* Fan-out / Fan-in execution pattern
* Data validation & cleaning layer
* Exit handler for guaranteed cleanup
* Idempotent processing stages

---

## Dataset

Synthetic **50,000-row e-commerce sales dataset** generated inside the workflow.

Injected anomalies:

* negative prices
* incorrect totals
* failed transactions

This simulates real production dirty data.

---

## Pipeline Stages

| Stage        | Purpose                        |
| ------------ | ------------------------------ |
| Generate CSV | Create raw sales dataset       |
| Split        | Partition for parallel workers |
| Validate     | Detect invalid records         |
| Merge        | Aggregate partitions           |
| Clean        | Remove corrupted data          |
| Analytics    | Compute revenue & order count  |
| Cleanup      | Always runs post workflow      |

Output:

```
analytics_report.json
{
  "orders": <count>,
  "revenue": <sum>
}
```

---

## Tech Stack

* Kubernetes
* Argo Workflows
* Python (Pandas)
* Docker

---

## Running

```bash
# Install Argo
kubectl create namespace argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo-workflows/stable/manifests/install.yaml

# Build image
docker build -t myrepo/python-pandas:3.9 .

# Submit workflow
kubectl create -f data-analytics-pipeline.yaml -n argo
```

---

## What This Project Shows

This project demonstrates how large-scale data pipelines operate in production systems by combining orchestration, parallelism, reliability, and automated cleanup in a distributed environment.
