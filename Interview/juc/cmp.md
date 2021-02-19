## Hash表
    hash函数、MD5、SHA
## hash冲突
    - 线性探索（开放寻址发）
    - 链式地址法，
## CHM
    jdk1.7 -> segment 分段锁
    jdk1.8 -> node 加锁
        数组大于64，链表长度大于8，变红黑树
## 源码设计
    使用方法内本地变量存储共享变量：可以提升性能
### put第一阶段
    第一次循环,初始化Node<k,v>[] table
    第二次，new node();
### put第二阶段
    对node加锁，synchronized(f)
### put第三阶段（元素个数的统计和更新）

### AddCount() 添加元素个数 （初始化阶段）
- 直接去访问baseCount累加元素个数
- 找到CounterCell[]随机的某个下标位置，value=v+x() 表示记录元素个数
- 如果前面都失败，则进入到fullAddCount()
### AddCount() 添加元素个数 （元素个数更新阶段）

### 扩容阶段
 - 当元素个数大于阀值的时候
 - 如果此时正在扩容，会协助扩容
