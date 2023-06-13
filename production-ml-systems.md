Synopsis: This course covers how to implement the various flavors of production ML systems— static, dynamic, and continuous training; static and dynamic inference; and batch and online processing. You delve into TensorFlow abstraction levels, the various options for doing distributed training, and how to write distributed training models with custom estimators.

# 01. Intro to Advanced ML on Google Cloud

Static vs Dynamic training (like physica vs fashion) In dynamic environments like fashion, we'll have to constantly re-train our models

Dynamic training can make use of 3 potential architectures (Cloud Funciton, App Enginer, DataFlow)

3 use-cases for dynamic training

1. file arrives in Cloud Storage -> Launch Cloud Func -> Start Train job on AI platform -> AI platform produces new model
2. user web request -> start AI plt tr job -> new model
3. Pub/Sub topic -> consumed by Dataflow and aggregated -> write agg data in BigQuery -> AIP job launched on new data in BQ -> new model

Serving design decision:

one of the goals: minimize latency

Static serving: pre-computed labels stored in look-up tables Dynamic serving: computes label on demand

there's a space time trade-off 
![](Pasted%20image%2020230204191816.png)


# 02. Architecting Production ML Systems

## 02.01 - Architecting ML Systems 
## 02.02 - Data Extraction, Analysis, and Preparation
## 02.03 - Model Training, Eval, and Validation
## 02.04 - Trained model, prediction service, and performance monitoring
## 02.05 - Training design decisions
## 02.06 - Serving design decisions
## 02.07 - Designing from scratch
## 02.08 - Using Vertex AI
## 02.09 - Lab: Structured Data Prediction using Vertex AI Platform

# 03. Designing Adaptable Systems
# 04. Designing High-Performance ML Systems
# 05. Building Hybrid ML Systems

