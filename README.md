# my-easy-kube-scheduler
### controller文件夹
这里存放调度器过程代码
调度器的工作是：为新创建的pod在集群中寻找最合适的node，并将pod调度到该node上。
- 自定义的调度器主要可以分为两个环节：predicate 和 priorities。
  - controller/predicate.go：对节点进行按一定的条件过滤，这里利用需要被调度的pod的名称长度和随机数来对节点进行过滤。若pod的名称长度大于10，那么这个节点就有50/101的概率被批准。否则，不被批准。
  - controller/priorities.go: 对已过滤的节点进行打分操作。利用的打分规则是，利用被调度的pod的名称及其命名空间的名称的长度和对[0, 最大优先级]的随机数进行取模,求得的余数作为该节点的分数。
  最后调度器找分数最高的节点与该pod进行绑定（bind）。若有多个分数最高的节点，那么调度器将随机选取。

### kubernetes文件夹
这里存放的是调度器的配置文件
