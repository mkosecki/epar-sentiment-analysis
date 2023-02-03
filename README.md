<!-- PROJECT LOGO -->
<br />
<div align="center">


  <h3 align="center">Epar Bayer Problem - Sentiment analysis</h3>

  <p align="center">
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#questions-and-answers">Questions and answers</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#sources">Sources</a></li>
  </ol>
</details>

## Questions and answers

<p>

**Is the dataset balanced?**

No, the dataset is not balanced well. It is shown that Positive category occurs more frequent than Negative and Neutral.

Positive: 160
Negative 36
Neutral: 70

Lenghts of sentences are quite different with max char length of 431, min char length 39 and the average of approx. 161 length.

**Is the amount of data sufficient for allowing a hold-out dataset?**

Well, not really. I don't think such amount of data could be representative for the given problem. However as always it depends. If we have pretrained model on simalar task with the same kind of data, and we're in a hurry. The model may (assumption) consist on, within its space, some information to finetune it on this amount of data. After all it is better to have much more data if the hold-out dataset is something we want to have.

**Do you have enough data to consider deep neural architectures or might good feature engineering with more shallow models suffice?**

For the normal training, such amount of data is not sufficient. For finetuning, maybe ... but the model probabily need to first finetuned on some task with this kind of data. With such small portion of data I would normally consider going machine learning approach with usage of feature engineering. But for some rapid check I will go with DL.

**During the data collection process, for some sentences multiple experts disagreed on the sentiment of a given sentence, how could you capture such an ambiguity in your model and potentially \notify users about such unclear instances?**

We can inform users about the sentiment scores assigned to each class by model. If the model calculate similar scores (we can set some threshold value ie. S(Positive) - S(Negative) < Threshold) to each category for the given sentence.

**Think beyond the pure sentiment analysis of sentences, e.g. how would you automatically extract relevant sentencesfrom EPARs and ensure that the analysis is only applied to specific sections? It is worth to explore some EPARs on the EMA website (e.g. consider the ones for Bayer products Eylea1 or Xarelto2).**

If the documents are PDF we can use OCR or othe image analysis approach to extract necessary data. The EPAR documents has specific structure, by verifying the ones that where added to the homework folder I can say that table of contents are mostly the same. If know in which sections are the most relevant data we can focus on it by extracting the nessary information.

**How could an architecture (preferably in the cloud) to industrialize the sentiment analysis model look like?**

It depends if want to customize a lot of things or we want to go with some simplest solutions. We can use AWS Gateway with AWS ECS, K8S Pods and deployed services as containers. Examples, suppose each service is on ECS:
- Front/Backend API Services -> ML Proxy API (FastAPI) Service -> ModelAPI Service -> S3 storage with model registry // scalability handled by AWS
- Front/Backend API Services -> Triton Server Service// scalability handled by AWS, required custom metrics to let LB know how to scale
- Front/Backend API Services -> Proxy API Producer Service -> Broker Service -> Consumer Service with Model // // scalability handled by AWS, need to add custom metric related to Broker queue size for LB


**What could go wrong in the long run once the model is deployed and used on live data?**

Model will become old enough that for the new data will be useless. 
The data will be wrongly classified with high confidence from the model side, so it won't be visible uncertaintity in model results/logs.

**What tools and methodologies would be helpful to deploy and maintain the model in the long run?**

Tools: MLflow, BentoML, Amazon Sagemaker, DVC 
Methodology: Contenarization, CI/CD, A/B tests. We can go with different strategies of how new models should be available for the usage:
- New and old model handle requests but only old send response to the user, New model is under validation
- Similar as above but some samples of the new model responses are sent back to the users
- Deploy new model to services but do not kill old instances before the new ones are registered by LB

</p>

## License

Closed license, only to be used only for Bayer interview purposes of Marcin Kosecki

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Contact

Marcin Kosecki - kosecki.marin.jan@gmail.com

## Sources

* [Sentiment analysis of COVID-19 social media data through machine learning](https://link.springer.com/article/10.1007/s11042-022-13492-w)
* [Epar sentiment github](https://github.com/mkosecki/epar-sentiment)
* [Drug sentiment analysis](https://github.com/mkosecki/drug_sentiment_analysis)
* [Sentiment analysis in healthcare] (https://www.researchgate.net/publication/337653541_Sentiment_Analysis_in_Healthcare_A_Brief_Review)
* [EMA] (https://www.ema.europa.eu/en)
* [Explainability Transfomer] (https://github.com/hila-chefer/Transformer-Explainability)

<p align="right">(<a href="#readme-top">back to top</a>)</p>
