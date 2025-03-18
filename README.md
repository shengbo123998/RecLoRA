# RecLoRA
RecLoRA
https://zhuanlan.zhihu.com/p/716763547

第一个问题： 目前的模型在<img width="723" alt="image" src="https://github.com/user-attachments/assets/8be66154-d972-4c33-a1ac-be284c0aaf60" />
实现个性化，在LORA没有实现个性化

第二个问题：其次，在提示或表示层中使用终身个性化行为序列存在有效性与效率的问题。由于大型语言模型（LLMs）对输入长度敏感且有输入大小的限制，它们在处理长行为序列时表现不佳，导致有效性上限。此外，随着输入行为序列长度的增加，训练和推理所需的时间迅速增加，从而导致效率问题。

第三，现有方法在大规模工业数据集上缺乏可扩展性，主要是由于训练效率低下。即使使用LoRA技术，对推荐数据进行微调也会消耗大量的计算资源和时间，对于大规模数据集（通常涉及数百万甚至数十亿条记录）来说，效率极低。虽然有些研究建议在训练数据的小部分（例如少于10%）上微调大型语言模型（LLMs）以平衡推荐性能和训练效率，这种方法限制了大型语言模型（LLMs）对完整训练空间的了解，导致性能不佳。因此，如何在确保训练效率的同时使大型语言模型（LLMs）充分感知训练空间仍然是一个开放的研究问题。

为了解决上述问题，我们提出了一种新的大型语言模型个性化低秩适应推荐框架(即RecLORA)。具体来说，我们维护一组并行、独立的LORA权重，而不是像以前的工作44,80)中建议的那样使用单个静态ORA模块。如图1所示，这种个性化LORA模块的功能是将个性化用户知识融入大型语言模型中，作为参数个性化的策略。我们分配一个连续的CRM，并根据个性化LORA权重调整用户表示。更确切地说，我们使用软路由方法，在CRM的引导下聚合元LORA权重。RecLORA不是在输入提示中扩展用户行为序列，而是在序列CRM中分配更长的序列，显著提高了有效性，而成本增加可以忽略不计。此外，CRM在完整训练空间上进行预训练，因此根据CRM动态生成LORA权重可以起到放大镜的作用，将一小部分数据观察到的训练信号扩展到整个数据空间，而不会增加LLM的训练时间。
主要贡献总结如下:
我们提出了RecLoRA框架来解决三个主要限制:(1)为了构建推荐中LLMs的个性化低秩适应，我们采用了一个包含个性化知识的并行元LoRA机制。(2)为了缓解与行为序列扩展相关的有效性和效率问题，我们设计了一个长-短模式检索器。(3)为了将LLMs的接受域扩展到整个训练空间，我们开发了Few2Many放大策略。在本部分中，我们全面解释了我们的RecLoRA框架，包括其网络架构和训练过程。
（1）直接为每个用户训练一个LORA是不行的。： 然而，这种方法是不可扩展的，因为其模型复杂性随着用户数量的增加而线性增长，导致大量的内存消耗。此外，它忽略了不同用户行为模式之间的重叠和共性，这可能导致性能不佳
![image](https://github.com/user-attachments/assets/8fbf12ce-1b1d-4a0e-a964-59384573121a)
