# spring 注解

> 使用spring有段时间了，前段时间对spring的俩大核心概念控制反转与依赖注入有了一定的了解，但是还存在一定的困惑，其实这俩个概念的名字有点复杂，但是最近以此对spring注解的探索中开始这俩个概念的具体实现有了一定的理解，总的理解就是spring为应用创建了一个类似于楚门的世界中类似的环境。

- 四个注解

问题的起因在于，在使用springboot的过程中，配置完Dao 和 Mapper XML 之后法向，spring并不能初始化Dao层的bean,本质上Java代码都是在写类，代码运行时通过类初始化一个个实列，初始化实例需要new 关键字，但是知道你有没有发现在spring应用中，如果你的类使用了spring注解，在应用中可以不需要通过new关键字就可以实例化。

先说下上面的问题，上面的问题是因为Dao相关的类没有被springboot扫描到，目前还没有发现可以使用哪个注解能够时Dao层相关的类自动初始化，不知道为啥？ ````想想可能是因为Dao 层中不是类而是接口，这也是为什么在service层中我们的@Service注解要写在service层的实现类上而不是卸载接口上```

好接着一开始的话，在spring中是可以不通过new关键字来实例化的，一切交给spring，在早期的spring当中普遍使用的是通过xml 配置bean 来实例化，现在我们会看到普遍使用bean来初始化实例。

在spring中有这么几个比较常用的注解: @Component,@Repository,@Service,@Controller这样几个注解，这几个注解都是可以作用在类上的，他们有一个共同的特点是能够在项目启动期间将类实例化并加载到spring容器当中，在需要使用的时候可以直接使用。

其中我们已最为简单的@Component注解来说，他的作用就是一个将实例化好的bean加载到spring容器当中，当我们需要在类当中使用的时候，通过依赖注入的方法注入到类当中。

我们可以通过2种方法实现依赖的注入：

1. 通过注解进行依赖注入 @Autowired
2. 还有一种方法是，因为@Service,@Controller这样的注解类，在项目启动的初期必然是会被实例化的，无论如何实例话，总是需要调用构造函数的，所以我们可以通过构造函数进行依赖注入。

````java
@Component
public class SysMenu {

    // 菜单编号
    private int mid;

    // 菜单名称
    private String mname;

    // 菜单层级
    private int mdepth;

    // 菜单icon
    private String micon;

    // 菜单跳转路由
    private String mroutename;

    // 菜单的父级菜单
    private int mpid;

    // 菜单所属菜单组编号
    private int mgid;

    // 菜单是否存在子菜单
    private Boolean issub;

    // 子菜单列表
    private List<SysMenu> children;

    public SysMenu() {
        System.out.println("SysMenu正在被实例化");
        this.mid = 0;
        this.mname = "";
        this.mdepth = 0;
        this.micon = "";
        this.mroutename = "";
        this.mpid = 0;
        this.mgid = 0;
        this.issub = false;
        this.children = new ArrayList<>();
    }

    private SysMenu(Builder builder) {
        this.mid = builder.mid;
        this.mname = builder.mname;
        this.mdepth = builder.mdepth;
        this.micon = builder.micon;
        this.mroutename = builder.mroutename;
        this.mpid = builder.mpid;
        this.mgid = builder.mgid;
        this.issub = builder.issub;
        this.children = builder.children;
    }

    public static Builder newSysMenu() {
        return new Builder();
    }

    public int getMid() {
        return mid;
    }

    public void setMid(int mid) {
        this.mid = mid;
    }

    public String getMname() {
        return mname;
    }

    public void setMname(String mname) {
        this.mname = mname;
    }

    public int getMdepth() {
        return mdepth;
    }

    public void setMdepth(int mdepth) {
        this.mdepth = mdepth;
    }

    public String getMicon() {
        return micon;
    }

    public void setMicon(String micon) {
        this.micon = micon;
    }

    public String getMroutename() {
        return mroutename;
    }

    public void setMroutename(String mroutename) {
        this.mroutename = mroutename;
    }

    public int getMpid() {
        return mpid;
    }

    public void setMpid(int mpid) {
        this.mpid = mpid;
    }

    public int getMgid() {
        return mgid;
    }

    public void setMgid(int mgid) {
        this.mgid = mgid;
    }

    public Boolean getIssub() {
        return issub;
    }

    public void setIssub(Boolean issub) {
        this.issub = issub;
    }

    public List<SysMenu> getChildren() {
        return children;
    }

    public void setChildren(List<SysMenu> children) {
        this.children = children;
    }


    public static final class Builder {
        private int mid;
        private String mname;
        private int mdepth;
        private String micon;
        private String mroutename;
        private int mpid;
        private int mgid;
        private Boolean issub;
        private List<SysMenu> children;

        private Builder() {
        }

        public SysMenu build() {
            return new SysMenu(this);
        }

        public Builder mid(int mid) {
            this.mid = mid;
            return this;
        }

        public Builder mname(String mname) {
            this.mname = mname;
            return this;
        }

        public Builder mdepth(int mdepth) {
            this.mdepth = mdepth;
            return this;
        }

        public Builder micon(String micon) {
            this.micon = micon;
            return this;
        }

        public Builder mroutename(String mroutename) {
            this.mroutename = mroutename;
            return this;
        }

        public Builder mpid(int mpid) {
            this.mpid = mpid;
            return this;
        }

        public Builder mgid(int mgid) {
            this.mgid = mgid;
            return this;
        }

        public Builder issub(Boolean issub) {
            this.issub = issub;
            return this;
        }

        public Builder children(List<SysMenu> children) {
            this.children = children;
            return this;
        }
    }
}
····

····java
````
