# **[SAND: Towards High-Performance Serverless Computing](https://www.usenix.org/system/files/conference/atc18/atc18-akkus.pdf)**

## Abstract

This paper presents SAND, a new serverless computing system that provides lower latency, better resource efficiency and more elasticity than existing serverless platforms. To achieve these properties, SAND introduces two key techniques: 1) application-level sandboxing, and 2) a hierarchical message bus. We have implemented and deployed a complete SAND system. Our results show that SAND outperforms the state-of-the-art serverless platforms significantly. For example, in a commonly-used image processing application, SAND achieves a 43% speedup compared to Apache OpenWhisk.

**Categoria: System proposal, Cold Start, Runtime Environment.**

## Problema
Endereça o problema de cold-start e como as plataformas comerciais fizeram para mitigar: utilizando uma pool de containers quentes, porém a solução desperdiça recursos.

O artigo tem como foco principal construir uma plataforma serverless que reduz os desperdício de recursos (o cold-start). Além disso, tenta mitigar o problema de encadeamento de chamada de funções.

## Solução
A ideia do SAND é que cada função deployed tenha seu sandbox, que é um container, e o container poderá executar quantas funções for possível, basicamente funciona como o OpenFaaS.

Para mitigar o problema de Cold Start, quando o container da função é levantado, é criado um grain worker que carrega o código da função e escuta as mensagens que chegam no message bus para processá-las.

Assim, quando uma requisição chega o código já está carregado e grain worker se clona para criar uma grain instance e executar o código da função.

## Avaliação e Resultados
Foram realizadas duas avaliações de desempenho comparando a performance de cold e warm start do SAND com o OpenWhisk e AWS Greengrass.

Avaliação 1:
- **Metodologia**: duas funções em Python *F1* e *F2* em que *F1* chama *F2*.
- **Métrica**: *F2* startup timestamp - *F1* end of execution timestamp.
- **Cenários**: ***Cold*** (quando as funções não possuiam nenhum container em estado *warm*) e ***Warm*** (quando as funções possuiam containers em estado *warm*).
- **Resultados**: ***Warm*** SAND outperforms OpenWhisk and Greengrass, both with warm containers, by 8.32× and 3.64×. ***Cold*** SAND’s speedups are 562× and 358× compared to OpenWhisk and Greengrass with cold containers.
- **Crítica**: a comparação do cenário ***Cold*** do SAND com o AWS Greengrass e OpenWhisk não é justa pelo fato do SAND não realizar o escalonamento de *containers*.

Avaliação 2:
- **Metodologia**: um *pipeline* de processamento de imagem com 4 funções consecutivas em Python.
- **Métrica**: cada função computava seu tempo de execução, então duas métricas são calculadas, o tempo total para executar o pipeline (TT) e tempo de computação (TC) das 4 funções. TT - TC pode ser usado como uma estimativa do preço do *cold start*.
- **Cenários**: Comparação dos resultados do SAND com AWS Step Functions, IBM Cloud Functions e OpenWhisk.
- **Resultados**: Compared to other platforms, we find that SAND achieves the lowest overhead for running a series of functions. For example, SAND reduces the overhead by 22.0× compared to OpenWhisk, even after removing the time spent on extra functionality not supported in SAND yet (e.g., authentication and authorization). We notice that OpenWhisk re-launches the Python interpreter for each function invocation, so that libraries are loaded before a request can be handled. In contrast, SAND’s grain worker loads a function’s libraries only once, which are then shared across forked grain instances handling the requests.
- **Crítica**: Apesar de apresentar o melhor resultado, não é possível ter uma comparação confiável com outras plataformas como AWS Step Functions e IBM Cloud Functions pelo fato da infraestrutura ser naturalmente diferente, o SAND e OpenWhisk foram avaliados na mesma infraestrutura local.

Em teoria seria esperado que a utilização de memória do SAND fosse maior do que as soluções do OpenWhisk, sempre que o container é levantado o código da função já é carregado, porém, com o aumento da utilização das funções o OpenWhisk precisa alocar e manter uma poll de containers aumentando significativamente o uso de memória.

## Contribuição
O SAND tenta resolver o problema da etapa 3 no container, ou seja, não é necessário que o provedor da *cloud* gerencie nenhum tipo de serviço de provisionamento.

## Críticas
O artigo teve bons resultados de diminuição de cold start, mas talvez as comparações que eles fizeram com outras plataformas não foram justas pelo fato do SAND não se preocupar com a elasticidade, mas as plataformas comparadas sim (overhead da orquestração de containers).

Os resultados do SAND são bons pelo fato de eliminar a etapa 3 do cold start, contudo, SAND possui um suporte limitado à linguagens:

- "Non-fork Runtime Support. SAND makes a trade-off to balance performance and isolation by using process forking for function executions. The downside is that SAND currently does not support language runtimes without native forking (e.g., Java and NodeJS)".

Além disso, um trade-off do SAND é que quando a carga for grande, um container talvez não tenha recursos suficientes para computar bem todas as funções, por exemplo, gargalo de CPU.

## Relacionados
1. [Slacker: Fast distribution with lazy docker containers] identifies packages that are critical when launching a container.
2. [Cloud event programming paradigms: Applications and analysis] investigated latencies in existing serverless frameworks.
3. [SOCK: Rapid task provisioning with serverless-optimized containers] create a cache of pre-warmed Python interpreters, so that functions can be launched with an interpreter that has already loaded the necessary libraries.
4. [Serverless computing: Design, implementation, and performance] proposed a queuing scheme with workers announcing their availability in warm and cold queues, where containers can be reused and new containers can be created, respectively
