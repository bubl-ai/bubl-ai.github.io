---
title: What is Quantization?
date: 2024-04-01 12:00:00 -0
categories: [LLM, Quantization]
tags: []
---

Operating an LLM on a simple computer can be a challenge and this is where quantization plays a critical role as it reduces the precision of numbers in the model. For example, it might change from 32-bit floating point to 4-bit integers. But there's a trade-off: while this makes the model more efficient, it can significantly reduce accuracy because it can't no longer represent subtle differences as well.

There are two main ways to quantize LLMs:

1. **Post-Training Quantization (PTQ)**: This method quantizes a model after it's already trained. It adjusts the model's parameters and activations to lower precision.

2. **Quantization-Aware Training (QAT)**: Unlike PTQ, QAT considers quantization during the training process itself. The model learns to work with lower precision from the start. This helps mitigate accuracy loss compared to post-training quantization. QAT aims to find a better balance between model efficiency and accuracy by incorporating quantization into training.

In summary, the goal of quantization is to make the model use fewer computational resources while keeping its performance as high as possible. Here is a more detailed list of the advantages to consider:

- **Efficiency & Compatibility**: Smaller models with lower precision weights use less bandwidth on networks and work well with different hardware.

- **Speed & Resource Optimization**: Lower precision calculations mean faster computations and inference, important for real-time applications and efficient resource use.

- **Scalability & Deployment**: With lower memory footprint, it's easier to scale deployment to meet growing usage demands.

- **Memory & Storage Savings**: Reduced memory usage and smaller model sizes mean they can run on devices with limited memory, reducing costs.

Examples on how to use LlamaIndex to do quantization can be found [in our repository](https://github.com/bubl-ai/llamaindex-project/blob/main/examples/quantization/mistral_quantization.ipynb), and on [this post by LLamaIndex](https://twitter.com/llama_index/status/1711049190649061786).