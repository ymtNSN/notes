ThreadLocal声明的变量是线程私有的成员变量，每个线程都有该变量的副本，线程对变量的修改对其他线程不可见

InheritableThreadLocal声明的变量同样是线程私有的，但是子线程可以使用同样的InheritableThreadLocal类型变量从父线程继承InheritableThreadLocal声明的变量，父线程无法拿到其子线程的。即使可以继承，但是子线程对变量的修改对父线程也是不可见的。

原理
## ThreadLocal原理：


Thread有一个类型为ThreadLocal.ThreadLocalMap变量：threadLocals，ThreadLocalMap底层是一个Entry数组，用于保存ThreadLocal引用和真正变量（程序中销售人的主管姓名）的值

看下面的代码和注释了解如何将值放入threadLocals的

调用ThreadLocal对象的set()方法
​```java
   public void set(T value) {
        //获取到当前线程对象
        Thread t = Thread.currentThread();
        //从当前线程对象中拿threadLocals变量
        ThreadLocalMap map = getMap(t);
        //如果threadLocals不为空，则调用它的set方法，将key=当前ThreadLocal对象 value=主管姓名
        if (map != null)
            map.set(this, value);
        //如果threadLocals为空，则新创建一个ThreadLocalMap
        else
            createMap(t, value);
    }
 ​```java

## InheritableThreadLocal原理：


InheritableThreadLocal继承自ThreadLocal，因此可以想到它自己要么重写，要么新增了部分方法。从源码看只是重写了childValue、getMap、createMap方法，其中getMap和createMap都是父类set方法中使用到的，因此主要是改变了父类set方法的行为。

getMap获取的是InheritableThreadLocal类型的变量

createMap创建的是InheritableThreadLocal类型的实例

但是从这点上看是不能达到子线程继承父线程的变量的。再看看Thread的init方法中比以前新增了如下代码：

//如果当前线程的InheritableThreadLocal类型变量inheritThreadLocals为空且父线程的
//inheritThreadLocals不为空，则复制父线程的inheritThreadLocals中的内容给当前线程的
//inheritThreadLocals，是值复制不是引用赋值
if (inheritThreadLocals && parent.inheritableThreadLocals != null)
            this.inheritableThreadLocals =
                ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
上面说到重写了三个方法，其中的childValue还没有找到使用的地方，现在该它出场了，就是在继承变量的时候，创建新的ThreadLocalMap使用到了。
​```java
createInheritedMap->ThreadLocalMap->{

//创建一个新的Entry数组

table = new Entry[len];
//复制数据

Object value = key.childValue(e.value);
Entry c = new Entry(key, value);
table[h] = c;
}
​```java
​```java
测试代码：
ThreadLocal测试
写一个销售人类，该类有一个主管人员成员变量（ThreadLocal声明），将销售人对象传给不同的工作线程，在工作线程中打印销售人的主管，其结果应该是为NULL，因为主管是声明为线程本地变量，是不能共享的

package com.jv.jdk.thread.threadlocal;
​
import lombok.Data;
​
@Data
public class Salesman {
    private ThreadLocal<String> charger = new ThreadLocal<>();
    public Salesman(String charger){
        this.charger.set(charger);
    }
}
package com.jv.jdk.thread.threadlocal;
​
​
import lombok.extern.log4j.Log4j2;   //@Log4j2注解可以很方便做日志输出
​
import java.util.concurrent.CountDownLatch;
​

@Log4j2
public class Worker implements Runnable{
    private Salesman salesman = null;
    private CountDownLatch countDownLatch = null;
​
    public Worker(Salesman salesman, CountDownLatch countDownLatch){
        this.salesman = salesman;
        this.countDownLatch = countDownLatch;
    }
    @Override
    public void run() {
        log.info(Thread.currentThread().getName()+":"+salesman.getCharger().get());
        countDownLatch.countDown();
    }
}

```
测试类

   @Test
    public void threadLocal(){
        CountDownLatch countDownLatch = new CountDownLatch(2);
        Salesman salesman = new Salesman("刘墙东");
        Thread thread_1 = new Thread(new Worker(salesman,countDownLatch));
        Thread thread_2 = new Thread(new Worker(salesman,countDownLatch));
        thread_1.start();
        thread_2.start();
        try {
            countDownLatch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
测试结果：

2018.08.30 at 14:47:45 CST 14-Thread-1 INFO  com.jv.jdk.thread.threadlocal.Worker run() @20  - Thread-1:null
2018.08.30 at 14:47:45 CST 15-Thread-2 INFO  com.jv.jdk.thread.threadlocal.Worker run() @20  - Thread-2:null
InheritableThreadLocal测试
与上面的代码重写SalesMan类
​```java
package com.jv.jdk.thread.threadlocal;
​
import lombok.Data;
​
@Data
public class SalesmanInherit extends Salesman{
    private InheritableThreadLocal<String> charger = new InheritableThreadLocal<>();
    public SalesmanInherit(String charger){
        super(charger);
        this.charger.set(charger);
    }
}
   @Test
    public void inheritablThreadLocal(){
        CountDownLatch countDownLatch = new CountDownLatch(2);
        Salesman salesman = new Salesman("刘墙东");
        SalesmanInherit salesmanInherit = new SalesmanInherit("刘墙东");
        Thread thread_1 = new Thread(new Worker(salesmanInherit,countDownLatch));
        Thread thread_2 = new Thread(new Worker(salesmanInherit,countDownLatch));
        thread_1.start();
        thread_2.start();
        try {
            countDownLatch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    ​```java
测试结果：

2018.08.30 at 14:51:28 CST 15-Thread-2 INFO  com.jv.jdk.thread.threadlocal.Worker run() @20  - Thread-2:刘墙东
2018.08.30 at 14:51:28 CST 14-Thread-1 INFO  com.jv.jdk.thread.threadlocal.Worker run() @20  - Thread-1:刘墙东