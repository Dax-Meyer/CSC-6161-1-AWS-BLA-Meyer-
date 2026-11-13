# Amazon SageMaker AI: Machine Learning Concepts, Implementation, and Best Practices

## Overview

Amazon SageMaker AI is an AWS service that provides tools for developing, training, deploying, and managing machine learning models. It supports the machine learning lifecycle, bringing together capabilities for data preparation, feature management, model development, training, deployment, monitoring, and governance.

This repository provides an overview of the concepts and AWS services involved in building and operating machine learning solutions with Amazon SageMaker AI. It follows a practical example throughout: predicting an individual's AWS certification exam score using information such as practice test results, study hours, and completed lessons.

## 1. Machine Learning Fundamentals

### The machine learning lifecycle

A typical machine learning workflow includes:

1. **Collect and prepare data** – Gather relevant information, clean it, address missing values and inconsistencies, and transform it into a useful format.
2. **Create and select features** – Identify input variables that can help the model make predictions.
3. **Train a model** – Select an appropriate algorithm and use prepared data to train it.
4. **Evaluate the model** – Measure how well the model performs, using data that was not used for training where appropriate.
5. **Deploy the model** – Make the model available to an application or other process for inference.
6. **Monitor the model** – Check data and model performance after deployment and investigate changes over time.

### Supervised learning

Supervised learning uses labeled data, where examples include the expected answer.

- **Regression** predicts a numerical value. For example, a model could estimate an expected exam score.
- **Classification** predicts a category. For example, a model could estimate whether a student is likely to pass or fail.
- **K-nearest neighbors (KNN)** makes predictions using similar examples in the dataset.

### Unsupervised learning

Unsupervised learning works with data that does not have predefined labels.

- **K-means** groups similar records into clusters. It could be used to identify groups of students with similar study habits.
- **Principal component analysis (PCA)** reduces the number of features while retaining as much important information as possible.

Other machine learning applications discussed include anomaly detection, natural language processing, image processing, and time-series forecasting. DeepAR, for example, uses recurrent neural networks to forecast time-series data such as future sales based on historical information.

## 2. SageMaker Tools for Data and Model Development

### SageMaker Studio

SageMaker Studio provides a unified environment for working with machine learning. It brings together tools for preparing data, building models, running experiments, training models, and managing workflows.

### SageMaker Data Wrangler

Data Wrangler helps users import, explore, clean, and transform datasets. It supports data-quality checks, visualizations, and feature engineering.

Feature engineering means creating or modifying input variables to make them more useful to a model. For example, individual practice test scores could be combined into an average practice score.

### SageMaker Feature Store

A feature is an individual input variable used by a machine learning model. In the exam-score example, features could include study hours, practice scores, and completed lessons.

SageMaker Feature Store provides a centralized place to store, organize, discover, and reuse machine learning features.

- **Online store:** Designed for low-latency access to feature values, such as when an application needs features for a real-time prediction.
- **Offline store:** Maintains historical feature information and supports training, analysis, and larger data-processing workloads.

Using a feature store can help teams maintain consistency between features used during training and features used for inference.

### SageMaker Clarify and Ground Truth

- **SageMaker Clarify** helps examine potential bias in datasets and models and provides model explainability information about features that influence predictions.
- **SageMaker Ground Truth** supports the creation of labeled datasets through human data annotation and review. Human feedback is also relevant to approaches such as reinforcement learning from human feedback (RLHF).

### SageMaker JumpStart and Canvas

- **SageMaker JumpStart** provides access to pretrained machine learning models and ready-to-use solutions that can be customized.
- **SageMaker Canvas** provides a visual interface for building machine learning models without writing all of the underlying code. It also supports automated machine learning through SageMaker Autopilot.

AWS also provides purpose-built AI services, including Amazon Rekognition for image and video analysis, Amazon Comprehend for text analysis, and Amazon Textract for extracting information from documents.

## 3. Implementing a Machine Learning Workflow

The following steps describe a possible workflow for the exam-score prediction example.

### Step 1: Define the problem

Determine the prediction goal, the information required, and how success will be measured. Predicting an exam score is a regression problem because the output is a numerical value.

### Step 2: Prepare data

Use SageMaker Data Wrangler to import and explore the dataset, identify missing values, remove duplicates, correct inconsistencies, and apply transformations. Feature engineering can create more useful inputs, such as an average practice test score.

### Step 3: Manage features

Store reusable features in SageMaker Feature Store. The online store can support low-latency inference use cases, while the offline store can support historical analysis and model training.

### Step 4: Train and evaluate a model

SageMaker supports built-in algorithms and custom training approaches. Linear regression is one possible choice for predicting exam scores.

After training, evaluate the model using appropriate metrics. For regression, examples include:

- **Mean Squared Error (MSE)**
- **Root Mean Squared Error (RMSE)**

Compare results against a baseline and evaluate with data that was not used during training.

### Step 5: Tune hyperparameters

Hyperparameters are settings that influence how an algorithm learns. SageMaker Automatic Model Tuning can run multiple training trials to explore specified hyperparameter ranges and compare results against an objective metric.

Tuning can help identify promising configurations, but it does not guarantee a perfect model.

### Step 6: Deploy for inference

SageMaker supports several inference options:

- **Real-time inference:** For individual predictions where low latency is important.
- **Serverless inference:** Automatically adjusts capacity based on incoming requests and can suit intermittent traffic, although response latency may vary.
- **Asynchronous inference:** For larger requests or workloads that take longer to process; Amazon S3 can store associated request and response data.
- **Batch inference:** Processes a dataset in batches rather than handling individual requests continuously.

The appropriate option depends on request size, traffic patterns, latency requirements, and cost.

### Step 7: Automate workflows with SageMaker Pipelines

SageMaker Pipelines can connect machine learning tasks into repeatable workflows. A pipeline might prepare data, train a model, run tuning, evaluate results, and register a model. Quality checks and Clarify checks can also be included.

These repeatable workflows support machine learning operations (MLOps) and continuous integration and continuous delivery practices for machine learning.

### Step 8: Track experiments with MLflow

MLflow is an open-source platform for managing parts of the machine learning lifecycle. Experiment tracking records information such as training runs, parameters, metrics, and model artifacts. Teams can compare different model versions and configurations. SageMaker supports MLflow tracking server capabilities.

## 4. Security and Governance

### AWS Shared Responsibility Model

AWS is responsible for security **of** the cloud, including the underlying infrastructure, data centers, and physical hardware. Customers are responsible for security **in** the cloud, including configuring permissions, protecting data, managing access, and securing applications.

Using a managed service does not remove the need to configure security appropriately.

### IAM and least privilege

AWS Identity and Access Management (IAM) controls who can access AWS resources and which actions they can perform.

The principle of least privilege means giving users and applications only the permissions they need. For example, a data scientist may need permissions to prepare datasets and train models, while a production deployment role may only need permission to deploy approved models.

SageMaker Role Manager can help define permissions for different machine learning personas and responsibilities. Prefer temporary credentials and centralized identity management over shared long-term access keys.

### Network isolation

SageMaker supports network isolation for certain training and inference containers. When enabled, these containers cannot make network calls, including calls to Amazon S3. This can be useful for sensitive workloads, but required inputs and outputs must be planned for because the containers cannot access other network resources.

Private networking, VPC configurations, security groups, and controlled network access are also important considerations.

### Data protection

Machine learning datasets can contain sensitive or confidential information. AWS encryption capabilities can protect data at rest and in transit. AWS Key Management Service (KMS) helps organizations manage encryption keys.

Good data-protection practices also include restricting dataset access, avoiding unnecessary storage of sensitive information, and following organizational data-handling policies.

### Model governance

- **SageMaker Model Cards** document information such as a model's intended use, training details, limitations, and potential risks.
- **SageMaker Model Registry** provides a centralized location to manage model versions, metadata, and approval statuses. Approval workflows can help ensure that a model is reviewed before production deployment.
- **SageMaker Model Dashboard** provides a centralized view of models and associated information, including monitoring information.

## 5. Monitoring and Responsible AI

### SageMaker Model Monitor

Model performance can change after deployment because the data supplied to a model may change over time. SageMaker Model Monitor can monitor aspects of model performance, including data quality, model quality, and bias.

Teams can establish baselines and configure monitoring schedules to identify changes that exceed acceptable thresholds. For example, a bank's loan-application model may be affected by changes in customer behavior or economic conditions. Monitoring can help identify changes that require investigation, updated data, retraining, and evaluation before a replacement model is deployed.

Monitoring should be continuous, not limited to the development stage.

### Bias and explainability with SageMaker Clarify

A model can perform well overall while producing poorer results for certain groups. SageMaker Clarify helps examine potential bias in datasets and models using statistical metrics. It also supports explainability by helping identify which features influenced a prediction.

These capabilities are especially important in high-impact areas such as lending, hiring, and healthcare. Automated metrics should not replace human review. Statistical evaluation, domain expertise, and human oversight should be considered together.

## 6. Production Best Practices

Practical considerations for production machine learning workloads include:

1. Apply least-privilege IAM permissions.
2. Encrypt sensitive data and use appropriate network protections.
3. Validate datasets before training and monitor data quality after deployment.
4. Use Model Registry and approval workflows to control which models reach production.
5. Document intended use, limitations, and risks with Model Cards.
6. Monitor model quality, bias, and explainability throughout the model lifecycle.
7. Use automated pipelines and repeatable processes for consistent development and deployment.
8. Monitor infrastructure costs, prediction latency, and resource utilization.

A model that performs well technically may still be unsuitable for production if it is too expensive or too slow for the application.

## Key Takeaway

Amazon SageMaker AI supports much more than model training. It provides capabilities for managing the broader machine learning lifecycle: preparing data, managing features, training and tuning models, deploying inference endpoints or jobs, automating workflows, securing resources, governing model versions, and monitoring performance.

A successful machine learning solution requires attention to the model itself as well as the data, infrastructure, security, governance, and ongoing operational processes around it.

## Topics and AWS Services

- Amazon SageMaker AI
- SageMaker Studio
- SageMaker Data Wrangler
- SageMaker Feature Store
- SageMaker Clarify
- SageMaker Ground Truth
- SageMaker JumpStart
- SageMaker Canvas and Autopilot
- SageMaker Automatic Model Tuning
- SageMaker real-time, serverless, asynchronous, and batch inference
- SageMaker Pipelines
- MLflow experiment tracking
- SageMaker Model Cards, Model Registry, Model Dashboard, and Model Monitor
- AWS IAM, VPC, and KMS
- Amazon Rekognition, Comprehend, and Textract

## Youtube
- Video 1: https://youtu.be/BQ_wcpjXhuM?si=5d_QgJfUIOn19lL_
- Video 2: https://youtu.be/mFLswHhy1do?si=Cbc0qRqtC39CqwzD
- Video 3: https://youtu.be/b5ucAeg4YEc?si=lQ4H6YyIc9Uew1xM

## LinkedIn
- Post 1: https://lnkd.in/p/g28vcgYF
- Post 2: https://lnkd.in/p/gQ6sKrkK
- Post 3: https://lnkd.in/p/gy7JrC-e
