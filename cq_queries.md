---
layout: default
title: examples
---

# KGC 2026 Competency Questions and Example Queries

## Focus: Evaluation Measures

### Competency Question 1

What article is measure M attributed to?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX prov: <http://www.w3.org/ns/prov#>
PREFIX av: <https://www.omg.org/spec/Commons/AnnotationVocabulary/>
SELECT ?source
WHERE {
    ?measure a efemo:Faithfulness_Measure .
    ?measure rdfs:label "M" .
    ?measure prov:wasAttributedTo ?doc .
    ?doc av:directSource ?source .
}
```

### Competency Question 2

What explanation modality, granularity, and evaluation method does measure M use?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?modality ?granularity ?eval_method
WHERE {
    ?measure a efemo:Faithfulness_Measure ;
             rdfs:label "M" ;
             efemo:measure_evaluates_modality ?modality ;
             efemo:measure_has_granularity ?granularity ;
             efemo:measure_uses_method ?eval_method .
}
```

### Competency Question 3

What faithfulness assumptions are required to measure proxy P?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?assumption
WHERE {
    ?proxy a efemo:Faithfulness_Proxy ;
           rdfs:label "P" ;
           efemo:proxy_requires_assumption ?assumption .
}
```

## Focus: Measure Recommendations

### Competency Question 4

What measures evaluate explanation modality D at granularity G using proxy characteristic P?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?measure
WHERE {
    ?measure a efemo:Faithfulness_Measure ;
             efemo:measure_evaluates_modality ?modality ;
             efemo:measure_has_granularity ?granularity ;
             efemo:faithfulness_measure_uses_proxy ?proxy .
    ?modality rdfs:label "D" .
    ?granularity rdfs:label "G" .
    ?proxy rdfs:label "P" .
}
```

### Competency Question 5

What measures require model access level L while using an evaluation method of type E?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?measure
WHERE {
    ?measure a efemo:Faithfulness_Measure ;
             efemo:measure_requires_access_level ?access ;
             efemo:measure_uses_method ?method .
    ?access rdfs:label "L" .
    ?method a ?method_type .
    ?method_type rdfs:label "E" .
}
```

### Competency Question 6

Are there any measures with the same explanation modality, granularity, and model access requirements as measure M?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?measure2
WHERE {
    ?measure1 a efemo:Faithfulness_Measure ;
              rdfs:label "M" ;
              efemo:measure_evaluates_modality ?modality ;
              efemo:measure_has_granularity ?granularity ;
              efemo:measure_requires_access_level ?access .

    ?measure2 a efemo:Faithfulness_Measure ;
              efemo:measure_evaluates_modality ?modality ;
              efemo:measure_has_granularity ?granularity ;
              efemo:measure_requires_access_level ?access .

    FILTER ( ?measure1 != ?measure2 )
}
```

## Focus: Analysis of Measures

### Competency Question 7

Do measures M and N share any assumptions?

*SPARQL Query:*
```PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?assumption
WHERE {
    ?measure1 a efemo:Faithfulness_Measure ;
              rdfs:label "M" ;
              efemo:measure_makes_assumption ?assumption .

    ?measure2 a efemo:Faithfulness_Measure ;
              rdfs:label "N" ;
              efemo:measure_makes_assumption ?assumption .
}

```

### Competency Question 8

What assumption is most commonly made when evaluating explanations with modality D?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?assumption (COUNT(?assumption) as ?assumption_count)
WHERE {
    ?measure a efemo:Faithfulness_Measure ;
             efemo:measure_evaluates_modality ?modality ;
             efemo:measure_makes_assumption ?assumption .
    ?modality rdfs:label "D" .
}
GROUP BY ?assumption
ORDER BY DESC(?assumption_count)
LIMIT 1
```

### Competency Question 9

List the proxy characteristics used in evaluations that require model access level L by popularity.

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?proxy (COUNT(?measure) as ?num_measures)
WHERE {
    ?measure a efemo:Faithfulness_Measure ;
	efemo:measure_requires_access_level ?access ;
	efemo:faithfulness_measure_uses_proxy ?proxy .
    ?access rdfs:label "L" .
}
GROUP BY ?proxy
ORDER BY DESC(?num_measures)
```

### Competency Question 10

What explanation modality is evaluated by the fewest measures (of those included in the system)?

*SPARQL Query:*
```
PREFIX efemo: <http://www.semanticweb.org/villad4/ontologies/efemo#>
PREFIX eo: <https://purl.org/heals/eo#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT ?modality (COUNT(?measure) as ?num_measures)
WHERE {
    ?modality a eo:ExplanationModality .
    OPTIONAL {
        ?measure a efemo:Faithfulness_Measure ;
                 efemo:measure_evaluates_modality ?modality .
    }
}
GROUP BY ?modality
ORDER BY ASC(?num_measures)
LIMIT 1
```