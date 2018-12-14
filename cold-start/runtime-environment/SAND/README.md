# **[SAND: Towards High-Performance Serverless Computing](https://www.usenix.org/system/files/conference/atc18/atc18-akkus.pdf)**

This paper presents SAND, a new serverless computing system that provides lower latency, better resource efficiency and more elasticity than existing serverless platforms. To achieve these properties, SAND introduces two key techniques: 1) application-level sandboxing, and 2) a hierarchical message bus. We have implemented and deployed a complete SAND system. Our results show that SAND outperforms the state-of-the-art serverless platforms significantly. For example, in a commonly-used image processing application, SAND achieves a 43% speedup compared to Apache OpenWhisk.


# Problema
Endereça o problema de cold-start e como as plataformas comerciais fizeram para mitigar: utilizando uma poll de containers quentes, porém a solução desperdiça recursos.
    
O artigo tem como foco principal construir uma plataforma serverless que reduz os desperdício de recursos (o cold-start). Além disso, tentar mitigar o problema de encadeamento de chamada de funções.

## Solução
A ideia do SAND é que cada função deployed tenha seu sandbox, que é um container, e o container poderá executar quantas funções for possível, basicamente funciona como o OpenFaaS.
    
Para mitigar o problema de Cold Start, quando o container da função é levantado, é criado um grain worker que carrega o código da função e escuta as mensagens que chegam no message bus para processá-las.

Assim, quando uma requisição chega o código já está carregado e grain worker se clona para criar uma grain instance e executar o código da função.

## Resultados
Foram realizadas duas avaliações de desempenho comparando a performance de cold e warm start do SAND com o OpenWhisk e AWS Greengrass.

A primeira avaliação tinha como foco principal avaliar o desempenho no cenário de chamada encadeada de funções:
- "SAND incurs a significantly shorter (Python) function interaction latency. SAND outperforms OpenWhisk and Greengrass, both with warm containers, by 8.32× and 3.64×, respectively. Furthermore, SAND’s speedups are 562× and 358× compared to OpenWhisk and Greengrass with cold containers".

A segunda avaliação foi em um cenário de um pipeline para o processamento de imagens utilizando Python com 4 funções: 
- "Compared to other platforms, we find that SAND achieves the lowest overhead for running a series of functions. For example, SAND reduces the overhead by 22.0× compared to OpenWhisk, even after removing the time spent on extra functionality not supported in SAND yet (e.g., authentication and authorization). We notice that OpenWhisk re-launches the Python interpreter for each function invocation, so that libraries are loaded before a request can be handled. In contrast, SAND’s grain worker loads a function’s libraries only once, which are then shared across forked grain instances handling the requests."

Nesta segunda avaliação não foi possível ter resultados de comparações confiáveis com outras plataformas como AWS Step Functions e IBM Cloud Functions pelo fato da infraestrutura ser naturalmente diferente, o SAND e OpenWhisk foram avaliados na mesma infraestrutura local.

Em teoria seria esperado que a utilização de memória do SAND fosse maior do que as soluções do OpenWhisk, sempre que o container é levantado o código da função já é carregado, porém, com o aumento da utilização das funções o OpenWhisk precisa alocar e manter uma poll de containers aumentando significativamente o uso de memória.

## Críticas
O artigo teve bons resultados de diminuição de cold start, mas talvez a comparação que eles fizeram com outras plataformas não foram justas pelo fato do SAND não se preocupar com a elasticidade, mas as plataformas comparadas sim (overhead da orquestração de containers).
    
Os resultados do SAND são bons pelo fato de eliminar a etapa 3 do cold start, contudo, SAND possui um suporte limitado à linguagens:

- "Non-fork Runtime Support. SAND makes a trade-off to balance performance and isolation by using process forking for function executions. The downside is that SAND currently does not support language runtimes without native forking (e.g., Java and NodeJS)".
    
O trade-off do SAND é que quando a carga for grande, um container talvez não tenha recursos suficientes para computar bem todas as funções, por exemplo, gargalo de CPU.

