# java对象序列化

* 一切皆对象

刚开始接触对象的时候，老板就对我说java中一切接对象，说实在的对此我并没有什么深刻的认识，那时候我还在主要写javascript，准确的是写 react,后来开始写了，发现，写的代码就是一个个的class啊！这什么鬼，不是一切皆对象的么，这是万物基于类啊！

* 类、实例、对象

随着码出的java代码越来许多，遇到的问题也越来越多，在不断的搜索过程当中，接触到最多的三个字是类、实例、对象，类就是我们手敲的代码，看看我们的代码，每个java文件对应的就是一个类，对象可以解释，万物皆对象嘛！什么都可以看做对象。三者连起来说可以说是 对象是类的实例，我认为类是抽象的，单纯的类不会有任何的作用，他需要实例化创建对象，才能发挥作用。

* 对象 与 函数

对象包含属性以及一系列对属性的操作方法，函数则为操作数据的方法，对象本身可以保存方法处理的数据，对象本身可以作为结果，仔细想了下，其实javascript也是基于对象的，我们每创建一个方法，如果将当前的作用域看做一个对象的话，函数其实也是这个对象的方法。

* java的序列化

最近在网上找了些代码来读，发现这些代码中的bean都是采用实现Serializable这个父类来写的，并且都会有一个serialVersionUID，他们为什么要这么做呢！这样做的好处是什么？

创建一个序列化类：

```java
package droolscours;

import java.io.Serializable;

public class Person implements Serializable {

    private static final long serialVersionUID = 1396622207289722435L;

    private Integer age;
    private String name;
    private String desc;

    // 构造函数重载
    public Person() {

    }

    public Person(Integer age, String name, String desc) {
        this.age = age;
        this.name = name;
        this.desc = desc;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    public static long getSerialversionuid() {
        return serialVersionUID;
    }

}
```

将对象序列化

```java
package droolscours;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class TestSerial {
    // 可能会抛出IO异常
    public static void main(String[] args) throws IOException {
        // 在同一个包下类可以直接使用
        Person person = new Person(1234, "xu", null);
        System.out.println("Person Serial" + person);
        FileOutputStream fos = new FileOutputStream("Person.txt");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(person);
        oos.flush();
        oos.close();
        System.out.println("=================over----------");
    }
}
```

将对象反序列化

```java
package droolscours;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class TestDeserial {

    public static void main(String[] args) throws IOException, ClassNotFoundException
    {
        Person person;

        FileInputStream fis = new FileInputStream("Person.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);
        person = (Person) ois.readObject();
        ois.close();
        System.out.println("Person Deserial" + person.getName());
    }
}
```

上面三段代码的作用在于演示了如何创造一个可序列化的类；如何序列化一个对象保存到文件当中；如何从文件中还原一个被序列化的对象。

一般情况下对象是被保存在内存当中的，java的对象是有状态的，所谓的状态Person person = new Person\(1234, "xu", null\); 这段代码就是在创建对象的时候设置了一个状态，这个状态下，对象中的age属性值为1234，name属性值为xu,通过序列化，我们可以将对象的这些状态保存在文件当中，其实我当时在想，对象中的方法能否被保存下来呢！不会，我大致看了下序列化后的文件，文件中保存了对象属性的类型以及对象属性的值，以及类名。

第三段代码当中，通过序列化对象文件，我们通过反序列化从而再次生成对象，该对象会恢复到被序列化之前的状态，注意在第二段代码和第三段代码中的Person类的serialVersionUID 要是相同的哦，不然是无法反序列化的。

说了这么多，类的序列化有什么作用呢？作用就在于便于传输，在分布式系统当中，对象可能会在不同的端进行处理，好像目前看到就这个作用。

2017-09-12

如果我们需要将对象数据保存到文件或是数据库时，同样的我们需要使用对象序列化的基础来实现对象的存储。

~完

