# **[SOCK: Rapid Task Provisioning with Serverless-Optimized Containers](https://www.usenix.org/system/files/conference/atc18/atc18-oakes.pdf)**

## Abstract

Serverless computing promises to provide applications with cost savings and extreme elasticity. Unfortunately, slow application and container initialization can hurt common-case latency on serverless platforms. In this work, we analyze Linux container primitives, identifying scalability bottlenecks related to storage and network isolation.

O SOCK foi integrado à plataforma OpenLambda para a geração de resultados.

**Categoria: System proposal, Cold Start, Runtime Environment.**

## Problema
Foca no problema de [Cold Start](../../README.md) para as [etapas 2 e 3](../../README.md).

O problema da etapa 2 é justificado com a análise da escalabilidade das operações necessárias para criação de containers como:
1. Modificação de namespaces
  - **IPC**: Foi observado que possui uma latência “inofensiva” e a operação não se degrada com o aumento de uso concorrente.

  - **Mounts**: Foi observado que possui uma latência “inofensiva” e a operação não se degrada com o aumento de uso concorrente.

  - **Network**: Foi observado que a latência da operação de criação do namespace se degrada à medida que o uso concorrente aumenta.

2. Isolamento do sistema de arquivos
  - Duas estratégias foram comparadas:
    1. **AUFS**:
      - Possui um bom desempenho e a performance não se degrada à medida que o número usuários concorrentes aumenta.
    2. **Bind**:
      - Possui um bom desempenho e a performance não se degrada à medida que o número usuários concorrentes aumenta.
      - Normalmente 2 vezes mais rápido do que o AUFS.

3. Criação de cgroups
  - Não sofre com problemas de escalabilidade, porém o reuso favorece o aumento da quantidade de operações por segundo.

O problema da etapa 3 é justificado com a análise do tempo para médio para importar os pacotes de Python mais populares no repositório PyPI. O valor do tempo médio para o *import* é de 100ms, se uma aplicação *serverless* utiliza vários pacotes com o objetivo de realizar o reuso de código, então o tempo para começar a responder à uma requisição pode ser longo.


## Solução
O artigo tem como foco principal construir uma solução de *containerização* focada em aplicações *serverless* e um provisionador focado no carregamento proativo das bibliotecas mais utilizadas entre aplicações.

Para diminuir o tempo de *Cold Start* da **etapa 2** foi criada uma solução de containerização com as seguintes melhorias:
  1. SOCK utiliza *Bind Mounts* ao invés do AUFS para montar o sistema de arquivos, além disso, utiliza operações de */chroot* mais simples e eficientes.

  2. SOCK faz a reutilização de *cgroups* e não aplica o *unshare* dos *namespaces* de *mount* e *network*.

Para diminuir o tempo de *Cold Start* da **etapa 3** foi criado um sistema de provisionamento que pré-importa as bibliotecas de Python mais utilizadas pelas aplicações:
  - Ideia do Zygote utilizado no Android para aplicações Java, que era utilizar cache para os pacotes mais utilizados, e dessa forma as aplicações fazem copy-on-write dos pacotes das bibliotecas, eliminando a necessidade de carregar os pacotes do disco e a duplicação de memória.
  - Os processos das funções são filhos dos processos do Zygote que já possui algumas bibliotecas pre-importadas.


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
