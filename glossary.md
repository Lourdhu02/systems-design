# Glossary

Two hundred plus terms an ML systems engineer hits in production. One-line definitions. The "see" link points to the module where the term is load-bearing.

The list is alphabetical, not pedagogical. Use it as a lookup, not a curriculum.

---

## A

- **A/B test** — Split traffic between two variants of a model or system; measure a metric difference. See [Module 11](./11-mlops-and-ci-cd/README.md).
- **Accuracy** — Fraction of predictions that match labels. Useless on imbalanced data. See [Module 00](./00-foundations/README.md).
- **Activation checkpointing** — Recompute activations during the backward pass instead of storing them; trades compute for memory. See [Module 03](./03-training-infra/README.md).
- **AdamW** — Adam with decoupled weight decay (Loshchilov & Hutter, 2017). The default optimizer for transformer training.
- **Adversarial example** — Input crafted to flip a model's prediction with minimal perturbation. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **AKS / EKS / GKE** — Managed Kubernetes on Azure / AWS / GCP. See [Module 03](./03-training-infra/README.md).
- **Algolia** — Hosted search service; reference architecture for low-latency text search. See [Module 08](./08-search-and-ranking/README.md).
- **ANN (Approximate Nearest Neighbor)** — Sub-linear vector search; the workhorse of retrieval. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Apache Iceberg** — Open table format on object storage; the lakehouse default since ~2022. See [Module 01](./01-data-platform/README.md).
- **Arize / WhyLabs / Fiddler** — ML observability vendors. See [Module 10](./10-monitoring-and-drift/README.md).
- **Audit log** — Append-only record of who did what to a model or dataset. Required by GDPR and the EU AI Act. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **AUC / ROC-AUC** — Area under the ROC curve; ranking-quality metric robust to class imbalance.
- **Autoscaler** — Component that adjusts the number of replicas based on load. GPU autoscaling is hard because cold start is slow. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Avro / Parquet / ORC** — Columnar and row-oriented file formats; Parquet is the analytical default. See [Module 01](./01-data-platform/README.md).

## B

- **Backfill** — Re-compute historical features or predictions after a pipeline change. See [Module 09](./09-real-time-ml/README.md).
- **Bandit (multi-armed)** — Online decision algorithm that balances exploration and exploitation. See [Module 07](./07-recommendation-systems/README.md).
- **Batch inference** — Predictions computed offline for many inputs at once; cheap per request, stale. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Batch size (effective)** — Per-step batch × gradient accumulation × data-parallel world size. See [Module 03](./03-training-infra/README.md).
- **BERT** — Bidirectional encoder transformer (Devlin et al., 2018); the canonical pre-trained text encoder before LLMs took over.
- **BF16 / FP16 / FP8** — Reduced-precision floating-point formats for training and serving. See [Module 03](./03-training-infra/README.md).
- **Bias-variance trade-off** — Underfit vs overfit. The classical ML framing; less central for LLMs.
- **Blue-green deploy** — Two parallel environments; swap traffic atomically. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **BM25** — Lexical ranking function (Robertson & Zaragoza, 2009). Still the strongest baseline for many search problems. See [Module 08](./08-search-and-ranking/README.md).
- **Bucketization** — Discretizing continuous features into bins. Cheap but lossy.

## C

- **Cache (KV)** — Stored key/value tensors in transformer attention; the dominant memory cost during LLM decode. See [Module 06](./06-llm-serving-and-rag/README.md).
- **Cache hit ratio (prompt cache)** — Fraction of prompt tokens served from cache. The dominant lever for LLM cost. See [Module 06](./06-llm-serving-and-rag/README.md).
- **Canary deploy** — Route a small fraction of traffic to a new model; promote on health. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Candidate generation** — First stage of a recommender; produces a few thousand items from millions. See [Module 07](./07-recommendation-systems/README.md).
- **CCPA** — California Consumer Privacy Act. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **CDC (Change Data Capture)** — Stream of row-level changes from an OLTP database. See [Module 01](./01-data-platform/README.md).
- **Chinchilla scaling** — Compute-optimal model / data scaling for LLMs (Hoffmann et al., DeepMind, 2022). See [Module 12](./12-cost-multitenancy-scaling/README.md).
- **Chunking (RAG)** — Splitting documents into retrievable units. The most under-rated lever in RAG quality. See [Module 06](./06-llm-serving-and-rag/README.md).
- **Cold start** — A user or item with no interactions; recsys's hardest problem. See [Module 07](./07-recommendation-systems/README.md).
- **Concept drift** — The label distribution given input changes over time. See [Module 10](./10-monitoring-and-drift/README.md).
- **Continuous batching** — LLM serving optimization that admits new requests to the in-flight batch every step (Yu et al., Orca, 2022). See [Module 06](./06-llm-serving-and-rag/README.md).
- **Contracts (data contracts)** — Producer-side schema and quality guarantees. See [Module 01](./01-data-platform/README.md).
- **CoreWeave / Lambda / Crusoe** — GPU-first cloud providers. See [Module 03](./03-training-infra/README.md).
- **COGS** — Cost of goods sold; the per-request infra cost of an ML feature. See [Module 12](./12-cost-multitenancy-scaling/README.md).
- **Cross-encoder** — Joint encoder over (query, document); high accuracy, slow. Used as a reranker. See [Module 08](./08-search-and-ranking/README.md).
- **CUDA graph** — Captured sequence of CUDA ops replayed in one launch; cuts kernel launch overhead.

## D

- **Dagster / Airflow / Prefect / Flyte / Kubeflow / Metaflow** — Pipeline orchestrators. See [Module 11](./11-mlops-and-ci-cd/README.md).
- **Data card** — Document describing a dataset (provenance, license, schema). See [Module 10](./10-monitoring-and-drift/README.md).
- **Data contract** — See "Contracts."
- **Data drift** — The input distribution changes. See [Module 10](./10-monitoring-and-drift/README.md).
- **Data lake** — Files on object storage; schema on read. See [Module 01](./01-data-platform/README.md).
- **Data lakehouse** — Lake + ACID table format (Iceberg / Delta / Hudi). See [Module 01](./01-data-platform/README.md).
- **Data warehouse** — Columnar relational store, schema on write (BigQuery, Snowflake, Redshift). See [Module 01](./01-data-platform/README.md).
- **DataHub** — Open lineage catalog (LinkedIn, 2020). See [Module 01](./01-data-platform/README.md).
- **DDP (Distributed Data Parallel)** — PyTorch's data-parallel training. See [Module 03](./03-training-infra/README.md).
- **Delta Lake** — Databricks's ACID table format. See [Module 01](./01-data-platform/README.md).
- **Differential privacy (DP)** — Formal privacy guarantee via noise injection. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **DiskANN** — Disk-resident graph ANN index (Subramanya et al., Microsoft, 2019). See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Dolly / hot-key / skew** — A single key receives a disproportionate share of traffic. See [Module 02](./02-feature-stores/README.md).
- **DPO** — Direct preference optimization (Rafailov et al., 2023); preference fine-tuning without an explicit reward model.
- **Dynamic batching** — Server-side batching of concurrent requests before model invocation. See [Module 04](./04-serving-online-batch-streaming/README.md).

## E

- **Edge inference** — Model runs on the user's device. Low latency, no PII leaves device, hard to update. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Embedding** — A dense vector representation of a discrete object. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Embedding lock-in** — You can't change the embedding model without re-embedding the corpus. See [Module 06](./06-llm-serving-and-rag/README.md).
- **Eval (offline / online)** — Measuring model quality on a held-out set vs in production traffic. See [Module 06](./06-llm-serving-and-rag/README.md).
- **EU AI Act** — EU regulation classifying AI systems by risk (in force since 2024). See [Module 13](./13-privacy-fairness-ethics/README.md).
- **Event sourcing** — Storing every state-changing event; rebuild state by replay.
- **Exploration / exploitation** — The bandit trade-off. See [Module 07](./07-recommendation-systems/README.md).

## F

- **Fairness (group / individual)** — Equal outcomes across protected groups vs equal outcomes for similar individuals. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **Feast** — Open-source feature store (originally Gojek + Google, 2019). See [Module 02](./02-feature-stores/README.md).
- **Feature** — A signal fed to a model.
- **Feature freshness** — Time between an event happening and the feature being available for inference. See [Module 02](./02-feature-stores/README.md).
- **Feature store** — Two-store system for sharing features between training and serving. See [Module 02](./02-feature-stores/README.md).
- **Federated learning** — Train across decentralized data without centralizing it (McMahan et al., Google, 2017). See [Module 13](./13-privacy-fairness-ethics/README.md).
- **Few-shot prompt** — Prompt with examples but no fine-tuning.
- **Flink** — Stateful stream processor (Apache). See [Module 09](./09-real-time-ml/README.md).
- **FlashAttention** — Memory-efficient, IO-aware attention kernel (Dao et al., 2022). The default since 2023.
- **Flyte** — Kubernetes-native orchestrator (Lyft, 2020). See [Module 11](./11-mlops-and-ci-cd/README.md).
- **FSDP (Fully Sharded Data Parallel)** — PyTorch's ZeRO-equivalent sharded data parallel. See [Module 03](./03-training-infra/README.md).
- **Functional drift** — The relationship between features and label changes. See "concept drift."

## G

- **GDPR** — EU General Data Protection Regulation (2018). See [Module 13](./13-privacy-fairness-ethics/README.md).
- **GeMM** — General matrix multiply; the kernel that dominates ML compute.
- **GPU** — Graphics processing unit; the training and (increasingly) serving workhorse. See [Module 03](./03-training-infra/README.md).
- **GPU pooling** — Sharing GPUs across tenants or workloads via MIG, MPS, or scheduling. See [Module 12](./12-cost-multitenancy-scaling/README.md).
- **Gradient accumulation** — Sum gradients across micro-batches before stepping; cheap way to grow effective batch size. See [Module 03](./03-training-infra/README.md).
- **Great Expectations / Soda / dbt tests** — Data quality assertions. See [Module 01](./01-data-platform/README.md).
- **Greenplum / DuckDB / ClickHouse** — Analytical databases.

## H

- **H100 / H200 / B100 / B200 / GB200** — NVIDIA Hopper / Blackwell training GPUs. See [Module 03](./03-training-infra/README.md).
- **HNSW** — Hierarchical Navigable Small World graph ANN (Malkov & Yashunin, 2018). The default in-memory index. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Hot key** — See "dolly."
- **Hudi** — Uber's ACID table format (2017). See [Module 01](./01-data-platform/README.md).
- **Hybrid retrieval** — Combine lexical (BM25) and vector retrieval; rerank. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Hyperparameter** — A knob set before training (learning rate, batch size).

## I

- **IaC (Infrastructure as Code)** — Terraform / Pulumi / CloudFormation. See [Module 11](./11-mlops-and-ci-cd/README.md).
- **Idempotency** — Same input always produces the same effect; necessary for safe retries.
- **Index (vector / inverted)** — Data structure that maps from a query to candidate documents.
- **Inference** — Forward pass at serving time.
- **Iceberg** — See "Apache Iceberg."
- **IVF (Inverted File Index)** — Coarse-grained ANN partitioning. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **IVF-PQ** — IVF + product quantization; the disk-friendly default at billion-scale. See [Module 05](./05-vector-dbs-and-retrieval/README.md).

## J

- **JAX** — Google's array library with `jit`, `vmap`, `pmap`. The default for TPU training.

## K

- **Kafka** — Distributed log; the spine of most streaming systems. See [Module 09](./09-real-time-ml/README.md).
- **Kueue** — Kubernetes-native batch scheduler (kubernetes-sigs, 2023). See [Module 03](./03-training-infra/README.md).
- **KV cache** — See "Cache (KV)."

## L

- **Label** — The ground-truth target a model is trained to predict.
- **Label drift** — The marginal distribution of labels changes. See [Module 10](./10-monitoring-and-drift/README.md).
- **Lakehouse** — See "Data lakehouse."
- **Lambda architecture** — Parallel batch and speed layers, results merged at query time (Marz, 2014). Mostly superseded by streaming.
- **Latency (p50 / p95 / p99 / p999)** — Tail percentiles of request latency. See [Module 00](./00-foundations/README.md).
- **Learning to rank (LTR)** — Pointwise, pairwise, or listwise ranking models. See [Module 07](./07-recommendation-systems/README.md).
- **Lineage** — The graph of which datasets and features feed which models. See [Module 01](./01-data-platform/README.md).
- **LLM** — Large language model.
- **LoRA / QLoRA** — Low-rank adapters for parameter-efficient fine-tuning (Hu et al., 2021; Dettmers et al., 2023).

## M

- **Materialized view** — Pre-computed query result.
- **Megatron-LM** — NVIDIA's transformer-parallel training framework (Shoeybi et al., 2019). See [Module 03](./03-training-infra/README.md).
- **Metaflow** — Netflix's workflow orchestrator (2019). See [Module 11](./11-mlops-and-ci-cd/README.md).
- **MIG (Multi-Instance GPU)** — Hardware partitioning of NVIDIA A100 / H100 / H200 GPUs. See [Module 12](./12-cost-multitenancy-scaling/README.md).
- **Michelangelo** — Uber's end-to-end ML platform (2017). See [Module 14](./14-case-studies-deep-dives/README.md).
- **Minerva** — Airbnb's metrics platform (2018). See [Module 01](./01-data-platform/README.md).
- **MLflow** — Open-source experiment tracking and model registry.
- **MLPerf** — Benchmark suite for training and inference (MLCommons). See [Module 03](./03-training-infra/README.md).
- **Model card** — Document describing a model (intended use, eval, limitations) (Mitchell et al., 2019). See [Module 10](./10-monitoring-and-drift/README.md).
- **Model registry** — Versioned store of model binaries with metadata. See [Module 11](./11-mlops-and-ci-cd/README.md).
- **MPS (Multi-Process Service)** — NVIDIA software GPU sharing.

## N

- **Negative sampling** — Sample non-interacted items as negatives during training; the heart of two-tower training. See [Module 07](./07-recommendation-systems/README.md).
- **NVLink / NVSwitch** — High-bandwidth GPU interconnect. See [Module 03](./03-training-infra/README.md).
- **NDCG** — Normalized discounted cumulative gain; standard ranking metric.

## O

- **Object storage** — S3, GCS, Azure Blob. The bedrock of the modern data stack.
- **OpenLineage** — Open standard for lineage events. See [Module 01](./01-data-platform/README.md).
- **Online inference** — Synchronous, per-request prediction. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Online learning** — Model weights update from each new example. Rare in production; usually frequent batch retraining wins. See [Module 09](./09-real-time-ml/README.md).
- **OLAP / OLTP** — Analytical vs transactional workloads.

## P

- **PagedAttention** — vLLM's virtual-memory-style KV cache (Kwon et al., 2023). See [Module 06](./06-llm-serving-and-rag/README.md).
- **Parquet** — Columnar file format; the analytical default. See [Module 01](./01-data-platform/README.md).
- **PEFT (Parameter-Efficient Fine-Tuning)** — Family including LoRA, prefix tuning, prompt tuning.
- **PII** — Personally identifiable information. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **PinSage** — Pinterest's graph convolutional recommender (Ying et al., 2018). See [Module 14](./14-case-studies-deep-dives/README.md).
- **Pipeline parallel** — Split layers across devices and pipeline micro-batches through them. See [Module 03](./03-training-infra/README.md).
- **Point-in-time correctness (PIT)** — Joining features at the timestamp the prediction would have been made. See [Module 02](./02-feature-stores/README.md).
- **Prefill / decode** — Two phases of LLM inference: parallel prompt encoding vs sequential token generation. See [Module 06](./06-llm-serving-and-rag/README.md).
- **Privacy budget (epsilon)** — Total DP noise spend. See [Module 13](./13-privacy-fairness-ethics/README.md).
- **Prompt cache** — Server-side reuse of pre-attention prompt prefixes across requests. See [Module 06](./06-llm-serving-and-rag/README.md).
- **PyTorch FSDP** — See "FSDP."

## Q

- **Quantization (PTQ / QAT)** — Post-training or training-time reduction of weight precision (INT8, INT4, FP4). See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Query understanding** — Intent classification, expansion, spelling correction. See [Module 08](./08-search-and-ranking/README.md).

## R

- **RAG (Retrieval-Augmented Generation)** — Generate with external retrieved context (Lewis et al., 2020). See [Module 06](./06-llm-serving-and-rag/README.md).
- **Ray** — Distributed compute framework (UC Berkeley + Anyscale). See [Module 03](./03-training-infra/README.md).
- **Recall@k** — Fraction of relevant items in the top k. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Reranker** — Model that re-orders a small candidate set with a heavier scorer. See [Module 08](./08-search-and-ranking/README.md).
- **REST / gRPC** — Request / response protocols.
- **RLHF** — Reinforcement learning from human feedback (Christiano et al., 2017; Ouyang et al., 2022).
- **Rollout / rollback** — Deploying a new model and unwinding it. See [Module 11](./11-mlops-and-ci-cd/README.md).

## S

- **SageMaker / Vertex AI / Azure ML** — Hyperscaler managed ML platforms.
- **Sampling (stratified, importance)** — Drawing data with a bias to a useful subset.
- **Scaling law** — Empirical relation between compute, data, parameters, and loss. See [Module 12](./12-cost-multitenancy-scaling/README.md).
- **ScaNN** — Google's ANN library (Guo et al., 2020). See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Schema evolution** — Adding / removing / renaming columns over time. See [Module 01](./01-data-platform/README.md).
- **Shadow deploy** — Run a new model on real traffic without serving its responses. See [Module 11](./11-mlops-and-ci-cd/README.md).
- **Sharding** — Partitioning data or model state across nodes.
- **Skew (training-serving)** — Features computed differently at training and serving time. See [Module 00](./00-foundations/README.md).
- **SLA / SLO / SLI** — Service Level Agreement / Objective / Indicator. See [Module 00](./00-foundations/README.md).
- **Slurm** — HPC workload manager; still common in academic and on-prem GPU clusters. See [Module 03](./03-training-infra/README.md).
- **Speculative decoding** — Use a small draft model to propose tokens; verify with the large model (Leviathan et al., 2023). See [Module 06](./06-llm-serving-and-rag/README.md).
- **Streaming inference** — Predictions emitted from a streaming pipeline. See [Module 04](./04-serving-online-batch-streaming/README.md).

## T

- **Tensor parallel (TP)** — Split a single matmul across devices. See [Module 03](./03-training-infra/README.md).
- **Tecton** — Commercial feature store. See [Module 02](./02-feature-stores/README.md).
- **TF Serving / Triton / TorchServe** — Model servers. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Throughput** — Requests (or tokens) per second.
- **Token / tokenizer** — Subword unit and the algorithm that produces it (BPE, SentencePiece).
- **TPU** — Google's tensor processing unit. See [Module 03](./03-training-infra/README.md).
- **Train-serve skew** — See "Skew."
- **Two-tower model** — Separate query and item encoders sharing a similarity space; the workhorse of retrieval. See [Module 07](./07-recommendation-systems/README.md).

## U

- **Uplift modeling** — Predicting the causal effect of a treatment per individual.
- **UTC** — The only timezone an ML pipeline should ever store timestamps in.

## V

- **Vector database** — Storage + ANN index over embeddings. See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **Vespa** — Yahoo's serving engine (open-sourced 2017). See [Module 05](./05-vector-dbs-and-retrieval/README.md).
- **vLLM** — High-throughput LLM serving framework with PagedAttention (Kwon et al., 2023). See [Module 06](./06-llm-serving-and-rag/README.md).
- **Volcano** — Kubernetes batch scheduler for HPC / ML workloads. See [Module 03](./03-training-infra/README.md).

## W

- **Warm pool** — Pre-initialized replicas held to absorb traffic spikes. See [Module 04](./04-serving-online-batch-streaming/README.md).
- **Watermark** — Stream processing: estimated event-time progress. See [Module 09](./09-real-time-ml/README.md).
- **Webdataset / Mosaic Streaming** — Streaming dataloaders for large training jobs. See [Module 03](./03-training-infra/README.md).

## X

- **XGBoost / LightGBM / CatBoost** — Gradient-boosted decision trees; the default for tabular ranking until ~2022 and still strong.

## Y

- **YAML** — Configuration format; the lingua franca of K8s and CI.

## Z

- **ZeRO (Zero Redundancy Optimizer)** — Microsoft DeepSpeed's sharded optimizer states / gradients / parameters (Rajbhandari et al., 2020). The conceptual ancestor of FSDP. See [Module 03](./03-training-infra/README.md).
- **Zero-shot** — Inference without examples in the prompt.
