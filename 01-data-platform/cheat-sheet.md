# 01 — Data Platform cheat sheet

1. **Default new build in 2026: Iceberg on object storage**, queried by multiple engines, governed by a catalog. The lakehouse pattern (Armbrust et al., CIDR 2021) has won the new-build battle.

2. **Warehouse for BI only. Lakehouse for BI + ML.** Models can't read Snowflake or BigQuery from a GPU node easily; they can read Parquet on S3 from anywhere.

3. **Three ingestion shapes:** batch (files), CDC (row changes), event stream (semantic facts). A real platform has all three.

4. **CDC mirrors state; events record history.** Don't conflate them.

5. **Table format pick:** Iceberg for engine neutrality, Delta if you're Databricks-centric, Hudi if upserts/CDC sinks dominate.

6. **Four cost levers:** partitioning, clustering, file size (128 MB - 1 GB), statistics.

7. **The cheapest query is one that doesn't scan files.** Min/max metadata makes counts free.

8. **Schema evolution at the storage layer is not the same as schema evolution at the semantic layer.** Producer-side contracts cover the gap.

9. **Three lines of defence for data quality:** producer contracts (Buf/Protobuf), reader expectations (Great Expectations / dbt tests), lineage (OpenLineage + DataHub).

10. **Snapshot-isolate your training reads.** Pin to an Iceberg / Delta snapshot ID so the table can't shift under you mid-training.
