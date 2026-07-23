# Competency Questions

The competency questions define the information requirements that guided the development of the Automation Bias Ontology (ABO).

The questions are grouped into three categories:
- AI Model and System
- Environmental Context
- Agent Profile


## CQ1. What AI output was provided to the human operator as part of the human oversight process?

**Key concepts:** `abo:HumanOversightActivity` , `abo:AISystemTask` , `abo:HumanOperator` , `abo:AIOutput` , `abo:AIRecommendation` , `abo:AIExplanation` ,`abo:InformationArtifact`


```sparql
PREFIX abo:  <https://w3id.org/abo#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

SELECT ?activity ?recommendationValue ?explanation WHERE {
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?task abo:hadTaskInformation ?rec .
  ?rec a abo:AIRecommendation ;
       abo:hasRecommendationValue ?recommendationValue .
  ?task abo:hadTaskInformation ?exp .
  ?exp a abo:AIExplanation ;
       abo:hasExplanation ?explanation .
}
```

| activity | recommendationValue | explanation |
| :--- | :--- | :--- |
| <https://w3id.org/abo#OversightActivity_Case1> | "BI RADS 1" | "SHAP" |
| <https://w3id.org/abo#OversightActivity_Case2> | "BI RADS 1" | "SHAP" |
| <https://w3id.org/abo#OversightActivity_Case3> | "BI RADS 1" | "SHAP" |


## CQ2. What AI model version produced the AI output associated with this human oversight process?

**Key Concepts**: `abo:AIOutput`, `AIModel`, `HumanOversightActivity`

```sparql
SELECT DISTINCT ?activity ?modelName ?modelVersion WHERE {
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?task abo:hadTaskInformation ?output .
  ?output prov:wasAttributedTo ?model .
  ?model a abo:AIModel ;
         abo:hasModelName ?modelName ;
         abo:hasModelVersion ?modelVersion .
}

```
| activity | modelName | modelVersion |
| :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "V1" | "V1" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "V1" | "V1" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "V1" | "V1" |
