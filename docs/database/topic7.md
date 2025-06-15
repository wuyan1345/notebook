---
count: true
---

# Normalization

## Decomposition
+ Lossless decomposition: 
    + No information is lost after decomposition.
    + The original relation can be reconstructed from the decomposed relations.

## Functional Dependencies
+ Functional dependency (FD): A relationship between attributes, where one attribute uniquely determines another.
    + Notation: `X -> Y` means if two tuples have the same value for `X`, they must also have the same value for `Y`.
+ Trival
    + A functional dependency is trivial（平凡的）if it is satisfied by all instances of a relation
+ Closure

## Boyce-Codd Normal Form
+ A relation is in Boyce-Codd Normal Form (BCNF) if for every non-trivial functional dependency `X -> Y`, `X` is a superkey.
<img src="../8.png" style="max-width: 80%; height: auto;">

## Third Normal Form
+ A relation is in Third Normal Form (3NF) if for every non-trivial functional dependency `X -> Y`, at least one of the following conditions holds:
    + `X` is a superkey.
    + `Y` is a prime attribute (part of a candidate key).

## Canonical Cover
+ A canonical cover (or minimal cover) is a set of functional dependencies that is equivalent to a given set of functional dependencies but has no redundant dependencies.