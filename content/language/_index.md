+++
title = "Language"
date = 2020-01-25T18:11:03-05:00
weight = 2
chapter = true
+++

Xalgo is a table-based DSL for describing [computational
rules](../concepts/on.rule.systems) that act on tabular table. The language is
inspired by and partially resembles [SQL](https://en.wikipedia.org/wiki/SQL) and
[CQL](https://en.wikipedia.org/wiki/Apache_Cassandra#Main_features). Both SQL
and CQL are **also** based on tabular / columnar computation models.

Xalgo aims to *not be Turing complete*. The ongoing design of the language has
careful avoided any attempt to make the language into a general purpose
programming language. The goal of this resistence is to *avoid side-effects* of
the language that have not be predetermined by the design team.


