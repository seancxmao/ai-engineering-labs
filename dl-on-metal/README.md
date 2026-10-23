# Apple Silicon, Metal

概念

* Apple Silicon 是硬件平台
* Metal 是 GPU 编程技术
* MPS 是构建在 Metal 之上的高性能计算库，PyTorch 的 mps 是使用MPS进行 GPU 计算的后端
* MLX-Metal 是面向 Apple GPU 的 Metal 后端组件，mlx 是机器学习框架的核心，它可以使用mlx-metal作为后端。

Metal 是苹果统一提供的底层 GPU 编程技术，而 PyTorch 和 MLX 各自实现了框架自己的 GPU 后端，都可以基于 Metal 利用 Apple GPU。
