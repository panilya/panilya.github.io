+++ 
author = "Illia Pantsyr" 
title = "Could GPU hardware failures limit AI training scaling?" 
date = "2024-11-23" 
tags = [ "llm", "ai"] 
+++

A fresh analytical [report](https://epoch.ai/blog/hardware-failures-wont-limit-ai-scaling) from Epoch AI on the continued scaling of training capacity. In this work, they attempt to answer the following question: to what extent is scaling limited by hardware issues?

Graphics processing units (GPUs) can fail during training for various reasons: memory damage, disconnections/restarts, or network issues. Even a single slightly slowed-down GPU can become a bottleneck for the entire system if not replaced promptly.

When Meta trained its largest model, Llama 3.1 with 405B parameters, on 16,000 GPUs, there were over 400 hardware failures over 54 days—roughly one every three hours. If this is scaled up to setups with over a million GPUs, failures would occur every few minutes.

A failure almost always results in the loss of data in memory, disrupting training. To mitigate this, models are regularly saved during training (this process is called “checkpointing”) to preserve the training state (including the model itself and optimizer statistics). This allows recovery from a recent point in training immediately after a failure, enabling the process to continue.

However, saving checkpoints takes time, and training cannot proceed if the time spent saving, loading, and synchronizing exceeds the interval between hardware failures. For instance, during the training of the Llama 3.1 405B model, progress was saved to storage with a throughput of 2 TB/s. Saving the necessary ~5 TB of information took about 2.5 seconds.

If we fix the model size, maintain storage bandwidth, and keep the hardware failure frequency constant, training could theoretically scale up to ~70 million GPUs. However, on such a massive cluster, it’s likely that even larger models would be trained (this is more efficient in terms of final quality), and as model size grows, so does the amount of data that needs to be saved.

The authors estimate that with the current commonly accepted pace of scaling, clusters could grow to ~4 million GPUs, which is still more than what’s planned before 2030 (rumors suggest a target of 1M chips). And this is without leveraging advanced saving techniques (e.g., reserving part of the memory on all GPUs and splitting the model among them. Such a subnet within the GPUs themselves is faster than external storage. More details on this in the article).

Thus, these kinds of issues (for now) do not limit scaling. Overcoming hardware failures will remain a serious engineering challenge, requiring efficient on-the-fly GPU replacements, maintenance, and safeguards against unforeseen events. But this impacts only the speed of training, not its feasibility.

### Citing / Source

Alexander Erben and Ege Erdil (2024), "Hardware Failures Won’t Limit AI Scaling". Published online at epoch.ai. Retrieved from: 'https://epoch.ai/blog/hardware-failures-wont-limit-ai-scaling' [online resource]
