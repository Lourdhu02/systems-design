# 01 — Data Platform pitfalls

1. **Tiny files in object storage.** Thousands of 5 MB Parquet files kill query planning. Run compaction on a schedule.

2. **Partitioning by a high-cardinality column.** Partitioning by `user_id` creates millions of partitions; the metastore dies. Partition by date, cluster by user.

3. **No table format on the lake.** Plain Parquet on S3 means no atomic writes, no time travel, no schema evolution discipline. Adopt Iceberg, Delta, or Hudi from day one.

4. **Using CDC where you needed events.** CDC tells you the current state. If you needed to know what changed and when, you're now reconstructing history from snapshots. Don't.

5. **Producer-owned schemas without contracts.** "Production added a new column" should fail CI, not the dashboard at 2 AM.

6. **Ignoring file statistics.** Disabling Parquet column statistics to save a few percent on write throughput is the most expensive five-percent savings you'll ever see.

7. **Training over a mutating table.** Two days into a training run, the upstream CDC sink rewrites half the table. The training set you thought you had is gone. Pin to a snapshot id.

8. **Treating the warehouse as the source of truth.** The warehouse is a copy. The source of truth is the producer's event stream + the OLTP DB. If the warehouse is wrong, fix the pipeline, don't patch the warehouse.

9. **No retention or TTL.** Logs accumulate forever, storage grows, and a five-year-old prediction-log table costs you $200k/month in object storage you have no use for.

10. **Catalog with no owners.** Tables in the catalog with no `owner` field become haunted within twelve months. Make `owner` a required column and break CI when it's missing.
