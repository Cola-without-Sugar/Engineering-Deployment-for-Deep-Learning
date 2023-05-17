## Tensor-RT

（是什么？）

TensorRT是英伟达(nvdia)公司为了方便模型在推理的过程中，充分使用计算资源推出的一个推理加速的工具。

（为什么能够加速？）

![img](https://cdn.jsdelivr.net/gh/Cola-without-Sugar/markdown_img/202305151211539.webp)

* 精度校准 (Precision Calibration) ：模型在推理或训练阶段使用的数值带宽往往是32位或16位数据，这使得推理权重参数占用的显存增加且计算更加复杂。
* TensorRT支持FP16和INT8的计算。TensorRT在推理的时候可以降低模型参数的位宽来进行低精度推理，以达到加速推断的目的
* layer和tensor融合 (Layer & Tensor Fusion)：CUDA核心计算张量的速度是很快的，但是往往大量的时间是浪费在CUDA核心的启动和对每一层输入/输出张量的读写操作上面，这造成了内存带宽的瓶颈和GPU资源的浪费。
* TensorRT通过对层间的横向或纵向合并（合并后的结构称为CBR，意指 convolution, bias, and ReLU layers are fused to form a single layer），使得层的数量大大减少。横向合并可以把卷积、偏置和激活层合并成一个CBR结构，只占用一个CUDA核心。纵向合并可以把结构相同，但是权值不同的层合并成一个更宽的层，也只占用一个CUDA核心。合并之后的计算图（图4右侧）的层次更少了，占用的CUDA核心数也少了，因此整个模型结构会更小，更快，更高效。
* 内核自动调谐 (Kernel Auto-Tuning)：
  * 网络模型在推理计算时，是调用GPU的CUDA核进行计算的。TensorRT可以针对不同的算法，不同的网络模型，不同的GPU平台，进行 CUDA核的调整，以保证当前模型在特定平台上以最优性能计算
* 多数据流执行 (Multi-Stream Execution)：
  * 多数据流扩展

* 动态张量内存 (Dynamic Tensor Memory)：
  * 在每个tensor的使⽤期间，TensorRT会为其指定显存，避免显存重复申请，减少内存占⽤和提⾼重复使⽤效率。

（怎么做？）







### 附录

#### 问题记录

使用Tensor-RT进行推理，使用的效果如何？

需要部署在yolo v5-5.0版本上，所以需要设计实验进行验证
