@prefix : <https://w3id.org/abo#> .
@prefix abo: <https://w3id.org/abo#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix xml: <http://www.w3.org/XML/1998/namespace> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix prov: <http://www.w3.org/ns/prov#> .
@base <https://w3id.org/abo#> .

<https://w3id.org/abo#data> rdf:type owl:Ontology ;
                             owl:imports <https://w3id.org/abo#> ;
                             rdfs:label "ABO instance data: two radiology human-oversight use cases"@en ;
                             rdfs:comment "Instance data populating the Automation Bias Ontology (ABO) with two mammography BI-RADS oversight scenarios for patient/task Alice123. Values not provided in the case descriptions (task complexity, operator experience, file reference/source) are included as clearly commented placeholders."@en .

#################################################################
#    Shared entities (operator, profile, AI model, metrics)
#################################################################

### Operator
:Operator_DrBob a abo:HumanOperator ;
                rdfs:label "Dr. Bob" .

### Operator profile
:OperatorProfile_Bob a abo:OperatorProfile ;
                    abo:hasAILiteracyScore "6.5"^^xsd:decimal ;
                    abo:hasAILiteracyScoreUnit "10"^^xsd:int ;
                    abo:hasRole "Junior Radiologist" ;
                    abo:hasExperience "3.0"^^xsd:decimal .

### AI model
:AIModel_V1 a abo:AIModel ;
            abo:hasModelName "V1" ;
            abo:hasModelVersion "V1" ;
            rdfs:label "BI-RADS AI reader, version V1" .

### Performance metrics for the AI model
:Metric_PreDeploy a abo:PerformanceMetric ;
                  abo:hasMetricName "AUC-ROC (pre-deployment)" ;
                  abo:hasMetricValue "0.94"^^xsd:float ;
                  abo:hasMetricUnit "score" .

:Metric_PostDeploy a abo:PerformanceMetric ;
                   abo:hasMetricName "Interval cancer rate (post-deployment)" ;
                   abo:hasMetricValue "4.0"^^xsd:float .

:AIModel_V1 abo:hadPerformanceMetrics :Metric_PreDeploy ,
                                      :Metric_PostDeploy .

#################################################################
#    Case 1
#################################################################

### Task information artifacts (available to the operator)
:MedHistory_Case1 a abo:InformationArtifact ;
                  rdfs:label "Medical history (Case 1)" .

:AIRec_Case1 a abo:AIRecommendation ;
              rdfs:label "AI recommendation (Case 1): BI RADS 1" ;
              abo:hasRecommendationValue "BI RADS 1" ;
              prov:wasAttributedTo :AIModel_V1 .

:AIExp_Case1 a abo:AIExplanation ;
              rdfs:label "AI explanation (Case 1): SHAP" ;
              abo:hasExplanation "SHAP" ;
              prov:wasAttributedTo :AIModel_V1 .

### Task complexity
:TaskComplexity_Case1 a abo:TaskComplexity ;
                      abo:hasTaskComplexityValue "D" ;
                      abo:hasTaskComplexityMeasurementUnit "BI RADS" .

### Workload (environmental factor)
:Workload_Case1 a abo:Workload ;
                abo:hasMeasurementName "TTR reduction" ;
                abo:hasMeasurementValue "40.0"^^xsd:float ;
                abo:hasMeasurementUnit "%" .

### Interaction evidence
:Evidence_Case1 a abo:InteractionEvidence ;
                abo:hasEvidenceID "EV244" ;
                abo:hasDuration "39"^^xsd:decimal ;
                abo:hasFileReference "logs/EV244.json" ;
                abo:hasFileSource "Audit log" ;
                rdfs:comment "hasFileReference and hasFileSource are illustrative placeholders; not provided in the case description."@en .

### Human interaction (artifacts actually accessed)
:Interaction_Case1 a abo:HumanInteraction ;
                   prov:entity :MedHistory_Case1 ;
                   abo:hadInteractionEvidence :Evidence_Case1 .

### Operator context
:Context_Case1 a abo:HumanOperatorContext ;
               prov:agent :Operator_DrBob ;
               abo:hadProfile :OperatorProfile_Bob ;
               abo:hadEnvironment :Workload_Case1 .

### AI system task
:Task_Case1 a abo:AISystemTask ;
            abo:hasTaskID "Alice123" ;
            abo:hadTaskComplexity :TaskComplexity_Case1 ;
            abo:hadTaskInformation :AIRec_Case1 ,
                                   :AIExp_Case1 ,
                                   :MedHistory_Case1 .

### Human oversight activity
:OversightActivity_Case1 a abo:HumanOversightActivity ;
                         abo:wasOversightOf :Task_Case1 ;
                         prov:qualifiedAssociation :Context_Case1 ;
                         prov:qualifiedUsage :Interaction_Case1 ;
                         prov:startedAtTime "2023-11-24T17:37:00"^^xsd:dateTime ;
                         prov:endedAtTime "2023-11-24T17:39:00"^^xsd:dateTime .

### Oversight result
:Result_Case1 a abo:OversightResult ;
              abo:hasOversightDecision "BI RADS 1" ;
              abo:hasOversightDecisionNote "Return for scan after a year" ;
              prov:wasGeneratedBy :OversightActivity_Case1 .

#################################################################
#    Case 2
#################################################################

### Task information artifacts (available to the operator)
:MedHistory_Case2 a abo:InformationArtifact ;
                  rdfs:label "Medical history (Case 2)" .

:SeniorReport_Case2 a abo:InformationArtifact ;
                    rdfs:label "Senior radiologist report: BI RADS 4" .

:AIRec_Case2 a abo:AIRecommendation ;
              rdfs:label "AI recommendation (Case 2): BI RADS 1" ;
              abo:hasRecommendationValue "BI RADS 1" ;
              prov:wasAttributedTo :AIModel_V1 .

:AIExp_Case2 a abo:AIExplanation ;
              rdfs:label "AI explanation (Case 2): SHAP" ;
              abo:hasExplanation "SHAP" ;
              prov:wasAttributedTo :AIModel_V1 .

### Task complexity
:TaskComplexity_Case2 a abo:TaskComplexity ;
                      abo:hasTaskComplexityValue "D" ;
                      abo:hasTaskComplexityMeasurementUnit "BI RADS" .

### Workload (environmental factor)
:Workload_Case2 a abo:Workload ;
                abo:hasMeasurementName "TTR increase" ;
                abo:hasMeasurementValue "60.0"^^xsd:float ;
                abo:hasMeasurementUnit "%" .

### Interaction evidence
:Evidence_Case2 a abo:InteractionEvidence ;
                abo:hasEvidenceID "EV576" ;
                abo:hasDuration "120"^^xsd:decimal ;
                abo:hasFileReference "logs/EV576.json" ;
                abo:hasFileSource "Audit log" ;
                rdfs:comment "hasFileReference and hasFileSource are illustrative placeholders; not provided in the case description."@en .

### Human interaction (artifacts actually accessed)
:Interaction_Case2 a abo:HumanInteraction ;
                   prov:entity :MedHistory_Case2 ,
                               :SeniorReport_Case2 ,
                               :AIExp_Case2 ;
                   abo:hadInteractionEvidence :Evidence_Case2 .

### Operator context
:Context_Case2 a abo:HumanOperatorContext ;
               prov:agent :Operator_DrBob ;
               abo:hadProfile :OperatorProfile_Bob ;
               abo:hadEnvironment :Workload_Case2 .

### AI system task
:Task_Case2 a abo:AISystemTask ;
            abo:hasTaskID "Alice123" ;
            abo:hadTaskComplexity :TaskComplexity_Case2 ;
            abo:hadTaskInformation :AIRec_Case2 ,
                                   :AIExp_Case2 ,
                                   :MedHistory_Case2 ,
                                   :SeniorReport_Case2 .

### Human oversight activity
:OversightActivity_Case2 a abo:HumanOversightActivity ;
                         abo:wasOversightOf :Task_Case2 ;
                         prov:qualifiedAssociation :Context_Case2 ;
                         prov:qualifiedUsage :Interaction_Case2 ;
                         prov:startedAtTime "2023-01-12T09:30:00"^^xsd:dateTime ;
                         prov:endedAtTime "2023-01-12T09:32:00"^^xsd:dateTime .

### Oversight result
:Result_Case2 a abo:OversightResult ;
              abo:hasOversightDecision "BI RADS 4" ;
              abo:hasOversightDecisionNote "Stereotactic biopsy recommended" ;
              prov:wasGeneratedBy :OversightActivity_Case2 .

