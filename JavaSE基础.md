### Java基础



- 问：为什么继承自抽象类的类中方法的访问修饰符不能比抽象类中的访问修饰符更窄？

  答：因为在使用抽象类引用子类对象的时候，如果子类的访问权限低于抽象类，那么就无法调用子类对象的方法。所以，子类重写方法的权限应该比父类更广。

| 访问修饰符 | 当前类 | 同一包下 | 子类 | 其他包 |
| ---------- | ------ | -------- | ---- | ------ |
| public     | √      | √        | √    | √      |
| protected  | √      | √        | √    | ×      |
| default    | √      | √        | ×    | ×      |
| private    | √      | ×        | ×    | ×      |

- 同步、异步、阻塞、非阻塞以及它们之间相互组合

  - **同步与异步**
    同步和异步关注的是**消息通信机制** (synchronous communication/ asynchronous communication)
    所谓同步，就是在发出一个*调用*时，在没有得到结果之前，该*调用*就不返回。但是一旦调用返回，就得到返回值了。
    换句话说，就是由*调用者*主动等待这个*调用*的结果。

    而异步则是相反，**调用在发出之后**，这个调用就直接返回了，所以没有返回结果**。换句话说，当一个异步过程调用发出后，调用者不会立刻得到结果。而是在调用发出后，**被调用者通过状态、通知来通知调用者，或通过回调函数处理这个调用。

  - **阻塞与非阻塞**

    阻塞和非阻塞关注的是**程序在等待调用结果**（消息，返回值）时的状态.

    阻塞调用是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才会返回。
    非阻塞调用指在不能立刻得到结果之前，该调用不会阻塞当前线程。

- 重载与重写
  - 重写：重新实现父类或者接口中的方法（not static、not final）
  - 重载：重新实现类中的方法，要求方法的签名要不同
    - JVM中：方法签名 = 修饰符 + （返回值类型）+ （全类名）+ 方法名 + 形参类型列表
    - Java重载条件中：方法签名 = 方法名 +　形参类型列表

- 多线程辅助类：CountDownLatch、CyclicBarrier、Semaphore

- 死锁产生的条件

- wait、notify、notifyAll

- 并发

- Java NIO提供了与标准IO不同的IO工作方式： 

  - **Channels and Buffers（通道和缓冲区）**：标准的IO基于字节流和字符流进行操作的，而NIO是基于通道（Channel）和缓冲区（Buffer）进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。
  - **Asynchronous IO（异步IO）**：Java NIO可以让你异步的使用IO，例如：当线程从通道读取数据到缓冲区时，线程还是可以进行其他事情。当数据被写入到缓冲区时，线程可以继续处理它。从缓冲区写入通道也类似。
  - **Selectors（选择器）**：Java NIO引入了选择器的概念，选择器用于监听多个通道的事件（比如：连接打开，数据到达）。因此，单个的线程可以监听多个数据通道。







### 相关题目

1. 面向对象特征

   1. 抽象：

      - 数据抽象：抽象类（abstract class)中的default、protected、public属性
      - 行为抽象：抽象类（abstract method），接口类（interface）

   2. 继承：类继承、接口继承。

   3. 封装：将类的数据成员封装在类中，提供API访问类的数据成员，更容易实现对对象状态的控制。

   4. 多态：

      - 编译时多态：方法的重载（前绑定）
      - 运行时多态：方法的重写（后绑定）

      ​

2. Integer与int的区别

   Integer类是int这种基本类型的包装类，其他七中基本类型也都有对应的包装类。

   区别：

   1. == 作用于int时比较的是值，作用于Integer时比较的是引用的地址
   2. Integer.equals(Object obj) 内部首先判断对象是否属于Integer，然后比较Integer对象内部的value值

   例：

   ```java
   @Test
       public void test1() {
           /*
           在将字面量赋值给对象的时候，会调用Integer.valueOf()方法，Integer类内部维护了一个
           私有静态内部类IntegerCache，用来缓存值在[-128, 127]范围内的Integer对象。
           注：127并不是一个准确的数，具体需要看操作的系统环境
           */
           Integer a = 127;
           Integer b = 127;
           System.out.println(a == b); // true
           a = 128;
           b = 128;
           System.out.println(a == b); // false
       }

       @Test
       public void test2() {
           Integer a = 127; // 自动装箱
           int b = 127;
           System.out.println(a == b); // true
           System.out.println(a == 127); // true, 自动拆箱
           System.out.println(a.equals(127)); // 首先将127装箱成Integer对象，然后比较Integer对象的value值
       }
   ```

   ​

3. ```java
   public native String intern();
   // 首先查询常量池中是否包含字符串，有则返回常量池中的引用，否则创建新的字符串并放入常量池中
   ```

4. String s = new String(“xyz”);
   创建了几个String Object？（ A ）
   A. 1
   B. 2
   C. 3
   D. 4

5. 对象相等性判断准则：

   1. 非空性: obj != null
   2. 一致性: 多次判断，结果相同
   3. 自反性: a.equals(b) == b.equals(a)
   4. 传递性: a.equals(b) == true, b.equals(c) == true 那么 a.equals(c) == true 
   5. 对称性: a.equals(b) == true, b.equals(a) == true

6. 字符串的+操作其本质是创建了StringBuilder对象进行append操作，然后将拼接后的StringBuilder对象用toString方法处理成String对象

7. 静态内部类（static InnerClass）与内部类(InnerClass)的区别

   静态内部类的创建不依赖外部类，内部类的创建依赖于外部类对象

   ```java
   public class OutterClass {

       public static class InnerClass {
           public void method() {
               System.out.println(getClass().getName() + " method()");
           }
       }

       public class InnerClass2{
           public void method() {
               System.out.println(getClass().getName() + " method()");
           }
       }

       public void method() {
           System.out.println(getClass().getName() + " method()");
       }

   }

   public class Test {

       public static void main(String[] args) {
           OutterClass outterClass = new OutterClass();
           // 静态内部类相当于类的成员
           OutterClass.InnerClass innerClass = new OutterClass.InnerClass();
           // 内部类相当于类的实例属性
           OutterClass.InnerClass2 innerClass2 = outterClass.new InnerClass2();
           outterClass.method();
           innerClass.method();
           innerClass2.method();
           // com.jackaroo.chap01.item04.OutterClass method()
   	   // com.jackaroo.chap01.item04.OutterClass$InnerClass method()
           // com.jackaroo.chap01.item04.OutterClass$InnerClass2 method()
       }

   }
   ```

8. Java中对象克隆（Clone）

   1. Java对象实现Cloneable接口，重写clone()方法
   2. Java对象的输入输出流，需要类实现Serializable接口（父类实现该接口，所有子类都可以进行序列化；子类实现该接口但父类没有实现，那么会丢失父类中的数据）
   3. 利用JSON类库将对象序列化为JSON形式的字符串，在需要Java对象时将JSON字符串反序列化为Java对象（常用的JSON类库：Jackson，FastJson）

9. 程序的输出结果：

   ```java
   class Annoyance extends Exception {}
   class Sneeze extends Annoyance {}

   public class CollectionTest {

       @Test
       public void test() {
           try {
               try {
                   throw new Sneeze();
               }
               catch ( Annoyance a ) {
                   // Sneeze是Annoyance的子类，所以可以catch到
                   System.out.println("Caught Annoyance");
                   throw a; // 将异常继续向上抛
               }
           }
           catch ( Sneeze s ) {
               // 捕获到内部try..catch抛出的异常
               System.out.println("Caught Sneeze");
               return ;
           }
           finally {
               // 无论如何都会执行，并且会覆盖return（建议不要这么做）
               System.out.println("Hello World!");
           }
           /* 输出结果：
           Caught Annoyance
           Caught Sneeze
           Hello World!
           */
       }

   }
   ```

10. 线程的基本状态以及之间的转换

   ![1532508887597](D:\ProgramLearning\笔记\JavaSE基础\assets\1532508887597.png)

11. 遍历目录下所有的文件

   1. ```java
      File dir = new File("src/main/resources");
      if (dir.isDirectory()) {
          for (File item : dir.listFiles()) {
              if (item.isFile()) {
                  System.out.println(item.getName());
              }
          }
      }
      ```

   2. ```java
      Files.walk(Paths.get("src/main/resources"), 1).forEach(e -> {
          if (Files.isRegularFile(e)) {
              System.out.println(e.getFileName());
          }
      });
      ```

   3. ```java
      Files.walkFileTree(Paths.get("src/main/resources"), new SimpleFileVisitor<Path>() {
          @Override
          public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
              System.out.println(file.getFileName());
              return FileVisitResult.CONTINUE;
          }
      });
      ```

12.  接口interface不是继承自Object

   如果一个接口定义是最顶级的(没有 super interfaces)，那么这个接口会自动声明一个 abstract member method 结构体来代表所有来自 Object 类（一切类的superclass）中的public方法（包括这些方法的签名、返回类型以及抛出的异常）

   再换种说法——意思是接口隐含定义了一套与Object类中的方法签名完全相同的方法，所以，我们在程序中调用接口的那些与Object中具有相同签名的方法时，编译器不会报错！

13. 关于垃圾回收，下列说法正确的是（ C  ）
   A. Jdk1.6的默认垃圾回收器是G1
   B. PermGen内存区域不会被垃圾回收器回收
   C. 引用计数算法是一种常用的垃圾回收算法
   D. 执行垃圾回收时，java代码不会停止运行