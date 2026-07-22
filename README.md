# Automation-Bias-Ontology-
The Automation Bias Ontology (ABO) is an ontology for representing contextual information surrounding human oversight decisions in AI-assisted decision-making systems. ABO extends the W3C PROV Ontology (PROV-O) to capture provenance information related to human operators, AI systems, decisions, and the operational context in which decisions are made.

The ontology aims to support the assessment of potential automation bias by enabling structured representation of observable contextual evidence associated with human-AI decision-making processes.

 #### Namespace: 
 #### Version: v0.1

## Motivation

Artificial intelligence systems are increasingly used to support decision-making in high-risk domains. Although human oversight is required to ensure safe AI deployment, research has shown that human operators may become overly reliant on automated recommendations. This behaviour, known as automation bias, can occur when human operators accept AI-generated outputs without sufficient independent evaluation.

Assessing potential automation bias requires more than recording the final decision. The context surrounding the decision, including information about the AI system, human operator, and operational environment, must also be captured.

While provenance models such as W3C PROV-O describe relationships between entities, activities, and agents, they do not explicitly represent contextual factors relevant to automation bias.

ABO addresses this gap by providing a PROV-O-aligned model for representing decision provenance and contextual information relevant to human oversight.

## Scope

ABO currently focuses on modelling contextual information associated with:

| Category | Examples |
|---|---|
| AI System | AI recommendations, model information, explanations, performance information |
| Human Operator | Expertise, AI literacy, oversight decisions, interaction information |
| Operational Context | Workload, task complexity, accessed information |

ABO represents contextual evidence that can support analysis of automation bias. It does not directly determine whether automation bias has occurred.


## Repository Structure

| Directory | Description |
|---|---|
| `ontology/` | OWL ontology files |
| `docs/` | Widoco-generated ontology documentation |
| `competency-questions/` | Competency questions used during ontology development |
| `examples/` | Example ontology instances |
| `images/` | Figures and diagrams |


## Publication

## Documentation

## Acknowledgements 

# License 

