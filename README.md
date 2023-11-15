# Flink学习
[笔记参考1](https://blog.csdn.net/qq_46485161/article/details/120792090?spm=1001.2014.3001.5502)</br>
[笔记参考2](https://blog.csdn.net/qq_46485161/article/details/120792350?spm=1001.2014.3001.5502)</br>
[笔记参考3](https://blog.csdn.net/qq_46485161/article/details/120804955?spm=1001.2014.3001.5502)</br>
[笔记参考4](https://blog.csdn.net/qq_46485161/article/details/120802688?spm=1001.2014.3001.5502)</br>
[笔记参考5](https://blog.csdn.net/qq_46485161/article/details/120835845?spm=1001.2014.3001.5502)</br>
[笔记参考6](https://blog.csdn.net/qq_46485161/article/details/120876642?spm=1001.2014.3001.5502)</br>
[笔记参考7](https://blog.csdn.net/qq_46485161/article/details/121003783?spm=1001.2014.3001.5502)</br>
[笔记参考8](https://blog.csdn.net/qq_46485161/article/details/121005498?spm=1001.2014.3001.5502)</br>


## Flink基本概念
### Flink Cluster
一般情况下，Flink 集群是由一个 Flink Master 和一个或多个 Flink TaskManager 进程组成的分布式系统。
### Flink Master
Flink Master 是 Flink Cluster 的主节点。它包含三个不同的组件：Flink Resource Manager、Flink Dispatcher、运行每个 Flink Job 的 Flink JobManager。
### Flink JobManager
JobManager 是在 Flink Master 运行中的组件之一。JobManager 负责监督单个作业 Task 的执行。以前，整个 Flink Master 都叫做 JobManager。
### Flink TaskManager
TaskManager 是 Flink Cluster 的工作进程。Task 被调度到 TaskManager 上执行。TaskManager 相互通信，只为在后续的 Task 之间交换数据。
### Event
Event 是对应用程序建模的域的状态更改的声明。它可以同时为流或批处理应用程序的 input 和 output，也可以单独是 input 或者 output 中的一种。Event 是特殊类型的 Record。
### Record
Record 是数据集或数据流的组成元素。Operator 和 Function接收 record 作为输入，并将 record 作为输出发出。
### Partition
分区是整个数据流或数据集的独立子集。通过将每个 Record 分配给一个或多个分区，来把数据流或数据集划分为多个分区。在运行期间，Task 会消费数据流或数据集的分区。改变数据流或数据集分区方式的转换通常称为重分区。
### Function
Function 是由用户实现的，并封装了 Flink 程序的应用程序逻辑。大多数 Function 都由相应的 Operator 封装。
### Operator
Logical Graph 的节点。算子执行某种操作，该操作通常由 Function 执行。Source 和 Sink 是数据输入和数据输出的特殊算子。
### Logical Graph
Logical Graph 是一种描述流处理程序的高阶逻辑有向图。节点是Operator，边代表输入/输出关系、数据流和数据集中的之一。
### Instance
Instance 常用于描述运行时的特定类型(通常是 Operator 或者 Function)的一个具体实例。由于 Apache Flink 主要是用 Java 编写的，所以，这与 Java 中的 Instance 或 Object 的定义相对应。在 Apache Flink 的上下文中，parallel instance 也常用于强调同一 Operator 或者 Function 的多个 instance 以并行的方式运行。
### Operator Chain
算子链由两个或多个连续的 Operator 组成，两者之间没有任何的重新分区。同一算子链内的算子可以彼此直接传递 record，而无需通过序列化或 Flink 的网络栈。
### Task
Task 是 Physical Graph 的节点。它是基本的工作单元，由 Flink 的 runtime 来执行。Task 正好封装了一个 Operator 或者 Operator Chain 的 parallel instance。
### Physical Graph
Physical graph 是一个在分布式运行时，把 Logical Graph 转换为可执行的结果。节点是 Task，边表示数据流或数据集的输入/输出关系或 partition。
### Sub Task
Sub-Task 是负责处理数据流 Partition 的 Task。”Sub-Task”强调的是同一个 Operator 或者 Operator Chain 具有多个并行的 Task 。
### Transformation
Transformation 应用于一个或多个数据流或数据集，并产生一个或多个输出数据流或数据集。Transformation 可能会在每个记录的基础上更改数据流或数据集，但也可以只更改其分区或执行聚合。虽然 Operator 和 Function 是 Flink API 的“物理”部分，但 Transformation 只是一个 API 概念。具体来说，大多数（但不是全部）Transformation 是由某些 Operator 实现的。
