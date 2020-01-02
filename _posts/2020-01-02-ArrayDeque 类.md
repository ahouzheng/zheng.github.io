---
title: ArrayDeque 类
Auther: ahou
Layout: post
---

ArrayDeque 是一个基于循环数组实现的双端队列，可以作为队列和栈使用，作为队列和栈使用时，由于数组可以**根据索引直接寻址**，并且没有把**对象包装成链表节点的开销**，性能要优于LinkedList。

#### head属性指向队列的头部对象索引，tail指向将要插入对象的尾部索引。队列为空时，head与tail重合

#### 默认数组大小为16，但初始的数组大小element.length为16+1, 原因是tail指向将要出入对象的索引，

