# computer-systems-reading

Paper trail for the UFCG/LSD reading group on systems.

## Caching 2023.2

1. [FIFO can be Better than LRU: the Power of Lazy Promotion and Quick Demotion](https://dl.acm.org/doi/pdf/10.1145/3593856.3595887). Juncheng Yang, Ziyue Qiu, Yazhuo Zhang, Yao Yue, and K. V. Rashmi. HotOS 2023.
2. [TinyLFU: A Highly Efficient Cache Admission Policy](https://arxiv.org/pdf/1512.00727.pdf). Gil Einziger, Roy Friedman, Ben Manes. Euromicro International Conference on Parallel, Distributed and Network-Based Processing (PDP) 2015.
3. [The CacheLib Caching Engine: Design and Experiences at Scale](https://www.usenix.org/system/files/osdi20-berg.pdf). Benjamin Berg, Daniel S. Berger, Sara McAllister, Isaac Grosof, Sathya Gunasekar, Jimmy Lu, Michael Uhlar, Jim Carrig, Nathan Beckmann, Mor Harchol-Balter, Gregory R. Ganger. USENIX 2020.
4. [Flashield: a Hybrid Key-value Cache that Controls Flash Write Amplification](https://www.usenix.org/system/files/nsdi19-eisenman.pdf). Assaf Eisenman, Asaf Cidon, Evgenya Pergament, Or Haimovich, Ryan Stutsman, Mohammad Alizadeh, Sachin Katti. USENIX 2019.
5. [Adaptive Online Cache Capacity Optimization via Lightweight Working Set Size Estimation at Scale](https://www.usenix.org/system/files/atc23-gu.pdf). Rong Gu, Simian Li, Haipeng Dai, Hancheng Wang, Yili Luo, Bin Fan, Ran Ben Basat, Ke Wang, Zhenyu Song, Shouwei Chen, Beinan Wang, Yihua Huang, Guihai Chen. USENIX 2023.
6. [FairRide: Near-Optimal, Fair Cache Sharing](https://www.usenix.org/system/files/conference/nsdi16/nsdi16-paper-pu.pdf). Qifan Pu and Haoyuan Li, Matei Zaharia, Ali Ghodsi, Ion Stoica. USENIX 2016.
7. [CacheSack: Theory and Experience of Google’s Admission Optimization for Datacenter Flash Caches](https://dl.acm.org/doi/pdf/10.1145/3582014). Tzu-Wei Yang, Seth Pollen, Mustafa Uysal, Arif Merchant, Homer Wolfmeister, and Junaid Khalid. ACM Trans 2023.
8.  [Flexible and efficient multi-tenant persistent memory caching](https://www.usenix.org/system/files/fast22-wu.pdf). Kan Wu, Kaiwei Tu, Yuvraj Patel, Rathijit Sen, Kwanghyun Park, Andrea Arpaci-Dusseau, Remzi Arpaci-Dusseau. USENIX 2022.
9.  [Memshare: a Dynamic Multi-tenant Key-value Cache](https://www.usenix.org/system/files/conference/atc17/atc17-cidon.pdf). Asaf Cidon, Daniel Rushton, Stephen M. Rumble, Ryan Stutsman. USENIX 2017.
10. [dCat Dynamic Cache Management for Efficient, Performance-sensitive Infrastructure-as-a-Service](https://dl.acm.org/doi/pdf/10.1145/3190508.3190555). Cong Xu, Karthick Rajamani, Alexandre Ferreira, Wesley Felter, Juan Rubio, and Yang Li. EuroSys 2018.
11. [Dynamic Partitioning of Shared Cache Memory](https://link.springer.com/article/10.1023/B:SUPE.0000014800.27383.8f). G. E. Suh, L. Rudolph & S. Devadas. The Journal of Supercomputing 2004.
12. [An analytical model for cache replacement policy performance](https://dl.acm.org/doi/10.1145/1140103.1140304). Fei Guo, Yan Solihin. Association for Computing Machinery 2006.
13. [Implementation of Multi-tenancy Adaptive CacheAdmission Model](https://iopscience.iop.org/article/10.1088/1742-6596/2026/1/012045/pdf). Huajun Maet al. Journal of Physics: Conference Series 2021.
14. [Software-Defined Caching: Managing Caches in Multi-Tenant Data Centers](https://dl.acm.org/doi/10.1145/2806777.2806933). Ioan Stefanovici, Eno Thereska, Greg O’Shea, Bianca Schroeder, Hitesh Ballani, Thomas Karagiannis, Antony Rowstron, Tom Talpey. SoCC '15: Proceedings of the Sixth ACM Symposium on Cloud Computing 2015.
15. [Move Fast and Meet Deadlines: Fine-grained Real-time Stream Processing with Cameo](https://www.usenix.org/system/files/nsdi21-xu.pdf). Le Xu, Shivaram Venkataraman, Indranil Gupta, Luo Mai, Rahul Potharaju. USENIX 2021.
16. [Henge: Intent-driven Multi-Tenant Stream Processing](https://lexu1.web.engr.illinois.edu/files/henge-cr.pdf). Faria Kalim, Le Xu, Sharanya Bathey, Richa Meherwal, Indranil Gupta. SoCC '18: Proceedings of the ACM Symposium on Cloud Computing October 2018.

## SOCC 2022

https://acmsocc.org/2022/schedule.html

1. [DeepScaling: microservices autoscaling for stable CPU utilization in large scale cloud systems.](https://dl.acm.org/doi/pdf/10.1145/3542929.3563469)
    * when: TBD
    * who: Ítallo Silva

1. [How to Fight Production Incidents? An Empirical Study on a Large-scale Cloud Service.](https://dl.acm.org/doi/abs/10.1145/3542929.3563482)
    * when: TBD
    * who: Wesley Santos

## Previous editions

### (2020.2)

1. Cadden, J., Unger, T., Awad, Y., Dong, H., Krieger, O., & Appavoo, J. SEUSS: skip redundant paths to make serverless fast. EuroSys 2020
    * when: [05/oct]
    * who: Paulo Feitosa
    * preso: [link](https://docs.google.com/presentation/d/1jOMqBlnAhanPUS4p4aVZurtxKrPcRTlUp4fr8KiPmIY/edit?usp=sharing)
  
2. Serverless in the Wild: Characterizing and Optimizing the Serverless Workload at a Large Cloud Provider, USENIX ATC 2020
   * when: [13/10]
   * who: Matheus Lacerda

3. Carver: [Finding Important Parameters for Storage System Tuning, USENIX FAST 2020](https://www.usenix.org/conference/fast20/presentation/cao-zhen)
   * when: [27/oct]
   * who: João Mafra
   
4. [Read as Needed: Building WiSER, a Flash-Optimized Search Engine](https://www.usenix.org/system/files/fast20-he.pdf)
   * when: [10/nov]
   * who: Gabriel Tavares
