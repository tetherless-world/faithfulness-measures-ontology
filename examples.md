---
layout: default
title: examples
---

# Competency Questions and Example Queries

Several example usage scenarios, associated competency questions, and SPARQL queries are given below.

## Competency Question 1
*Usage Scenario:* An explainability researcher is looking for faithfulness measures to evaluate an existing explanation method. They want to use a state-of-the-art model that produces chain-of-thought explanations, but it is only available through API inference. They are specifically interested in robustness-based evaluation methods since they have a large token budget and can let the code run over the weekend. The researcher would prefer a local measure to determine the faithfulness of each explanation individually.

*Competency Question:* I’m looking for faithfulness measure recommendations: the system is a black-box LLM, looking to evaluate the faithfulness of chain-of-thought explanations, preference for local measures, preference for using a robustness-based evaluation method.

*Ontology process:* The system first infers that the measure must only require inference-level access or lower and must be able to evaluate chain-of-thought style of explanation. The system also knows the user prefers measures that have a local granularity and use robustness-based methodologies. The system will first query for measures that have no access requirements higher than inference-level access, evaluate chain-of-thought explanation modality, are local measures, and use robustness-based methods. If any are found, the system will present the name of the measures and the paper the measures are from, unordered. If nothing is found, the system notifies the user and removes the restrictions on granularity and methods, presenting any newly found results.

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX indv: <http://www.semanticweb.org/villad4/ontologies/2025/11/efemo_indv#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX av: <https://www.omg.org/spec/Commons/AnnotationVocabulary/>
SELECT ?measure ?source
WHERE { 
	?measure a efemo:Faithfulness_Measure .
	?measure efemo:evaluates_modality efemo:cot_explanation_modality .
	?measure efemo:has_granularity efemo:local_scope .
	?measure efemo:uses_method ?method .
	?method a efemo:Robustness_Evaluation_Method .
	?measure prov:wasAttributedTo ?doc .
	?doc av:directSource ?source .
	?measure efemo:requires_access_level ?access .
	FILTER NOT EXISTS{?access efemo:higher_access_level efemo:inference_access_level .}
}
```
*Expected Answer:*
Unfaithfulness Explained by Bias ;	"Turpin, M., Michael, J., Perez, E., & Bowman, S. (2023). Language models don't always say what they think: Unfaithful explanations in chain-of-thought prompting. Advances in Neural Information Processing Systems, 36, 74952-74965."	http://www.semanticweb.org/villad4/ontologies/2025/11/ 

## Competency Question 2

*Usage Scenario:* A research intern is researching explanation faithfulness when they read about DeYoung et al.’s (2020) COMP measure. It sounds very familiar, but they can’t find a paper they’ve already read that discusses anything similar. They want a list of all similar measures to find the one they read about earlier.

*Competency Question:* Are there any measures similar to DeYoung et al.’s (2020) COMP measure?

*Ontology process:* The system finds the method called “COMP”, and looks for a distinct but similar faithfulness measure. (Similarity is defined as measures that measure the same proxy, use the same type of evaluation method, evaluate the same modality, have the same granularity level, the same model specificity, and at least one shared assumption and shared access level). If any are found, the system will present the name of the measures and the paper the measures are from, unordered.

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX indv: <http://www.semanticweb.org/villad4/ontologies/2025/11/efemo_indv#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX av: <https://www.omg.org/spec/Commons/AnnotationVocabulary/>
SELECT ?measure ?source
WHERE { 

	?measure a efemo:Faithfulness_Measure .
	?measure efemo:measures_proxy ?proxy .
	?measure efemo:evaluates_modality ?mod .
	?measure efemo:has_granularity ?scope .
	?measure efemo:has_model_specificity ?spec .
	?measure efemo:requires_access_level ?access .

	indv:eraser_comprehensiveness efemo:measures_proxy ?proxy .
	indv:eraser_comprehensiveness efemo:evaluates_modality ?mod .
	indv:eraser_comprehensiveness efemo:has_granularity ?scope .
	indv:eraser_comprehensiveness efemo:has_model_specificity ?spec .
	indv:eraser_comprehensiveness efemo:requires_access_level ?access .

	?measure prov:wasAttributedTo ?doc .
	?doc av:directSource ?source .

	FILTER ( ?measure != indv:eraser_comprehensiveness )
}
```
*NOTE: This property could not be inferred through SWRL rules due to significantly slowing down the reasoner*

*Expected Answer:*
Decision Flip - Most Informative Token ;	"Chrysostomou, G., & Aletras, N. (2021). Improving the faithfulness of attention-based explanations with task-specific information for text classification. arXiv preprint arXiv:2105.02657."

## Competency Question 3

*Usage Scenario:* A researcher is trying to create a new faithfulness measure that uses self-consistency as a proxy. All of the ones they’ve heard of are local measures, so they’re trying to create a global measure for evaluating an entire model. They want to check if there are any related measures they’ve missed to read through those papers.

*Competency Question:* Are there any existing global measures that use self-consistency as a proxy?

*Ontology process:* The system queries for any measures that use the self-consistency proxy and have global granularity. If any are found, the system will present the name of the measures and the paper the measures are from, unordered.

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX av: <https://www.omg.org/spec/Commons/AnnotationVocabulary/>
SELECT ?measure ?source
WHERE { 
	?measure a efemo:Faithfulness_Measure .
	?measure efemo:measures_proxy efemo:self_consistency_proxy .
	?measure efemo:has_granularity efemo:global_scope .
	?measure prov:wasAttributedTo ?doc .
	?doc av:directSource ?source
}
```
*Expected Answer:*
Global Consistency ;	"Dasgupta, S., Frost, N., & Moshkovitz, M. (2022, June). Framework for evaluating faithfulness of local explanations. In International Conference on Machine Learning (pp. 4794-4815). PMLR."

## Competency Question 4

*Usage Scenario:* An explainability researcher has read a lot of papers about the ‘attention is not explanation’ debate, and believes they have created a new attention-based explainability method that solves some of the issues brought up in the literature. They are using a local copy of a model and have full access to it for evaluation. They want to find any potential measures that could be used to evaluate the faithfulness to see if they’ve actually solved any known issues.

*Competency Question:* I have a new explainability technique that uses the attention mechanism of a model to determine which parts of the input are the most important. I have full access to the model whose explanations are being evaluated. What measures are available for me to use?

*Ontology process:* The system infers that any faithfulness measure must be able to evaluate attention-based explanations, though this will be loosened to general feature-based explanations if necessary. No restrictions on model access level will be placed. Found measures will be presented to the user with the name of the measures and the paper the measures are from, unordered.

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX indv: <http://www.semanticweb.org/villad4/ontologies/2025/11/efemo_indv#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX av: <https://www.omg.org/spec/Commons/AnnotationVocabulary/>
SELECT ?measure ?source
WHERE { 
	?measure a efemo:Faithfulness_Measure .
	?measure efemo:evaluates_modality efemo:attention_map_explanation_modality .
	?measure prov:wasAttributedTo ?doc .
	?doc av:directSource ?source
}
```
*Expected Answer:* 
Decision Flip - Most Informative Token ;	"Chrysostomou, G., & Aletras, N. (2021). Improving the faithfulness of attention-based explanations with task-specific information for text classification. arXiv preprint arXiv:2105.02657."
Unfaithfulness Explained by Bias ;	"Turpin, M., Michael, J., Perez, E., & Bowman, S. (2023). Language models don't always say what they think: Unfaithful explanations in chain-of-thought prompting. Advances in Neural Information Processing Systems, 36, 74952-74965."	
RandomPermute ;	"Jain, S., & Wallace, B. C. (2019). Attention is not explanation. arXiv preprint arXiv:1902.10186."	
Comprehensiveness ;	"DeYoung, J., Jain, S., Rajani, N. F., Lehman, E., Xiong, C., Socher, R., & Wallace, B. C. (2020, July). ERASER: A benchmark to evaluate rationalized NLP models. In Proceedings of the 58th annual meeting of the association for computational linguistics (pp. 4443-4458)."

## Competency Question 5

*Usage Scenario:* A financial advisor has been using an LLM when analyzing applications for home loans. The LLM provides natural language explanations along with its suggestions, but the advisor isn’t sure if the explanations are actually good, since they’re not an expert on explainability. A coworker suggested Dasgupta et al.’s (2022) local sufficiency measure, but the advisor isn’t sure if they can use it, since they only have inference access to the model through the API. 

*Competency Question:* I want to use Dasgupta et al.’s (2022) local sufficiency measure to evaluate some explanations that my model produced. How much model access do I need?

*Ontology process:* The system finds the method called “local sufficiency” and returns any required access levels.

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX indv: <http://www.semanticweb.org/villad4/ontologies/2025/11/efemo_indv#>
SELECT ?access
WHERE { 
	indv:local_sufficiency efemo:requires_access_level ?access
}
```
*Expected Answer:* Inference Access Level
