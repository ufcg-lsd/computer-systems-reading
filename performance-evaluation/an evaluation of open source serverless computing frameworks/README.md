# **[An evaluation of open source serverless computing frameworks](https://users.aalto.fi/~premsag1/docs/mohanty2018serverless.pdf)**

## Abstract

Recent advancements in virtualization and software architecture have led to the new paradigm of serverless computing, which allows developers to deploy applications as stateless functions without worrying about the underlying infrastructure. Accordingly, a serverless platform handles the lifecycle, execution and scaling of the actual functions; these need to run only when invoked or triggered by an event. Thus, the major benefits of serverless computing are low operational concerns and efficient resource management and utilization. Serverless computing is currently offered by several public cloud service providers. However, there are certain limitations on the public cloud platforms, such as vendor lock-in and restrictions on the computation of the functions. Open source serverless frameworks are a promising solution to avoid these limitations and bring the power of serverless computing to on-premise deployments. However, these frameworks have not been evaluated before. Thus, we carry out a comprehensive feature comparison of popular open source serverless computing frameworks. We then evaluate the performance of selected frameworks: Fission, Kubeless and OpenFaaS. Specifically, we characterize the response time and ratio of successfully received responses under different loads and provide insights into the design choices of each framework.

**Categoria: performance evaluation, benchmarking, serverless computing, functions-as-a-service.**

## Problema e Solução
Utilizar plataformas públicas de computação em núvem para aplicações serverless possue limitações. Uma alternativa para contornar essas limitações seria utilizar plataformas de código aberto. Contudo, essas plataformas até então não tiveram seu desempenho avaliado. Sendo assim, esse artigo provê uma comparação de desempenho de plataformas serverless de código aberto, avaliando sob diferente cargas de trabalho o tempo de resposta e a taxa de respostas com sucesso.   

## Avaliação e Resultados



## Conclusão 
Os experimentos executados evidenciam que o Kubeless possue maior performace consistente dentre as 3 plataformas, sendo seu desempenho justificado por sua arquitetura simples. Também conclui-se que para funções simples, Fission e OpenFaaS mantiveram baixa média e mediana de latência, mas que para funções de uso mais intenso da CPU esses frameworks em si devem ser escalados a fim de evitar gargalos individuais de seus componentes. 

## Críticas

## Relacionados
