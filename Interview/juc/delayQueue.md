## 队列元素
    DelayQueue<E extends Delayed>的队列元素需要实现Delayed接口

    由Delayed定义可以得知，队列元素需要实现getDelay(TimeUnit unit)方法和compareTo(Delayed o)方法, getDelay定义了剩余到期时间，compareTo方法定义了元素排序规则，注意，元素的排序规则影响了元素的获取顺序

## 内部存储结构　　
DelayedQuene的元素存储交由优先级队列存放。

public class DelayQueue<E extends Delayed> extends AbstractQueue<E> implements BlockingQueue<E> {
    private final transient ReentrantLock lock = new ReentrantLock();
    private final PriorityQueue<E> q = new PriorityQueue<E>();//元素存放
 DelayedQuene的优先级队列使用的排序方式是队列元素的compareTo方法，优先级队列存放顺序是从小到大的，所以队列元素的compareTo方法影响了队列的出队顺序。

若compareTo方法定义不当，会造成延时高的元素在队头，延时低的元素无法出队。   

## 获取队列元素
　　非阻塞获取
```java
    public E poll() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            E first = q.peek();
            if (first == null || first.getDelay(NANOSECONDS) > 0)
                return null;
            else
                return q.poll();
        } finally {
            lock.unlock();
        }
    }
------------------------------------------------------------------------------------------------------------------------
　　PriorityQueue队列peek()方法。
public E peek() {
    return (size == 0) ? null : (E) queue[0];
}
```
　　由代码我们可以看出，获取元素时，总是判断PriorityQueue队列的队首元素是否到期，若未到期，返回null，所以compareTo()的方法实现不当的话，会造成队首元素未到期，当队列中有到期元素却获取不到的情况。因此，队列元素的compareTo方法实现需要注意。

## 阻塞方式获取

```java
public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            for (;;) {
                E first = q.peek();
                if (first == null) //没有元素，让出线程，等待java.lang.Thread.State#WAITING
                    available.await();
                else {
                    long delay = first.getDelay(NANOSECONDS);
                    if (delay <= 0) // 已到期，元素出队
                        return q.poll();
                    first = null; // don't retain ref while waiting
                    if (leader != null) 
                        available.await();// 其它线程在leader线程TIMED_WAITING期间，会进入等待状态，这样可以只有一个线程去等待到时唤醒，避免大量唤醒操作
                    else { Thread thisThread = Thread.currentThread(); leader = thisThread; try { available.awaitNanos(delay);// 等待剩余时间后，再尝试获取元素，他在等待期间，由于leader是当前线程，所以其它线程会等待。 } finally { if (leader == thisThread) leader = null; } } } } } finally { if (leader == null && q.peek() != null) available.signal(); lock.unlock(); } }

```
