# **[Agile Cold Starts for Scalable Serverless](https://www.usenix.org/system/files/hotcloud19-paper-mohan.pdf)**

## Abstract
The Serverless or Function-as-a-Service (FaaS) model capitalizes on lightweight execution by packaging code and dependencies together for just-in-time dispatch. Often a container environment has to be set up afresh – a condition called “coldstart", and in such cases, performance suffers and overheadsmount, both deteriorating  rapidly under high concurrency. Caching and reusing previously employed containers ties up memory and risks information leakage. Latency for cold starts is frequently due to work and wait-times in setting up various dependencies – such as in initializing networking elements. This paper proposes a solution that pre-crafts such resources and then dynamically reassociates them with baseline containers. Applied to networking, this approach demonstrates an order of magnitude gain in cold starts, negligible memory consumption, and flat startup time under rising concurrency.

**Category: System proposal, Cold Start, Pause Container, Container Startup Optimization.**

## Problem
The paper showed that under concurrency the container startup time is the major contributor to the cold start latency and a subsequent analysis showed that the container network creation and initialization inside the host accounts for 90% of the startup time due to a [single global lock](https://lkml.org/lkml/2017/4/21/533) shared across the network namespace, so under high concurrency to create containers inside a host/VM the container startup time increases linearly as the number of concurrent creations.

## Solution
The main idea of the paper solution is to pre-create and initialize network namespaces and share them to containers under its initialization. Sharing a pre-established network namespace to a container avoids the creation and initialization of a new network namespace during the container startup. The network pre-creation and initialization concept is called [Pause Containers](https://www.ianlewis.org/en/almighty-pause-container) and is adopted by Kubernetes.

To bring the idea to a serverless platform the paper proposes a pool management system - pause container pool manager (PCPM) that is responsible to create pause containers on the host/VM startup or configuration, attach and detach pause containers to function containers on startup and termination.

## Evaluation and Results
The authors implemented a PCPM-based OpenWhisk and evaluate it under [1, 10, 20, 50, 80, 100] concurrent cold invocations, the results showed an 80% redution on cold start execution time when compared with the unmodified OpenWhisk version.

## Contribution
SOCK already proposed a solution to avoid network namespace creation and initialization during container startup by not isolating the host and container network namespace, this decision may lead to security drawbacks. Therefore, PCPM does not require significant changes on the platform architecture design.

## Review
The paper do not detail how the experiments were executed to obtain samples of cold start executions and lacks details on evaluation metrics.

The authors state that cold start is influenced just by the container startup and our findings showed the cold start also can be affected by the application startup.
