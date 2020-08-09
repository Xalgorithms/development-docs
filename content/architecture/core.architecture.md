+++
title = "Core Architecture"
weight = 1
parent = Architecture
+++

{{% notice info %}}

This document is *a work in progress*. It has been published publicly to help
reviewers access the document. Some information is missing or incomplete.

{{% /notice %}}

[start with the catalog / phone book metaphor - that's a decent chance of having something holistic - maybe even consumers distributing]


# Structure

![Core Architecture Diagram](/images/architecture/interlibr.overview.svg)

[summary of what the diagram explains]

The Interlibr platform implements a data storage platform for [rules oughtomation](/concepts/glossary). It allows [rule makers](/concepts/glossary) to store rules or reference tables that describe how rules should be [matched and applied](/concepts/matching). It provides access to _federated index_ of this information so that [rule takers](/concepts/glossary) can select and evaluate particular oughtomation that applies to their context.

This document describes software implemented by the Xalgorithms Foundation that forms the core of an Interlibr implementation.

## Rule repositories

Central to the Interlibr platform is the storage and version history of rules and lookup tables. For the Xalgorithms Interlibr implementation, rules are stored in [git]() repositories hosted by [GitHub]() or [Gitlab](). These repositories are considered to be _federated_ - a single oughtomation can be composed of rules and lookup tables that are retain in different repositories.

Rules and lookup tables are stored in a specialized JSON format that is designed so that each rule or lookup table can be understood in isolation. All information about the rule or lookup table can be determined by reading the file that describes it.

## XA Rule Maker

The rule maker implemented by Xalgorithms is a web application that privides an IDE for rules and lookup tables. It is capable of reading and writing from multiple rule repositories.

## XA Rule Taker

Xalgorithms has implemented shared components that can be used to perform rules oughtomation.

## Indexer

[The indexer is a central service. Multiple instances of it can be run. All it does is collate information stored in a set of Git repositories into a single DB. The DB contains the information that any XA-RT needs to determine “in-effect” and “applicable”. The actual rules are just URLs to the rule files in Git.]
