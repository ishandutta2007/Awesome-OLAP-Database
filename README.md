# Awesome-OLAP-Database

## Top OLAP Database Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Columnar Storage, Vectorized Execution & Real-Time Analytical Throughput*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **OLAP Databases**. These tools are built for analytical workloads — scanning billions of rows, aggregating wide tables, and answering queries in seconds rather than minutes — using columnar storage, vectorized execution, and aggressive compression instead of row-by-row transactional processing.

**Examples** include DuckDB, ClickHouse, Apache Doris, StarRocks, Trino, Apache Druid, Google BigQuery, Snowflake, Amazon Redshift, and Databricks SQL (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, in-process analytics, and transparent query execution — ideal for data engineers choosing between embedded analytics, distributed OLAP engines, and managed cloud warehouses.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Google BigQuery** — Serverless, fully managed data warehouse with columnar storage and automatic scaling. **Best for GCP-native organizations** wanting zero infrastructure management.

- **Snowflake** — Multi-cloud data warehouse with separated storage and compute, supporting Iceberg tables natively. **Best for enterprises** wanting elastic compute with strong data sharing.

- **Amazon Redshift** — AWS-managed data warehouse with Redshift Serverless and AQUA acceleration. **Best for AWS-native organizations** with existing Redshift investments.

- **Databricks SQL** — Managed SQL warehouse on Delta Lake with Photon engine acceleration. **Best for teams already on Databricks** wanting SQL analytics alongside Spark workloads.

- **ClickHouse Cloud** — Managed ClickHouse with usage-based pricing and automatic scaling. **Best for teams wanting ClickHouse performance** without cluster operations [citation:7].

- **Azure Synapse** — Microsoft's analytics service combining SQL pools with Spark integration. **Best for Azure-native enterprises.**

## Open-Source GitHub Projects

- **[DuckDB](https://github.com/duckdb/duckdb)**  
  **The in-process analytical database that runs anywhere** — MIT licensed. **Columnar engine with parallel execution and larger-than-memory processing** [citation:1]. **DuckDB 2.0.0 LTS** (October 2026) brings **triggers** (BEFORE/AFTER, row and statement level), **PEG-based SQL parser** replacing the Yacc/Bison grammar, **self-hosted extension repositories** with RSA-signed extensions, **data-modifying CTEs**, **NEAREST joins for vector search**, and **ICU-free timezone/calendar/collation support** [citation:2][citation:9]. **Storage format v2.0** loads column metadata lazily and enables `DICT_FSST` string compression by default, so wide tables open faster with less memory [citation:9]. **Selective Late Materialization** outperforms traditional early/late materialization by 14.7% and 8.9% respectively on Join Order Benchmark, with up to 76.7% latency reduction for individual queries [citation:8]. **Best for local analytics, data science workflows, and embedded SQL** — the "SQLite for analytics" [citation:7].

- **[ClickHouse](https://github.com/ClickHouse/ClickHouse)**  
  **The columnar OLAP engine built for maximum query throughput** — Apache 2.0 licensed, **DB-Engines rank #26 overall** [citation:5]. **MergeTree storage engine family** with vectorized execution and aggressive compression. **The strongest single-table aggregation performance** among open-source OLAP engines — designed to "process each analytical query as fast as possible" [citation:5]. **Simpler operational footprint than Druid** — no ZooKeeper dependency (ClickHouse Keeper instead), no strict ingestion/query node separation, and schema evolution via `ALTER TABLE` without rewriting data [citation:12]. **Trade-offs**: **Upsert is weak** — ReplacingMergeTree requires `FINAL` for correct reads during queries, and is unsuitable for high-frequency updates [citation:6]. **Lakehouse integration lags Doris and StarRocks** [citation:13]. **Best for log analytics, user behavior analysis, and read-heavy workloads** where single-table scan speed matters more than complex joins [citation:13].

- **[Apache Doris](https://github.com/apache/doris)**  
  **The MPP analytical engine balancing high concurrency and complex queries** — Apache 2.0 licensed. **FE (Frontend) + BE (Backend) architecture** with Paxos for metadata consistency, **no ZooKeeper required**, and **MySQL protocol compatibility** [citation:6]. **Unique Key model supports real-time Upsert** — write and query latest data without waiting for compaction [citation:6]. **Doris 4.1.4** added Merge-on-Write table ANN indexing, multimodal file embedding, and OceanBase CDC streaming [citation:4]. **Doris 3.1** introduced flexible column updates (different rows updating different columns in one import), binary search for partition pruning (**16x speedup on 136,000 partitions**), and MOW lock optimization for storage-compute separation [citation:11]. **Benchmarks**: outperformed ClickHouse and DuckDB on ASOF JOIN across 11 scenarios [citation:3]. **Best for enterprise reporting** requiring both high-concurrency point queries and analytical SQL with real-time updates [citation:13].

- **[StarRocks](https://github.com/StarRocks/starrocks)**  
  **The CBO-optimized OLAP engine for complex joins and lakehouse queries** — Apache 2.0 licensed. **Rewritten query engine** with the most mature cost-based optimizer among open-source OLAP engines — **5-10x faster than ClickHouse on star/snowflake schema JOINs** is common [citation:6]. **Primary Key model with LSM-Tree** for Upsert that outperforms Doris Unique Key on high-frequency update throughput and query latency [citation:6]. **Shared-Data mode (3.0+)** with K8s Operator provides near-Snowflake elasticity — compute scales in seconds, data on object storage [citation:6]. **Best lakehouse integration** among the three — Iceberg and Hudi catalogs are most complete [citation:13]. **Best for complex multi-table joins and elastic lakehouse deployments** [citation:7].

- **[Trino](https://github.com/trinodb/trino)**  
  **The distributed SQL query engine for federated analytics** — Apache 2.0 licensed. **Queries data where it lives** — Hive, Cassandra, relational databases — without moving the underlying data [citation:7]. **Best for cross-source ad-hoc queries** where you need to join data across heterogeneous systems without ETL [citation:13].

- **[Apache Druid](https://github.com/apache/druid)**  
  **The real-time analytics datastore for sub-second queries on streaming data** — Apache 2.0 licensed. **Pre-aggregates data at ingestion via rollup**, reducing data volume scanned by queries [citation:12]. **Streaming ingestion natively through Kafka/Kinesis** with data visible for query within seconds [citation:12]. **Trade-offs**: **more complex to operate than ClickHouse** — requires ZooKeeper, metadata database, separate services per role, and deep storage [citation:12]. **Best for fixed-dimension dashboards** with predictable rollup query patterns and guaranteed sub-second latency [citation:12].

### Additional Strong Open-Source Options

- **Apache Pinot** — Real-time distributed OLAP datastore built for user-facing analytics at high QPS, with pluggable indexing and upsert support.
- **Apache Kylin** — Distributed analytics engine for sub-second queries on massive datasets over Hadoop.
- **Databend** — Open-source cloud data warehouse with Rust-based architecture.
- **ByConity** — Open-source cloud data warehouse forked from ClickHouse with separated storage and compute.
- **chDB** — In-process OLAP SQL engine powered by ClickHouse, embedded like DuckDB [citation:14].

**Frameworks for building custom systems**: Combine **DuckDB** for local analytics and embedded SQL, **ClickHouse** for maximum single-table scan throughput on logs and events, **Apache Doris** for enterprise reporting with real-time Upsert, **StarRocks** for complex joins and lakehouse elasticity, **Trino** for federated cross-source queries, and **Apache Druid** for fixed-dimension streaming dashboards — to build an OLAP stack tailored to your query patterns, update frequency, and operational maturity.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- OLAP databases differ significantly in architecture, update capability, and operational complexity. **The key selection dimension is your workload's failure mode** — ClickHouse fails on high-frequency Upsert and complex joins; Doris and StarRocks fail at extreme single-table scan throughput; Druid fails on ad-hoc raw data queries [citation:6][citation:12]. **ClickHouse's Upsert is append-only at heart** — ReplacingMergeTree requires `FINAL` for correct reads and is unsuitable for CDC-heavy pipelines [citation:6]. **StarRocks' CBO is the most mature** for multi-table JOINs, but requires more BE memory for Primary Key indexes [citation:6]. **Druid's operational overhead is real** — ZooKeeper, metadata DB, and deep storage are mandatory [citation:12]. **Many enterprises end up with multi-engine combinations** — ClickHouse for logs, Doris/StarRocks for reports, Trino for federation — trading operational complexity for workload-appropriate performance [citation:13].
- Self-hosted open-source solutions require proper cluster sizing, partition strategy design, and monitoring of compaction and query latency.

---

**Made for data engineers and architects choosing analytical engines that match their query patterns and update frequency.**
Let's make OLAP databases more open, workload-appropriate, and operationally transparent.
