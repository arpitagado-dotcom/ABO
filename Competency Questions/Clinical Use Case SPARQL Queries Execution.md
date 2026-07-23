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
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#](
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

SELECT DISTINCT ?activity ?recommendationValue ?modelName ?modelVersion WHERE {
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?task abo:hadTaskInformation ?aiOutput .
  ?aiOutput a abo:AIRecommendation ;
            abo:hasRecommendationValue ?recommendationValue ;
            prov:wasAttributedTo ?model .
  ?model a abo:AIModel ;
         abo:hasModelName ?modelName ;
         abo:hasModelVersion ?modelVersion .
}
```
| activity | recommendationValue | modelName | modelVersion |
| :--- | :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "BI RADS 1" | "V1" | "V1" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "BI RADS 1" | "V1" | "V1" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "BI RADS 1" | "V1" | "V1" |


## CQ3: What were the performance metrics of the AI model when the human operator had the same output as the AI model during a human oversight process?

**Key Concept:*
```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

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

## CQ4.  What was this operator’s workload at the time of this human oversight decision? 

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

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

## CQ5: What information artifacts were accessed by the human operator, and what interaction evidence was recorded to support this during this human oversight process?

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

SELECT ?activity ?accessedArtifact ?label ?evidenceID ?duration ?fileSource WHERE {
  ?activity a abo:HumanOversightActivity ;
            prov:qualifiedUsage ?interaction .
  ?interaction a abo:HumanInteraction ;
               prov:entity ?accessedArtifact ;
               abo:hadInteractionEvidence ?evidence .
  ?evidence a abo:InteractionEvidence ;
            abo:hasEvidenceID ?evidenceID ;
            abo:hasDuration ?duration ;
            abo:hasFileSource ?fileSource .
  OPTIONAL { ?accessedArtifact rdfs:label ?label . }
}
```
| activity | accessedArtifact | label | evidenceID | duration | fileSource |
| :--- | :--- | :--- | :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | [https://w3id.org/abo#MedHistory_Alice](https://w3id.org/abo#MedHistory_Alice) | "Medical history (Alice123)" | "EV244" | "39"^^xsd:decimal | "Audit log" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | [https://w3id.org/abo#MedHistory_Alice](https://w3id.org/abo#MedHistory_Alice) | "Medical history (Alice123)" | "EV462" | "420"^^xsd:decimal | "Audit log" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | [https://w3id.org/abo#AIExp_Alice](https://w3id.org/abo#AIExp_Alice) | "AI explanation (Alice123): SHAP" | "EV462" | "420"^^xsd:decimal | "Audit log" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | [https://w3id.org/abo#MedHistory_Alice](https://w3id.org/abo#MedHistory_Alice) | "Medical history (Alice123)" | "EV689" | "420"^^xsd:decimal | "Audit log" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | [https://w3id.org/abo#AIExp_Alice](https://w3id.org/abo#AIExp_Alice) | "AI explanation (Alice123): SHAP" | "EV689" | "420"^^xsd:decimal | "Audit log" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | [https://w3id.org/abo#SeniorReport_Alice](https://w3id.org/abo#SeniorReport_Alice) | "Senior radiologist report: BI RADS 4" | "EV689" | "420"^^xsd:decimal | "Audit log" |

## CQ6: What was the complexity of the task during this human oversight process?

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

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

# Agent Profile

## CQ7: Which human operators were associated with this task?

```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

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



## CQ8: What was the human oversight decision of the human operator for this AI output?
```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

SELECT ?activity ?recommendationValue ?oversightDecision ?oversightDecisionNote WHERE {
  ?result a abo:OversightResult ;
          prov:wasGeneratedBy ?activity ;
          abo:hasOversightDecision ?oversightDecision .
  ?activity a abo:HumanOversightActivity ;
            abo:wasOversightOf ?task .
  ?task abo:hadTaskInformation ?aiOutput .
  ?aiOutput a abo:AIRecommendation ;
            abo:hasRecommendationValue ?recommendationValue .
  OPTIONAL { ?result abo:hasOversightDecisionNote ?oversightDecisionNote . }
}
```
| activity | recommendationValue | oversightDecision | oversightDecisionNote |
| :--- | :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "BI RADS 1" | "BI RADS 1" | "Return for scan after a year" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "BI RADS 1" | "BI RADS 4" | "Stereotactic biopsy recommended" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "BI RADS 1" | "BI RADS 4" | "Stereotactic biopsy recommended" |

## CQ9: What was this operator’s level of expertise during this human oversight process? 
```sparql
PREFIX abo:  [https://w3id.org/abo#]
PREFIX prov: [http://www.w3.org/ns/prov#]
PREFIX rdfs: [http://www.w3.org/2000/01/rdf-schema#]
PREFIX xsd:  [http://www.w3.org/2001/XMLSchema#]

SELECT ?activity ?experience ?role WHERE {
  ?activity a abo:HumanOversightActivity ;
            prov:qualifiedAssociation ?context .
  ?context abo:hadProfile ?profile .
  ?profile abo:hasExperience ?experience ;
           abo:hasRole ?role .
}
```
| activity | experience | role |
| :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "3.0"^^xsd:decimal | "Junior Radiologist" |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "3.0"^^xsd:decimal | "Junior Radiologist" |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "15.0"^^xsd:decimal | "Senior Radiologist" |

## CQ10: What was this operator’s AI literacy score during this human oversight process?
```sparql
PREFIX abo:  <https://w3id.org/abo#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>
SELECT ?activity ?operatorName ?aiLiteracyScore ?aiLiteracyScoreUnit WHERE {
  ?activity a abo:HumanOversightActivity ;
            prov:qualifiedAssociation ?context .
  ?context prov:agent ?operator ;
           abo:hadProfile ?profile .
  ?operator rdfs:label ?operatorName .
  ?profile abo:hasAILiteracyScore ?aiLiteracyScore ;
           abo:hasAILiteracyScoreUnit ?aiLiteracyScoreUnit .
}
```
| activity | operatorName | aiLiteracyScore | aiLiteracyScoreUnit |
| :--- | :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "Dr. Bob" | "6.5"^^xsd:decimal | "10"^^xsd:integer |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "Dr. Bob" | "6.5"^^xsd:decimal | "10"^^xsd:integer |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "Dr. Jay" | "4.0"^^xsd:decimal | "10"^^xsd:integer |

## CQ11: What was the duration this human operator spent on this human oversight process?
```sparql
PREFIX abo:  <https://w3id.org/abo#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd:  <http://www.w3.org/2001/XMLSchema#>
SELECT ?activity ?operatorName ?duration WHERE {
  ?activity a abo:HumanOversightActivity ;
            prov:startedAtTime ?startedAt ;
            prov:endedAtTime ?endedAt ;
            prov:qualifiedAssociation ?context .
  ?context prov:agent ?operator .
  ?operator rdfs:label ?operatorName .
  BIND((?endedAt - ?startedAt) AS ?duration)
}
```
| activity | operatorName | duration |
| :--- | :--- | :--- |
| [https://w3id.org/abo#OversightActivity_Case1](https://w3id.org/abo#OversightActivity_Case1) | "Dr. Bob" | "PT2M"^^xsd:dayTimeDuration |
| [https://w3id.org/abo#OversightActivity_Case2](https://w3id.org/abo#OversightActivity_Case2) | "Dr. Jay" | "PT7M"^^xsd:dayTimeDuration |
| [https://w3id.org/abo#OversightActivity_Case3](https://w3id.org/abo#OversightActivity_Case3) | "Dr. Bob" | "PT8M"^^xsd:dayTimeDuration |
