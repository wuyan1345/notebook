---
count: true
---

# Entity-Relationship Model

## Design Alternatives
+ two major pitfalls
    + Redundancy : a bad design may result in repeat information
    + Incompleteness :  a bad design may make certain aspects of the enterprise difficult or impossible to model

## Entity Relationship Model 
+ Entity Sets
    + a set of entities of the same type that share the same properties
    + weak entity sets
        + an entity set that does not have a primary key and is dependent on another entity set
        + identified by a partial key

!!! example "Entity Sets"
    <img src="../6.png" style="max-width: 80%; height: auto;">

+ Relationship Sets
    +  a mathematical relation among n ≥ 2 entities, each taken from entity sets
    + degree of a relationship set
        + the number of entity sets that participate in the relationship
+ Roles
    + each entity in a relationship set plays a role in the relationship
!!! example "Attributes"
    + properties of an entity or relationship
    + simple/composite
        + simple : atomic, indivisible
        + composite : can be divided into subparts
    + single-valued/multi-valued
        + single-valued : can have only one value
        + multi-valued : can have multiple values
    + derived
        + can be computed from other attributes

<img src="../7.png" style="max-width: 80%; height: auto;">

## Mapping Cardinality Constraints
+ Cardinality constraints
    + specify the number of entities in one entity set that can be associated with entities in another entity set
    + types
        + one-to-one (1:1)
        + one-to-many (1:N)
        + many-to-one (N:1)
        + many-to-many (M:N)
+ Participation constraints
    + specify whether all or only some entities in an entity set participate in a relationship set
    + types
        + total participation
        + partial participation

## Primary Key for Relationship Sets
+ The choice of the primary key for a relationship set depends on the mapping cardinality of the relationship set

## Reduction to Relation Schemas
+ Entity Sets
    + e.g. student(ID, name, tot_cred)
+ Relationship Sets
    + e.g. advisor = (s_id, i_id)

## Other Aspects of Database Design
+ Functional Requirements
+ Data Flow, Workflow
+ Schema Evolution