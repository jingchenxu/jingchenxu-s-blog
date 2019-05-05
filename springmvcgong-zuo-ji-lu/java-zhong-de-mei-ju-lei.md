# Java 中的枚举类

## 枚举类常用的使用场景

枚举类在Java项目中最常用的使用场景就是参数，从实际的业务出发比如说是订单状态，我们通常会使用 01 02 03 这样的字符串作为参数，但是这样的参数不够生动形象啊，比如 01 表示 未支付 02 表示 已支付 03 表示已评价，在代码中出现一堆的 01 02 03不是很好的体验，咋办，使用枚举类，具体代码如下：

```java
public class test {
    public enum OrderSatus {
        NOPAY("01", "未支付"), PAYED("02", "已支付"), JUDGED("03", "已评价");

        private final String id;
        private final String name;

        private OrderSatus (String id, String name) {
            this.id = id;
            this.name = name;
        }

        public String getId() {
            return id;
        }

        public String getName() {
            return name;
        }

        public static OrderSatus getById (String id) {
            for (OrderSatus orderStatus: values()) {
                if (orderStatus.getId().equals(id)) {
                    return orderStatus;
                }
            }
            return null;
        }
    }

    public static void main(String[] args) {
        System.out.println(OrderSatus.PAYED.getName());
    }
}
```

## 枚举对象作为switch条件

在日常的代码中，代码既是未能够正常运行，同样也要能让继任者或是协同者更好的阅读代码，就像是在阅读文字一样，从我的角度来看，枚举类就起到了这样的作用，枚举类可创建的对象是有限的，每个对象中的对象名是对状态的总体描述，对象的id属性是面向数据库的状态，而name是面向人类，让人类更好的理解当前的状态为所代表的意义，switch条件支持以枚举类对象作为条件，在编程的时候有助于我们编写出更加语义化的代码。

## 简单的介绍以下枚举类

1. 枚举类 enum 和class、interface的地位是一样的，枚举类也是类但是可实例话的实例是有限的，而且这些实例都是枚举类自行创建的，不需要通过new关键字初始化实例。
2. 使用enum定义的枚举类默认继承了java.lang.Enum, 而不是继承Object类。枚举类可以实现一个或多个接口。
3. 枚举类的所有实例都必须放在第一行展示，不需要使用new关键字，不需要显示调用构造器。自动添加public static final 修饰。
4. 使用enum定义、非抽象的枚举类默认使用final修复，不可以被继承。
5. 枚举类的构造器只能是私有的。

