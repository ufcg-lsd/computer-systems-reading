# **[Evaluation of Production Serverless Computing Environments](http://dsc.soic.indiana.edu/publications/Evaluation_of_Production_Serverless_Computing_Environments.pdf)**

## Abstract
Serverless computing provides a small runtime container to execute lines of codes without infrastructure management which is similar to Platform as a Service (PaaS) but a functional level. Amazon started the event-driven compute named Lambda functions in 2014 with a 25 concurrent limitation, but it now supports at least a thousand of concurrent invocation to process event messages generated by resources like databases, storage and system logs. Other providers, i.e., Google, Microsoft, and IBM offer a dynamic scaling manager to handle parallel requests of stateless functions in which additional containers are provisioning on new compute nodes for distribution. However, while functions are often developed for microservices and lightweight workload, they are associated with distributed data processing using the concurrent invocations. We claim that the current serverless computing environments can support dynamic applications in parallel when a partitioned task is executable on a small function instance. We present results of throughput, network bandwidth, a file I/O and compute performance regarding the concurrent invocations. We deployed a series of functions for distributed data processing to address the elasticity and then demonstrated the differences between serverless computing and virtual machines for cost efficiency and resource utilization.

**Category: performance evaluation, benchmarking, serverless computing, functions-as-a-service, public cloud infrastructure.**

## Links
- [IEEEXplore document](https://ieeexplore.ieee.org/document/8457830)
- [ResearchGate publication](https://www.researchgate.net/publication/324362882_Evaluation_of_Production_Serverless_Computing_Environments)
- [Digital Science Center, Indiana University Bloomington (pdf)](http://dsc.soic.indiana.edu/publications/Evaluation_of_Production_Serverless_Computing_Environments.pdf)
- [Github repository](https://github.com/lee212/FaaS-Evaluation)