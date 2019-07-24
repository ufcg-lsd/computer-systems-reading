# **[Agile Cold Starts for Scalable Serverless](https://www.usenix.org/system/files/hotcloud19-paper-mohan.pdf)**

## Abstract
The Serverless or Function-as-a-Service (FaaS) model capitalizes on lightweight execution by packaging code and dependencies together for just-in-time dispatch. Often a container environment has to be set up afresh – a condition called “coldstart", and in such cases, performance suffers and overheadsmount, both deteriorating  rapidly under high concurrency. Caching and reusing previously employed containers ties up memory and risks information leakage. Latency for cold starts is frequently due to work and wait-times in setting up various dependencies – such as in initializing networking elements. This paper proposes a solution that pre-crafts such resources and then dynamically reassociates them with baseline containers. Applied to networking, this approach demonstrates an order of magnitude gain in cold starts, negligible memory consumption, and flat startup time under rising concurrency.

**Categoria: System proposal, Cold Start, Pause Container, Container Startup Optimization.**

## Problema
The paper showed that under concurrency the container startup time is the major contributor to the cold start latency and a subsequent analysis showed that the container network creation and initialization inside the host accounts for 90% of the startup time due to a [single global lock](https://lkml.org/lkml/2017/4/21/533) shared across the network namespace, so under high concurrency to create containers inside a host/VM the container startup time increases linearly as the number of concurrent creations.

## Solução
The main idea of the paper solution is to pre-create and initialize network namespaces and share them to containers under its initialization. Sharing a pre-established network namespace to a container avoids the creation and initialization of a new network namespace during the container startup. 


## Avaliação e Resultados
O objetivo da avaliação é responder 4 perguntas:
  1. *What speedups do SOCK containers provide OpenLambda*?
  2. *Does built-in package support reduce cold-start latency for applications with dependencies*?
  3. *How does SOCK scale with the number of lambdas and packages*?
  4. *How does SOCK compare to other platforms for a real workload*?

Avaliação 1:
- **Metodologia**: comparar o tempo de *startup* do *container* do SOCK e Docker com 1, 10 e 20 chamadas concorrentes.
- **Métrica**: *container startup time*.
- **Resultados**: no cenário com 1 usuário concorrente o SOCK é pelo menos 6x mais rápido do que o Docker. Por outro lado, com 10 usuários concorrentes o SOCK é 18x mais rápido do que o Docker.

Avaliação 2:
- **Metodologia**: .
- **Métrica**: .
- **Cenários**: .
- **Resultados**: .
- **Crítica**: .

Avaliação 3:
- **Metodologia**: .
- **Métrica**: .
- **Cenários**: .
- **Resultados**: .
- **Crítica**: .

Avaliação 4:
- **Metodologia**: avaliação do tempo de *cold start* utilizando uma aplicação em Python que muda o tamanho de uma imagem. Para garantir o cenário de *cold start* as requisições eram realizadas após a modificação do código na plataforma.
- **Métrica**: latência das requisições, assim como o tempo de computação da função.
- **Cenários**: comparação com AWS Lambda e OpenWhisk com uma função de tamanho 8.3MB (aplicação + dependências) levando em consideração suas dependências.
- **Resultados**: “SOCK cold” has a platform latency of 365 ms, 2.8× faster than AWS Lambda and 5.3× faster than OpenWhisk. “SOCK cold” compute time is also shorter than the other compute times because all package initialization happens after the handler starts running for the other platforms, but SOCK performs package initialization work as part of the platform.

## Contribuição
A principal contribuição do *SOCK* é tentar resolver o problema de *cold start* na etapa 3 utilizando mecanismos de provisionamento para gerenciar a utilização das bibliotecas pelas funções, ou seja, o *SOCK* tenta manter em *cache* as bibliotecas mais utilizadas pelas funções para que não seja necessário reimportá-las.

## Críticas
Os resultados do *SOCK* são muito bons, pois, diminui o custo da etapa 2 e 3, porém, aparentemente para cada linguagem é necessário implementar o serviço de provisionamento específico.

- "We further generalize Zygote provisioning and build a package-aware caching system.".

É necessário que o provedor da plataforma gerencie um serviço de provisionamento do Zygote.

## Relacionados
1. [Pipsqueak: Lean Lambdas with Large Libraries] SOCK implements and extends the earlier Pipsqueak proposal for efficient package initialization.
2. [Unikernels: Library Operating Systems for the Cloud] Recent alternatives to traditional containerization are based on library operating systems, enclaves, and unikernels.
3. [Rethinking the Library OS from the Top Down] Recent alternatives to traditional containerization are based on library operating systems, enclaves, and unikernels.
4. [From Zygote to Morula: Fortifying Weakened ASLR on Android] observed that forking many child processes from the same parent without calling exec undermines address-space randomization; their solution was Morula, a system that runs exec every time, but maintains a pool of preinitialized interpreters; this approach trades overall system throughput for randomization.
