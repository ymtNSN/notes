# Print、Thread.sleep 就可以引发更新变量值

# lock 汇编指令
  总内存、缓存（一级、二级。。。）

## CPU 层面的高速缓存

# 总线锁

# 缓存锁

  # 缓存一致性协议
    MESI、MSI、MOSI

    MESI -> 表示四种缓存状态
    修改、独占、共享、失效

# 重排序
    通过内存屏障禁止了指令重排序

# java内存模型（不同的CPU、不同操作系统）、抽象的命令

## 内存屏障指令
    屏障类型：loadload、storeload、loadstore、storestore

# javap -v Demo.class 看指令

# happens-before模型
    1.程序顺序规则
        不能改变程序的执行结果（在单线程下，执行结果不变）
        依赖关系，如果两个指令存在依赖关系，则不能重排序
    2.传递性规则
    3.volatile规则
        volatile变量规则：volatile写不允许前面的其他变量重排序
    4.监视器锁规则
    5.start规则
    6.join规则