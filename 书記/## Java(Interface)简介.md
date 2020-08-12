[toc]

### Java(Interface)简介
接口（Interface），在JAVA编程语言中是一个抽象类型，是抽象方法的集合，接口通常以interface来声明。有时候必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是Java不支持多重继承。有了接口，就可以得到多重继承的效果。

接口(interface)是抽象方法和常量值的定义的集合。

接口无法被实例化，但是可以被实现。一个实现接口的类，必须实现接口内所描述的所有方法，否则就必须声明为抽象类。另外，在 Java 中，接口类型可用来声明一个变量，他们可以成为一个空指针，或是被绑定在一个以此接口实现的对象。

### 接口与类相似点

- 一个接口可以有多个方法。
- 接口文件保存在 .java 结尾的文件中，文件名使用接口名。
- 接口的字节码文件保存在 .class 结尾的文件中。
- 接口相应的字节码文件必须在与包名称相匹配的目录结构中。

### 接口与类的区别

- 接口不能用于实例化对象。
- 接口没有构造方法。
- 接口中所有的方法必须是抽象方法。
- 接口不能包含成员变量，除了 static 和 final 变量。
- 接口不是被类继承了，而是要被类实现。
- 接口支持多继承

### 实例

```
interface Animal {
   public void eat();　　//抽象的方法
   public void travel();
}
```
### 接口的实现

当类实现接口的时候，类要实现接口中所有的方法。否则类必须声明为抽象的类。

```JAVA
...implements 接口名称[, 其他接口名称, 其他接口名称..., ...] ...
```

### 实现接口的抽象方法

```JAVA
public class MammalInt implements Animal{
   public void eat(){
      System.out.println(Mammal eats);
   }
   public void travel(){
      System.out.println(Mammal travels);
   } 
   public static void main(String args[]){
      MammalInt m = new MammalInt();
      m.eat();
      m.travel();
   }
}
```