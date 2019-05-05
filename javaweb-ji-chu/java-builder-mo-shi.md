# Java builder 模式

> Java 的builder 模式一开始怀疑于JavaScript中的链式调用类似，但是后来发现，其实还是有区别的，公司的JavaBean都是通过代码生成工具生成的，对应的构造函数无法接收指定的参数，所以每次赋值的时候只能一遍遍的是用set,写多了，也就开始烦了，偶尔看到同事的Android代码中可以采用类似于链式调用的方法进行赋值，就觉得很不错，于是探索了一下，其实这就是所谓的builder模式。

* 如何实现builder模式

```java
public class TestUser {
    public static int count = 0;

    private String userName;

    private int userAge;

    private String userPhone;

    public String getUserName() {
        System.out.println("开始获取userName");
        return userName;
    }

    public int getUserAge() {
        System.out.println("开始获取userAge");
        return userAge;
    }

    public String getUserPhone() {
        System.out.println("开始获取userPhone");
        return userPhone;
    }

    public void setUserName(String userName) {
        System.out.println("开始设置userName");
        this.userName = userName;
    }

    public void setUserAge(int userAge) {
        System.out.println("开始设置userAge");
        this.userAge = userAge;
    }

    public void setUserPhone(String userPhone) {
        System.out.println("开始设置userPhone");
        this.userPhone = userPhone;
    }

    private TestUser(UserBuilder builder) {
        userName = builder._userName;
        userAge = builder._userAge;
        userPhone = builder._userPhone;
    }

    public TestUser (String userName, int userAge, String userPhone) {
        this.userName = userName;
        this.userAge = userAge;
        this.userPhone = userPhone;
    }

    public static class UserBuilder {
        private String _userName;
        private int _userAge;
        private String _userPhone;

        public UserBuilder(String userName, int userAge) {
            this._userName = userName;
            this._userAge = userAge;
        }

        public UserBuilder userName (String userName) {
            _userName = userName;
            return this;
        }

        public UserBuilder userAge (int userAge) {
            _userAge = userAge;
            return this;
        }

        public UserBuilder userPhone (String userPhone) {
            _userPhone = userPhone;
            return this;
        }

        public TestUser build () {
            return new TestUser(this);
        }
    }
}
```

* 一些感悟

写多了get\set代码，会产生一些幻觉，那就是JavaBean必须要包含get\set，其实不然，这么做的原因只是为了对外暴露统一的操作方法，添加了对外的统一方法也能更好的进行数据拦截。

在内部类中可能不能使用builder模式，原因你试一下就知道了。

builder模式本质其实就是临时创建了一个类的实例用于接收初始化参数，最后调用的build方法其实还是调用一个类的私有构造函来创建一个实例。

