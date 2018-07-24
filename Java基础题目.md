### 笔试、面试中Java基础题



- 问：为什么继承自抽象类的类中方法的访问修饰符不能比抽象类中的访问修饰符更窄？

  答：因为在使用抽象类引用子类对象的时候，如果子类的访问权限低于抽象类，那么就无法调用子类对象的方法。所以，子类重写方法的权限应该比父类更广。

| 访问修饰符 | 当前类 | 同一包下 | 子类 | 其他包 |
| ---------- | ------ | -------- | ---- | ------ |
| public     | √      | √        | √    | √      |
| protected  | √      | √        | √    | ×      |
| default    | √      | √        | ×    | ×      |
| private    | √      | ×        | ×    | ×      |



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

4. 对象相等性判断准则：

   1. 非空性: obj != null
   2. 一致性: 多次判断，结果相同
   3. 自反性: a.equals(b) == b.equals(a)
   4. 传递性: a.equals(b) == true, b.equals(c) == true 那么 a.equals(c) == true 
   5. 对称性: a.equals(b) == true, b.equals(a) == true

5. 字符串的+操作其本质是创建了StringBuilder对象进行append操作，然后将拼接后的StringBuilder对象用toString方法处理成String对象

6. 静态内部类（static InnerClass）与内部类(InnerClass)的区别

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