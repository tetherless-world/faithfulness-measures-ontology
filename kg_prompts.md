---
layout: default
title: kg_prompts
---

## Generation Prompts

These prompt templates are used to extract information from scientific papers on explanation faithfulness measures. These prompt templates include slots for definitions, examples, and other properties of concepts in the EFEMO ontology. Terms from other ontologies are also used, and represented by their IRI. These slots are filled by the information present in the EFEMO ontology, retrieved from the online resource.
For ease of reading, few-shot examples have been removed from the prompts.

### Find Faithfulness Measures

Does document {doc_name} introduce any faithfulness measures? A faithfulness measure is {definition\[Faithfulness_Measure\]}. Present the name and a description of each faithfulness measure introduced in JSON format.

### Find Datasets

What datasets are used in the experiment(s) in document {doc_name}? Present both the name and the full citation of the source document. Use JSON format.

### Find Explanation Generation Methods

What methods are used to generate the explanations in the experiment(s) in document {doc_name}? For each method, determine how much model access is needed to form the explanation and the modality of the explanations that are produced. The possible model access levels are: {instances\[Model_Access_Level\]}. An explanation's modality is {description\[https://purl.org/heals/eo#ExplanationModality\]}. The categories of explanation modality are: {categories\[https://purl.org/heals/eo#ExplanationModality\]}. Present each method's information in JSON format.

### Determine Measure Explanation Modality

What are the modalities of the explanations that the {measure_name} measure evaluates? An explanation's modality is {description\[https://purl.org/heals/eo#ExplanationModality\]}. The categories of explanation modality are: {categories\[https://purl.org/heals/eo#ExplanationModality\]}. Present the explanation modalities evaluated in JSON format.

### Determine Measure Model Specificty

Is the {measure_name} measure specific to a type of AI model? Present the models it can be applied to in JSON format. Here are example model specificities: {instances\[Model_Specific\]}.

### Determine Measure Model Access

How much access to the AI model does the {measure_name} measure need? The possible model access levels are: {instances\[Model_Access_Level\]}. Present the required model access levels in JSON format.

### Determine Measure Range

Is the {measure_name} measure binary or graded? The measure is binary if it is {definition\[Binary_Faithfulness_Measure\]}. The measure is graded if it is {definition\[Graded_Faithfulness_Measure\]}. Answer with either: {"range":"binary"} or {"range":"graded"}.

### Determine Measure Granularity

Is the {measure_name} measure a local or global measure? A local measure has {definition\[local_scope\]}. A global measure has {definition\[global_scope\]}. Answer with either: {"granularity":"local"} or {"granularity":"global"}.

### Find Measure Evaluation Method

Describe the evaluation method of {measure_name} measure. An evaluation method is {definition\[Evaluation_Method\]}. Present the description in JSON format.

### Determine Measure Evaluation Method: Computational vs Human Method

Is the evaluation method for {measure_name} measure a {label\[Computational_Based_Evaluation_Method\]} or a {label\[Human_Based_Evaluation_Method\]}? A {label\[Computational_Based_Evaluation_Method\]} is {definition\[Computational_Based_Evaluation_Method\]}. A {label\[Human_Based_Evaluation_Method\]} is {definition\[Human_Based_Evaluation_Method\]}. Answer with either: {"category":"computational"} or {"category":"human"}.

### Determine Measure Evaluation Method: Computational Sub-Category

What category(s) do that evaluation method belong to? There are 4 options. The options are: {label\[correctness_evaluation_method\]} ({definition\[correctness_evaluation_method\]}), {label\[stability_evaluation_method\]} ({definition\[stability_evaluation_method\]}), {label\[comprehensive_evaluation_method\]} ({definition\[comprehensive_evaluation_method\]}), and {label\[surrogate_model_method\]} ({definition\[surrogate_model_method\]}). Answer in JSON format, as in the following example: {"category":\["{label\[correctness_evaluation_method\]}"\]}. If you do not think it belongs to any category, answer {"category":[]}.

### Determine Measure Evaluation Method: Human Sub-Category

What category(s) do that evaluation method belong to? There are 2 options. The options are: {label\[access_the_classifier_method\]} ({definition\[access_the_classifier_method\]}) and {label\[find_alignment_method\]} ({definition\[find_alignment_method\]}). Answer in JSON format, as in the following example: {"category":\["{label\[access_the_classifier_method\]}"\]}. If you do not think it belongs to any category, answer {"category":[]}.

### Determine Measure Evaluation Method: Broad Category

What category(s) do the evaluation method for {measure_name} measure belong to? There are 6 options. The options are: {label\[Perturbation_Based_Evaluation_Method\]} ({definition\[Perturbation_Based_Evaluation_Method\]}), {label\[Predictive_Power_Evaluation_Method\]} ({definition\[Predictive_Power_Evaluation_Method\]}), {label\[Self_Predictive_Power_Evaluation_Method\]} ({definition\[Self_Predictive_Power_Evaluation_Method\]}), {label\[Robustness_Evaluation_Method\]} ({definition\[Robustness_Evaluation_Method\]}), {label\[White_Box_Evaluation_Method\]} ({definition\[White_Box_Evaluation_Method\]}), and {label\[Axiomatic_Evaluation_Method\]} ({definition\[Axiomatic_Evaluation_Method\]}). Answer in JSON format, as in the following example: {"category":\["{label\[Perturbation_Based_Evaluation_Method\]}"\]}. If you do not think it belongs to any category, answer {"category":[]}.

### Determine Measure Evaluation Method: Whitebox Sub-Category

Is the evaluation method for {measure_name} measure a {label\[Transparent_Model_Evaluation_Method\]} or a {label\[Transparent_Task_Evaluation_Method\]}? A {label\[Transparent_Model_Evaluation_Method\]} is {definition\[Transparent_Model_Evaluation_Method\]}. A {label\[Transparent_Task_Evaluation_Method\]} is {definition\[Transparent_Task_Evaluation_Method\]}. Answer with either: {"category":"transparent task"} or {"category":"transparent model"}.

### Find Measure Proxy Characteristic

Since faithfulness cannot be directly measured, the {measure_name} measure must measure something else. This proxy characteristic is {definition\[Faithfulness_Proxy\]}. Common proxy characteristics are {label\[self_consistency_proxy\]} ({definition\[self_consistency_proxy\]}), {label\[feature_importance_agreement_proxy\]} ({definition\[feature_importance_agreement_proxy\]}), and  {label\[model_sensitivity_proxy\]} ({definition\[model_sensitivity_proxy\]}). If {measure_name} uses one of these proxy characteristics, put it in JSON format. If {measure_name} uses a new proxy characteristic, put the name and definition of it in JSON format.
