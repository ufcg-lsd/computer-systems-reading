# **[Agile Cold Starts for Scalable Serverless](https://www.usenix.org/system/files/hotcloud19-paper-mohan.pdf)**

## Abstract
The Serverless or Function-as-a-Service (FaaS) model capitalizes on lightweight execution by packaging code and dependencies together for just-in-time dispatch. Often a container environment has to be set up afresh – a condition called “coldstart", and in such cases, performance suffers and overheadsmount, both deteriorating  rapidly under high concurrency. Caching and reusing previously employed containers ties up memory and risks information leakage. Latency for cold starts is frequently due to work and wait-times in setting up various dependencies – such as in initializing networking elements. This paper proposes a solution that pre-crafts such resources and then dynamically reassociates them with baseline containers. Applied to networking, this approach demonstrates an order of magnitude gain in cold starts, negligible memory consumption, and flat startup time under rising concurrency.

**Categoria: System proposal, Cold Start, Pause Container, Container Startup Optimization.**

## Problem
The paper showed that under concurrency the container startup time is the major contributor to the cold start latency and a subsequent analysis showed that the container network creation and initialization inside the host accounts for 90% of the startup time due to a [single global lock](https://lkml.org/lkml/2017/4/21/533) shared across the network namespace, so under high concurrency to create containers inside a host/VM the container startup time increases linearly as the number of concurrent creations.

## Solution
The main idea of the paper solution is to pre-create and initialize network namespaces and share them to containers under its initialization. Sharing a pre-established network namespace to a container avoids the creation and initialization of a new network namespace during the container startup. The network pre-creation and initialization concept is called [Pause Containers](https://www.ianlewis.org/en/almighty-pause-container) and is adopted by Kubernetes.

To bring the idea to a serverless platform the paper proposes a pool management system - pause container pool manager (PCPM) that is responsible to create pause containers on the host/VM startup or configuration, attach and detach pause containers to function containers on startup and termination.

## Evaluation and Results
To evaluate the proposed solution the authors implemented a PCPM-based OpenWhisk version and compare 

## Contribution


## Review


## Related Work
1. [Pipsqueak: Lean Lambdas with Large Libraries] SOCK implements and extends the earlier Pipsqueak proposal for efficient package initialization.
2. [Unikernels: Library Operating Systems for the Cloud] Recent alternatives to traditional containerization are based on library operating systems, enclaves, and unikernels.
3. [Rethinking the Library OS from the Top Down] Recent alternatives to traditional containerization are based on library operating systems, enclaves, and unikernels.
4. [From Zygote to Morula: Fortifying Weakened ASLR on Android] observed that forking many child processes from the same parent without calling exec undermines address-space randomization; their solution was Morula, a system that runs exec every time, but maintains a pool of preinitialized interpreters; this approach trades overall system throughput for randomization.
