---
layout: post
title: "Seldon Webinar Notes - ML Explainability"
author: "Daniel E"
categories: notes
tags: [notes, data-science, machine-teaching, statistical, explainability, interpretability]
image: expl2.jpg
---

In a bid to stay up-to-date with the latest developments in the field of AI, I attended a webinar organised by [Seldon](https://www.seldon.io/), where they discussed explainability as a requirement in machine learning that is of growing importance.

### Keywords
notes, data-science, machine-teaching, statistical, explainability, interpretability

# Seldon's Webinar on Machine Learning Interpretability and Explainability
In this article I refined my webinar notes into a piece of writing that I and others can revisit. 

_Often used to gain a competitive advantage and unlock business potential, machine learning helps organisations scale and run processes autonomously, however there is always room for potential errors. Algorithms are complicated by nature and in some cases “what” isn’t enough - you need to understand why models behave the way they do and their biases, particularly in highly regulated industries._

In this webinar I joined Janis Klaise, Head of Data Science Engineering at Seldon and Ed Shee as they discussed the foundations of model explainability, why model explainability is mandatory for machine learning in production, how model explanations help address bias, and methodologies for interpreting AI/Machine Learning driven processes.
Here's what I learnt:

#Summary
Explainability is a very difficult topic, with an inexact definition.
The problem in question is developing frameworks and methods for engineers to understand how a ML model works under the hood when it's making predictions, what's driving its decisions.
Explainability depends on type of model and type of data. 

ML has become much more empirically driven, harder problems have arisen as time passed (eg speech recognition). Before machine learning we had statistical methods and stats are not enough for lots of nonlinear relationships. From an explainability point of view stats are quite intuitive, you can probably extrapolate how a statistical model reaches a certain conclusion.

But it becomes an especially tricky issue with a learning model like a neural network because once trained it’s like a black box. Once again we're interested in finding out is why a given model has made a set prediction on a set datapoint. What if there's a subset of features that are not visible to the human eye? Ideally we'll run an explainability method and it will tell us how the model decided on whether the cell is cancerous or not, in a medical use case. And this problem is becoming more and more prominent as technology advances. 

In an insurance use case, an explainer method would automatically detect variables like "car_parked_on_drive" (meaning chance of it getting broken into is lower) as being instrumental in predicting an accurate premium (lower in this case).

To build on the medical use case, in medical imaging explainability is critical because it’s a matter of life and death. A doctor may need to know how an outcome was reached on say calling a cell cancerous or not, and the reasoning behind it. Finding that out can be used to in turn train doctors to recognise ML detected parts, and if a certain detections is frequent, maybe a doctor can learn that and further refine the learning model.

"Explainer methods could also detect artefacts in your data/features that are negatively impacting your results. In only a subset of use cases will features be human interpretable because learning models are better than humans at making at use non-interpretable features. 
You may also learn what information you need to consider or test further. An explainer method can show you what useful features from a dataset can be best used in training, and that may go against what you initially thought was the best choice. It may occur that the model is correct and your assumptions in feature selections were incorrect (i.e. due to human bias).


# Q&A
Explainer algorithms depend on algorithms and data. One may be able to use them on classifiers but not regressors. You have black box models (you give it data and you get predictions – production API exposed to world) and white/grey box models (you can look at implementation, or TF gradient propagation thru TF graph thanks to TensorFlow)
**Daniel**: "Wouldn't features in a ConvNet become less and less human readable as the images progress through the layers? When Janis mentioned observing a model picking up on an unexpected area of an image, can you really code an explainer algorithm that detects that definitely? Or just narrow down?"
**Janis**: "Issue is interpretation not coding the explainer method. But yeah it’s a limitation. You’ll have to combine data issues with what you get from explainer method and come up with a conclusion yourself. LIME is an early explanation algorithm. Black box, perturbation based method. Statistical on how responses vary. What model responses around datapoint and give feature importances to dataset. If prediction on decision boundary, explanation may be a little bit unintuitive. LIME is no longer viable. Given the advanced problems learning algorithms face now.

**Other attendee**: Unsupervised learning good practices of Explainability?
**Janis**: "Broad question, depends on task. Clustering, why are these datapoints assigned to these clusters and not these other ones. Not explored in literature. Classification, regression, maybe some structured outputs (text generation) are main ones that look at Explainability. Watch that space."


**Daniel**: "Wouldn't features in a ConvNet become less and less human readable as the images progress through the layers? When Janis mentioned observing a model picking up on an unexpected area of an image, can you really code an explainer algorithm that detects that definitely? Or just narrow down?"

**Janis**: "Issue is interpretation not coding the explainer method. But yeah it’s a limitation. You’ll have to combine data issues with what you get from explainer method and come up with a conclusion yourself. LIME is an early explanation algorithm. Black box, perturbation based method. Statistical on how responses vary. What model responses around datapoint and give feature importances to dataset. If prediction on decision boundary, explanation may be a little bit unintuitive. LIME is no longer viable. Given the advanced problems learning algorithms face now.

**Other attendee**: Unsupervised learning good practices of Explainability?

**Janis**: "Broad question, depends on task. Clustering, why are these datapoints assigned to these clusters and not these other ones. Not explored in literature. Classification, regression, maybe some structured outputs (text generation) are main ones that look at Explainability. Watch that space."

**Daniel**: "I am looking to contribute to my first open-source project! Is Alibi a good place to start?" 

**Janis**:  "On Alibi contribution. First issue tag (where bugs are grouped) hasn’t been used extensively, and issues that open up are not easy to deal with. Alibi has 3 libraries: Explain, Detect and Monitoring if you'd like to contribute"

## Uncategorised notes
In a business setting one needs to interpret ML algs to stakeholders with dashboards. One way could be visualisation techniques.

Transformer based models and NLP and other deep Learning support by Alibi  - blackbox (Python function, alg, model including deep learning) – you only forward call method to explainer algorithms. Anchor (?)  - Images, tabular datasets

Whitebox methods expect a type of dataset. E.g. Requires gradients for e.g. or sumn, can only take PyTorch cos they allow that peek inside. Alibi have an enterprise deployment platform if you don’t want to implement things yourself.

Sound, audio classifier for explain method? Sound data tools, timeseries data in raw form. Timeseries explanations is new in academia/literature. Easiest cases first. Image recognition, tabular data, text recognition are basic and then edge cases like sound/timeseries detection.

Linear models need statistical – exploratory data analysis to get rid of corellations that may affect outputs. But in general marginal distributions of features, that reveals issues immediately. Several columns are constant, theres no predictive power to these, and can also influence my model in a bad way. Causality feature selection scholar.google.com. Feature selection is a big field. Are your own biases correct. Tempting to include features in dataset because you assume they may be good.

Fairness library : scikit.lego  - building blocks of skl pipeline, has interesting fair learning/feature selection implementations. So for car insurance you cant use gender anymore as predictor but there may be other features that together can determine the gender. So get rid of them all.


## Learn more:
*[Alibi Explain library (open source drift detection algorithms)](https://go.seldon.io/e/702803/SeldonIO-alibi/9lnw4/404447027?h=3neI9R25ICFgCco2L9Ue1V4PtIdMOvBbMkokDZW1ToU): Library for explainability at Seldon started as research piece code. Now getting a lot of interest in industry. Now wrapped with an interface. Scikitlearn like. Extensive documentation. You choose a scenario and youm may or may not have to fit it w a dataset. Then simply hit explain.
*[Seldon Deploy (enterprise model deployment, management and explainability)](https://go.seldon.io/e/702803/seldon-deploy/9lnw6/404447027?h=3neI9R25ICFgCco2L9Ue1V4PtIdMOvBbMkokDZW1ToU)
*[Research paper: Monitoring and explainability of models in production](https://go.seldon.io/e/702803/ility-of-models-in-production-/9lnw8/404447027?h=3neI9R25ICFgCco2L9Ue1V4PtIdMOvBbMkokDZW1ToU)

*[LIME Local Interpretable Model-agnostic Explanations](https://towardsdatascience.com/lime-explain-machine-learning-predictions-af8f18189bfe)