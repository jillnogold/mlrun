(monitoring)=

# Monitor and alert

```{note}
Monitoring is supported by Iguazio's streaming technology, and open-source integration with Kafka.
```

```{note}
This is currently in Tech Preview.
```


By definition, ML models in production make inferences on constantly changing data. Even models that have been trained on massive data sets, 
with the most meticulously labelled data, start to degrade over time, due to concept drift. Changes in the live environment due to 
changing behavioral patterns, seasonal shifts, new regulatory environments, market volatility, etc., can have a big impact on a 
trained model’s ability to make accurate predictions.

Model performance monitoring is a basic operational task that is implemented after an AI model has been deployed. 
Model monitoring is natively built in to the Iguazio MLOps Platform, along with a wide range of 
model management features and ML monitoring reports. It monitors all of your models in a single, simple, dashboard.

Model monitoringdetects concept drift, anomalies, data skew, and model drift in real-time. Even if you are running hundreds of
models simultaneously, you can be sure to spot and remediate the one that has drifted.

The MLRun's model monitoring service includes built-in model monitoring and reporting capability. With monitoring you get
out-of-the-box analysis of:

- **Model performance**: Machine learning models train on data. It is important you know how well they perform in production.
  When you analyze the model performance, it is important you monitor not just the overall model performance, but also the
  feature-level performance. This gives you better insights for the reasons behind a particular result
- **Data drift**: The change in model input data that potentially leads to model performance degradation. There are various
  statistical metrics and drift metrics that you can use to identify data drift.
- **Concept drift**: Applies to the target. Sometimes the statistical properties of the target variable, which the model is
  trying to predict, change over time in unforeseen ways.
- **Automated retraining**: When drift is detected, Iguazio automatically starts the entire training pipeline to retrain the model, including 
  all relevant steps in the pipeline. The output is a production-ready challenger model, ready to be deployed. This automatically 
  keeps your models up to date.
- **Operational performance**: Applies to the overall health of the system: the data (e.g., whether all the
  expected data arrives to the model) as well as the model (e.g., response time, and throughput). 

You have the option to set up notifications on various channels once an issue is detected. For example, you can set-up notification
to your IT via email and Slack when operational performance metrics pass a threshold. You can also set-up automated actions, for example,
call a CI/CD pipeline when data drift is detected and allow a data scientist to review the model with the revised data.

Refer to the [**model monitoring & drift detection tutorial**](../tutorials/05-model-monitoring.html) for an end-to-end example.

**In this section**

```{toctree}
:maxdepth: 1

model-monitoring-deployment
initial-setup-configuration
```
