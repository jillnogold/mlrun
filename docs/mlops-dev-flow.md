(mlops-dev-flow)=
# MLOps development workflow <!-- omit in toc -->

ML applications require you to implement the following stages in a scalable and reproducible way:

1. [Ingest and process data](#ingest)
2. [Develop and train models](#develop)
2. [Deploy models and applications](#deploy)
2. [Monitor and alert](#monitor)

MLRun automates the MLOps work. It simplifies & accelerates the time to production

<a id="ingest"></a>
## Ingest and process data

There is no ML without data. Before everything else, ML teams need access to historical and/or online data from multiple sources, and they 
must catalog and organize the data in a way that allows for simple and fast analysis (for example, by storing data in columnar data 
structures, such as Parquet).

In most cases, the raw data cannot be used as-is for machine learning algorithms for various reasons such as:

- The data is low quality (missing fields, null values, etc.) and requires cleaning and imputing.
- The data needs to be converted to numerical or categorical values which can be processed by algorithms.
- The data is unstructured in text, json, image, or audio formats, and needs to be converted to tabular or vector formats.
- The data needs to be grouped or aggregated to make it meaningful.
- The data is encoded or requires joins with reference information.
- The ML process starts with manual exploratory data analysis and feature engineering on small data extractions. In order to bring accurate models into production, ML teams must work on larger datasets and automate the process of collecting and preparing the data.

Furthermore, batch collection and preparation methodologies such as ETL, SQL queries, and batch analytics don’t work well for operational or 
real-time pipelines. As a result, ML teams often build separate data pipelines which use stream processing, NoSQL, and containerized micro-
services. 80% of data today is unstructured, so an essential part of building operational data pipelines is to convert unstructured textual, 
audio, and visual data into machine learning- or deep learning-friendly data organization.

<img src="_static/images/data-collect-and-prep.png" alt="data-collection-and-preparation" width="800" /><br>

MLOps solutions should incorporate a [feature store](./feature-store/feature-store.html) that defines the data collection and transformations 
just once for both batch and real-time scenarios, processes features automatically without manual involvement, and serves the features from 
a shared catalog to training, serving, and data governance applications. Feature stores must also extend beyond traditional analytics and 
enable advanced transformations on unstructured data and complex layouts.

<a id="develop"></a>
## Develop and train models
Whether it’s deep learning or machine learning, MLRun allows you to train your models at scale and capture all the relevant metadata for experiments tracking and lineage. 

With MLOps, ML teams build machine learning pipelines that automatically collect and prepare data, select optimal features, run training 
using different parameter sets or algorithms, evaluate models, and run various model and system tests. All the executions, along with their 
data, metadata, code and results must be versioned and logged, providing quick results visualization, to compare them with past results and 
understand which data was used to produce each model.

Pipelines can be more complex—for example, when ML teams need to develop a combination of models, or use Deep Learning or NLP.

<img src="_static/images/model-dev.png" alt="training" width="800" /><br>


ML pipelines can be triggered manually, or preferably triggered automatically when:

- The code, packages or parameters change
- The input data or feature engineering logic changes
- Concept drift is detected, and the model needs to be re-trained with fresh data

ML pipelines:

- Are built using micro-services (containers or serverless functions), usually over Kubernetes.
- Have all their inputs (code, package dependencies, data, parameters) and the outputs (logs, metrics, data/features, artifacts, models) tracked for every step in the pipeline, in order to reproduce and/or explain the experiment results.
- Use versioning for all the data and artifacts used throughout the pipeline.
- Store code and configuration in versioned Git repositories.
- Use Continuous Integration (CI) techniques to automate the pipeline initiation, test automation, and for the review and approval process.

Pipelines should be executed over scalable services or functions, which can span elastically over multiple servers or containers. This way, 
jobs complete faster, and computation resources are freed up once they complete, saving significant costs.

The resulting models are stored in a versioned model repository along with metadata, performance metrics, required parameters, statistical 
information, etc. Models can be loaded later into batch or real-time serving micro-services or functions.

<a id="deploy"></a>
## Deploy models and applications
With MLRun, in addition to a batch inference, you can deploy a robust and scalable {ref}`real-time pipeline <serving-graph>` for more complex and online scenarios. 
MLRun uses Nuclio, an open source serverless framework for creating real-time pipelines for {ref}`model deployment <deployment>`.

Once an ML model has been built, it needs to be integrated with real-world data and the business application or front-end services. The 
entire application, or parts thereof, need to be deployed without disrupting the service. Deployment can be extremely challenging if the ML 
components aren’t treated as an integral part of the application or production pipeline.

Production pipelines usually consist of:

- Real-time data collection, validation, and feature engineering logic
- One or more model serving services
- API services and/or application integration logic
- Data and model monitoring services
- Resource monitoring and alerting services
- Event, telemetry, and data/features logging services

The different services are interdependent. For example, if the inputs to a model change, the feature engineering logic must be upgraded 
along with the model serving and model monitoring services. These dependencies require online production pipelines (graphs) to reflect these 
changes.

<img src="_static/images/build-online-ml-services.png" alt="building-online-ml-services" width="800" /><br>

Production pipelines can be more complex when using unstructured data, deep learning, NLP or model ensembles, so having flexible mechanisms 
to build and wire up the pipeline graphs is critical.

Production pipelines are usually interconnected with fast streaming or messaging protocols, so they should be elastic to address traffic and 
demand fluctuations, and they should allow non-disruptive upgrades to one or more elements of the pipeline. These requirements are best 
addressed with fast serverless technologies.

Production pipeline development and deployment flow:

1. Develop production components:
   - API services and application integration logic
   - Feature collection, validation, and transformation
   - Model serving graphs
2. Test online pipelines with simulated data
2. Deploy online pipelines to production
2. Monitor models and data and detect drift
2. Retrain models and re-engineer data when needed
2. Upgrade pipeline components (non-disruptively) when needed

<a id="monitor"></a>
## Monitor and alert

Once the model is deployed, use MLRun to track the [operational statistics](./monitoring/model-monitoring-deployment.html#architecture) as well as [identify drift](./monitoring/model-monitoring-deployment.html#drift-analysis).
When drift is identified, MLRun can trigger the training pipeline to train a new model.

AI services and applications are becoming an essential part of any business. This trend brings with it liabilities, which drive further 
complexity. ML teams need to add data, code and experiment tracking, monitor data to detect quality problems, and [monitor models](./monitoring/index.html) to detect concept drift and improve model accuracy through the use of AutoML techniques and ensembles, and so on.

Nothing lasts forever, not even carefully constructed models that have been trained using mountains of well-labeled data. ML teams need to 
react quickly to adapt to constantly changing patterns in real-world data. Monitoring machine learning models is a core component of MLOps 
to keep deployed models current and predicting with the utmost accuracy, and to ensure they deliver value long-term.

## MLOps tasks

`````{div} 

````{grid} 4 
:gutter: 2

```{grid-item-card} Project management and CI/CD automation
:columns: 12
:text-align: center
:link: ./projects/project.html
```

```{grid-item-card} Ingest and process data
:text-align: center
:link: ./data-prep/index.html
```

```{grid-item-card} Develop and train models 
:text-align: center
:link: ./development/index.html
```

```{grid-item-card} Deploy models and apps
:text-align: center
:link: ./deployment/index.html
```

```{grid-item-card} Monitor and alert
:text-align: center
:link: ./monitoring/index.html
```

````

`````

The [**MLOps development workflow**](./mlops-dev-flow.html) section describes the different tasks and stages in detail.
MLRun can be used to automate and orchestrate all the different tasks or just specific tasks (and integrate them with what you have already deployed).

### Project management and CI/CD automation

In MLRun the assets, metadata, and services (data, functions, jobs, artifacts, models, secrets, etc.) are organized into projects.
Projects can be imported/exported as a whole, mapped to git repositories or IDE projects (in PyCharm, VSCode, etc.), which enables versioning, collaboration, and CI/CD. 
Project access can be restricted to a set of users and roles.
{bdg-link-primary-line}`more... <./projects/project.html>`

`````{div} full-width
{octicon}`mortar-board` **Docs:**
{bdg-link-info}`Projects and automation <./projects/project.html>`
{bdg-link-info}`CI/CD integration <./projects/ci-integration.html>`
<br> {octicon}`code-square` **Tutorials:**
{bdg-link-primary}`Quick start <./tutorials/01-mlrun-basics.html>`
{bdg-link-primary}`Automated ML pipeline <./tutorials/04-pipeline.html>`
<br> {octicon}`video` **Videos:**
{bdg-link-warning}`Quick start <https://youtu.be/xI8KVGLlj7Q>`
`````

### Ingest and process data

MLRun provides abstract interfaces to various offline and online [**data sources**](./store/datastore.html), supports batch or realtime data processing at scale, data lineage and versioning, structured and unstructured data, and more. 
In addition, the MLRun [**Feature store**](./feature-store/feature-store.html) automates the collection, transformation, storage, catalog, serving, and monitoring of data features across the ML lifecycle and enables feature reuse and sharing.
{bdg-link-primary-line}`more... <./data-prep/index.html>`

`````{div} full-width
{octicon}`mortar-board` **Docs:**
{bdg-link-info}`Ingest and process data <ingesting-process-data>`
{bdg-link-info}`Feature store <./feature-store/feature-store.html>`
{bdg-link-info}`Data and artifacts <./concepts/data.html>`
<br> {octicon}`code-square` **Tutorials:**
{bdg-link-primary}`Quick start <./tutorials/01-mlrun-basics.html>`
{bdg-link-primary}`Feature store <./feature-store/basic-demo.html>`
`````

### Develop and train models

MLRun allows you to easily build ML pipelines that take data from various sources or the Feature Store and process it, train models at scale with multiple parameters, test models, track each experiment, and register, version and deploy models, etc. MLRun provides scalable built-in or custom model training services that integrate with any framework and can work with 3rd party training/auto-ML services. You can also bring your own pre-trained model and use it in the pipeline.
{bdg-link-primary-line}`more... <./development/index.html>`

`````{div} full-width
{octicon}`mortar-board` **Docs:**
{bdg-link-info}`Develop and train models <development>`
{bdg-link-info}`Model training and tracking <./development/model-training-tracking.html>`
{bdg-link-info}`Batch runs and workflows <./concepts/runs-workflows.html>`
<br> {octicon}`code-square` **Tutorials:**
{bdg-link-primary}`Train, compare, and register models <./tutorials/02-model-training.html>`
{bdg-link-primary}`Automated ML pipeline <./tutorials/04-pipeline.html>`
<br> {octicon}`video` **Videos:**
{bdg-link-warning}`Train and compare models <https://youtu.be/bZgBsmLMdQo>`
`````

### Deploy models and applications

MLRun rapidly deploys and manages production-grade real-time or batch application pipelines using elastic and resilient serverless functions. MLRun addresses the entire ML application: intercepting application/user requests, running data processing tasks, inferencing using one or more models, driving actions, and integrating with the application logic.
{bdg-link-primary-line}`more... <./deployment/index.html>`

`````{div} full-width
{octicon}`mortar-board` **Docs:**
{bdg-link-info}`Deploy models and applications <deployment>`
{bdg-link-info}`Realtime pipelines <./serving/serving-graph.html>`
{bdg-link-info}`Batch inference <./deployment/batch_inference.html>`
<br> {octicon}`code-square` **Tutorials:**
{bdg-link-primary}`Realtime serving <./tutorials/03-model-serving.html>`
{bdg-link-primary}`Batch inference <./tutorials/07-batch-infer.html>`
{bdg-link-primary}`Advanced pipeline <./tutorials/07-batch-infer.html>`
<br> {octicon}`video` **Videos:**
{bdg-link-warning}`Serve pre-trained models <https://youtu.be/OUjOus4dZfw>`
`````

### Monitor and alert

Observability is built into the different MLRun objects (data, functions, jobs, models, pipelines, etc.), eliminating the need for complex integrations and code instrumentation. With MLRun, you can observe the application/model resource usage and model behavior (drift, performance, etc.), define custom app metrics, and trigger alerts or retraining jobs.
{bdg-link-primary-line}`more... <./monitoring/index.html>`

`````{div} full-width
{octicon}`mortar-board` **Docs:**
{bdg-link-info}`Monitor and alert <monitoring>`
{bdg-link-info}`Model monitoring overview <./monitoring/model-monitoring-deployment.html>`
<br> {octicon}`code-square` **Tutorials:**
{bdg-link-primary}`Model monitoring and drift detection <./tutorials/05-model-monitoring.html>`
`````

<a id="core-components"></a>
## MLRun core components

MLRun includes the following major components:

````{grid} 6
:gutter: 2

```{grid-item-card} Project management & automation (SDK, API, etc.)
:columns: 12
:text-align: center
:link: ./projects/project.html
```

```{grid-item-card} Serverless functions
:columns: 6 4 4 2
:text-align: center
:link: ./runtimes/functions.html
```

```{grid-item-card} Data & artifacts
:columns: 6 4 4 2
:text-align: center
:link: ./concepts/data.html
```

```{grid-item-card} Feature store
:columns: 6 4 4 2
:text-align: center
:link: ./feature-store/feature-store.html
```

```{grid-item-card} Batch runs & workflows 
:columns: 6 4 4 2
:text-align: center
:link: ./concepts/runs-workflows.html
```

```{grid-item-card} Real-time pipelines
:columns: 6 4 4 2
:text-align: center
:link: ./serving/serving-graph.html
```

```{grid-item-card} Monitoring
:columns: 6 4 4 2
:text-align: center
:link: ./monitoring/index.html
```

````

**{ref}`Project management <projects>`:** A service (API, SDK, DB, UI) that manages the different project assets (data, functions, jobs, workflows, secrets, etc.) and provides central control and metadata layer.  

**{ref}`Serverless functions <Functions>`:** An automatically deployed software package with one or more methods and runtime-specific attributes (such as image, libraries, command, arguments, resources, etc.).

**{ref}`Data and artifacts <data-feature-store>`:** Glueless connectivity to various data sources, metadata management, catalog, and versioning for structured/unstructured artifacts.

**{ref}`Feature store <feature-store>`:** Automatically collects, prepares, catalogs, and serves production data features for development (offline) and real-time (online) deployment using minimal engineering effort.

**{ref}`Batch Runs and workflows <workflows>`:** Execute one or more functions with specific parameters and collect, track, and compare all their results and artifacts.

**{ref}`Real-time serving pipeline <serving-graph>`:** Rapid deployment of scalable data and ML pipelines using real-time serverless technology, including API handling, data preparation/enrichment, model serving, ensembles, driving and measuring actions, etc.

**{ref}`Real-time monitoring <monitoring>`:** Monitors data, models, resources, and production components and provides a feedback loop for exploring production data, identifying drift, alerting on anomalies or data quality issues, triggering retraining jobs, measuring business impact, etc.

