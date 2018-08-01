### Java中的泛型

​	只在程序源码中存在，在编译后的字节码文件中，就已经被替换为原来的原始类型（Raw Type，也称为裸类型）了，并且在**相应的地方插入了强制转型代码**，因此对于运行期的Java语言来说，ArrayList<Integer>与ArrayList<String>就是同一个类。所以说泛型技术实际上是Java语言的一颗语法糖，**Java语言中的泛型实现方法称为类型擦除**，基于这种方法实现的泛型被称为伪泛型。

- 泛型又叫类型化参数，与方法参数类似，泛型也有**类型形参**和**类型实参**

  ```java
  public class Main2 {
      public static void main(String[] args) {
          /*这里的String是类型实参*/
          Generic<String> generic = new Generic<>("abc");
          System.out.println(generic.getVal());
      }
  }
  /*这里的T是类型形参*/
  class Generic<T> {
      private T val;
      public Generic(T val) {this.val = val;}
      public T getVal() {return val;}
  }
  ```

- 泛型接口

  ```java
  interface Generator<T> {
      T create(T val);
  }
  /*在实现接口时，未指定类型实参，需要将泛型声明到类中*/
  class MyGenerator<T> implements Generator<T> {
      private T val;
      @Override
      public T create(T val) {
          return val;
      }
  }
  /*在实现接口时，指定了类型实参*/
  class StringGenerator implements Generator<String> {
      @Override
      public String create(String val) {
          return val;
      }
  }
  ```



- 泛型通配符

  ```java
  Generic<Integer> generic2 = new Generic<>(123);
  Generic<Number> generic3 = new Generic<>(456);
  generic3 = generic2;// 编译错误，即使Integer是Number的子类，但是不能这样转换
  ```

  ```java
  // ? 是类型实参，表示某种类型
  Generic<?> generic4 = new Generic<>(123);
  System.out.println(generic4.getVal());
  // 由于？属于类型占位符，可以代表任意类型，所以可以这样赋值
  generic4 = generic2;
  ```

- 泛型方法

  ```java
  /**
   * public 后的 T 表示泛型方法的类型形参，前面例子中的方法不属于泛型方法；这里的T可以是类的泛型的类型    形数也可以是方法自己的类型形参。
   */
  public <T> T genericMethod(Class<T> clazz) throws IllegalAccessException, InstantiationException {
      return clazz.newInstance();
  }
  ```

- 静态泛型方法：由于泛型是在运行时确定实际的类型的，所以不能使用类或接口上的类型形参；因此就应该使用泛型方法

  ```java
  // 这里的类型形参独立于类上或者接口上声明的类型形参，这里方法返回值前面的类型形参声明必须存在
  public static <E> E getElement(E e) {
      return e;
  }
  ```

- 泛型使用要求

  - 不能使用基本类型（primitive type）
  - 不管该限定是类还是接口，统一都使用关键字extends（上界，类型形参为任何继承或实现自该类）或super（下界，指定类的基类）
  - 可以使用&符号给出多个限定
  -  如果限定既有接口也有类，那么类必须只有一个，并且放在首位置（类优先原则）

  ​

- 使用反射技术来破坏泛型的限制

```java
@Test
public void test() throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
    // 使用反射来破坏泛型的限制
    List<Integer> list = new ArrayList<>();
    list.add(1);
    list.getClass().getMethod("add", Object.class).invoke(list, "234");
    for (int i = 0; i < list.size(); i++) {
        System.out.println(list.get(i));
    }
}
```



- 原始类型（Raw Type）

  原始类型就是泛型信息被擦除后，在编译成字节码后变量的真实类型。类型变量被擦除后，使用限定名替换，对于extends多个的限定名的情况，使用第一个限定名替换；没有指定限定名的则使用Object替换。