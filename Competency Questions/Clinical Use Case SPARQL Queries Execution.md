# Competency Questions

The competency questions define the information requirements that guided the development of the Automation Bias Ontology (ABO).

The questions are grouped into three categories:
- AI Model and System
- Environmental Context
- Agent Profile

# AI Model and System

## CQ1: What AI output was provided to the human operator as part of the human oversight process?

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


## CQ2: What AI model version produced the AI output associated with this human oversight process?

**Key Concepts**: `abo:AIOutput`, `AIModel`, `HumanOversightActivity`

```sparql
PREFIX abo:  <https://w3id.org/abo#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>

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

## CQ3: What explanation was provided by the AI system for this AI output during this human oversight process?

**Key Concepts**: `AIExplanation`, `AIOutput`, `HumanOversightActivity`

```sparql
PREFIX abo:  <https://w3id.org/abo#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>
SELECT ?activity ?explanation WHERE {
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?task abo:hadTaskInformation ?exp .
  ?exp a abo:AIExplanation ;
       abo:hasExplanation ?explanation .
}

```
| activity | explanation |
| :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "SHAP" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "SHAP" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "SHAP" |


## CQ4: What were the performance metrics of the AI model when the human operator had the same output as the AI model during a human oversight process?

**Key Concept:*
```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#](http://www.w3.org/ns/prov#)
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#](http://www.w3.org/2001/XMLSchema#)

SELECT ?activity ?operatorDecision ?aiRecommendation ?metricName ?metricValue ?metricUnit WHERE {
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?result a abo:OversightResult ;
          prov:wasGeneratedBy ?activity ;
          abo:hasOversightDecision ?operatorDecision .
  ?task abo:hadTaskInformation ?rec .
  ?rec a abo:AIRecommendation ;
       abo:hasRecommendationValue ?aiRecommendation ;
       prov:wasAttributedTo ?model .
  FILTER(?operatorDecision = ?aiRecommendation)
  ?model a abo:AIModel ;
         abo:hadPerformanceMetrics ?metric .
  ?metric abo:hasMetricName ?metricName ;
          abo:hasMetricValue ?metricValue .
  OPTIONAL { ?metric abo:hasMetricUnit ?metricUnit . }
}
```
| activity | operatorDecision | aiRecommendation | metricName | metricValue | metricUnit |
| :--- | :--- | :--- | :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "BI RADS 1" | "BI RADS 1" | "AUC-ROC (pre-deployment)" | "0.94"^^xsd:float | "score" |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "BI RADS 1" | "BI RADS 1" | "Interval cancer rate (post-deployment)" | "4.0"^^xsd:float | |

# Environmental Context

## CQ5.  What was this operator’s workload at the time of this human oversight decision? 

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#](http://www.w3.org/ns/prov#)
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#](http://www.w3.org/2001/XMLSchema#)

SELECT ?activity ?measurementName ?measurementValue ?measurementUnit WHERE {
  ?activity a abo:HumanOversightActivity ;
            prov:qualifiedAssociation ?context .
  ?context abo:hadEnvironment ?workload .
  ?workload abo:hasMeasurementName ?measurementName ;
            abo:hasMeasurementValue ?measurementValue ;
            abo:hasMeasurementUnit ?measurementUnit .
}
```
| activity | measurementName | measurementValue | measurementUnit |
| :--- | :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "TTR reduction" | "40.0"^^xsd:float | "%" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "TTR reduction" | "5.0"^^xsd:float | "%" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "TTR increase" | "20.0"^^xsd:float | "%" |

## CQ6: What information artifacts were accessed by the human operator, and what interaction evidence was recorded to support this during this human oversight process?


## CQ7: What was the complexity of the task during this human oversight process?

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#](http://www.w3.org/ns/prov#)
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#](http://www.w3.org/2001/XMLSchema#)

SELECT ?activity ?taskComplexityValue ?taskComplexityMeasurementUnit WHERE {
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?task abo:hadTaskComplexity ?complexity .
  ?complexity abo:hasTaskComplexityValue ?taskComplexityValue ;
              abo:hasTaskComplexityMeasurementUnit ?taskComplexityMeasurementUnit .
}
```
| activity | taskComplexityValue | taskComplexityMeasurementUnit |
| :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "D" | "BI RADS" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "D" | "BI RADS" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "D" | "BI RADS" |

## CQ8: Which human operators were associated with this task?

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#](http://www.w3.org/ns/prov#)
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#](http://www.w3.org/2001/XMLSchema#)

SELECT DISTINCT ?taskID ?operator ?role WHERE {
  ?task a abo:AISystemTask ;
        abo:hasTaskID ?taskID .
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task ;
            prov:qualifiedAssociation ?context .
  ?context prov:agent ?operator ;
           abo:hadProfile ?profile .
  ?profile abo:hasRole ?role .
}
```
| taskID | operator | role |
| :--- | :--- | :--- |
| "Alice123" | [https://w3id.org/abo#Operator_DrBob](https://w3id.org/abo#Operator_DrBob) | "Junior Radiologist" |
| "Alice123" | [https://w3id.org/abo#Operator_DrJay](https://w3id.org/abo#Operator_DrJay) | "Senior Radiologist" |


# Agent Profile

## CQ9: What was the human oversight decision of th3 human operator for this AI output?
```sparql
PREFIX abo:  [https://w3id.org/abo#](https://w3id.org/abo#)
PREFIX prov: [http://www.w3.org/ns/prov#](http://www.w3.org/ns/prov#)
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#](http://www.w3.org/2000/01/rdf-schema#)
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#](http://www.w3.org/2001/XMLSchema#)

SELECT ?activity ?oversightDecision ?oversightDecisionNote WHERE {
  ?result a abo:OversightResult ;
          prov:wasGeneratedBy ?activity ;
          abo:hasOversightDecision ?oversightDecision .
  OPTIONAL { ?result abo:hasOversightDecisionNote ?oversightDecisionNote . }
}
```
| activity | oversightDecision | oversightDecisionNote |
| :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "BI RADS 1" | "Return for scan after a year" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "BI RADS 4" | "Stereotactic biopsy recommended" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "BI RADS 4" | "Stereotactic biopsy recommended" |

## CQ10:  
