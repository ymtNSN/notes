## reentrantLock(重入锁)
## reentrantReadWWriteLock（重入读写锁）
    读多写少的情况
    读和读不互斥
    写和读互斥
    写和写互斥
## 思考锁的设计
    （互斥）
    锁的互斥特性-> 共享资源 -> 标记（0无锁，1有锁）
    没有抢占到锁的线程？ —> 释放CPU资源（等待->唤醒）
    等待的线程怎么存储? -> 数据结构去存储一些列等待中的线程，FIFO
    公平和非公平（能否插队）
    重入的特性（识别是否同一个？线程）
    
    （技术方案）
    volatile state
    wait/notify|condition 需要唤醒指定线程，LockSupport.park()-> unpark() unsafe中的方法
    双向链表
    逻辑层面实现
    在某一个地方存储当前获得锁的线程ID,判断下次抢占锁的线程是否为同一个。