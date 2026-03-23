# 🛡️ NovaEco Backup & Recovery (Backup)

![Component ID](https://img.shields.io/badge/ID-BACKUP-orange)
![Layer](https://img.shields.io/badge/Layer-Infrastructure-blue)
![Type](https://img.shields.io/badge/Type-CronJob-green)

> **The Disaster Recovery Engine.**
> Orchestrates the scheduled snapshotting, encryption, and off-site archival of critical system state across the decentralized NovaEco ecosystem.

## 📖 Overview

The **NovaEco Backup Service** is the insurance policy of the Digital Public Infrastructure (DPI). It guarantees Business Continuity and regulatory compliance for environmental audits by managing the lifecycle of critical system data (e.g., environmental ledgers, identity mappings, and raw IoT telemetry).

Because NovaEco enforces strict Domain-Driven Data Ownership, this service operates as a decentralized CronJob orchestrator. It does not connect to underlying databases directly; instead, it securely queries the owning microservices to dump their state.

* **Role:** Snapshot Orchestration, Artifact Versioning, & Lifecycle Tiering.
* **Input:** `CreateSnapshot` gRPC streams from stateful domain services.
* **Output:** Encrypted blobs in Cloud Storage (S3/MinIO) and Purge Directives.

---

## ⚠️ Architectural Migration Notice (V2)

**This repository is part of the NovaEco v2 Polyrepo Architecture.**
Active development and migration from the legacy v1 prototype into this dedicated infrastructure component is scheduled for **Q2/Q3 2026**. 

---

## 🌟 Key Capabilities (Target Architecture)

### 1. Decentralized Orchestration
Rather than maintaining complex DB credentials and managing table locks across polyglot engines, the Backup service simply acts as a client. It calls the `CreateSnapshot` RPC on target services, receiving a stream of bytes which it then bundles. This preserves the strict separation of concerns between decentralized worker nodes.

### 2. Point-in-Time Alignment
Manages the complexity of cross-domain backups. It ensures that a snapshot of the environmental ledgers (`novaeco-balance`) is perfectly time-aligned with the identity states (`novaeco-auth`) and telemetry data, allowing for a coherent restoration of the full ecosystem state.

### 3. Zero-Trust Encryption
Data never leaves the secure enclave in plain text. Archives are compressed and encrypted (AES-256 / GPG) *before* transmission to the cloud provider, protecting sensitive municipal or enterprise supply chain data.

### 4. Regulatory Compliance & WORM Storage
To satisfy rigorous institutional ESG audits and EU Corporate Sustainability Reporting Directive (CSRD) mandates, critical impact logs are archived to **WORM** (Write Once, Read Many) storage buckets. This ensures that historical environmental claims cannot be tampered with or retroactively altered.

### 5. Automated Archival & Tiering (Sustainability)
To decouple data volume growth from energy consumption, this service acts as the executor of the platform's Hot/Cold data tiering policy. After successfully verifying that historical telemetry is safely compressed and archived in cold storage, it invokes a `PurgeColdData` command to securely delete old records from the high-power active databases.

---

## 🤝 Contributing

We welcome contributors to help build the open-source infrastructure for the circular economy. 

Please see the central [**NovaEco Organization README**](https://github.com/novaeco-tech) for our overall contribution guidelines, Code of Conduct, and ecosystem roadmap.

## 📄 License

This project is licensed under the **Apache License 2.0**. See the [LICENSE](LICENSE) file for details.
