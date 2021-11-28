---
layout: default
parent: Concepts
nav_order: 2
title: "Matching"
date: 2020-06-26T07:51:53-04:00
---

Interlibr federates repositories of rules providing the facility for
[rule-takers](/concepts/glossary) to match rules that are applicable to their
data and to apply those rules over their data. An index of rules is maintained
by Interlibr and provided to rule-takers. This index enables a basic matching
algorithm to be run by the rule-taker:

1. Is a rule _in-effect_?
1. Is a rule _applicable_?
1. Apply the rule.

# Effective rules

A rule is *effective* if it matches the jurisdiction (country and region)
specified in a transaction. If effective dates and times are specified in the
expression of the rule, then the date and time in the transaction **must fall
within** this effective period.

# Applicable rules

A rule is *applicable* if applying the rule would recommend changes to the
information offered in the transaction. When writing the rule, the rule-maker
supplies a set of conditions on the transaction data that would indicate whether
their rule would recommend changes when those conditions are met. For example, a
rule that calculates a conditional value-added tax might require that the
transaction data include fields that specific price and product classification.

# Effective versus applicable

On first glance, it seems as if *effective* and *applicable* are very
similar and could possibly be expressed in a single manner. Therefore,
we should consider why the two different ideas exist.

The rules that are indexed by Interlibr are each entries in a large ontology
classified according to industry, jurisdication, time, etc. The *effectiveness*
of a rule is a form of meta-organization within this ontology. Rule-makers are
able to use this classification to broadly partition documents that will be
processed. Therefore, *in-effect* is the classification of a rule.

Each individual rule represents a very small *program* or *computation*. As part
of the expression of this computation, we can indicate what types of data a rule
will affect. Knowing whether a rule will result in anything allows us to
eliminate rules that do nothing, saving computation time. To do this pruning,
specific information about the rule and specific introspection of the
transaction data is required. Therefore, we intentionally perform this second
partition as a distinct phase.
