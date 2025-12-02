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
TODO
```

## Competency Question 2

*Usage Scenario:* A research intern is researching explanation faithfulness when they read about DeYoung et al.’s (2020) COMP measure. It sounds very familiar, but they can’t find a paper they’ve already read that discusses anything similar. They want a list of all similar measures to find the one they read about earlier.

*Competency Question:* Are there any measures similar to DeYoung et al.’s (2020) COMP measure?

*Ontology process:* The system finds the method called “COMP”, and looks for a distinct but similar faithfulness measure. (Similarity is defined as measures that measure the same proxy, use the same type of evaluation method, evaluate the same modality, have the same granularity level, the same model specificity, and at least one shared assumption and shared access level). If any are found, the system will present the name of the measures and the paper the measures are from, unordered.

*SPARQL Query:*
```
TODO
```

## Competency Question 3

*Usage Scenario:* A researcher is trying to create a new faithfulness measure that uses self-consistency as a proxy. All of the ones they’ve heard of are local measures, so they’re trying to create a global measure for evaluating an entire model. They want to check if there are any related measures they’ve missed to read through those papers.

*Competency Question:* Are there any existing global measures that use self-consistency as a proxy?

*Ontology process:* The system queries for any measures that use the self-consistency proxy and have global granularity. If any are found, the system will present the name of the measures and the paper the measures are from, unordered.

*SPARQL Query:*
```
TODO
```

## Competency Question 4

*Usage Scenario:* An explainability researcher has read a lot of papers about the ‘attention is not explanation’ debate, and believes they have created a new attention-based explainability method that solves some of the issues brought up in the literature. They are using a local copy of a model and have full access to it for evaluation. They want to find any potential measures that could be used to evaluate the faithfulness to see if they’ve actually solved any known issues.

*Competency Question:* I have a new explainability technique that uses the attention mechanism of a model to determine which parts of the input are the most important. I have full access to the model whose explanations are being evaluated. What measures are available for me to use?

*Ontology process:* The system infers that any faithfulness measure must be able to evaluate attention-based explanations, though this will be loosened to general feature-based explanations if necessary. No restrictions on model access level will be placed. Found measures will be presented to the user with the name of the measures and the paper the measures are from, unordered.

*SPARQL Query:*
```
TODO
```

## Competency Question 5

*Usage Scenario:* A financial advisor has been using an LLM when analyzing applications for home loans. The LLM provides natural language explanations along with its suggestions, but the advisor isn’t sure if the explanations are actually good, since they’re not an expert on explainability. A coworker suggested Dasgupta et al.’s (2022) local sufficiency measure, but the advisor isn’t sure if they can use it, since they only have inference access to the model through the API. 

*Competency Question:* I want to use Dasgupta et al.’s (2022) local sufficiency measure to evaluate some explanations that my model produced. How much model access do I need?

*Ontology process:* The system finds the method called “local sufficiency” and returns any required access levels.

*SPARQL Query:*
```
TODO
```
