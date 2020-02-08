+++
title = "Platform Architecture"
weight = 2
+++

The architecture of the Interlibr platform is inspired by several recent trends
in [cloud-native](https://www.cncf.io/about/faq/) platform design.

The deployment model aims to provide support to applications via a [serverless
model](https://github.com/cncf/wg-serverless/tree/master/whitepapers/serverless-overview). Developers
who use Interlibr to implement their applications should not have to be
concerned about _how_ the platform is deployed. They should be able to integrate
with it _as-a-Service_.

The interaction model is _event-based_, inspired by
[CQRS](https://en.wikipedia.org/wiki/Command–query_separation). The ability to
_motivate change_ within the platform is kept distinct from _access to stored
information_. Any changes that do occur are delivered to integrated applications
as events that indicate changes to state.

Originally, the core technical framework used by the platform was
[SMACK](mesosphere.com/blog/smack-stack-new-lamp-stack/). As the development
team has matured, we have moved away from some elements of this
framework. Kubernetes has become the defacto orchestration model for
cloud-native applications, rather than Mesos. In earlier phases of development,
Spark was unnecessarily expensive to deploy for minimal benefit. Therefore, both
technologies have been eliminated from the current architecture. The platform
still uses Kafka for processing queues, Cassandra for storage of tabular data
and Akka as the primary async processing foundation.

# Primary Structure

[write about kernel and shell]

# Kernel

## Tables

## Jobs

[network of jobs]

# Shell

## Submissions

## Rules

## Queries

## Revisions


