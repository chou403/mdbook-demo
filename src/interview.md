# Java 中线程的状态

5 种状态一般是针对传统的线程状态来说（操作系统层面）

![image-20230823173134948](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823173134948.png)

Java 中给线程准备了 6 种状态

![image-20230823173427644](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823173427644.png)

![image-20230823173819528](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230823173819528.png)



NEW：Thread 对象被创建出来，但是还没有执行 start 方法

RUNNABLE：Thread 对象调用了 start 方法，就为 RUNNABLE 状态（CPU 调度/没有调度）

BLOCKED：synchronized 没有拿到同步锁，被阻塞的情况

WAITING：调用 wait 方法就会处于 WAITING 状态，需要被手动唤醒

TIMED_WAITING：调用 sleep 方法或者 join 方法，会被自动唤醒，无需手动唤醒

TERMINATED：run 方法执行完毕，线程生命周期到头了

> **1）新建状态（NEW）**: 当我们创建一个新的Thread对象时，该线程就处于新建状态，例如：Thread t = new Thread();
>
> **2）可运行状态（RUNNABLE）**: 当线程对象调用start()方法后，线程进入可运行状态。在这个状态下，线程已经做好了准备，随时等待CPU调度执行，这个状态包括了"就绪"和"运行"状态。
>
> **3）阻塞状态（BLOCKED）**: 线程在等待获取一个锁以进入或重新进入同步代码块时，它会进入阻塞状态。只有当该锁被释放并且线程被调度去获取这个锁，线程才能转换到RUNNABLE状态。
>
> **4）等待状态（WAITING）**: 线程进入等待状态，是因为它调用了其它线程的join方法，或者调用了无参数的wait方法。在这种情况下，线程会等待另一个线程的操作完成或者等待notify/notifyAll消息。
>
> **5）定时等待状态（TIMED_WAITING**）: 线程进入定时等待状态，是因为它调用了sleep或者带有指定时间的wait或join方法。在指定的时间过去之后，线程会自动返回RUNNABLE状态。如果它是由于调用wait或join方法进入的定时等待状态，还需要等待notify/notifyAll消息或者等待join的线程终止。
>
> **6）终止状态（TERMINATED）**: 线程任务执行完毕或者由于异常而结束，线程就会进入终止状态。在这个状态下，线程的生命周期实际上已经结束了，它不能再转换到其他任何状态。

BLOCKED、WAITING、TIMED_WAITING：都可以理解为阻塞、等待状态，因为处在这三种状态下，CPU 不会调度当前线程

 ```java
 NEW:
 Thread thread = new Thread(()->{
 
 });
 System.out.println(thread.getState());
 
 RUNNABLE:
 Thread thread = new Thread(()->{
     while (true) {
 
     }
 });
 thread.start();
 System.out.println(thread.getState());
 
 BLOCKED:
 Object object = new Object();
 Thread thread = new Thread(()->{
     synchronized (object) {
 
     }
 });
 synchronized (object) {
     thread.start();
     Thread.sleep(500);
     System.out.println(thread.getState());
 }
 
 WAITING:
 Object object = new Object();
 Thread thread = new Thread(() -> {
     synchronized (object) {
         try {
             object.wait();
         } catch (InterruptedException e) {
             e.printStackTrace();
         }
     }
 });
 thread.start();
 Thread.sleep(500);
 System.out.println(thread.getState());
 
 TIMED_WAITING:
 Thread thread = new Thread(() -> {
     try {
         Thread.sleep(1000);
     } catch (InterruptedException e) {
         e.printStackTrace();
     }
 });
 thread.start();
 Thread.sleep(500);
 System.out.println(thread.getState());
 
 TERMINATED:
 Thread thread = new Thread(() -> {
     try {
         Thread.sleep(500);
     } catch (InterruptedException e) {
         e.printStackTrace();
     }
 });
 thread.start();
 Thread.sleep(1000);
 System.out.println(thread.getState());
 ```



# Java 中如何停止线程

Java中有以下三种方法可以终止正在运行的线程：

1. 使用退出标志，使线程正常退出，也就是当 run() 方法完成后线程中止。这种方法需要在循环中检查标志位是否为 true，如果为 false，则跳出循环，结束线程。
2. 使用 stop() 方法强行终止线程，但是不推荐使用这个方法，该方法已被弃用。这个方法会导致一些清理性的工作得不到完成，如文件，数据库等的关闭，以及数据不一致的问题。
3. 使用 interrupt() 方法中断线程。这个方法会在当前线程中打一个停止的标记，并不是真的停止线程。因此需要在线程中判断是否被中断，并增加相应的中断处理代码。如果线程在 sleep() 或 wait() 等操作时被中断，会抛出 InterruptedException 异常。

**使用标记位中止线程**

使用退出标志，使线程正常退出，也就是当 run() 方法完成后线程中止，是一种比较简单而安全的方法。这种方法需要在循环中检查标志位是否为 true，如果为 false，则跳出循环，结束线程。这样可以保证线程的资源正确释放，不会导致数据不一致或其他异常问题。

例如，下面的代码展示了一个使用退出标志的线程类：

```java
public class ServerThread extends Thread {
    //volatile修饰符用来保证其它线程读取的总是该变量的最新的值
    public volatile boolean exit = false;
    @Override
    public void run() {
        ServerSocket serverSocket = new ServerSocket(8080);
        while (!exit) {
            serverSocket.accept(); //阻塞等待客户端消息
            //do something
        }
    }
}
```

在主方法中，可以通过修改标志位来控制线程的退出:

```java
public static void main(String[] args) {
    ServerThread t = new ServerThread();
    t.start();
    //do something else
    t.exit = true; //修改标志位，退出线程
}
```

这种方法的优点是简单易懂，缺点是需要在循环中不断检查标志位，可能会影响性能。另外，如果线程在 sleep() 或 wait() 等操作时被设置为退出标志，它也不会立即响应，而是要等到阻塞状态结束后才能检查标志位并退出。

**使用 stop() 方法强行终止线程**

使用 stop() 方法强行终止线程，是一种不推荐使用的方法，因为它会导致一些严重的问题。stop() 方法会立即终止线程，不管它是否在执行一些重要的操作，如关闭文件，释放锁，更新数据库等。这样会导致资源泄露，数据不一致，或者其他异常错误。

stop() 方法会立即释放该线程所持有的所有的锁，导致数据得不到同步，出现数据不一致的问题。例如，如果一个线程在修改一个对象的两个属性时被 stop() 了，那么可能只修改了一个属性，而另一个属性还是原来的值。这样就造成了对象的状态不一致。

例如，下面的代码展示了一个使用 stop() 方法的线程类:

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        try {
            FileWriter fw = new FileWriter("test.txt");
            fw.write("Hello, world!");
            Thread.sleep(1000); //模拟耗时操作
            fw.close(); //关闭文件
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

在主方法中，可以通过调用 stop() 方法来强行终止线程：

```java
public static void main(String[] args) {
    MyThread t = new MyThread();
    t.start();
    //do something else
    t.stop(); //强行终止线程
}
```

这种方法的缺点是很明显的，如果在关闭文件之前调用了 stop() 方法，那么文件就不会被正确关闭，可能会造成数据丢失或损坏。而且，stop() 方法会抛出 ThreadDeath 异常，如果没有捕获处理这个异常，那么它会向上层传递，可能会影响其他线程或程序的正常运行 。因此，使用 stop() 方法强行终止线程是一种**非常危险而不负责任**的做法，应该尽量避免使用。

**使用interrupt() 方法中断线程**

`Thread.interrupt()`它能帮助我们在一个线程中断另一个线程。尽管它被命名为“interrupt”，但实际上它并不会立即停止一个线程的执行，而是设置一个中断标志，表示这个线程已经被中断。它的具体行为取决于被中断线程当前的状态以及如何响应中断。

`interrupt`是`Thread`对象一个内部字段，用来表示它的中断状态。这个字段是由Java虚拟机（JVM）管理的，对应用程序代码是不可见的。

以下是有关`Thread.interrupt()`的一些重要事项：

1. **对于非阻塞状态的线程**：如果线程处于运行状态，并且没有执行任何阻塞操作，那么调用`interrupt()`方法只会设置线程的中断状态，并不会影响线程的继续执行。线程需要自己检查这个中断状态，并决定是否停止执行。常见的检查方式包括调用`Thread.interrupted()`（这会清除中断状态）或者`Thread.currentThread().isInterrupted()`（不会清除中断状态）。
2. **对于阻塞状态的线程**：如果线程处于阻塞状态，如调用了`Object.wait()`, `Thread.join()`或者`Thread.sleep()`方法，那么线程会立即抛出`InterruptedException`，并且清除中断状态。
3. **对于已经停止的线程**：如果线程已经停止，那么调用`interrupt()`方法不会有任何影响。

`interrupt()`方法为我们提供了一种通用的、协作式的线程停止机制。它允许被中断的线程决定如何处理中断请求，可以立即停止，也可以忽略中断，或者继续执行一段时间然后再停止。

以下是一个使用`interrupt()`方法的例子：

```java
Thread t = new Thread(() -> {
    while (!Thread.currentThread().isInterrupted()) {
        // 执行任务
    }
});

t.start();

// 在另一个线程中中断t线程
t.interrupt();
```

这个例子中，线程`t`会一直执行，直到它的中断状态被设置。这是通过检查`Thread.currentThread().isInterrupted()`实现的。当`t.interrupt()`被调用时，线程`t`的中断状态被设置，因此线程将退出循环并结束执行。

需要注意的是，如果线程在响应中断时需要执行一些清理工作，或者需要抛出一个异常来通知上游代码，那么就需要在捕获`InterruptedException`后，手动再次设置中断标志。这是因为当`InterruptedException`被抛出时，中断状态会被清除。例如：

```java
while (!Thread.currentThread().isInterrupted()) {
    try {
        // 执行可能抛出InterruptedException的任务
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        // 捕获InterruptedException后，再次设置中断标志
        Thread.currentThread().interrupt();
    }
}
```



# Java 中 wait 和 sleep 方法的区别

sleep 方法和 wait 方法都是用来将线程进入休眠状态的，并且 sleep 和 wait 方法都可以响应 interrupt 中断，也就是线程在休眠的过程中，如果收到中断信号，都可以进行响应并中断，且都可以抛出 InterruptedException 异常，那 sleep 和 wait 有什么区别呢？接下来，我们一起来看。

**区别一：语法使用不同**

wait 方法必须配合`synchronized `一起使用，不然在运行时就会抛出 IllegalMonitorStateException 的异常。wait 方法会将持有锁的线程从owner 扔到 WaitSet 集合中，这个操作是在修改 ObjectMonitor 对象，如果没有持有 synchronized 锁的话，是无法操作 ObjectMonitor 对象的。

而 sleep 可以单独使用，无需配合 synchronized 一起使用。

**区别二：所属类不同**

wait 方法属于 Object 类的方法，而 sleep 属于 Thread 类的方法

**区别三：唤醒方式不同**

sleep 方法必须要传递一个超时时间的参数，且过了超时时间之后，线程会自动唤醒。而 wait 方法可以不传递任何参数，不传递任何参数时表示永久休眠，直到另一个线程调用了 notify 或 notifyAll 之后，休眠的线程才能被唤醒。也就是说 **sleep 方法具有主动唤醒功能，而不传递任何参数的 wait 方法只能被动的被唤醒**。

**区别四：释放锁资源不同**

**wait 方法会主动的释放锁，而 sleep 方法则不会**。

**区别五：线程进入状态不同**

调用 sleep 方法线程会进入 TIMED_WAITING 有时限等待状态，而调用无参数的 wait 方法，线程会进入 WAITING 无时限等待状态**。** 

sleep 和 wait 都可以让线程进入休眠状态，并且它们都可以响应 interrupt 中断，但二者的区别主要体现在：语法使用不同、所属类不同、唤醒方式不同、释放锁不同和线程进入的状态不同。

# 并发编程的三大特性

**原子性**

JMM（Java Memory Model）。不同的硬件和不同的操作系统在内存上的操作有一定差异的，Java 为了解决相同代码在不同操作系统上出现的各种问题，用 JMM 屏蔽掉各种硬件和操作系统带来的差异。让 Java 的并发编程可以做到跨平台。

JMM 规定所有变量都会存储在主内存中，在操作的时候，需要从主内存中复制一份到线程内存（CPU 内存），在线程内部做计算，**然后再写回主内存中（不一定)。**

原子性的定义：原子性指一个操作是不可分割的，不可中断的，一个线程在执行时，另一个线程不会影响到他。

保证并发编程的原子性

- synchronized

  可以在方法上追加 synchronized 关键字或采用同步代码块的形式保证原子性。synchronized 可以让多线程同时操作临界资源，同一个时间点，只会有一个线程操作临界资源。

- cas

  compare and swap 也就是比较和交换，它是一条 CPU 的并发原语。它在替换内存中某个位置的值时，首先查看内存中的值与预期的值是否一致，如果一致，执行替换操作。这个操作是一个原子性操作。Java 中基于Unsafe 的类提供了对 cas 操作的方法，jvm 会帮我们将方法实现 cas 汇编指令。但是要清楚，cas 只是比较和交换，在获取原值的这个操作上，需要自己实现。

- lock

  lock 锁是JDK1.5 由Doug lea 研发的，它的性能相比 synchronized 在JDK1.5 的时期，性能好了很多，但是在 JDK1.6 优化之后，性能相差不大，但是如果设置并发比较多时，推荐 ReentrantLock 锁，性能会更好。

- ThreadLocal

  ThreadLocal 保证原子性的操作，是不让多线程去操作临界资源，让每个线程去操作属于自己的数据。

**可见性**

可见性问题是基于 CPU 位置出现的，CPU 处理速度非常快，相对 CPU 来说，去主内存获取数据这个事情太慢了，CPU 就提供了 L1、L2、L3 的三级缓存，每次去主内存拿完数据后，就会存储到 CPU 的三级缓存，每次去三级缓存中取数据，效率肯定会提升。

这就带来了问题，现在 CPU 都是多核的，每个线程的工作内存（CPU 三级缓存）都是独立的，会告知每个线程中修改时，只该自己的工作内存，没有及时的同步到主内存中，导致数据不一致问题。

解决可见性的方式

- volatile

  volatile 是个关键字，用来修饰成员变量。如果属性被 volatile 修饰，相当于会告诉 CPU，对当前属性的操作，不使用 CPU 三级缓存，必须去和主内存进行操作。

  volatile 的内存语义

  - volatile 属性被写：当写一个 volatile 变量，JMM 会将当前线程对应的CPU 缓存及时的刷新到主内存中。
  - volatile 属性被读：当读一个 volatile 变量，JMM 会将对应的 CPU 缓存中的内存设置为无效，必须去主内存中读取共享变量。

  其实加了 volatile 就是告知CPU，对当前属性的读写操作，不使用 CPU 缓存，加了 volatile 修饰的属性，会在转为汇编之后，追加一个 lock 的前缀，CPU 执行这个指令时，如果带有 lock 前缀会做两个事情：

  - 将当前处理器缓存行的数据写回到主内存
  - 这个写回的过程，在其他CPU 内核的缓存中，直接无效

- synchronized

  synchronized 也是可以解决可见性问题的，synchronized 的内存语义。

  如果涉及到了 synchronized 的同步代码块或者同步方法，获取锁资源之后，将内存获取的变量在 CPU 缓存中删除，必须去主内存中重新拿数据，而且在释放锁之后，会立即将 CPU 缓存中的数据同步到主内存中。

- lock

  lock 锁保证可见性的方式和 synchronized 完全不同，synchronized 基于它的内存语义，在获取锁和释放锁的时候，对 CPU 缓存做同步到主内存的操作。

  lock 锁是基于 volatile 实现的，lock 锁内部在进行加锁和释放锁的时候，会对一个 volatile 修饰的 state 属性进行加减操作。

  如果对 volatile 修饰的属性进行写操作，CPU 会执行带有 lock 前缀的指令，CPU 会将修改的数据，从 CPU 缓存立即同步到主内存中，同时也会将其他属性立即同步到主内存中，还会将其他 CPU 缓存行中的这个数据设置为无效，必须从主内存中拉取。

- final

  final 修饰的属性，在运行期间是不允许被修改的，这样一来就间接性的保证了可见性，所有多线程读取 final 属性，值肯定是一样的。

  final 并不是说每次取数据从主内存读取，他没有这个必要，而且 final 和 volatile 不可以同时修饰一个属性。

  final 修饰的属性已经不允许再次被写了，而 volatile 是保证每次读写操作去内存中读取，并且 volatile 会影响一定的性能，就不需要同时修饰。

**有序性**

所谓有序性是指程序代码在执行过程中先后顺序，由于 Java 在编译器以及运行期的优化，导致了代码的执行顺序未必就是开发者编写代码的顺序，比如

```java
int x = 10;
int y = 0;
x++;
y = 20
```

上面这段代码定义了两个 int 类型的变量 x 和 y，对 x 进行自增操作，对 y 进行赋值操作，从编写程序的角度看上面代码肯定是顺序执行下去的，但是 JVM 真正地运行这段代码的时候未必会是这样的顺序，比如  y = 20 语句有可能会在 x++ 语句的前面执行，这种情况就是通常所说的指令重排。

一般来说，处理器为了提高程序的运行效率，可能会对输入的代码指令做一定的优化，它不会百分百的保证代码的执行顺序严格按照编写代码中的顺序进行，但是它会保证程序的最终运算结果是编码时所期望的那样，比如上文中的 x++ 和 y = 20，不管它们的执行顺序如何，执行完上面四行代码之后得到的结果肯定都是 x =11,y = 20。

当然对指令的重排序要严格遵守指令之间的数据依赖关系，并不是可以任意进行重排序的，比如下面的代码片段。

```java
int x = 10;
int y = 0;
x++;
y=x+1;
```

对于这段代码有可能它的执行顺序就是代码本身的顺序，有可能发生了重排序导致 int y=0 优于 int x =10 执行，但是绝对不能出现 y= x+1 优于 x++ 执行的执行情况，如果一个指令 x 在执行的过程中需要用到指令 y 的执行结果，那么处理器会保证指令 y 在指令 x 之前执行，这就好比 y = x+1 执行前肯定要先执行 x++ 一样。

在单线程情况下，无论怎样的重排序最终都会保证程序的执行结果和代码顺序执行结果完全一致的，但是在多线程情况下，如果有序性得不到保证，那么很有可能就会出现非常大的问题，比如下面的代码片段

```java
private boolean initialized = false;
private Context context;
public Context load(){
    if（!initialized）{
        context = loadContext();
        initialized = true;
    }
    return context;
}
```

上面代码使用 boolean 变量 initialized 来控制 context 是否已经被加载过，在单线程下，无论怎样的重排序，最终返回给使用者的 context 都是可用的。如果在多线程的情况下发生了重排序，比如 context = loadContext 的执行顺序被重排序到 initialized = true; 的后面，那么这就是灾难性的。比如第一个线程首先判断 initialized = false，然后准备执行 loadContext 方法，但由于重排序，将 initialized 设置为 true，此时如果另外一个线程也执行 load 方法，发现此时 initialized 已经为 true 了,则返回一个还未被加载的 context，那么在程序的运行过程中势必会出现错误。

在 Java 中，.java 文件的内容会被编译，在执行前需要再次转为 CPU 可以识别的指令，CPU 在执行这些指令时，为了提升执行效率，在不影响最终结果的前提下（满足一些需求），会对指令进行重排。

指令乱序执行的原因，是为了尽可能的发挥 CPU 的性能。

Java 中的程序是乱序执行的。

保证有序性的方式

- as-if-serial

- happens-before

  具体规则

  1. 单线程 happen-before 原则：在同一个线程中，书写在前面的操作 happen-before 后面的操作。
  2. 锁的 happen-before 原则：同一个所得 unlock 操作 happen-before 此锁的 lock 操作。
  3. volatile 的 happens-before 原则：对一个 volatile 变量的写操作 happen-before 对此变量的任何操作。
  4. happen-before 的传递性原则：如果 A 操作 happen-before B 操作，B 操作 happen-before C 操作，那么 A 操作 happen-before C 操作。
  5. 线程启动的 happen-before 原则：同一个线程的 start 操作 happen-before 此线程的其他方法。
  6. 线程中断的 happen-before 原则：对线程 interrupt 方法的调用 happen-before 被中断线程的检测到中断发送的代码。
  7. 线程终结的 happen-before 原则：线程中所有的操作都 happen-before 线程的终止检测。
  8. 对象创建的 happen-before 原则：一个对象的初始化完成先于他的finalize 方法调用。

  JMM 只有在不出现上述 8 中情况时，才不会触发指令重排效果。

  不需要过分的关注 happen-before 原则，只需要可以写出线程安全的代码就可以了。

- volatile

  如果需要让程序对某一个属性的操作不出现指令重排，除了满足 happens-before 的原则外，还可以基于 volatile 修饰属性，从而对这个属性的操作，就不会出现指令重排的问题了。

  volatile 如何实现的禁止指令重排？

  内存屏障概念，将内存屏障看成一个指令。

  会在两个操作之间，添加上一道指令，这个指令就可以避免上下执行的其他指令进行重排。

# 什么是 CAS？有什么优缺点

在高并发的业务场景下，线程安全问题是必须考虑的，在JDK5之前，可以通过synchronized或Lock来保证同步，从而达到线程安全的目的。但synchronized或Lock方案属于互斥锁的方案，比较重量级，加锁、释放锁都会引起性能损耗问题。

而在某些场景下，我们是可以通过JUC提供的CAS机制实现无锁的解决方案，或者说是它基于类似于乐观锁的方案，来达到非阻塞同步的方式保证线程安全。

**什么是 CAS**

`CAS`是`Compare And Swap`的缩写，直译就是**比较并交换**。CAS是现代CPU广泛支持的一种对内存中的共享数据进行操作的一种特殊指令，这个指令会对内存中的共享数据做原子的读写操作。其作用是让CPU比较内存中某个值是否和预期的值相同，如果相同则将这个值更新为新值，不相同则不做更新。

本质上来讲CAS是一种无锁的解决方案，也是一种基于乐观锁的操作，可以保证在多线程并发中保障共享资源的原子性操作，相对于synchronized或Lock来说，是一种轻量级的实现方案。

Java中大量使用了CAS机制来实现多线程下数据更新的原子化操作，比如AtomicInteger、CurrentHashMap当中都有CAS的应用。但Java中并没有直接实现CAS，CAS相关的实现是借助`C/C++`调用CPU指令来实现的，效率很高，但Java代码需通过JNI才能调用。比如，Unsafe类提供的CAS方法（如compareAndSwapXXX）底层实现即为CPU指令cmpxchg。

**CAS 的基本流程**

![image-20230825103445318](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230825103445318.png)

在上图中涉及到三个值的比较和操作：修改之前获取的（待修改）值A，业务逻辑计算的新值B，以及待修改值对应的内存位置的C。

整个处理流程中，假设内存中存在一个变量i，它在内存中对应的值是A（第一次读取），此时经过业务处理之后，要把它更新成B，那么在更新之前会再读取一下i现在的值C，如果在业务处理的过程中i的值并没有发生变化，也就是A和C相同，才会把i更新（交换）为新值B。如果A和C不相同，那说明在业务计算时，i的值发生了变化，则不更新（交换）成B。最后，CPU会将旧的数值返回。而上述的一系列操作由CPU指令来保证是原子的。

在《Java并发编程实践》中对CAS进行了更加通俗的描述：我认为原有的值应该是什么，如果是，则将原有的值更新为新值，否则不做修改，并告诉我原来的值是多少。

在上述路程中，我们可以很清晰的看到乐观锁的思路，而且这期间并没有使用到锁。因此，相对于synchronized等悲观锁的实现，效率要高非常多。

**基于 CAS 的 AtomicInteger 使用**

关于CAS的实现，最经典最常用的当属AtomicInteger了，我们马上就来看一下AtomicInteger是如何利用CAS实现原子性操作的。为了形成更新鲜明的对比，先来看一下如果不使用CAS机制，想实现线程安全我们通常如何处理。

在没有使用CAS机制时，为了保证线程安全，基于synchronized的实现如下：

```java
public class ThreadSafeTest {

 public static volatile int i = 0;

 public synchronized void increase() {
  i++;
 }
}
```

至于上面的实例具体实现，这里不再展开，很多相关的文章专门进行讲解，我们只需要知道为了保证i++的原子操作，在increase方法上使用了重量级的锁synchronized，这会导致该方法的性能低下，所有调用该方法的操作都需要同步等待处理。

那么，如果采用基于CAS实现的AtomicInteger类，上述方法的实现便变得简单且轻量级了：

```java
public class ThreadSafeTest {

 private final AtomicInteger counter = new AtomicInteger(0);

 public int increase(){
  return counter.addAndGet(1);
 }
}
```

之所以可以如此安全、便捷地来实现安全操作，便是由于AtomicInteger类采用了CAS机制。下面，我们就来了解一下AtomicInteger的功能及源码实现。

**CAS 的 AtomicInteger 类**

`AtomicInteger`是java.util.concurrent.atomic 包下的一个原子类，该包下还有`AtomicBoolean`, `AtomicLong`,`AtomicLongArray`, `AtomicReference`等原子类，主要用于在高并发环境下，保证线程安全。

**AtomicInteger常用API**

```java
public final int get()：获取当前的值
public final int getAndSet(int newValue)：获取当前的值，并设置新的值
public final int getAndIncrement()：获取当前的值，并自增
public final int getAndDecrement()：获取当前的值，并自减
public final int getAndAdd(int delta)：获取当前的值，并加上预期的值
void lazySet(int newValue): 最终会设置成newValue,使用lazySet设置值后，可能导致其他线程在之后的一小段时间内还是可以读到旧的值。
```

上述方法中，getAndXXX格式的方法都实现了原子操作。具体的使用方法参考上面的addAndGet案例即可。

**AtomicInteger 核心源码**

```java
public class AtomicInteger extends Number implements java.io.Serializable {
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;
    static {
        try {
            // 用于获取value字段相对当前对象的“起始地址”的偏移量
            valueOffset = unsafe.objectFieldOffset(AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }

    private volatile int value;

    //返回当前值
    public final int get() {
        return value;
    }

    //递增加detla
    public final int getAndAdd(int delta) {
        // 1、this：当前的实例 
        // 2、valueOffset：value实例变量的偏移量 
        // 3、delta：当前value要加上的数(value+delta)。
        return unsafe.getAndAddInt(this, valueOffset, delta);
    }

    //递增加1
    public final int incrementAndGet() {
        return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
    }
...
}
```

上述代码以AtomicInteger#incrementAndGet方法为例展示了AtomicInteger的基本实现。其中，在static静态代码块中，基于Unsafe类获取value字段相对当前对象的“起始地址”的偏移量，用于后续Unsafe类的处理。

在处理自增的原子操作时，使用的是Unsafe类中的getAndAddInt方法，CAS的实现便是由Unsafe类的该方法提供，从而保证自增操作的原子性。

同时，在AtomicInteger类中，可以看到value值通过volatile进行修饰，保证了该属性值的线程可见性。在多并发的情况下，一个线程的修改，可以保证到其他线程立马看到修改后的值。

通过源码可以看出， `AtomicInteger` 底层是通过**volatile**变量和CAS两者相结合来保证更新数据的原子性。其中关于Unsafe类对CAS的实现，我们下面详细介绍。

**CAS 的工作原理**

CAS的实现原理简单来说就是由**Unsafe类**和其中的**自旋锁**来完成的，下面针对源代码来看一下这两块的内容。

**Unsafe 类**

在AtomicInteger核心源码中，已经看到CAS的实现是通过Unsafe类来完成的，先来了解一下Unsafe类的作用。

sun.misc.Unsafe是JDK内部用的工具类。它通过暴露一些Java意义上说“不安全”的功能给Java层代码，来让JDK能够更多的使用Java代码来实现一些原本是平台相关的、需要使用native语言（例如C或C++）才可以实现的功能。该类不应该在JDK核心类库之外使用，这也是命名为Unsafe（不安全）的原因。

JVM的实现可以自由选择如何实现Java对象的“布局”，也就是在内存里Java对象的各个部分放在哪里，包括对象的实例字段和一些元数据之类。

Unsafe里关于对象字段访问的方法把对象布局抽象出来，它提供了objectFieldOffset()方法用于获取某个字段相对Java对象的“起始地址”的偏移量，也提供了getInt、getLong、getObject之类的方法可以使用前面获取的偏移量来访问某个Java对象的某个字段。在AtomicInteger的static代码块中便使用了objectFieldOffset()方法。

Unsafe类的功能主要分为内存操作、CAS、Class相关、对象操作、数组相关、内存屏障、系统相关、线程调度等功能。这里我们只需要知道其功能即可，方便理解CAS的实现，注意不建议在日常开发中使用。

**Unsafe 和 CAS**

AtomicInteger调用了Unsafe#getAndAddInt方法：

```java
public final int incrementAndGet() {
    return unsafe.getAndAddInt(this, valueOffset, 1) + 1;
}
```

上述代码等于是AtomicInteger调用UnSafe类的CAS方法，JVM帮我们实现出汇编指令，从而实现原子操作。

在Unsafe中getAndAddInt方法实现如下：

```java
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));
    return var5;
}
```

getAndAddInt方法有三个参数：

- 第一个参数表示当前对象，也就是new的那个AtomicInteger对象；
- 第二个表示内存地址；
- 第三个表示自增步伐，在AtomicInteger#incrementAndGet中默认的自增步伐是1。

getAndAddInt方法中，首先把当前对象主内存中的值赋给val5，然后进入while循环。判断当前对象此刻主内存中的值是否等于val5，如果是，就自增（交换值），否则继续循环，重新获取val5的值。

在上述逻辑中核心方法是compareAndSwapInt方法，它是一个native方法，这个方法汇编之后是CPU原语指令，原语指令是连续执行不会被打断的，所以可以保证原子性。

在getAndAddInt方法中还涉及到一个实现**自旋锁**。所谓的自旋，其实就是上面getAndAddInt方法中的do while循环操作。当预期值和主内存中的值不等时，就重新获取主内存中的值，这就是自旋。

这里我们可以看到CAS实现的一个缺点：内部使用**自旋**的方式进行**CAS**更新（while循环进行CAS更新，如果更新失败，则循环再次重试）。如果长时间都不成功的话，就会造成CPU极大的开销。

另外，Unsafe类还支持了其他的CAS方法，比如`compareAndSwapObject`、` compareAndSwapInt`、`compareAndSwapLong`。

**CAS 的缺点**

`CAS`高效地实现了原子性操作，但在以下三方面还存在着一些缺点：

- 循环时间长，开销大；
- 只能保证一个共享变量的原子操作；
- ABA问题；

下面就这个三个问题详细讨论一下。

**循环时间长开销大**

在分析Unsafe源代码的时候我们已经提到，在Unsafe的实现中使用了自旋锁的机制。在该环节如果`CAS`操作失败，就需要循环进行`CAS`操作(do while循环同时将期望值更新为最新的)，如果长时间都不成功的话，那么会造成CPU极大的开销。如果JVM能支持处理器提供的pause指令那么效率会有一定的提升。

**只能保证一个共享变量的原子操作**

在最初的实例中，可以看出是针对一个共享变量使用了CAS机制，可以保证原子性操作。但如果存在多个共享变量，或一整个代码块的逻辑需要保证线程安全，CAS就无法保证原子性操作了，此时就需要考虑采用加锁方式（悲观锁）保证原子性，或者有一个取巧的办法，把多个共享变量合并成一个共享变量进行`CAS`操作。

**ABA问题**

虽然使用CAS可以实现非阻塞式的原子性操作，但是会产生ABA问题，ABA问题出现的基本流程：

- 进程P1在共享变量中读到值为A；
- P1被抢占了，进程P2执行；
- P2把共享变量里的值从A改成了B，再改回到A，此时被P1抢占；
- P1回来看到共享变量里的值没有被改变，于是继续执行；

虽然P1以为变量值没有改变，继续执行了，但是这个会引发一些潜在的问题。ABA问题最容易发生在lock free的算法中的，CAS首当其冲，因为CAS判断的是指针的地址。如果这个地址被重用了呢，问题就很大了（地址被重用是很经常发生的，一个内存分配后释放了，再分配，很有可能还是原来的地址）。

维基百科上给了一个形象的例子：你拿着一个装满钱的手提箱在飞机场，此时过来了一个火辣性感的美女，然后她很暖昧地挑逗着你，并趁你不注意，把用一个一模一样的手提箱和你那装满钱的箱子调了个包，然后就离开了，你看到你的手提箱还在那，于是就提着手提箱去赶飞机去了。

ABA问题的解决思路就是使用版本号：在变量前面追加上版本号，每次变量更新的时候把版本号加1，那么A->B->A就会变成1A->2B->3A。

另外，从Java 1.5开始，JDK的Atomic包里提供了一个类AtomicStampedReference来解决ABA问题。这个类的compareAndSet方法的作用是首先检查当前引用是否等于预期引用，并且检查当前标志是否等于预期标志，如果全部相等，则以原子方式将该引用和该标志的值设置为给定的更新值。

**@Contended 注解有什么用**

@Contended是[Java](https://so.csdn.net/so/search?q=Java&spm=1001.2101.3001.7020) 8中引入的一个注解，用于减少多线程环境下的“伪共享”现象，以提高程序的性能。

要理解@Contended的作用，首先要了解一下什么是伪共享（False Sharing）。

**什么是伪共享**

伪共享（False Sharing）是多线程环境中的一种现象，涉及到CPU的缓存机制和缓存行（Cache Line）。

现代CPU中，为了提高访问效率，通常会在CPU内部设计一种快速存储区域，称为缓存（Cache）。CPU在读写主内存中的数据时，会首先查看该数据是否已经在缓存中。如果在，就直接从缓存读取，避免了访问主内存的耗时；如果不在，则从主内存读取数据并放入缓存，以便下次访问。

缓存不是直接对单个字节进行操作的，而是以块（通常称为“缓存行”）为单位操作的。一个缓存行通常包含64字节的数据。

在多线程环境下，如果两个或更多的线程在同一时刻分别修改存储在同一缓存行的不同数据，那么CPU为了保证数据一致性，会使得其他线程必须等待一个线程修改完数据并写回主内存后，才能读取或者修改这个缓存行的数据。尽管这些线程可能实际上操作的是不同的变量，但由于它们位于同一缓存行，因此它们之间就会存在不必要的数据竞争，这就是伪共享。

伪共享会降低并发程序的性能，因为它会增加缓存的同步操作和主内存的访问。解决伪共享的一种方式是尽量让经常被并发访问的变量分布在不同的缓存行中，例如，可以通过增加无关的填充数据，或者利用诸如Java的@Contended注解等工具。

@Contended 是Java 8引入的一个注解，设计用于减少多线程环境下的伪共享（False Sharing）问题以提高程序性能。

伪共享是现代多核处理器中一个重要的性能瓶颈，它发生在多个处理器修改同一缓存行（Cache Line）中的不同数据时。缓存行是内存的基本单位，一般为64字节。当一个处理器读取主内存中的数据时，它会将整个缓存行（包含需要的数据）加载到本地缓存（L1，L2或L3缓存）中。如果另一个处理器修改了同一缓存行中的其他数据，那么原先加载到缓存中的数据就会变得无效，需要重新从主内存中加载。这会增加内存访问的延迟，降低程序性能。

@Contended注解可以标注在字段或者类上。它能使得被标注的字段在内存布局上尽可能地远离其他字段，使得被标注的字段或者类中的字段分布在不同的缓存行上，从而减少伪共享的发生。

```java
public class Foo {
    @Contended
    long x;
    long y;
}
```

在这里，x被@Contended注解标记，所以x和y可能会被分布在不同的缓存行上，这样如果多个线程并发访问x和y，就不会引发伪共享。

需要注意的是，@Contended是JDK的内部API，它在Java 8中引入，但在默认情况下是不开放的，要使用需要添加JVM参数-XX:-RestrictContended，并且在编译时需要使用--add-exports java.base/jdk.internal.vm.annotation=ALL-UNNAMED。此外，过度使用@Contended可能会浪费内存，因为它会导致大量的内存空间被用作填充以保持字段间的距离。所以在使用时需要谨慎权衡内存和性能的考虑。

**简单案例**

在Java 8及以上版本中，@Contended注解是属于jdk的内部API，因此在正常情况下使用时需要打开开关-XX:-RestrictContended才能正常使用。同时需要注意的是，@Contended在JDK 9以后的版本中可能无法正常工作，因为JDK 9开始禁止使用Sun的内部API。

以下是一个@Contended注解的简单使用案例：
```java
import jdk.internal.vm.annotation.Contended;
 
public class ContendedExample {
 
    @Contended
    volatile long value1 = 0L;
 
    @Contended
    volatile long value2 = 0L;
 
    public void increaseValue1() {
        value1++;
    }
 
    public void increaseValue2() {
        value2++;
    }
 
    public static void main(String[] args) {
        ContendedExample example = new ContendedExample();
 
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 1000000; i++) {
                example.increaseValue1();
            }
        });
 
        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 1000000; i++) {
                example.increaseValue2();
            }
        });
 
        thread1.start();
        thread2.start();
 
        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
 
        System.out.println("value1: " + example.value1);
        System.out.println("value2: " + example.value2);
    }
}
```

这个例子中定义了两个使用了@Contended注解的volatile长整型字段value1和value2。两个线程分别对这两个字段进行增加操作。因为这两个字段使用了@Contended注解，所以他们会被分布在不同的缓存行中，减少了因伪共享带来的性能问题。但由于伪共享的影响在实际运行中并不容易直接观察，所以这个例子主要展示了@Contended注解的使用方式，而不是实际效果。

@Contended 注解，就是将一个缓存行的后面 7 个位置，填充上 7 个没有意义的数据。



**Java 中的四种引用类型**

Java 中的引用类型分别为**强、软、弱、虚**。

在 Java 中最常见的就是强引用，把一个对系那个赋给一个引用变量，这个引用变量就是一个强引用。当一个对象被强引用变量引用时，它始终处于可达状态，它是不可能被垃圾回收机制回收的，即使该对象以后永远都不会被用到 JVM 也不会回收。因此强引用时造成 Java 内存泄漏的主要原因之一。例如：Object obj = new Object()。

其次是软引用，对于只有软引用的对象来说，当系统内存足够时它不会被回收，当系统内存空间不足时它会被回收。软引用通话吃那个用在对内存敏感的程序中，作为缓存使用。例如：SoftRefenence softRef = new SoftRefenence()。

然后是弱引用，它比较引用的生存期更短，对于只有弱引用的对象来说，只要垃圾回收机制已运行，不管 JVM 的内存空间是否足够，总会回收该对象占用的内存。可以解决内存泄漏的问题，ThreadLocal 就是基于弱引用解决内存泄漏的问题。例如：WeakRefenence weakRef = new WeakRefenence()。

最后是虚引用，它不能单独使用，必须和引用队列联合使用。虚引用的主要作用是跟踪对系那个被垃圾回收的状态。例如：ReferenceQueue queue = new ReferenceQueue()；PhantomReference phantomRef = new PhantomReference(obj,queue)，不过在开发中，我们用的更多的还是强引用。



# ThreadLocal 的内存泄漏问题

**ThreadLocal 实现原理：**

- 每个 Thread 中都存储着一个成员变量，ThreadLocalMap
- ThreadLocal 本身不存储数据，像是一个工具类，基于 ThreadLocal 去操作 ThreadLocalMap
- ThreadLocalMap 本身就是基于 Entry[] 实现的，因为一个线程可以绑定多个 ThreadLocal，这样一来，可能需要存储多个数据，所以采用 Entry[] 的形式实现。
- 每个现有都自己独立的 ThreadLocalMap，再基于 ThreadLocal 对象本身作为 key，对 value 进行存取
- ThreadLocalMap 的 key 是一个弱引用，弱引用的特点是，即便有若医用，再 GC 时，也必须被回收。这里是为了在 ThreadLocal 对象失去引用后，如果 key 的引用是强引用，会导致 ThreadLocal 对象无法被回收。

**ThreadLocal 内存泄漏问题**

- 如果 ThreadLocal 引用丢失，key 因为弱引用会被 GC 回收掉，如果同时线程还没有被回收，就会导致内存泄漏，内存中的 value 无法被回收，同时也无法被获取到。
- 只需要在使用完毕 ThreadLocal 对象之后，及时的调用 remove 方法，移除 Entry 即可。



# Java 中锁的分类

1. **可重入锁、不可重入锁**

   Java 中提供的 synchronized，ReentrantLock，ReentrantReadWriteLock 都是可重入锁。

   **重入**：当前线程获取到 A 锁，在获取之后尝试再次获取 A 锁是可以直接拿到的

   **不可重入**：当前线程获取到 A 锁，在获取之后尝试再次获取 A 锁，无法获取到的，因为 A 锁被当前线程占用着，需要等待自己释放锁再获取锁。

2. **乐观锁、悲观锁**

   Java 中提供的 synchronized，ReentrantLock，ReentrantReadWriteLock 都是悲观锁。

   Java 中提供的 CAS 操作，就是乐观锁的一种实现。

   **悲观锁**：获取不到资源时，会将当前线程挂起（进入 BLOCKED、WAITING），线程挂起会涉及到用户态和内核态的切换，而这种切换是比较消耗资源的。

   - 用户态：JVM 可以自行执行的指令，不需要借助操作系统执行。
   - 内核态：JVM 不可以自行执行，需要操作系统才可以执行。

   **乐观锁**：获取不到资源，可以再次让 CPU 调度，重新尝试获取锁资源。

   Atomic 原子性类中，就是基于 CAS 乐观锁实现的。

3. **公平锁、非公平锁**

   Java 中提供的 synchronized 只能是非公平锁。

   Java 中提供的 ReentrantLock，ReentrantReadWriteLock 可以实现公平锁和非公平锁。

   **公平锁**：线程 A 获取到了锁资源，线程 B 没有获得，线程 B 去排队，线程 C 来了，锁被 A 持有，同时线程 B 在排队，直接排到 B 的后面，等待 B 拿到锁资源或者 B 取消后，才可以尝试去竞争锁资源。

   **非公平锁**：线程 A 获取到了锁资源，线程 B 没有获得，线程 B 去排队，线程 C 来了，先尝试竞争一波。

   - 拿到锁资源：开心，插队成功。
   - 没有拿到锁资源：依然要排到 B 的后面，等待 B 拿到锁资源或者 B 取消后，才可以尝试去竞争锁资源。

4. **互斥锁、共享锁**

   Java 中提供的 synchronized，ReentrantLock 是互斥锁。

   Java 中提供的 ReentrantReadWriteLock 有互斥锁也有共享锁。

   **互斥锁**：同一个时间点，只会有一个线程持有者当前互斥锁。

   **共享锁**：同一个时间点，当前共享锁可以被多个线程同时持有。



# synchronized 在 JDK1.6 的优化

在JDK1.5 的时候 Doug lee 推出了 ReentrantLock，lock 的性能远高于 synchronized，所以 JDK1.6 的时候团队就对 synchronized 进行了大量的优化。

**锁消除**：在 synchronized 修饰的代码中，如果不存在操作临界资源的情况，会触发锁消除，你即便写了 synchronized，也不会触发。

```java
public synchronized void method() {
  // 没有操作临界资源
  // 此时这个方法的 synchronized 可以认为没有
}
```

**锁膨胀**：如果在一个循环中，频繁的获取和释放资源，这样带来的消耗很大，锁膨胀就是将锁的范围放大，避免频繁的竞争和获取锁资源带来不必要的消耗。

```java
public void method() {
  for(int i = 0, i < 99999;i++) {
    synchronized(对象) {
      
    }
  }
}
// 这时上面的锁会触发锁膨胀
```

**锁升级**：ReentrantLock 的实现，是先基于乐观锁的 CAS 尝试获取锁资源，如果拿不到锁资源，才会挂起线程。

synchronized 在 JDK1.6 之前，完全就是获取不到锁，立即挂起当前线程，所以 synchronized 性能很差。

synchronized 就在 JDK1.6 做了锁升级的优化。

- **无锁、匿名偏向**：当前线程没有作为锁的存在。
- **偏向锁**：如果当前锁资源，只有一个线程在频繁的获取和释放，那么这个线程过来，只需要判断，当前指向的线程是否当前线程。
  - 如果是，直接拿着锁资源走。
  - 如果不是，基于 CAS 的方式，尝试将偏向锁指向当前线程，如果获取不到，触发锁升级，升级为轻量级锁。（偏向锁状态出现了所竞争的情况）
- **轻量级锁**：会采用自旋锁的方式去频繁的以 CAS 的形式获取锁资源（采用的是自适应自旋锁）
  - 如果成功获取到，拿着锁资源走。
  - 如果自旋了一定的次数，没拿到锁，锁升级。
- **重量级锁**：就是最传统的 synchronized 方式，拿不到锁资源，就挂起当前线程。（用户态&内核态）

# synchronized 的实现原理

synchronized 是基于对象实现的。

synchronized的底层实现是完全依赖JVM虚拟机的，所以先看看对象的存储结构。

**对象结构**

JVM是虚拟机，是一种标准规范，主要作用就是运行java的类文件的。而虚拟机有很多实现版本，HotSpot就是虚拟机的一种实现，是基于热点代码探测的，有JIT即时编译功能，能提供更高质量的本地代码。HotSpot 虚拟机中对象在内存中可分为对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）。其组成结构如下图：

![image-20230825223549259](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230825223549259.png)

1）实例数据：存放类的属性数据信息，包括父类的属性信息。如果是数组，那么实例部分还包括数组的长度，这部分内存按4字节对齐。

2）对齐填充：虚拟机要求对象起始地址必须是8字节的整数倍。填充数据不是必须存在的，仅仅是为了字节对齐。

3）对象头

对于元数据指针，虚拟机通过这个指针来确定这个对象是哪个类的实例；

对于标记字段，用于存储对象自身的运行时数据，其组成如下图

![img](https://raw.githubusercontent.com/chou401/pic-md/master/1746338-20230329144259342-1477226941.png)

锁信息占3位）在jdk1.6之前只有重量级锁，而1.6后对其进行了优化，就有了偏向锁和轻量级锁。

**上锁的原理**

JVM规范中描述：每个对象有一个监视器锁（monitor）。

monitorenter指向同步代码块开始的位置，monitorexit指向同步代码块结束的位置。

当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权，执行monitorexit 释放所有权，过程如下：

1. 如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者。
2. 如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1。
3. 如果其他线程已经占用了monitor，则该线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor的所有权。

Synchronized的语义底层是通过一个monitor的对象来完成，其实wait/notify等方法也依赖于monitor对象。

这就是为什么只有在同步的块或者方法中才能调用wait/notify等方法，否则会抛出java.lang.IllegalMonitorStateException的异常的原因。

**Synchronized是通过对象内部的一个叫做监视器锁（monitor）来实现的。**

但是监视器锁本质又是依赖于底层的操作系统的互斥锁（Mutex Lock）来实现的。而操作系统实现线程之间的切换这就需要从用户态转换到核心态，这个成本非常高，状态之间的转换需要相对比较长的时间，这就是为什么Synchronized效率低的原因。

因此，这种依赖于操作系统互斥锁（Mutex Lock）所实现的锁我们称之为“重量级锁”。



# 什么是 AQS

AQS 就是 AbstractQueuedSynchronized 抽象类，AQS 其实就是 JUC 并发包下的一个基类，JUC 下的很多内容都是基于 AQS 实现了部分功能，比如 ReentrantLock、ThreadPoolExecutor、阻塞队列、CountDownLacth、Semaphore、CyclicBarrier等等都是基于 AQS 实现的。

首先 AQS 中提供了一个由 volatile 修饰，并且采用 CAS 方式修改的 int 类型的 state 变量。

其次 AQS 中维护了一个双向链表，有 head，有 tail，并且每个节点都是 Node 对象。

```java
static final class Node {
  static final Node SHARED = new Node();
  static final Node EXCLUSIVE = null;
  static final int CANCELLED =  1;
  static final int SIGEL     = -1;
  static final int CONDITION = -2;
  static final int PROPAGATE = -3;
  // 上述的状态都存储在 waitStatus 
  volatile int waitStatus;
  volatile Node prev;
  volatile Node next;
  volatile Thread thread;

}
```



# AQS 唤醒节点时，为何从后往前找

```java
private void unparkSuccessor(Node node) {
    /*
     * If status is negative (i.e., possibly needing signal) try
     * to clear in anticipation of signalling.  It is OK if this
     * fails or if status is changed by waiting thread.
     */
    int ws = node.waitStatus;
    if (ws < 0)
        node.compareAndSetWaitStatus(ws, 0);

    /*
     * Thread to unpark is held in successor, which is normally
     * just the next node.  But if cancelled or apparently null,
     * traverse backwards from tail to find the actual
     * non-cancelled successor.
     */
    Node s = node.next;
    if (s == null || s.waitStatus > 0) {
        s = null;
        for (Node p = tail; p != node && p != null; p = p.prev)
            if (p.waitStatus <= 0)
                s = p;
    }
    if (s != null)
        LockSupport.unpark(s.thread);
}
```

在阅读 AQS 源码的过程中，也许会存在这样的困惑，为什么当next指针对应的节点为null 或者取消时，从tail 向前遍历寻找最近的一个非取消的节点。

当前任释放时，需要获取继任者；AQS的实现方式是从tail 向前遍历，之所以这样是与入队时的逻辑有关。
```java
/**
 * Inserts node into queue, initializing if necessary. See picture above.
 * @param node the node to insert
 * @return node's predecessor
 */
private Node enq(final Node node) {
    for (;;) {
        Node t = tail;
        if (t == null) { // Must initialize
            if (compareAndSetHead(new Node()))
                tail = head;
        } else {
            node.prev = t;
            if (compareAndSetTail(t, node)) {
            // 假设线程1执行到这里被挂起，此时 next 指针还没有关联到，后新来的线程 n 可能已经被排列到后面去了，所以当 t 被需要时，它的 next 指针还没有设置或者重置；故需要从后到前寻找，而如果找寻下一个，而这个可能被遗漏了；
            // 故api文档中有这样一说 (Or, said differently, the next-links are an optimization so that we don't usually need a backward scan.)
                t.next = node;
                return t;
            }
        }
    }
}
```

源码内部类Node上的注释中有对这个场景的完整描述：

```java
 final boolean isOnSyncQueue(Node node) {
    if (node.waitStatus == Node.CONDITION || node.prev == null)
        return false;
    if (node.next != null) // If has successor, it must be on queue
        return true;
    /*
     * node.prev can be non-null, but not yet on queue because
     * 节点的前置前置节点可能为非空，但是尚未在同步队列中，
     * the CAS to place it on queue can fail. So we have to
     * 因为cas 入队时可能失败。
     * traverse from tail to make sure it actually made it.  It
     * 所以我们不得不后序遍历以确保正确的发现它。
     * will always be near the tail in calls to this method, and
     * 它总是在靠近尾节点，当执行这个方法时
     * unless the CAS failed (which is unlikely), it will be
     * 除非cas 失败（这不太可能），所以我们不会遍历太多
     * there, so we hardly evertraverse much.
     */
    return findNodeFromTail(node);
 }
```

另一方面如果下一个节点时null（已经被GC ），如何找到下一个有效节点，也只能从后往前找了。



# ReentrantLock和synchronized的区别

在 Java 中，常用的锁有两种：synchronized（内置锁）和 ReentrantLock（可重入锁）。

**用法不同**

**synchronized 可用来修饰普通方法、静态方法和代码块，而 ReentrantLock 只能用在代码块上。**

使用 synchronized 修饰代码块：

```java
public void method() {
  // 加锁代码
  synchronized(this) {
    //...
  }
}
```

ReentrantLock 在使用之前需要先创建 ReentrantLock 对象，然后使用 lock 方法进行加锁，使用完之后再调用 unlock 方法释放锁，具体使用如下：

```java
public void lockExample() {
  private final ReentrantLock lcok = new ReentrantLock();
  public void method() {
    // lock 加锁操作
    lock.lock();
    try {
      // ...
    } finally {
      // 释放锁
      lock.unlock();
    }
  }
}
```

**获取锁和释放锁方式不同**

**synchronized 会自动加锁和释放锁**，当进入 synchronized 修饰的代码块之后会自动加锁，当离开 synchronized 的代码段之后会自动释放锁，如下图所示：

```java
public class APP {
  public void method() {
    int count = 0;
    synchronized(this) {
      for(int i = 0;i < 10;i++) {
        count++;
      }
    }
    System.out.println(count);
  }
}
```

**而 ReentrantLock 需要手动加锁和释放锁**，如下图所示：

```java
public void lockExample() {
  private final ReentrantLock lcok = new ReentrantLock();
  public void method() {
    // lock 加锁操作
    lock.lock();
    try {
      // ...
    } finally {
      // 释放锁
      lock.unlock();
    }
  }
}
```

> PS：在使用 ReentrantLock 时要特别小心，unlock 释放锁的操作一定要放在 finally 中，否者有可能会出现锁一直被占用，从而导致其他线程一直阻塞的问题。

**锁类型不同**

**synchronized 属于非公平锁，而 ReentrantLock 既可以是公平锁也可以是非公平锁。** 默认情况下 ReentrantLock 为非公平锁，这点查看源码可知：

```java
public ReentrantLock() {
  sync = new NonfairSync();
}
```

使用 new ReentrantLock(true) 可以创建公平锁，查看源码可知：

```java
public ReentrantLock(boolean fair) {
  sync = fair ? FairSync() : new NonfairSync();
}
```

**响应中断不同**

**ReentrantLock 可以使用 lockInterruptibly 获取锁并响应中断指令，而 synchronized 不能响应中断，也就是如果发生了死锁，使用 synchronized 会一直等待下去，而使用 ReentrantLock 可以响应中断并释放锁，从而解决死锁的问题**，比如以下 ReentrantLock 响应中断的示例：

![img](https://raw.githubusercontent.com/chou401/pic-md/master/v2-295dc5c5c1611e25ca7e32a12c1baab5_r.jpg)

以上程序的执行结果如下所示：

![img](https://raw.githubusercontent.com/chou401/pic-md/master/v2-41af01e5aee54030a2bd3429191b39ef_r.jpg)

**底层实现不同**

**synchronized 是 JVM 层面通过监视器（Monitor）实现的，而 ReentrantLock 是通过 AQS（AbstractQueuedSynchronizer）程序级别的 API 实现。** synchronized 通过监视器实现，可通过观察编译后的字节码得出结论，如下图所示：

![img](https://raw.githubusercontent.com/chou401/pic-md/master/v2-f801feb292fa284062ab77ed419e0770_r.jpg)

其中 monitorenter 表示进入监视器，相当于加锁操作，而 monitorexit 表示退出监视器，相当于释放锁的操作。 ReentrantLock 是通过 AQS 实现，可通过观察 ReentrantLock 的源码得出结论，核心实现源码如下：

![img](https://raw.githubusercontent.com/chou401/pic-md/master/v2-b76e76b03d158accc357568d8c360526_r.jpg)



# ReentrantReadWriteLock的实现原理

在很多场景下，我们用到的都是互斥锁，线程间相互竞争资源；但是有时候我们的场景会存在读多写少的情况，这个时候如果还是使用互斥锁，就会导致资源的浪费，为什么呢？

因为如果同时都在读的时候，是不需要锁资源的，只有读和写在同时工作的时候才需要锁资源，所以如果直接用互斥锁，肯定会导致资源的浪费。 

**提供的方法**

- readLock()：获取读锁对象（不是获取锁资源）
- writeLock()：获取写锁对象（不是获取锁资源）
- getReadLockCount()：获取当前读锁被获取的次数，包括线程重入的次数
- getReadHoldCount()：返回当前线程获取读锁的次数
- isWriteLocked()：判断写锁是否被获取
- getWriteHoldCount()：返回当前写锁被获取的次数 

**ReentrantReadWriteLock 还是基于 AQS 实现的，还是对 state 进行操作，拿到锁资源就去干活，如果没有拿到，依然去 AQS 队列中排队。**

**读锁操作：基于 state 高 16 位进行操作。**

**写锁操作：基于 state 低 16 位进行操作。**

**ReentrantReadWriteLock 依然是可重入锁。**

**写锁重入**：读写锁中写锁的重入方式，基本和 ReentrantLock 一致，没有什么区别，依然是对 state + 1 操作即可，只要确认持有锁资源的线程，是当前写锁线程即可，只不过之前 ReentrantLock 的重入次数是 state 的正数取值范围，但是读写锁中写锁范围就小了。

**读锁重入**：因为读锁是共享锁，读锁在获取锁资源操作时，是要对 state 的高 16 位进行 +1 操作。因为读锁是共享锁，所以同一时间会有多个读线程持有读锁资源。这样一来，多个读操作在持有读锁时，无法确认每个线程读锁重入的次数。为了去记录读锁重入的次数，每个读操作的线程，都会有一个 ThreadLocal 记录重入的次数。

**写锁的饥饿问题**：读锁是共享锁，当有线程持有读锁资源时，再来一个线程想要获取读锁，直接对 state 修改即可。在读锁资源先被占用后，来了一个写锁资源，此时，大量的需要获取读锁的线程请求锁资源，如果可以绕过写锁，直接拿资源，会造成写锁长时间无法获取写锁资源。

**锁降级**：当前线程先获取到写锁，然后再获取读锁，再把写锁释放，最后释放读锁。

**读锁在拿到锁资源后，如果再有读线程需要获取读锁资源，需要去 AQS 队列排队，如果队列的前面需要获取写锁资源的线程，那么后续读线程是无法拿到锁资源的。持有读锁的线程，只会让写锁线程之前的读线程拿到锁资源。**

**写锁的获取与释放**

获取锁的源码：

```java
protected final boolean tryAcquire(int acquires) {
    /*
     * Walkthrough:
     * 1. If read count nonzero or write count nonzero
     *    and owner is a different thread, fail.
     * 2. If count would saturate, fail. (This can only
     *    happen if count is already nonzero.)
     * 3. Otherwise, this thread is eligible for lock if
     *    it is either a reentrant acquire or
     *    queue policy allows it. If so, update state
     *    and set owner.
     */
    Thread current = Thread.currentThread();
    int c = getState();
    int w = exclusiveCount(c);
    if (c != 0) {
        // (Note: if c != 0 and w == 0 then shared count != 0)
        if (w == 0 || current != getExclusiveOwnerThread())
            return false;
        if (w + exclusiveCount(acquires) > MAX_COUNT)
            throw new Error("Maximum lock count exceeded");
        // Reentrant acquire
        setState(c + acquires);
        return true;
    }
    if (writerShouldBlock() ||
        !compareAndSetState(c, c + acquires))
        return false;
    setExclusiveOwnerThread(current);
    return true;
}
```

释放锁的源码:

```java
protected final boolean tryRelease(int releases) {
    if (!isHeldExclusively())
        throw new IllegalMonitorStateException();
    int nextc = getState() - releases;
    boolean free = exclusiveCount(nextc) == 0;
    if (free)
        setExclusiveOwnerThread(null);
    setState(nextc);
    return free;
}
```

**读锁的获取与释放**

获取锁的源码:

```java
protected final int tryAcquireShared(int unused) {
    /*
     * Walkthrough:
     * 1. If write lock held by another thread, fail.
     * 2. Otherwise, this thread is eligible for
     *    lock wrt state, so ask if it should block
     *    because of queue policy. If not, try
     *    to grant by CASing state and updating count.
     *    Note that step does not check for reentrant
     *    acquires, which is postponed to full version
     *    to avoid having to check hold count in
     *    the more typical non-reentrant case.
     * 3. If step 2 fails either because thread
     *    apparently not eligible or CAS fails or count
     *    saturated, chain to version with full retry loop.
     */
    Thread current = Thread.currentThread();
    int c = getState();
    if (exclusiveCount(c) != 0 &&
        getExclusiveOwnerThread() != current)
        return -1;
    int r = sharedCount(c);
    if (!readerShouldBlock() &&
        r < MAX_COUNT &&
        compareAndSetState(c, c + SHARED_UNIT)) {
        if (r == 0) {
            firstReader = current;
            firstReaderHoldCount = 1;
        } else if (firstReader == current) {
            firstReaderHoldCount++;
        } else {
            HoldCounter rh = cachedHoldCounter;
            if (rh == null ||
                rh.tid != LockSupport.getThreadId(current))
                cachedHoldCounter = rh = readHolds.get();
            else if (rh.count == 0)
                readHolds.set(rh);
            rh.count++;
        }
        return 1;
    }
    return fullTryAcquireShared(current);
}
```

释放锁的源码:

```java
protected final boolean tryReleaseShared(int unused) {
    Thread current = Thread.currentThread();
    if (firstReader == current) {
        // assert firstReaderHoldCount > 0;
        if (firstReaderHoldCount == 1)
            firstReader = null;
        else
            firstReaderHoldCount--;
    } else {
        HoldCounter rh = cachedHoldCounter;
        if (rh == null ||
            rh.tid != LockSupport.getThreadId(current))
            rh = readHolds.get();
        int count = rh.count;
        if (count <= 1) {
            readHolds.remove();
            if (count <= 0)
                throw unmatchedUnlockException();
        }
        --rh.count;
    }
    for (;;) {
        int c = getState();
        int nextc = c - SHARED_UNIT;
        if (compareAndSetState(c, nextc))
            // Releasing the read lock has no effect on readers,
            // but it may allow waiting writers to proceed if
            // both read and write locks are now free.
            return nextc == 0;
    }
}
```



# JDK中提供了哪些线程池

**newFixedThreadPool**

这个线程的特点是线程数是固定的。

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
    return new ThreadPoolExecutor(nThreads, nThreads,
                                  0L, TimeUnit.MILLISECONDS,
                                  new LinkedBlockingQueue<Runnable>());
}
```

构建时，需要给 newFixedThreadPool 方法提供一个 nThreads 的属性，而这个属性就是当前线程池线程的个数，当前线程池的本质其实就是使用 ThreadPoolExecutor。

构建好当前线程池后，线程个数已经固定好（线程是懒加载，在构建之初，线程并没有构建出来，而是随着任务的提交才会将线程在线程池中构建出来）。如果线程没有构建，线程会待着任务执行被创建和执行。如果线程都已经构建好了，此时任务会被放到 LinkedBlockingQueue 无界队列中存放，等待线程从 LinkedBlockingQueue 中去 take 出去，然后执行。

**newSingleThreadExecutor**

单例线程池，该线程池只有一个工作线程在处理任务，如果业务是顺序消费们可以采用该线程池。

这种线程池比较简单，它内部只有一个线程，会用唯一的工作线程来执行任务。它的原理和固定线程数量的线程池的原理是一样的，只不过这个时候它的线程数量就直接被设置为 1，也就是只有一个线程。

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService
        (new ThreadPoolExecutor(1, 1,
                                0L, TimeUnit.MILLISECONDS,
                                new LinkedBlockingQueue<Runnable>()));
}

private static class FinalizableDelegatedExecutorService
            extends DelegatedExecutorService {
    FinalizableDelegatedExecutorService(ExecutorService executor) {
        super(executor);
    }
    @SuppressWarnings("deprecation")
    protected void finalize() {
        super.shutdown();
    }
}
```

单例线程池，线程池中只有一个工作线程在处理任务。

如果业务中涉及了顺序消费，可以采用 newSingleThreadExecutor。

**newCachedThreadPool**

缓存线程池，corePoolSize为0，当第一次提交任务到线程池时，会直接构建一个工作线程，Integer.MAX_VALUE：意味着线程数量可以无限大，keepAliveTime为60S，60秒内没有任务进来，意味着线程空闲时间超过60S就会被杀死；如果在等待60秒期间有任务进来，他会再次拿到这个任务去执行，特点是：任务只要提交到该线程池就必然有工作线程处理。

它是可缓存的线程池，并且还会回收。它会把任务交给我们的线程，而且线程不够用的话，就会创建线程。如果线程过多，就会把这些线程给回收回来。

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                  60L, TimeUnit.SECONDS,
                                  new SynchronousQueue<Runnable>());
}
```

**newScheduledThreadPool**

定时线程池，创建一个定时线程池，即按一定的周期执行任务，即定时任务，或者设置定时时间，延迟执行任务，由于该线程池是继承ThreadPoolExecutor，所以本质上还是ThreadPoolExecutor线程池，只不过是在原来的基础上添加了定时功能，其原理是基于DelayQueue实现延迟执行，周期性执行，任务执行完毕，再次扔会到阻塞队列。

```java
public static ScheduledExecutorService newScheduledThreadPool(
            int corePoolSize, ThreadFactory threadFactory) {
    return new ScheduledThreadPoolExecutor(corePoolSize, threadFactory);
}

public ScheduledThreadPoolExecutor(int corePoolSize,
                                       ThreadFactory threadFactory) {
    super(corePoolSize, Integer.MAX_VALUE,
          DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,
          new DelayedWorkQueue(), threadFactory);
}
```

**newWorkStealingPool**

newWorkStealingPool简单翻译是**任务窃取**线程池。和别的4种不同，它用的是ForkJoinPool。使用ForkJoinPool的好处是，把1个任务拆分成多个“**小任务**”，把这些“**小任务**”分发到多个线程上执行。这些“**小任务**”都执行完成后，再将结果**合并**。之前的线程池中，多个线程共有一个阻塞队列，而newWorkStealingPool 中每一个线程都有一个自己的队列。当线程发现自己的队列没有任务了，就会到别的线程的队列里获取任务执行。可以简单理解为”**窃取**“。一般是自己的本地队列采取LIFO(后进先出)，窃取时采用FIFO(先进先出)，一个从头开始执行，一个从尾部开始执行，由于偷取的动作十分快速，会大量降低这种冲突，也是一种优化方式。

```java
public static ExecutorService newWorkStealingPool() {
    return new ForkJoinPool
        (Runtime.getRuntime().availableProcessors(),
         ForkJoinPool.defaultForkJoinWorkerThreadFactory,
         null, true);
}
```

# 线程池的核心参数有什么

为什么要自定义线程池， 首先 ThreadPoolExecutor 中，一种提供了 7 个参数，每个参数都是非常核心的属性，在线程池去执行任务时，每个参数都有决定性的作用。

但是如果直接采用 JDK 提供的方式去构建，可以设置的参数最多两个，这样就会导致对线程池的控制粒度很粗。所以在阿里规范中也推荐去自定义线程池。手动的去 new ThreadPoolExecutor 设置它的一些核心属性。

自定义构建线程池，可以细粒度的控制线程池，去管理内存的属性，并且针对一些参数的设置可能更好的在后期排查问题。

**自定义线程池七大参数**

- **int corePoolSize**

  核心线程数，创建线程池后不会立即创建核心线程，当有任务到达时才触发核心线程的创建，当线程池中的线程数目达到corePoolSize后，就会把到达的任务放到缓存队列当中。核心线程在allowCoreThreadTimeout被设置为true时会超时并被回收，默认情况下不会被回收。

- **int maximumPoolSize**

  最大线程数，代表当前线程池中，一共可以有多少个工作线程。当线程数大于或等于corePoolSize，且任务队列已满时，线程池会创建新的线程，直到线程数量达到maxPoolSize。如果线程数已等于maxPoolSize，且任务队列已满，则已超出线程池的处理能力，线程池会按照rejectedExecutionHandler配置的处理策略进行处理线程。

- **long keepAliveTime**

  非核心工作线程在阻塞队列位置等待的时间，当线程空闲时间达到keepAliveTime，该线程会退出，直到线程数量等于corePoolSize。如果allowCoreThreadTimeout设置为true，则所有线程均会退出直到线程数量为0。

- **TimeUnit unit**

  非核心工作线程在阻塞队列位置等待时间的单位。

- **BlockingQueue<Runnable> workQueue**

  任务没有在核心工作线程处理时，任务先扔到阻塞队列中。阻塞队列，用来存储等待执行的任务，新任务被提交后，会先进入到此工作队列中，任务调度时再从队列中取出任务。这里的阻塞队列有以下几种选择：

  - ArrayBlockingQueue：基于数组的有界阻塞队列，按FIFO排序
  - LinkedBlockingQueue：基于链表的无界阻塞队列（其实最大容量为Interger.MAX），按照FIFO排序
  -  PriorityBlockingQueue：具有优先级的无界阻塞队列，优先级通过参数Comparator实现
  - SynchronousQueue：一个不缓存任务的阻塞队列，也就是说新任务进来时，不会缓存，而是直接被调度执行该任务

- **ThreadFactory threadFactory**

  线程工厂，创建一个新线程时使用的工厂，可以用来设定线程名、是否为daemon线程等等。在构建线程的线程工作，可以设置thread 的一些信息。

- **RejectedExecutionHandler handler**

  当前线程池无法处理投递过来的任务时，执行当前的拒绝策略 。拒绝策略有以下：

  - AbortPolicy：当前拒绝策略在无法处理任务时，会抛出一个异常
  -  CallerRunPolicy：当前拒绝策略在线程池无法处理任务时，会将任务交给调用者处理
  - DiscardPolicy：当前拒绝策略在无法处理任务时，会直接将任务丢掉
  - DiscardOldestPolicy：当前拒绝策略在无法处理任务时,将队列中最早的任务丢掉，将当前**再次尝试**交给线程池处理
  - 自定义Policy：根据自己的任务，将任务扔到数据库，也可以做其他操作。
    

# 线程池的状态

线程池的5种状态：RUNNING、SHUTDOWN、STOP、TIDYING、TERMINATED。

```java
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
private static final int COUNT_BITS = Integer.SIZE - 3;
private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;

// runState is stored in the high-order bits
private static final int RUNNING    = -1 << COUNT_BITS;
private static final int SHUTDOWN   =  0 << COUNT_BITS;
private static final int STOP       =  1 << COUNT_BITS;
private static final int TIDYING    =  2 << COUNT_BITS;
private static final int TERMINATED =  3 << COUNT_BITS;
```

- RUNNING

  线程池一旦被创建，就处于 RUNNING 状态，任务数为 0，能够接收新任务，对已排队的任务进行处理。

- SHUTDOWN

  不接收新任务，但能处理已排队的任务。调用线程池的 shutdown() 方法，线程池由 RUNNING 转变为 SHUTDOWN 状态。

- STOP

  不接收新任务，不处理已排队的任务，并且会中断正在处理的任务。调用线程池的 shutdownNow() 方法，线程池由(RUNNING 或 SHUTDOWN ) 转变为 STOP 状态。

- TIDYING

  - SHUTDOWN 状态下，任务数为 0， 其他所有任务已终止，线程池会变为 TIDYING 状态，会执行 terminated() 方法。线程池中的 terminated() 方法是空实现，可以重写该方法进行相应的处理。
  - 线程池在 SHUTDOWN 状态，任务队列为空且执行中任务为空，线程池就会由 SHUTDOWN 转变为 TIDYING 状态。
  - 线程池在 STOP 状态，线程池中执行中任务为空时，就会由 STOP 转变为 TIDYING 状态。

- TERMINATED

  线程池彻底终止。线程池在 TIDYING 状态执行完 terminated() 方法就会由 TIDYING 转变为 TERMINATED 状态。

![img](https://raw.githubusercontent.com/chou401/pic-md/master/1408183-20191016162255593-1404242897.jpg)



# 线程池的执行流程

ThreadPoolExecutor 的 execute() 是提任务到线程池的核心方法，很重要。

```java
public void execute(Runnable command) {
  	// 提交的任务不能为 null
    if (command == null)
        throw new NullPointerException();
    /*
     * Proceed in 3 steps:
     *
     * 1. If fewer than corePoolSize threads are running, try to
     * start a new thread with the given command as its first
     * task.  The call to addWorker atomically checks runState and
     * workerCount, and so prevents false alarms that would add
     * threads when it shouldn't, by returning false.
     *
     * 2. If a task can be successfully queued, then we still need
     * to double-check whether we should have added a thread
     * (because existing ones died since last checking) or that
     * the pool shut down since entry into this method. So we
     * recheck state and if necessary roll back the enqueuing if
     * stopped, or start a new thread if there are none.
     *
     * 3. If we cannot queue task, then we try to add a new
     * thread.  If it fails, we know we are shut down or saturated
     * and so reject the task.
     */
  	// 获取核心线程 ctl，用于后面的判断
    int c = ctl.get();
  	// 如果工作线程个数小于核心线程数
  	// 满足要求，添加核心工作线程
    if (workerCountOf(c) < corePoolSize) {
      	// addWorker(任务，是核心线程吗)
      	// addWorker返回 true，代表添加工作线程成功
      	// addWorker返回 false，代表添加工作线程失败
      	// addWorker中会基于线程池状态，以及工作线程个数做判断，查看能否添加工作线程
        if (addWorker(command, true))
          	// 工作线程构建出来了，任务也交给 command 去处理了
            return;
      	// 说明线程池状态或者是工作线程个数发生了变化，导致添加失败，重新获取一次 ctl
        c = ctl.get();
    }
  	// 添加核心工作线程失败，调用一下
  	// 判断线程池状态是否是 RUNNING，如果是，正常基于阻塞队列的 offer 方法，将任务添加到阻塞队列
    if (isRunning(c) && workQueue.offer(command)) {
      	// 如果任务添加到阻塞队列成功，走 if 内部
      	// 如果任务在扔到阻塞队列之前，线程池状态突然改变了
      	// 重新获取 ctl
        int recheck = ctl.get();
      	// 如果线程池的状态不是 RUNNING，将任务从阻塞队列中移除
        if (! isRunning(recheck) && remove(command))
          	// 并且直接拒绝策略
            reject(command);
      	// 在这，说明阻塞队列有我刚刚放进去的任务
      	// 查看一下工作线程数是不是 0 个
      	// 如果工作线程为 0 个，需要添加一个非核心工作线程去处理阻塞队列中的任务
      	// 发生这种情况有两种：
      	// 1.构建线程池时，核心线程数为 0 个
      	// 2.即便有核心线程，可以设置核心线程也允许超时，设置 allowCoreThreadTimeOut 为 true，代表核心线程也可以关闭
        else if (workerCountOf(recheck) == 0)
          	// 为了避免阻塞队列中的任务饥饿，添加一个非核心工作线程去处理
            addWorker(null, false);
    }
  	// 任务添加到阻塞队列失败
  	// 构建一个非核心工作线程
  	// 如果添加非核心工作线程成功，直接完事
    else if (!addWorker(command, false))
      	// 添加失败，执行拒绝策略
        reject(command);
}
```

![image-20230828000746127](https://raw.githubusercontent.com/chou401/pic-md/master/image-20230828000746127.png)



# 线程池添加工作线程的流程

addWorker 中主要分成两大块去看

- 第一块：校验线程池的状态以及工作线程个数
- 第二块：添加工作线程并且启动工作线程

校验线程池的状态以及工作线程个数

```java
private boolean addWorker(Runnable firstTask, boolean core) {
    retry:
  	// 循环检查状态
    for (int c = ctl.get();;) {
        // Check if queue empty only if necessary.
      	// 检查线程池的运行状态是否 shutdown，任务队列是否为空等状态是否正常
        if (runStateAtLeast(c, SHUTDOWN)
            && (runStateAtLeast(c, STOP)
                || firstTask != null
                || workQueue.isEmpty()))
            return false;

      	// 循环更新状态，和线程数量的更新，都是使用 CAS 的模式更新
        for (;;) {
          	// 检查运行的线程数是否超标
            if (workerCountOf(c)
                >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))
                return false;
          	// CAS 方式更新线程数量
            if (compareAndIncrementWorkerCount(c))
              	// 更新成功后直接跳转到方法第一行，并且不在进入这些 for 循环中
              	// 因为状态啥的已经更新成功了
                break retry;
            c = ctl.get();  // Re-read ctl
          	// 如果 CAS 增加线程数失败，检查下状态是否还是和之前一样
            if (runStateAtLeast(c, SHUTDOWN))
              	// 不一样了，会跳转到第一行，并会重新进入 for 循环中走一遍流程
                continue retry;
            // else CAS failed due to workerCount change; retry inner loop
        }
    }

    boolean workerStarted = false;
    boolean workerAdded = false;
    Worker w = null;
    try {
      	// 新建一个 worker，内部包含线程
        w = new Worker(firstTask);
      	// 获取内部的线程对象
        final Thread t = w.thread;
        if (t != null) {
          	// 加锁，锁定
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                // Recheck while holding lock.
                // Back out on ThreadFactory failure or if
                // shut down before lock acquired.
                int c = ctl.get();

              	// 二次检查线程池的状态
                if (isRunning(c) ||
                    (runStateLessThan(c, STOP) && firstTask == null)) {
                  	// 池的状态没有问题，检查线程是否存活
                    if (t.getState() != Thread.State.NEW)
                        throw new IllegalThreadStateException();
                  	// worker 放入池的 hashset 中
                    workers.add(w);
                    workerAdded = true;
                    int s = workers.size();
                  	// 并将 worker 的数量赋予池中的 largestPoolSize 成员变量
                    if (s > largestPoolSize)
                        largestPoolSize = s;
                }
            } finally {
                mainLock.unlock();
            }
          	// worker 创建成功并加入 workers 成功
            if (workerAdded) {
              	// 启动线程
                t.start();
              	// 修改状态
                workerStarted = true;
            }
        }
    } finally {
        if (! workerStarted)
          	// 启动线程失败，进行失败处理
            addWorkerFailed(w);
    }
    return workerStarted;
}
```



# 线程池为何要构建空任务的非核心线程

```java
// 工作线程小于核心线程执行
if (workerCountOf(c) < corePoolSize) {
    if (addWorker(command, true))
        return;
    c = ctl.get();
}
if (isRunning(c) && workQueue.offer(command)) {
    int recheck = ctl.get();
    if (! isRunning(recheck) && remove(command))
        reject(command);
    else if (workerCountOf(recheck) == 0)
      	// 添加空任务
        addWorker(null, false);
}
else if (!addWorker(command, false))
    reject(command);
```

若是核心线程数设置的为0，我们第一次执行addWorker时，就会因为核心线程和工作线程都是0，不会执行第一块标红的区域，而是会执行第二块，而第二块是直接将任务添加到阻塞队列里面，此时是没有工作线程的，那阻塞队列里的任务由谁执行呢？所以在线程池的状态正常的情况下会添加一个空任务用于执行阻塞队列中的任务。

避免线程池出现工作队列有任务，但是没有工作线程处理。

线程池可以设置核心线程数是0个。这样，任务扔到阻塞队列，但是没有工作线程，这不凉凉了么。

线程池中的核心线程不是一定不会被回收，线程池中有一个属性，如果设置为true，核心线程也会被干掉。

```java
/**
 * If false (default), core threads stay alive even when idle.
 * If true, core threads use keepAliveTime to time out waiting
 * for work.
 */
private volatile boolean allowCoreThreadTimeOut;
```

在线程池中，当工作队列已满且活动线程数小于最大线程数时，会创建非核心线程来执行任务。即使是空任务，也可能会被分配给非核心线程来执行。这是因为线程池的设计考虑到以下几个方面的因素：

1. **任务处理的公平性**：
   空任务也被看作是一种任务，线程池需要公平地处理所有提交的任务。如果只有非空任务才会被分配给非核心线程，那么在任务队列中可能会积累大量的空任务，导致非核心线程一直处于空闲状态，而核心线程却忙于执行非空任务。
2. **响应时间的需求**：
   线程池旨在提供一种能够快速响应任务的机制。即使是空任务，也可以使线程池保持活跃状态，以便在有实际任务到来时能够立即分配线程进行执行，而不需要额外的线程创建开销。
3. **线程的复用性**：
   创建和销毁线程都需要一定的时间和资源开销。通过让非核心线程执行空任务，可以使线程池中的线程得到更好的复用，减少频繁地创建和销毁线程的开销。

总的来说，为了保持任务处理的公平性、快速响应时间和线程的复用性，线程池会将空任务也分配给非核心线程执行。

需要注意的是，空任务并不会占用实际的计算资源，因此它们不会对系统的整体性能产生负面影响。但是，在使用线程池时，确保任务的提交是有意义且合理的，避免无谓的空任务提交。

**空任务**

空任务（Empty Task）指的是在线程池中提交的一个任务，其执行过程中不需要执行任何实际的操作或逻辑。空任务本身不包含需要执行的代码，或者说它的执行代码为空或者只是一个空的循环。

空任务可能是由于以下原因之一而产生：

1. **任务队列的填充**：
   为了保持任务队列的饱满状态，或者为了占据队列中的位置以防止新任务被拒绝，可能会提交一些空任务。
2. **资源占用**：
   为了占用一定的系统资源或者保持线程池中的线程处于活跃状态，可能会提交一些空任务。

空任务的实际意义相对较小，因为它们没有具体的业务逻辑或计算任务。在实际应用中，通常会提交具有实际意义的任务来利用线程池的并发执行能力。

需要注意的是，过多的空任务可能会占用线程池的资源，导致性能下降。因此，在使用线程池时，应该确保任务的提交是有意义的，避免无谓的空任务提交。



# 线程池使用完毕为何必须shutdown()

线程池里面复用的是线程资源，而线程是系统资源的一种。所以关闭线程池是为了正确地终止线程池的运行并释放相关资源。下面是关闭线程池的重要原因：

- 释放资源：

  线程池内部会创建一定数量的线程以及其他相关资源，如线程队列、线程池管理器等。如果不及时关闭线程池，这些资源将一直占用系统资源，可能导致内存泄漏或资源浪费。

- 防止任务丢失：

  线程池中可能还有未执行的任务，如果不关闭线程池，这些任务将无法得到执行。关闭线程池时，会等待所有已提交的任务执行完毕，确保任务不会丢失。

- 优雅终止：

  关闭线程池可以让线程池中的线程正常执行完当前任务后停止，避免突然终止线程导致的资源释放不完整或状态不一致的问题。

- 避免程序阻塞：

  在某些情况下，如果不关闭线程池，程序可能会一直等待线程池中的任务执行完毕，从而导致程序阻塞，无法继续执行后续的逻辑。

因此，为了正确管理系统资源、避免任务丢失、保证程序的正常执行和避免阻塞，应当在不再需要线程池时及时关闭它。关闭线程池的一般做法是调用线程池的shutdown()方法，它会优雅地关闭线程池，等待已提交的任务执行完毕后才会终止线程池的运行。

```java
public void shutdown() {
    final ReentrantLock mainLock = this.mainLock;
    mainLock.lock();
    try {
      	// 权限检查
        checkShutdownAccess();
      	// 设置当前线程池状态为 SHUTDOWN，如果已经是这个状态直接返回
        advanceRunState(SHUTDOWN);
      	// 设置中断标准
        interruptIdleWorkers();
        onShutdown(); // hook for ScheduledThreadPoolExecutor
    } finally {
        mainLock.unlock();
    }
  	// 尝试将状态变为 TERMINATED
    tryTerminate();
}
```

shutdown()是线程池正常关闭的方法，它会先停止接收新的任务，然后等待已经提交的任务执行完毕后再停止。调用shutdown()后，线程池会逐渐停止，但不会立即停止。当线程池中的任务都执行完毕后，shutdown()会将所有的线程都关闭，这时线程池就终止了。

isShutdown()返回线程池是否已经调用过shutdown()，如果已经调用过，则返回true，否则返回false。

isTerminated()用于判断线程池中的所有任务是否已经执行完毕，并且所有线程都已经被关闭。如果是，则返回true，否则返回false。

awaitTermination()用于等待线程池中的任务执行完毕并关闭线程池。它会阻塞调用线程，直到线程池中的所有任务都执行完毕或者等待超时。该方法需要传入一个超时时间和时间单位，如果超时了，就会返回false，否则返回true。

shutdownNow()是强制关闭线程池的方法，它会尝试立即停止正在执行的任务，并返回等待执行的任务列表。调用shutdownNow()后，线程池会立即停止，但不保证所有正在执行的任务都能被停止。

需要注意的是，调用shutdownNow()会抛出InterruptedException异常，需要进行异常处理。并且，在使用shutdownNow()强制关闭线程池时，需要确保所有任务都能够正常停止，否则可能会导致任务数据丢失或其他问题。

**为何必须 shutdown()**

首先，线程池执行线程时也是通过Thread对象的start()来启动线程，这种方式的线程本身就会占用一个虚拟机栈，而虚拟机栈在JVM中属于GC Roots。

根据可达性分析算法，这个线程就不可能被回收。一直占用JVM的内存资源。这样就会造成一个问题，线程池如果没有执行shutdown或shutdownnow。

那么构建的所有核心线程就永远不能被回收，这样就会造成内存泄漏问题。除了线程内存的泄漏还有另外一个问题，线程池启动线程是基于Worker内部的Thread去启动的，当执行t.start之后，它会执行worker的run方法，接着调用runworker方法，而runworker方法的传入的是this就是当前的worker对象。

那么可以这样理解，我启动一个线程还指向Worker对象。那么worker对象也是不能被回收的，同时worker对象是线程池的内部类，就会出现内部类都不能被回收，那外部类整个线程池也不能被回收。



# 线程池的核心参数到底如何设置

**CPU 密集型和 IO 密集型**

线程任务可以分为 CPU 密集型和 IO 密集型。（平时开发基本上都是 IO 密集型任务）

CPU 密集型任务的特点是进行大量的计算，消耗 CPU 资源，比如计算圆周率、视频高清解码等。这种任务操作都是比较耗时间的操作，任务越多花在任务切换的时间就越多，CPU 执行任务效率就越低。所以，应当减少线程的数量，CPU 密集型任务同时进行的数量应当等于 CPU 的核心数。

IO 密集型的任务的特点是涉及到网络（调用三方接口）、磁盘 IO（文件操作）等。这类任务操作是 CPU 消耗很少，任务大部分时间都在等待 IO 操作完成（IO 的速度远低于 CPU 和内存的速度）。对于这种任务，任务越多，CPU 效率越高，但是也有限度。我们开发接口时，像调用别的应用接口，基本逻辑处理等，基本上都是 IO 密集型任务。

CPU 密集型尽量配置少的线程，核心线程配置：CPU 核数。而 IO 线程池应配置多的线程，核心线程配置：CPU 核数*2.这里 IO 密集型还有一种情况是线程易阻塞型，需要计算阻塞系数，核心线程配置：CPU 核数/1-阻塞系数（0.8-0.9 之间）。

**设计规则**

线程池的使用难度不大，难度在于线程池的参数并不好配置。主要难点在于任务类型无法控制，比如任务有 CPU 密集型、IO 密集型、混合型。因为 IO 我们无法直接控制，所以很多时间按照一些书上提供的一些方法，是无法解决问题的。想调试出一个符合当前任务情况的核心参数，最好的方式就是测试。需要将项目部署到测试环境或者是沙箱环境中，结果各种压测得到一个相对符合的参数。如果每次修改项目都需要重新部署，成本太高了，此时可以实现一个动态监控以及修改线程池的方案。

因为线程池的核心参数无非就是：

- corePoolSize：核心线程数
- maximumPoolSize：最大线程数
- workQueue：工作队列

线程池中提供了获取核心信息的 get 方法，同时也提供了动态修改核心属性的 set 方法。

也可以采用一些开源项目提供的方式去监控和修改。 比如 hippo4j。



# ConcurrentHashMap在1.8做了什么优化

JDK1.8 放弃了锁分段的做法，采用CAS和synchronized方式处理并发。以put操作为例，CAS方式确定key的数组下标，synchronized保证链表节点的同步效果。

JDK1.8 ConcurrentHashMap是**数组+链表**，或者**数组+红黑树**结构,并发控制使用**Synchronized关键字和CAS操作**。

1. 存储结构的优化

   数组+链表 -> 数组+链表+红黑树

2. 写数据加锁的优化

3. 扩容的优化

   协助扩容

4. 计数器的优化

   LongAddr -> Cell[] 分段和汇总

线程安全，但是复合操作时，只保证弱一致性/最终一致性。

# ConcurrentHashMap 的散列算法

当需要向 ConcurrentHashMap 中写入数据时，会根据 key 的 hashcode 来确定当前数据要放在数组的哪一个索引位置上。

```java
/**
 * Maps the specified key to the specified value in this table.
 * Neither the key nor the value can be null.
 *
 * <p>The value can be retrieved by calling the {@code get} method
 * with a key that is equal to the original key.
 *
 * @param key key with which the specified value is to be associated
 * @param value value to be associated with the specified key
 * @return the previous value associated with {@code key}, or
 *         {@code null} if there was no mapping for {@code key}
 * @throws NullPointerException if the specified key or value is null
 */
public V put(K key, V value) {
    return putVal(key, value, false);
}

/** Implementation for put and putIfAbsent */
final V putVal(K key, V value, boolean onlyIfAbsent) {
  	// 不允许 key 或者 value 值为 null（HashMap 没有这个限制）
    if (key == null || value == null) throw new NullPointerException();
  	// 根据 key 的 hashcode 计算出一个哈希值，后面得出当前 key-value 要存储在那个数组索引位置
    int hash = spread(key.hashCode());
    int binCount = 0;
    ...
}
```

当向 map 中放数据时，用的是 put() 方法，这个方法在 ConcurrentHashMap 的源码实际上是调用了 putVal() 方法。

方法中先对 key 和 value 值进行判空，ConcurrentHashMap 是不允许 key 或者 value 值为 null 的，这也是和 HashMap 的一个区别，然后开始计算 key 的 hashcode，以获取后面得出当前 key-value 要存储在哪个数组索引位置，而在计算 key 的 hash 值时，调用了一个名为 spread 的方法。

```java
/**
 * Spreads (XORs) higher bits of hash to lower and also forces top
 * bit to 0. Because the table uses power-of-two masking, sets of
 * hashes that vary only in bits above the current mask will
 * always collide. (Among known examples are sets of Float keys
 * holding consecutive whole numbers in small tables.)  So we
 * apply a transform that spreads the impact of higher bits
 * downward. There is a tradeoff between speed, utility, and
 * quality of bit-spreading. Because many common sets of hashes
 * are already reasonably distributed (so don't benefit from
 * spreading), and because we use trees to handle large sets of
 * collisions in bins, we just XOR some shifted bits in the
 * cheapest possible way to reduce systematic lossage, as well as
 * to incorporate impact of the highest bits that would otherwise
 * never be used in index calculations because of table bounds.
 */
// 将哈希值的高位传播（XOR）到低位，并将最高位强制为0。 因为表使用了2次方掩码，仅在当前掩码以上的位数不同的一组哈希值总是会发生碰撞。(已知的例子包括在小表中持有连续整数的Float键的集合)。因此，我们应用一个转换，将高位的影响向下分散。在速度、实用性和位传播的质量之间有一个权衡。因为许多常见的哈希集已经是合理分布的（所以不受益于传播），而且因为我们使用树来处理大集的碰撞，我们只是以最便宜的方式XOR一些移位的比特，以减少系统损失，以及纳入最高比特的影响，否则由于表的界限，永远不会被用于索引计算。
static final int spread(int h) {
    return (h ^ (h >>> 16)) & HASH_BITS;
}
```

这个方法将 key 的 hashcode 的高低 16 位进行 "^"（异或）运算，最终又与 HASH_BITS 进行 "&"（与）运算，此处巧妙的使用了一个转换，通过将 key 的 hashcode 右移 16 位，将其哈希值的高位向下传播到地位，并通过与 HASH_BITS 即0x7fffffff（Integer的最大值，最高位是0，其余都是1）进行的与运算将最高位强制为0。

```java
/*
 * Encodings for Node hash fields. See above for explanation.
 */
static final int MOVED     = -1; // 代表当前 hash 位置的数据正在扩容
static final int TREEBIN   = -2; // 代表当前 hash 位置下挂载的是一个红黑树
static final int RESERVED  = -3; // 预留当前索引位置
static final int HASH_BITS = 0x7fffffff; // usable bits of normal node hash
```

简单来讲就是之前用传统的方式计算 hashcode 时，由于数组长度没那么长，就导致只有低位参与计算，产生哈希冲突的概率较高，将高位与低位进行异或运算可以使得高位也参与到运算中，尽可能的减少哈希冲突，此外附属在哈希值中有特殊含义，因此采用了 spread() 方法中的方式避免生成负的哈希值。

```java
/** Implementation for put and putIfAbsent */
final V putVal(K key, V value, boolean onlyIfAbsent) {
    if (key == null || value == null) throw new NullPointerException();
    int hash = spread(key.hashCode());
    int binCount = 0;
    for (Node<K,V>[] tab = table;;) {
      	// n：数组长度
      	// i：当前 Node 要存放的索引位置
      	// f：当前数组 i 索引位置上的 Node 对象
      	// fn：当前数组 i 索引位置上数据的哈希值
        Node<K,V> f; int n, i, fh; K fk; V fv;
      	// 对是否进行过初始化的处理
      	// 判断数组是否进行过初始化 如果没有 调用 initTable() 进行初始化
        if (tab == null || (n = tab.length) == 0)
            tab = initTable();
      	// 已初始化 基于 i = (n - 1) & hash 计算出当前 Node 需要存储在哪个索引位置
      	// 通过 tabAt() 方法获取哈希表 i 位置上的数据
      	// 如果该位置为空 -> 该位置没有数据
      	// 基于 CAS 方法将数据放在 i 位置上 -> 成功放置后结束循环
        else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
            if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))
                break;                   // no lock when adding to empty bin
        }
      	// 如果哈希表 i 位置不为空（无法直接放在数组上，产生了哈希冲突）
      	// 判断当前位置是否在扩容 (fh = f.hash) == MOVED
      	// 如果等于-1，则说明数组正在进行扩容，会调用 helpTransfer() 方法进行协助扩容
        else if ((fh = f.hash) == MOVED)
            tab = helpTransfer(tab, f);
      	// 在不获取锁定的情况下检查第一个节点
        else if (onlyIfAbsent // check first node without acquiring lock
                 && fh == hash
                 && ((fk = f.key) == key || (fk != null && key.equals(fk)))
                 && (fv = f.val) != null)
            return fv;
      	// 如果不等于-1，则说明未在扩容期间，而且此时此位置下挂的不是一个链表，而是一个红黑树，接下来就是将数据存放到红黑树中的相应位置。
        else {
            V oldVal = null;
          	// 针对首个节点进行加锁操作，而不是segment，进一步减少线程冲突
            synchronized (f) {
                if (tabAt(tab, i) == f) {
                    if (fh >= 0) {
                        binCount = 1;
                        for (Node<K,V> e = f;; ++binCount) {
                            K ek;
                          	// 如果在链表中找到值为key的节点e，直接设置e.val = value即可。
                            if (e.hash == hash &&
                                ((ek = e.key) == key ||
                                 (ek != null && key.equals(ek)))) {
                                oldVal = e.val;
                                if (!onlyIfAbsent)
                                    e.val = value;
                                break;
                            }
                          	// 如果没有找到值为key的节点，直接新建Node并加入链表即可。
                            Node<K,V> pred = e;
                            if ((e = e.next) == null) {
                                pred.next = new Node<K,V>(hash, key, value);
                                break;
                            }
                        }
                    }
                  	// 如果首节点为TreeBin类型，说明为红黑树结构，执行putTreeVal操作。
                    else if (f instanceof TreeBin) {
                        Node<K,V> p;
                        binCount = 2;
                        if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                       value)) != null) {
                            oldVal = p.val;
                            if (!onlyIfAbsent)
                                p.val = value;
                        }
                    }
                    else if (f instanceof ReservationNode)
                        throw new IllegalStateException("Recursive update");
                }
            }
            if (binCount != 0) {
              	// 如果节点数>＝8，那么转换链表结构为红黑树结构。
                if (binCount >= TREEIFY_THRESHOLD)
                    treeifyBin(tab, i);
                if (oldVal != null)
                    return oldVal;
                break;
            }
        }
    }
		// 计数增加1，有可能触发transfer操作(扩容)。
    addCount(1L, binCount);
    return null;
}
```

**为什么 ConcurrentHashMap 的数组长度需要时 2^n**

```java
else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
    if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))
        break;                   // no lock when adding to empty bin
}
```

可以看到，在计算当前 Node 具体存放的索引位置时，使用了 n-1，n 代表数组长度，即这个索引位置是通过数组长度-1 与通过 spread() 方法返回的哈希值与运算计算出的，当数组长度为 2^n 时，可以最大程度的避免哈希冲突，因此有这个要求，就算给 ConcurrentHashMap 传入的大小不是 2^n，ConcurrentHashMap 也会计算出大于该数的最小 2^n，赋值给 n。

# ConcurrentHashMap 初始化流程

数组是懒加载的，第一次执行put()方法的时候才会进行初始化。

initTable()就是初始化方法，这里面有一个很重要的变量sizeCtl。

```java
/**
 * Initializes table, using the size recorded in sizeCtl.
 */
private final Node<K,V>[] initTable() {
  	// 声明标识
    Node<K,V>[] tab; int sc;
  	// 判断有没有初始化，并且完成 tab 的赋值
    while ((tab = table) == null || tab.length == 0) {
      	// 将 sizeCtl 赋值给 sc 变量，判断 sizeCtl 是否小于 0，即是否已经有线程在进行初始化了
        if ((sc = sizeCtl) < 0)
            Thread.yield(); // lost initialization race; just spin
      	// 可以尝试初始化数组，线程会以 CAS 的方式，将 sizeCtl 改为-1，代表当前线程可以初始化数组
        else if (U.compareAndSetInt(this, SIZECTL, sc, -1)) {
          	// U.compareAndSetInt(this, SIZECTL, sc, -1) 以 CAS 的方式 sizeCtrl 的值为-1
          	// 修改成功则开始初始化
            try {
              	// DCL 再次判断当前数组是否已经初始化完成
                if ((tab = table) == null || tab.length == 0) {
                  	// 开始初始化
                  	// sizeCtl > 0 就初始化 sizeCtl 长度的数组
                  	// sizeCtl = 0 就初始化默认长度的数组
                    int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                    @SuppressWarnings("unchecked")
                  	// 真正初始化操作
                    Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                    table = tab = nt;
                  	// 将 sc 赋值为下一次扩容的阈值
                  	// 如果n=16 n>>>2=4 16-4=12
                  	// 其实就是在当前数组的基础上，增加当前数组长度的 0.75
                    sc = n - (n >>> 2);
                }
            } finally {
                sizeCtl = sc;
            }
            break;
        }
    }
    return tab;
}
```

sizeCtl 是数组在初始化和扩容操作时一个控制变量，它的不同值代表不同含义

- '>0'：代表当前数组已经初始化完成，此时 sizeCtl 的值代表当前数组的扩容阈值，或者数组的初始化大小
- 0：代表当前数组还没初始化
- -1：代表当前数组正在初始化
- <-1：低于 16 为代表当前数组正在扩容的线程个数
  - 如果是 1 个线程，值就是-2
  - 如果是 2 个线程，值就是-3

初始化大致流程：

基于一个while循环，先去判断数组初始化了没有，如果没有，会再次比较sizeCtl的情况，如果正在初始化，则会通过Thread.yield()让出CPU资源，如果没有初始化，则会通过CAS的方式修改sizeCtl的值为-1，修改过后还会再次判断数组是否被初始化了（DCL，以免在当前线程修改sizeCtl的值的时候，已经有别的线程对当前数组完成了初始化），如果当前数组仍然没有被初始化，才会真正开始初始化。

# ConcurrentHashMap 扩容流程

主要有三种方式会触发扩容

1. 执行 treeifyBin() 方法进行链表转红黑树前，会先尝试通过 tryPresize() 方法进行数组扩容。

   当链表长度 > 8 时，会尝试将链表转为红黑树。

   ```java
   /**
    * Replaces all linked nodes in bin at given index unless table is
    * too small, in which case resizes instead.
    */
   // 在链表长度大于等于 8 时，尝试将链表转为红黑树
   private final void treeifyBin(Node<K,V>[] tab, int index) {
       Node<K,V> b; int n;
     	// 数组不能为空
       if (tab != null) {
         	// 在真正进行链表 -> 红黑树前，会先判断数组长度是否小于 MIN_TREEIFY_CAPACITY
         	// MIN_TREEIFY_CAPACITY 即 64
           if ((n = tab.length) < MIN_TREEIFY_CAPACITY)
             	// 如果数组长度小于 64，不能将链表转为红黑树，先尝试扩容操作
               tryPresize(n << 1);
           else if ((b = tabAt(tab, index)) != null && b.hash >= 0) {
               synchronized (b) {
                   if (tabAt(tab, index) == b) {
                       TreeNode<K,V> hd = null, tl = null;
                       for (Node<K,V> e = b; e != null; e = e.next) {
                           TreeNode<K,V> p =
                               new TreeNode<K,V>(e.hash, e.key, e.val,
                                                 null, null);
                           if ((p.prev = tl) == null)
                               hd = p;
                           else
                               tl.next = p;
                           tl = p;
                       }
                       setTabAt(tab, index, new TreeBin<K,V>(hd));
                   }
               }
           }
       }
   }
   ```

2. 执行 putAll() 方法时，会尝试通过 tryPreSize() 方法对数组进行扩容。

   putAll() 方法会传入一个 Map，原先的数组很可能会不够用，因此触发扩容。

   ```java
   /**
    * Copies all of the mappings from the specified map to this one.
    * These mappings replace any mappings that this map had for any of the
    * keys currently in the specified map.
    *
    * @param m mappings to be stored in this map
    */
   public void putAll(Map<? extends K, ? extends V> m) {
       tryPresize(m.size());
       for (Map.Entry<? extends K, ? extends V> e : m.entrySet())
           putVal(e.getKey(), e.getValue(), false);
   }
   ```

   向 tryPresize() 中传入需要添加的 Map 的 size。

   ```java
   /**
    * Tries to presize table to accommodate the given number of elements.
    *
    * @param size number of elements (doesn't need to be perfectly accurate)
    */
   // size 是将之前的数组长度，左移一位得到的结果
   private final void tryPresize(int size) {
     	// 如果扩容的长度达到了最大值，就使用最大值
     	// 否则需要保证数组的长度的为 2 的 n 次幂
     	// 这块的操作是为了初始化操作准备的，因为调用 putAll 方法时，也会触发 tryPresize 方法
     	// 如果刚刚 new 的ConcurrentHashMap 直接调用了 putAll 方法的话，会通过 tryPresize方法进行初始化
       int c = (size >= (MAXIMUM_CAPACITY >>> 1)) ? MAXIMUM_CAPACITY :
           tableSizeFor(size + (size >>> 1) + 1);
       int sc;
       while ((sc = sizeCtl) >= 0) {
           Node<K,V>[] tab = table; int n;
         	// 类似初始化的操作
           if (tab == null || (n = tab.length) == 0) {
               n = (sc > c) ? sc : c;
               if (U.compareAndSetInt(this, SIZECTL, sc, -1)) {
                   try {
                       if (table == tab) {
                           @SuppressWarnings("unchecked")
                           Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                           table = nt;
                           sc = n - (n >>> 2);
                       }
                   } finally {
                       sizeCtl = sc;
                   }
               }
           }
         	// 越界处理
           else if (c <= sc || n >= MAXIMUM_CAPACITY)
               break;
         	// transfer() 实现扩容操作
           else if (tab == table) {
             	// 计算扩容标识戳（用于协助扩容）
               int rs = resizeStamp(n);
             	// 代表没有线程正在扩容，我是第一个扩容的
               if (U.compareAndSetInt(this, SIZECTL, sc,
                                       (rs << RESIZE_STAMP_SHIFT) + 2))
                   transfer(tab, null);
           }
       }
   }
   ```

   ```java
   private final void transfer(Node<K,V>[] tab, Node<K,V>[] nextTab) {
     	// n = 数组长度
     	// stride = 每次线程一次性迁移多少数据到新数组
       int n = tab.length, stride;
     	// 基于 CPU 的内核数量来计算，每个线程一次性迁移多少长度的数据最合理
     	// NCPU = 4
     	// 数组长度为（1024-512-256-128）/ 4 = 32
     	// MIN_TRANSFER_STRIDE = 16，为每个线程迁移数据的最小长度
       if ((stride = (NCPU > 1) ? (n >>> 3) / NCPU : n) < MIN_TRANSFER_STRIDE)
           stride = MIN_TRANSFER_STRIDE; // subdivide range
     	// 第一个进来扩容的线程需要把新数组构建出来
       if (nextTab == null) {            // initiating
           try {
               @SuppressWarnings("unchecked")
             	// 将原数组长度左移一位，构建新数组长度
               Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n << 1];
             	// 赋值操作
               nextTab = nt;
           } catch (Throwable ex) {      // try to cope with OOME
             	// 到这里，说明已经达到了数组长度的最大取值范围
               sizeCtl = Integer.MAX_VALUE;
             	// 设置 sizeCtl 后直接结束
               return;
           }
         	// 将成员变量的新数组赋值
           nextTable = nextTab;
         	// 迁移数据时，用到的标识，默认为老数组长度
           transferIndex = n;
       }
     	// 新数组长度
       int nextn = nextTab.length;
     	// 在老数组迁移数据后，做的标识
       ForwardingNode<K,V> fwd = new ForwardingNode<K,V>(nextTab);
     	// 迁移数据时，需要用到的标识
     	// advance：true，代表当前线程需要接收任务，然后再执行迁移，如果为 false，代表已经接收完任务
       boolean advance = true;
     	// finishing：false，是否迁移结束
       boolean finishing = false; // to ensure sweep before committing nextTab
     	// 循环 i=15 代表当前线程迁移数据的索引值
     	// bound=0
       for (int i = 0, bound = 0;;) {
         	// f = null
         	// fh = 0
           Node<K,V> f; int fh;
         	// 当前线程要接收任务
           while (advance) {
             	// nextIndex = 16
             	// nextBound = 16
               int nextIndex, nextBound;
             	// 第一次进来，这两个判断肯定进不去
             	// 对 i 进行--，并且判断当前任务是否处理完毕
               if (--i >= bound || finishing)
                   advance = false;
             	// 判断 transferIndex 是否小于等于 0，代表没有任务可领取，结束了
             	// 在线程领取任务后，会对 transferIndex 进行修改，修改为 transferIndex - stride
             	// 在任务都领取完之后，transferIndex 肯定是小于等于 0 的，代表没有迁移数据的任务可以领取
               else if ((nextIndex = transferIndex) <= 0) {
                   i = -1;
                   advance = false;
               }
               else if (U.compareAndSetInt
                        (this, TRANSFERINDEX, nextIndex,
                         nextBound = (nextIndex > stride ?
                                      nextIndex - stride : 0))) {
                 	// 对 bound 赋值
                   bound = nextBound;
                 	// 对 i 赋值
                   i = nextIndex - 1;
                 	// 设置 advance 为 false，代表当前线程领取到任务了
                   advance = false;
               }
           }
           if (i < 0 || i >= n || i + n >= nextn) {
               int sc;
             	// finishing 为 true 代表扩容结束
               if (finishing) {
                 	// 将 nextTable 新数组设置为 null
                   nextTable = null;
                 	// 将当前数组的引用指向新数组
                   table = nextTab;
                 	// 重新计算扩容阈值 64-16=48
                   sizeCtl = (n << 1) - (n >>> 1);
                   return;
               }
             	// 当前线程没有接收到任务，让当前线程结束扩容操作
             	// 采用 CAS 的方式，将 sizeCtl - 1，代表当前并发扩容的线程数 - 1
               if (U.compareAndSetInt(this, SIZECTL, sc = sizeCtl, sc - 1)) {
                 	// sizeCtl 的高 16 位是基于数组长度计算的扩容戳，低 16 位是当前正在扩容的线程数
                   if ((sc - 2) != resizeStamp(n) << RESIZE_STAMP_SHIFT)
                     	// 代表当前线程并不是最后一个退出扩容的线程，直接结束当前线程扩容
                       return;
                 	// 如果是最后一个退出扩容的线程，将 finishing和advance 设置位 true
                   finishing = advance = true;
                 	// 将 i 设置为老数组长度，让最后一个线程再从尾到头再检查一下，是否数据全部迁移完毕
                   i = n; // recheck before commit
               }
           }
           else if ((f = tabAt(tab, i)) == null)
               advance = casTabAt(tab, i, null, fwd);
           else if ((fh = f.hash) == MOVED)
               advance = true; // already processed
           else {
               synchronized (f) {
                   if (tabAt(tab, i) == f) {
                       Node<K,V> ln, hn;
                       if (fh >= 0) {
                           int runBit = fh & n;
                           Node<K,V> lastRun = f;
                           for (Node<K,V> p = f.next; p != null; p = p.next) {
                               int b = p.hash & n;
                               if (b != runBit) {
                                   runBit = b;
                                   lastRun = p;
                               }
                           }
                           if (runBit == 0) {
                               ln = lastRun;
                               hn = null;
                           }
                           else {
                               hn = lastRun;
                               ln = null;
                           }
                           for (Node<K,V> p = f; p != lastRun; p = p.next) {
                               int ph = p.hash; K pk = p.key; V pv = p.val;
                               if ((ph & n) == 0)
                                   ln = new Node<K,V>(ph, pk, pv, ln);
                               else
                                   hn = new Node<K,V>(ph, pk, pv, hn);
                           }
                           setTabAt(nextTab, i, ln);
                           setTabAt(nextTab, i + n, hn);
                           setTabAt(tab, i, fwd);
                           advance = true;
                       }
                       else if (f instanceof TreeBin) {
                           TreeBin<K,V> t = (TreeBin<K,V>)f;
                           TreeNode<K,V> lo = null, loTail = null;
                           TreeNode<K,V> hi = null, hiTail = null;
                           int lc = 0, hc = 0;
                           for (Node<K,V> e = t.first; e != null; e = e.next) {
                               int h = e.hash;
                               TreeNode<K,V> p = new TreeNode<K,V>
                                   (h, e.key, e.val, null, null);
                               if ((h & n) == 0) {
                                   if ((p.prev = loTail) == null)
                                       lo = p;
                                   else
                                       loTail.next = p;
                                   loTail = p;
                                   ++lc;
                               }
                               else {
                                   if ((p.prev = hiTail) == null)
                                       hi = p;
                                   else
                                       hiTail.next = p;
                                   hiTail = p;
                                   ++hc;
                               }
                           }
                           ln = (lc <= UNTREEIFY_THRESHOLD) ? untreeify(lo) :
                               (hc != 0) ? new TreeBin<K,V>(lo) : t;
                           hn = (hc <= UNTREEIFY_THRESHOLD) ? untreeify(hi) :
                               (lc != 0) ? new TreeBin<K,V>(hi) : t;
                           setTabAt(nextTab, i, ln);
                           setTabAt(nextTab, i + n, hn);
                           setTabAt(tab, i, fwd);
                           advance = true;
                       }
                   }
               }
           }
       }
   }
   ```

   

   首先要确定扩容的大小

   ```java
   /**
    * The largest possible table capacity.  This value must be
    * exactly 1<<30 to stay within Java array allocation and indexing
    * bounds for power of two table sizes, and is further required
    * because the top two bits of 32bit hash fields are used for
    * control purposes.
    */
   private static final int MAXIMUM_CAPACITY = 1 << 30;
   ```

   大于最大值就取最大值（MAXIMUM_CAPACITY），不是 2^n 就计算大于它且离他最近的 2^n。

   ```java
   /**
    * Returns a power of two table size for the given desired capacity.
    * See Hackers Delight, sec 3.2
    */
   private static final int tableSizeFor(int c) {
       int n = -1 >>> Integer.numberOfLeadingZeros(c - 1);
       return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
   }
   
   public static int numberOfLeadingZeros(int i) {
       // HD, Count leading 0's
       if (i <= 0)
           return i == 0 ? 32 : 0;
       int n = 31;
       if (i >= 1 << 16) { n -= 16; i >>>= 16; }
       if (i >= 1 <<  8) { n -=  8; i >>>=  8; }
       if (i >= 1 <<  4) { n -=  4; i >>>=  4; }
       if (i >= 1 <<  2) { n -=  2; i >>>=  2; }
       return n - (i >>> 1);
   }
   ```

   接着就进入了 while 循环，

3. 执行 addCount() 操作时，如果当前元素个数达到了扩容阈值，也会进行扩容。

# ConcurrentHashMap 读取数据流程

ConcurrentHashMap 的数据查询都是以 get() 方法为入口的。

```java
/**
 * Returns the value to which the specified key is mapped,
 * or {@code null} if this map contains no mapping for the key.
 *
 * <p>More formally, if this map contains a mapping from a key
 * {@code k} to a value {@code v} such that {@code key.equals(k)},
 * then this method returns {@code v}; otherwise it returns
 * {@code null}.  (There can be at most one such mapping.)
 *
 * @throws NullPointerException if the specified key is null
 */
public V get(Object key) {
  	// tab：数组，e：查询指定位置的节点 n：数组长度
    Node<K,V>[] tab; Node<K,V> e, p; int n, eh; K ek;
  	// 基于传入的 key，计算 hash 值
    int h = spread(key.hashCode());
  	// 数组不为 null，数组上得有数据
    if ((tab = table) != null && (n = tab.length) > 0 &&
        // 查询对应数组的位置上的数据
        (e = tabAt(tab, (n - 1) & h)) != null) {
      	// hash 值一致+key 一致 返回 value
        if ((eh = e.hash) == h) {
          	// key 的==或者 equals 是否一致，如果一致，数组上就是要查询的数据
            if ((ek = e.key) == key || (ek != null && key.equals(ek)))
                return e.val;
        }
      	// hash 值<0 (特殊情况，会通过 find() 方法进行 value 的获取)
        else if (eh < 0)
          	// 三种情况：数组迁移走了、节点位置被占、红黑树
          	// 如果当前数据已经迁移到新数组 ，调用 ForwardingNode 中的 find()
            return (p = e.find(h, key)) != null ? p.val : null;
      	// 走链表操作
        while ((e = e.next) != null) {
          	// 如果 hash 值一致，并且 key 的==或者 equals 一致，返回当前链表位置的数据
            if (e.hash == h &&
                ((ek = e.key) == key || (ek != null && key.equals(ek))))
                return e.val;
        }
    }
  	// 如果上述三个流程都没有知道指定 key 对应的 value，那就是 key 不存在，返回 null 即可
    return null;
}
```

# ConcurrentHashMap 计数器的实现

计数器是用来统计 ConcurrentHashMap 的元素个数的，通过 addCount() 方法是吸纳，可以看到 addCount() 方法中有两个重要的对象：CounterCell[] 数组和 baseCount 变量。

```java
/**
 * Adds to count, and if table is too small and not already
 * resizing, initiates transfer. If already resizing, helps
 * perform transfer if work is available.  Rechecks occupancy
 * after a transfer to see if another resize is already needed
 * because resizings are lagging additions.
 *
 * @param x the count to add
 * @param check if <0, don't check resize, if <= 1 only check if uncontended
 */
private final void addCount(long x, int check) {
  	// b：原来的 baseCount
  	// s：是自增后的元素个数
    CounterCell[] cs; long b, s;
  	// 判断 CounterCell 不为 null，代表之前有冲突问题，有冲突直接进入 if 中
  	// 如果 CounterCell[] 为 null，直接执行 || 后面的 CAS 操作，直接修改 baseCount
    if ((cs = counterCells) != null ||
        // 如果对 baseCount++ 成功，直接告辞，如果 CAS 失败，直接进入 if 中
        !U.compareAndSetLong(this, BASECOUNT, b = baseCount, s = b + x)) {
      	// 进入这里，说明有并发问题
      	// 进入的方式有两种：
      	// 1.CounterCell[] 有值
      	// 2.CounterCell[] 无值，但是 CAS 失败
      	// m：数组长度 - 1
      	// c：当前线程基于随机数，获得到的数组上的某一个 CounterCell
        CounterCell c; long v; int m;
      	// 是否有冲突，默认为 true，代表没有冲突
        boolean uncontended = true;
      	// 判断 CounterCell[] 没有初始化，执行 fullAddCount 方法执行初始化
        if (cs == null || (m = cs.length - 1) < 0 ||
            // CounterCell[] 已经初始化，基于随机数拿到数组上的一个 CounterCell，如果为 null 执行 fullAddCount
            (c = cs[ThreadLocalRandom.getProbe() & m]) == null ||
            // CounterCell[] 已经初始化，并且指定索引位置上有 CounterCell
            // 直接 CAS 修改指定的 CounterCell 上的 value 即可
            // CAS 成功，直接告辞
            // CAS 失败，代表有冲突，uncontended = false，执行 fullAddCount 方法
            !(uncontended =
              U.compareAndSetLong(c, CELLVALUE, v = c.value, v + x))) {
            fullAddCount(x, uncontended);
            return;
        }
      	// 如果链表长度小于等于 1，不去判断扩容
        if (check <= 1)
            return;
      	// 将所有 CounterCell 中记录的数累加，得到最终的元素个数
        s = sumCount();
    }
  	// 判断 check 大于等于 0，remove 的操作就是小于 0的，因为添加时，才需要判断是否需要扩容
    if (check >= 0) {
        Node<K,V>[] tab, nt; int n, sc;
      	// 当前元素个数是否大于扩容阈值，并且数组不为 null，数组长度没有达到最大值
        while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&
               (n = tab.length) < MAXIMUM_CAPACITY) {
          	// 扩容表示戳
            int rs = resizeStamp(n) << RESIZE_STAMP_SHIFT;
            if (sc < 0) {
              	// 判断是否可以协助扩容
                if (sc == rs + MAX_RESIZERS || sc == rs + 1 ||
                    (nt = nextTable) == null || transferIndex <= 0)
                    break;
              	// 协助扩容
                if (U.compareAndSetInt(this, SIZECTL, sc, sc + 1))
                    transfer(tab, nt);
            }
          	// 没有线程执行扩容，我来执行扩容
            else if (U.compareAndSetInt(this, SIZECTL, sc, rs + 2))
                transfer(tab, null);
            s = sumCount();
        }
    }
}
```

他们是记录数据的两个位置：

- 并发量不高时，会通过 baseCount 进行追加
- 并发量较高时，CAS 试图操作 baseCount 失败后，会切换到 CounterCell[] 数组来计数，有多个 CounterCell 可供不同的线程进行选择。

当我们需要获取 ConcurrentHashMap 的元素个数时，会调用 size() 方法。

```java
/**
 * {@inheritDoc}
 */
public int size() {
    long n = sumCount();
    return ((n < 0L) ? 0 :
            (n > (long)Integer.MAX_VALUE) ? Integer.MAX_VALUE :
            (int)n);
}
```

而 size() 方法中调用了 sumCount() 方法

```java
final long sumCount() {
    CounterCell[] cs = counterCells;
    long sum = baseCount;
    if (cs != null) {
        for (CounterCell c : cs)
            if (c != null)
                sum += c.value;
    }
    return sum;
}
```

在 sumCount() 对 baseCount 还有数组 CounterCell[] 中的每个记录进行累加，返回最终值。



# 什么是BufferPool

**基本概念**

Buffer Pool：缓冲池，简称 BP。其作用是用来缓存表数据与索引数据，减少磁盘 IO 操作，提升效率。

Buffer Pool 由**缓存数据页（Page）**和对缓存数据页进行描述的**控制块**组成，控制块中存储着对应缓存页的所属的表空间、数据页的编号、以及对应缓存页在 Buffer Pool 中的地址等信息。

Buffer Pool 默认大小是 128M，以 Page 页为单位，Page 页默认大小 16k，而控制块的大小约为数据页的 5%，大概是 800 字节。

**如何判断一个页是否在 Buffer Pool 中缓存**

MySQL 中有一个哈希表数据结构，它使用表空间号+数据页号，作为一个 key，然后缓冲页对应的控制块作为 value。

- 当需要访问某个页的数据时，先从哈希表中根据表空间号+页号看看是否存在对应的缓存页。
- 如果有，则直接使用，如果没有，就从 free 链表中选出一个空闲的缓冲页，然后把磁盘中对应的页加载到该缓冲页的位置。



# InnoDB引擎如何管理Page页

**Page 页分类**

BP 的底层采用链表数据结构管理 Page。在 InnoDB 访问表记录和索引时会在 Page 页中缓存，以后使用可以减少磁盘 IO 操作，提升效率。

Page 根据状态可以分为三种类型：

- free page：空闲 page，未被使用
- clean page：被使用 page，数据没有被修改过
- dirty page：脏页，被使用 page，数据被修改过，Page 页中数据和磁盘的数据产生了不一致

**Page 页如何管理**

针对上面所说的三种 Page 类型，innodb 通过三种链表结构来维护和管理。

1. free list：表示空闲缓冲区，管理 free Page。
   - free 链表是把所有空闲的缓冲页对应的控制块作为一个个的节点放到一个链表中，这个链表便称之为 free 链表。
   - 基节点：free 链表中只有一个基节点是不记录缓存页信息（单独申请空间），它里面就存放了 free 链表的头节点的地址，尾节点的地址，还有 free 链表里当前有多少个节点。
2. Flush list：表示需要刷新到磁盘的缓冲区，管理 dirty page，内部 page 按修改时间排序。
   - InnoDB 引擎为了提高处理效率，在每次修改缓冲页后，并不是立刻把修改刷新到磁盘上，而是在未来的某个时间点进行刷新操作，所以需要使用到 flush 链表存储脏页，凡是被修改过的缓冲页对应的控制块都会作为节点加入到 flush 链表。
   - flush 链表的机构与 free 链表的结构相似。
3. lru list：表示正在使用的缓冲区，管理 clean page 和 dirty page，缓冲区以 midpoint 为基点，前面链表称为 new 列表区，存放经常访问的数据，占 63%；后面的链表称为 old 列表区，存放使用较少数据，占 37%。

# 为什么写缓冲区，仅适用于非唯一普通索引页

change buffer：写缓冲区，是针对二级索引（辅助索引）页的更新优化措施。

作用：在进行 dml 操作时，如果请求的辅助索引（二级索引）没有在缓冲池中时，并不会立刻将磁盘页加载到缓冲池，而是在 cb 记录缓冲变更，等未来数据被读取时，再将数据合并恢复到 bp 中。

1. change buffer 用于存储 SQL 变更操作，比如 insert/update/delete 等 SQL 语句
2. change buffer 中的每个变更操作都有其对应的数据页，并且该数据页未加载到缓存中
3. 当 change buffer 中变更操作对应的数据页加载到缓存中后，InnoDB 会把变更操作 merge 到数据页上
4. InnoDB 会定期加载 change buffer 中操作对应的数据页到缓存中，并 merge 变更操作

**change buffer 更新流程**

![image-20230905135508966](https://cdn.jsdelivr.net/gh/chou401/pic-md@master/image-20230905135508966.png)

写缓冲区，仅适用于非唯一普通索引页，为什么？

- 如果在索引设置唯一性，在进行修改时，InnoDB 必须要做唯一性校验，因此必须查询磁盘，做一次 IO 操作。会直接将记录查询到 BufferPool 中，然后在缓冲池修改，不会在 change buffer 操作。

# MySQL为什么要改进LRU算法

**普通 LRU 算法**

LRU = least recently used（最近最少使用）：就是末位淘汰法，新数据从链表头部加入，释放空间时从末尾淘汰。

![image-20230905151907748](https://cdn.jsdelivr.net/gh/chou401/pic-md@master/image-20230905151907748.png)

1.  当要访问某个页时，如果不在 buffer pool，需要把该页加载到缓冲池，并且把该缓冲页对应的控制块作为节点添加到 LRU 链表的头部。
2. 当要访问某个页时，如果在 buffer pool 中，则直接把该页对应的控制块移动到 LRU 链表的头部。
3. 当需要释放空间时，从末尾淘汰。

**普通 LRU 链表的优缺点**

优点

- 所有最近使用的数据都在链表表头，最近未使用的数据都在链表表尾，保证热数据能最快被获取到。

缺点

- 如果发生全表扫描（比如：没有建立河失的索引 or 查询时使用 select * 等），则有很大可能将真正的热数据淘汰掉。
- 由于 MySQL 中存在预读机制，很多预读的页都会被放到 LRU 链表的表头。如果这些预读的页都没有用到的话，这样，会导致很多尾部的缓冲页很快就会被淘汰。

**改进型 LRU 算法**

改进型 LRU：将链表分为 new 和 old 两个部分，加入元素时并不是从表头插入，而是从中间 midpoint 位置插入（就是说从磁盘中新读出的数据会放在冷数据区的头部），如果数据很快被访问，那么 page 就会向 new 列表头部移动，如果数据没有被访问，会逐步向 old 尾部移动，等待淘汰。

![image-20230905152906765](https://cdn.jsdelivr.net/gh/chou401/pic-md@master/image-20230905152906765.png)

冷数据区的数据页什么时候会被转到热数据区？

1. 如果该数据页在 LRU 链表中存在时间超过 1s，就将其移动到链表头部（链表指的是整个 LRU 链表）
2. 如果该数据页在 LRU 链表中存在的时间短于 1s，其位置不变（由于全表扫描有一个特点，就是它对某个页的频繁访问总耗时会很短）
3. 1s 这个时间是由参数 innodb_old_blocks_time 控制的

# 使用索引一定可以提升效率吗

索引就是排好序的，帮助我们进行快速查找的数据结构。

简单来讲，索引就是一种将数据库中的记录按照特殊形式存储的数据结构。通过索引，能够显著地提高数据查询的效率，从而提高服务器的性能。。

索引的优势与劣势

- 优点
  - 提高数据检索的效率，降低数据库的 IO 成本
  - 通过索引列对数据进行排序，降低数据排序的成本，降低了 CPU 的消耗
- 缺点
  - 创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加
  - 索引需要占物理空间，除了数据表占用数据空间之外，每一个索引还要占用一定的物理空间
  - 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，降低了数据的维护速度
- 创建索引的原则
  - 在经常需要搜索的列上创建索引，可以加快搜索的速度
  - 在作为主键的列上创建索引，强制该列的唯一性和组织表中数据的排列结构
  - 在经常用在链接的列上，这些列主要是一些外键，可以加快连接的速度
  - 在经常需要根据范围进行搜索的列上创建索引，因为索引已经排序，其制定的范围是连续的
  - 在经常需要排序的列上创建索引，因为索引已经排序，这样查询可以利用索引的排序，加快排序查询时间
  - 在经常使用在 where 子句中的列上面创建索引，加快条件的判断速度































































