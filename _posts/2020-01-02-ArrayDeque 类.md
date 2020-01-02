---
title: ArrayDeque 类
Auther: ahou
Layout: post
---

ArrayDeque 是一个基于循环数组实现的双端队列，可以作为队列和栈使用，作为队列和栈使用时，由于数组可以**根据索引直接寻址**，并且没有把**对象包装成链表节点的开销**，性能要优于LinkedList。

#### head属性指向队列的头部对象索引，tail指向将要插入对象的尾部索引。队列为空时，head与tail重合. 插入对象过程中，如果head与tail重合，进行扩容，对应grow(int needed) 方法

``` java
    private void grow(int needed) {
        // overflow-conscious code
        final int oldCapacity = elements.length;
        int newCapacity;
        // Double capacity if small; else grow by 50%
        int jump = (oldCapacity < 64) ? (oldCapacity + 2) : (oldCapacity >> 1);
        if (jump < needed
            || (newCapacity = (oldCapacity + jump)) - MAX_ARRAY_SIZE > 0)
            newCapacity = newCapacity(needed, jump);
        final Object[] es = elements = Arrays.copyOf(elements, newCapacity);
        // Exceptionally, here tail == head needs to be disambiguated
        if (tail < head || (tail == head && es[head] != null)) {
            // wrap around; slide first leg forward to end of array
            int newSpace = newCapacity - oldCapacity;
            System.arraycopy(es, head,
                             es, head + newSpace,
                             oldCapacity - head);
            for (int i = head, to = (head += newSpace); i < to; i++)
                es[i] = null;
        }
    }
```
如果原先容量小于64，扩容到原来的两倍，如果大于64，扩容原来的50%    
如果扩容增加的容量不满足needed值或者数组大小溢出(大于Integer.MAX_VALUE-8)时的处理： 
``` java
    /** Capacity calculation for edge conditions, especially overflow. */
    private int newCapacity(int needed, int jump) {
        final int oldCapacity = elements.length, minCapacity;
        if ((minCapacity = oldCapacity + needed) - MAX_ARRAY_SIZE > 0) {
            if (minCapacity < 0)
                throw new IllegalStateException("Sorry, deque too big");
            return Integer.MAX_VALUE;
        }
        if (needed > jump)
            return minCapacity;
        return (oldCapacity + jump - MAX_ARRAY_SIZE < 0)
            ? oldCapacity + jump
            : MAX_ARRAY_SIZE;   // 对应的情况是 oldCapacity + jump - MAX_ARRAY_SIZE==0
    }
```

#### 默认数组大小为16，但初始的数组大小element.length为16+1, 原因是tail指向将要出入对象的索引，

#### 实现了Deque,Queue,Stack对应的方法
包含Deque，queue 和stack的方法
双端队列：addFirst,offerFirst  getFirst, pollFirst  peekFirst  及对应的Last方法
单端队列：add,offer   get, poll  peek
栈： push   pop   peek  

####