# Reading list

Ranked external reading per module. "S" means start here (read first). "R" means recommended. "D" means depth (read if the topic owns your week).

## Cross-cutting

- **S** *Designing Data-Intensive Applications*, Martin Kleppmann (O'Reilly, 2017). System reliability vocabulary that the rest of the field borrows from.
- **S** *Designing Machine Learning Systems*, Chip Huyen (O'Reilly, 2022). The closest book to what this course is.
- **R** *Reliable Machine Learning*, Chen, Murphy, Zaharia et al. (O'Reilly, 2022). Production operations.
- **R** *Machine Learning Systems*, Vijay Janapa Reddi (open textbook, 2024). Embedded / efficient ML.

---

## 00 Foundations

- **S** DDIA chapters 1-2 (Kleppmann 2017). Reliability, scalability, maintainability.
- **S** Huyen chapters 1-2 (2022).
- **R** "Site Reliability Engineering," Betsy Beyer et al. (Google, O'Reilly, 2016). Specifically the SLO chapters.
- **R** "Hidden Technical Debt in Machine Learning Systems," Sculley et al. (NeurIPS 2015).

## 01 Data Platform

- **S** "Lakehouse: A New Generation of Open Platforms," Armbrust et al. (CIDR 2021).
- **S** "Apache Iceberg: An Architectural Look Under the Covers" (Tabular / Apache, 2022).
- **R** "Hudi: Streaming Data Lake Platform," Vinoth Chandar et al. (Uber / Apache, 2021).
- **R** "Apache Parquet" docs and the 2013 paper.
- **R** "Minerva: A Centralized Metric Platform" (Airbnb Engineering, 2018).
- **D** "DataHub: A Generalized Metadata Search & Discovery Tool" (LinkedIn, 2020).
- **D** "Building Reliable Data Pipelines," Jesse Anderson (O'Reilly, 2023).

## 02 Feature Stores

- **S** "Meet Michelangelo: Uber's Machine Learning Platform" (Uber Engineering, 2017).
- **S** "Feature Store as a Service" (Tecton, 2020).
- **R** Feast documentation and the Feast 0.40 architecture post (2024).
- **R** "Real-time Machine Learning at Pinterest" (Pinterest Engineering, 2022).
- **D** "ML Feature Engineering at Scale at LinkedIn" (LinkedIn Engineering, 2023).

## 03 Training Infra

- **S** "Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism," Shoeybi et al. (NVIDIA, 2019).
- **S** "PyTorch FSDP: Experiences on Scaling Fully Sharded Data Parallel," Zhao et al. (Meta, 2023).
- **R** "ZeRO: Memory Optimizations Toward Training Trillion Parameter Models," Rajbhandari et al. (Microsoft, 2020).
- **R** "GPipe: Efficient Training of Giant Neural Networks using Pipeline Parallelism," Huang et al. (Google, 2019).
- **R** MLPerf Training Rules and results, current round (MLCommons).
- **D** "Reducing Activation Recomputation in Large Transformer Models," Korthikanti et al. (NVIDIA, 2022).
- **D** "Pathways: Asynchronous Distributed Dataflow for ML," Barham et al. (Google, 2022).

## 04 Serving

- **S** "Scaling Machine Learning at Stripe" (Stripe Engineering, 2023).
- **S** "Things I Learnt From a Senior Software Engineer" series; specifically the serving piece — and DoorDash's "Maintaining Machine Learning Model Accuracy Through Monitoring" (2022).
- **R** NVIDIA Triton Inference Server docs (current release).
- **R** "Continuous Batching" (Anyscale blog, 2023).
- **D** "Clipper: A Low-Latency Online Prediction Serving System," Crankshaw et al. (NSDI 2017).

## 05 Vector DBs and Retrieval

- **S** "Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs," Malkov & Yashunin (TPAMI 2018).
- **S** "Billion-scale similarity search with GPUs," Johnson, Douze, Jégou (Facebook, 2019).
- **R** "DiskANN: Fast Accurate Billion-point Nearest Neighbor Search on a Single Node," Subramanya et al. (Microsoft, NeurIPS 2019).
- **R** "ScaNN: Accelerating Large-Scale Inference with Anisotropic Vector Quantization," Guo et al. (Google, ICML 2020).
- **D** Pinecone, Weaviate, Qdrant, Milvus, Vespa, OpenSearch docs (current releases).

## 06 LLM Serving and RAG

- **S** "Efficient Memory Management for Large Language Model Serving with PagedAttention," Kwon et al. (vLLM, SOSP 2023).
- **S** "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks," Lewis et al. (Facebook AI, NeurIPS 2020).
- **R** "Orca: A Distributed Serving System for Transformer-Based Generative Models," Yu et al. (OSDI 2022). Continuous batching.
- **R** "Fast Inference from Transformers via Speculative Decoding," Leviathan et al. (Google, ICML 2023).
- **R** Anthropic prompt caching documentation and "Building Effective Agents" (Anthropic, 2024-2025).
- **D** "FlashAttention: Fast and Memory-Efficient Exact Attention with IO-Awareness," Dao et al. (NeurIPS 2022).
- **D** "Lost in the Middle: How Language Models Use Long Contexts," Liu et al. (TACL 2023).

## 07 Recommendation Systems

- **S** "Deep Neural Networks for YouTube Recommendations," Covington et al. (RecSys 2016) and follow-ups through 2019.
- **S** "Graph Convolutional Neural Networks for Web-Scale Recommender Systems (PinSage)," Ying et al. (Pinterest, KDD 2018).
- **R** "BaRT: Bandits for Recommendations as Treatments" (Spotify, RecSys 2020).
- **R** "Sampling-Bias-Corrected Neural Modeling for Large Corpus Item Recommendations," Yi et al. (Google, RecSys 2019).
- **R** "Instagram's Explore Recommender System" (Meta Engineering, 2023).
- **D** "Two-Tower Models for Recommendation" — Google's papers (2019-2022).

## 08 Search and Ranking

- **S** "Probabilistic Relevance Framework: BM25 and Beyond," Robertson & Zaragoza (2009).
- **S** "Real-time Personalization at Etsy" (Etsy Engineering, 2022).
- **R** "ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT," Khattab & Zaharia (SIGIR 2020).
- **R** "How Algolia Builds Search Infrastructure" (Algolia Engineering, current).
- **D** "Pre-training Tasks for Embedding-Based Large-Scale Retrieval," Chang et al. (Google, ICLR 2020).

## 09 Real-time ML

- **S** "How Stripe Detects Fraud at Scale (Radar)" (Stripe Engineering, 2023).
- **S** "Streaming SQL Foundations" Apache Flink documentation.
- **R** Uber's "Real-time Data Infrastructure at Uber" (VLDB 2021).
- **R** "Online Learning for Recommender Systems," Bennett & Lanning (Netflix Prize retrospective, 2009).

## 10 Monitoring and Drift

- **S** "Monitoring Machine Learning Models in Production" (Chip Huyen, 2022). Long-form blog version.
- **S** "Model Cards for Model Reporting," Mitchell et al. (FAT* 2019).
- **R** "Failing Loudly: An Empirical Study of Methods for Detecting Dataset Shift," Rabanser et al. (NeurIPS 2019).
- **R** "Datasheets for Datasets," Gebru et al. (CACM 2021).
- **D** Evidently AI and WhyLabs blog archives.

## 11 MLOps and CI/CD

- **S** "MLOps: Continuous Delivery and Automation Pipelines in Machine Learning" (Google Cloud, 2020).
- **S** "TFX: A TensorFlow-Based Production-Scale Machine Learning Platform," Baylor et al. (KDD 2017).
- **R** Metaflow architecture posts (Netflix, 2019-2023).
- **R** Flyte and Dagster architecture pages (current).
- **D** "Continuous Delivery for Machine Learning" (Sato, Wider, Windheuser, martinfowler.com, 2019).

## 12 Cost, Multi-tenancy, Scaling

- **S** "Scaling Laws for Neural Language Models," Kaplan et al. (OpenAI, 2020).
- **S** "Training Compute-Optimal Large Language Models (Chinchilla)," Hoffmann et al. (DeepMind, 2022).
- **R** NVIDIA Multi-Instance GPU docs (current).
- **R** "The Economics of Large Language Models" (various, 2023-2024).
- **D** "Sky Computing: Vision for a New Cloud Computing Paradigm," Stoica & Shenker (HotOS 2021).

## 13 Privacy, Fairness, Ethics

- **S** "Communication-Efficient Learning of Deep Networks from Decentralized Data (Federated Averaging)," McMahan et al. (Google, AISTATS 2017).
- **S** "Fair prediction with disparate impact: A study of bias in recidivism prediction instruments," Chouldechova (Big Data 2017). The impossibility theorem.
- **R** EU AI Act (Regulation 2024/1689).
- **R** "The Algorithmic Foundations of Differential Privacy," Dwork & Roth (2014).
- **D** Anthropic and OpenAI red-teaming methodology posts (2024-2025).

## 14 Case Studies (deep dives)

Reading list overlaps with the per-module sources; see [`14-case-studies-deep-dives/`](./14-case-studies-deep-dives/) for the full set.

## 15 Capstone

No additional reading; the capstone synthesizes everything above.
