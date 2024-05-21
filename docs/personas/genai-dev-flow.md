(genai-dev-flow)=

# GenAI development workflow

The general stages of developing and scaling a GenAI model across an enterprise entails:
1. Leverage a pre-trained model
2. Personalize the model with your pre-trained data
2. Implement measures to reduce risk: data quality (preparing data before the training), testing (ensure the model is 
doing exactly what it should be doing), guard rails (answers are correct, not toxic, etc.), human feedback (examine it and return the model )
2. Build a scalable, automated, and continuous development environment

It's important to keep your design flexible. New solutions come out all the time, and you may find a better one in the near future.

These general stages are described in the following figure:

<img src="../_static/images/genai-flow.png" width="800" >

Two pipelines:
- Build and tune the model
take data, clean it, 
Take base model, add layers for your data, results in tuned model
Evaluate model to make sure it's doing the right thing
extensive testing
Deploy using automated deployment

- Realtime application pipeline
receives data from API/bot
contextualizes it with a state, possible vectors, doc to examine etc
prompt engineering - tuning the question to the model so it behaves exactly as you want
Feed it into the foundation model which gives result. The result is not necessarily the final result. Need to clean, format, check for toxic material, intellectual property
Monitor results in a monitoring system, examine behavior, label results, put a feedback group

MLRUN automates the flow of tuning, validation, optimizing the LLM to specific data - efficiently over elastic resources
Rapidly deploys scalable real-time serving and application pipelines that host the LLM as well as the data integration and business logic
Built-in monitoring for the LLM data, trining, model, and resources, with automated model retraining
Open solution support for various LLMs and flexible deployment options (any cloud, on-prem)

GenAI tracks:
- text
- images
- audio
- video

Important to evaluate
- Resources
- Many models in the function hub. How to choose your model? Sizing? Use case?

Spot instances GPI

How to work with vector DB

training/fine tuninghugging face