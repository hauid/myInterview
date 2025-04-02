### SpringBoot

#### IOC

**控制反转（Inversion of Control）是软件工程中的一项原则**，它将对象或程序部分的控制权转移到容器或框架中。在面向对象的编程中经常使用



#### **Spring Bean的定义**

​	在 Spring 中，**Bean** 是由 Spring 容器管理的一个对象。通常，Bean 代表了应用程序中的一个对象或服务。Spring 会负责创建、配置、初始化以及销毁这些对象。Bean 的定义通常是在 **配置文件（XML）** 或 **注解（Java 类）** 中进行的

在 Spring 中，Bean 可以通过以下方式定义：

##### 1. **使用注解**

- **`@Component`**：表示一个普通的 Bean 类，它会被自动扫描并注册到 Spring 容器中。
- **`@Service`**：用于定义服务层的 Bean，功能上与 `@Component` 类似，区别在于语义上表示它是一个服务层组件。
- **`@Repository`**：用于定义数据访问层的 Bean，它代表一个 DAO（数据访问对象）类。
- **`@Controller`**：用于定义控制器层的 Bean，通常用于 Spring MVC 的控制器类。



#### 生命周期

Bean 的生命周期概括起来就是 **4 个阶段**：

1. 实例化（Instantiation）
2. 属性赋值（Populate）为 bean 设置相关属性和依赖
3. 初始化（Initialization）
4. 缓存转移(把一二级的缓存删除)
5. 销毁（Destruction）



#### Spring三级缓存

**核心思想**:把Bean的实例化和Bean依赖注入进行分离

##### **三级缓存的目的**

Spring 容器在处理 **单例 Bean** 的创建和管理时，为了防止在并发情况下对同一 Bean 的重复实例化，采用了三级缓存的设计，避免在多个线程之间造成线程安全问题，并提高容器的性能。

三级缓存的作用如下：

1. **防止重复创建 Bean 实例**：通过一级缓存 `singletonObjects` 来存储完全初始化的 Bean，避免每次请求都重新实例化 Bean。
2. **解决循环依赖问题**：二级缓存和三级缓存解决了在 Bean 初始化过程中相互依赖的循环问题，确保即使有循环依赖，Bean 也能正常实例化。
3. **提高性能**：通过缓存 Bean 工厂方法，Spring 避免了每次都重新实例化 Bean，提升了应用的性能。



**一级缓存**：**`singletonObjects`**

第一级存放完全**初始化**好的Bean,可以直接使用

**二级缓存**：**`earlySingletonObjects`**

第二级存放原始的Bean,已经**实例化**但还没有进行**赋值**或依赖注入

暴露一个**部分初始化完成的 Bean**

**三级缓存**：**`singletonFactories`**

第三级存放Bean工厂对象,生成原始Bean对象放入二级缓存中(工厂设计模式)



三级缓存作用:aop







<img src="C:\Users\pqy\AppData\Roaming\Typora\typora-user-images\image-20250323184859665.png" alt="image-20250323184859665" style="zoom:50%;" />



##### 具体步骤

Spring 在创建单例 Bean 时会按照以下步骤使用三级缓存：

步骤 1：首先检查 **一级缓存（singletonObjects）**

- 当请求某个单例 Bean 时，Spring 会先检查该 Bean 是否已经在 **一级缓存**（`singletonObjects`）中。如果存在，则直接返回该实例。

步骤 2：如果一级缓存中没有，检查 **二级缓存（earlySingletonObjects）**

- 如果 **一级缓存** 中没有该 Bean，Spring 会检查 **二级缓存**（`earlySingletonObjects`）。**二级缓存** 存放的是那些已经部分初始化的单例 Bean。例如，如果一个 Bean 正在初始化，但该 Bean 引用的其他 Bean 还未初始化，Spring 会将部分初始化的实例放入二级缓存中，以便在出现循环依赖时，提前返回一个尚未完全初始化的 Bean。

步骤 3：如果二级缓存中没有，用三级缓存创建新的 Bean 并缓存

- 如果该 Bean 既不在 **一级缓存**，也不在 **二级缓存** 中，Spring 会开始实例化该 Bean。
- 在实例化过程中，Spring 会通过 **三级缓存（`singletonFactories`）** 保存一个工厂方法来创建该 Bean。创建过程中会调用工厂方法，返回一个新的 Bean 实例，放入三级缓存中。

步骤 4：将新创建的 Bean 放入 **一级缓存（singletonObjects）**

- 在实例化并完成初始化后，Bean 被完全创建并且所有的依赖项注入完成。此时，Spring 会将该 Bean 实例放入 **一级缓存（`singletonObjects`）** 中，表示这个 Bean 可以被安全地使用。

步骤 5：清理 **二级缓存（earlySingletonObjects）** 和 **三级缓存（singletonFactories）**

- Bean 完全初始化后，Spring 会清理 **二级缓存** 和 **三级缓存**，因为此时 Bean 已经完全初始化，可以安全地返回给容器中的所有组件。





#### @Autowired 进行依赖注入，常见的方式，有如下三种：

**1.属性注入(简洁,实际常用)**

优点：代码简洁、方便快速开发。

缺点：**隐藏了类之间的依赖关系、可能会破坏类的封装性(无方法,用的是反射实现)。**

**2.构造函数注入(官方推荐)**

优点：能清晰地看到类的依赖关系、用final提高了代码的安全性。

缺点：代码繁琐、如果构造参数过多，可能会导致构造函数臃肿。

注意：如果只有一个构造函数，@Autowired注解可以省略.(通常来说,也只有一个构造函数)

**3. setter注入(少)**

优点：保持了类的封装性，依赖关系更清晰。

缺点：需要额外编写setter方法，增加了代码量。

**注意事项:**
存在多个bean，框架不知道具体要注入哪个bean使用，所以就报错了。
	方案一：使用**@Primary**注解
当存在多个相同类型的Bean注入时，加上@Primary注解，来确定默认的实现。
@Primary
@Service
public class UserServiceImpl implements UserService {	xxx...	}

​	方案二：**使用@Qualifier注解**
指定当前要注入的bean对象. 在@Qualifier的value属性中,指定注入的bean的名称(方法第一个首字符默认小写).@Qualifier注解不能单独使用，必须配合@Autowired使用.
@Qualifier("userServiceImpl")

​	方案三：**使用@Resource注解**
是按照bean的名称进行注入。通过name属性指定要注入的bean的名称.
**@Resource(name = "userServiceImpl")**



**@Autowird 与 @Resource的区别**

- @Autowired 是**spring框架提供的注解**，而@Resource是**JDK提供的注解**
- @Autowired **默认是按照类型注入**，而@Resource是**按照bean名称注入**





#### 依赖注入（DI）是什么？

依赖注入是可以用来实现 IoC 的一种模式，其中被反转的控制是 **设置对象的依赖**。



#### 逻辑链

是什么 有什么用 具体怎么用 原理



aop基于动态代理机制

动态代理基于反射机制





#### **Spring AOP **

**将横切关注点（如日志、统计接口耗时,事务、权限等）与业务逻辑分离**，提升代码复用性和可维护性

在 AOP 中，有几个重要的概念和组件：

- **切面（Aspect）**：切面是 AOP 中的核心概念，表示一个模块化的横切关注点。它可以包含多个增强（如前置增强、后置增强、环绕增强等）。

- **连接点（Joinpoint）**：连接点是程序执行中的某个点，例如方法的执行。AOP 通过代理类来拦截这些连接点并执行增强。

  连接点并不是 AOP 的一部分，它是 AOP 能够插入增强逻辑的地方，是 AOP 处理的粒度单位。

  例子

  在 Java 中，连接点通常是方法的执行。比如：

  - 一个方法的调用：`proxy.addUser("Alice")`。
  - 方法执行时：`addUser()` 方法执行的过程。

- **切入点（Pointcut）**：切入点是一个表达式，用来匹配一组连接点，定义了哪些连接点需要应用切面（增强）。换句话说，切入点是连接点的**筛选器**，它告诉 AOP 代理应该在哪些连接点上织入增强逻辑。

  例子

  在 Java 中，切入点是通过表达式来选择的，例如：

  ​		`execution(* com.example.service.UserService.*(..))`：这表示所有 `UserService` 类中的方法执行都会被切入点匹配。

  ​		`@annotation(com.example.MyAnnotation)`：这表示所有带有 `@MyAnnotation` 注解的方法执行都会被切入点匹配。

- **增强（Advice）**：增强是切面中对目标方法的增强逻辑。它有不同的类型，如前置增强（before）、后置增强（after）、环绕增强（around）等。

- **目标对象（Target Object）**：目标对象是我们要增强的对象，也就是 AOP 代理的对象。
- **代理对象（Proxy）**：代理对象是通过 AOP 创建的代理，它在运行时替代了目标对象的角色。





对于@around

`@Around` 通知通过 **拦截-执行-处理** 的机制，提供了对目标方法的最大控制权。适用于需要精细控制方法执行流程的场景（如分布式锁、性能监控），但需谨慎使用以避免逻辑复杂性和性能问题。

1. **必须调用 `proceed()`**：
   如果忘记调用 `joinPoint.proceed()`，目标方法将不会执行。



#### 动态代理

**动态代理** 是一种在**运行时**动态**生成代理对象**的机制，用于在不修改原始类代码的前提下，增强目标方法的功能（如添加日志、事务等）。其核心是通过**代理对象**间接调用目标方法，实现功能的横向扩展。

场景1：
如果委托类没有实现接口的话，就不能使用newProxyInstance方法，进而不能使用**JDK动态代理**
场景2：
Cglib是针对类来实现代理的，对指定的目标类生成一个子类，通过方法拦截技术拦截所有父类方法的调用。因为是生成子类，所以就不能用在final修饰的类上。综合起来，就是 **被final修饰的类** ，不可以被spring代理



##### **JDK动态代理和CGLIB代理如何选择？**

二者都是基于反射原理

- **JDK代理**：目标类**实现接口**时使用（默认优先）。
- **CGLIB**：字节码增强,目标类**无接口**时使用（通过生成子类代理）。**基于继承,更加灵活**

```java
public interface UserService {
    void saveUser();
}

public class UserServiceImpl implements UserService {
    @Override
    public void saveUser() {
        System.out.println("保存用户");
    }
}

// JDK 动态代理
public class JdkProxy implements InvocationHandler {
    private Object target;

    public Object bind(Object target) {
        this.target = target;
        return Proxy.newProxyInstance(
            target.getClass().getClassLoader(),
            target.getClass().getInterfaces(),
            this
        );
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("[前置通知]");
        Object result = method.invoke(target, args); // 调用目标方法
        System.out.println("[后置通知]");
        return result;
    }
}
```



注解





#### 事务

**@Transactional 是声明式事务管理 编程中使用的注解**

**原理**:

**springAOP技术** 生成动态代理对象

**1 .添加位置**

1）**接口实现类或接口实现方法**上，而不是接口类中。
2）访问权限：**public 的方法才起作用**。@Transactional 注解应该只被应用到 public 方法上，这是由 **Spring AOP**的本质决定的。
系统设计：将标签放置在需要进行事务管理的方法上，而不是放在**所有接口实现类**上：只读的接口就不需要事务管理，由于配置了@Transactional就需要AOP拦截及事务的处理，可能影响系统性能。

**异常回滚的属性：rollbackFor** 
默认情况下，只有出现RuntimeException(运行时异常)才会回滚事务。
假如我们想让所有的异常都回滚，需要来配置@Transactional注解当中的rollbackFor属性，通**过rollbackFor这个属性可以指定出现何种异常类型回滚事务。**

事务传播行为：propagation
属性值			 					含义
**REQUIRED**			【默认值】需要事务，有则加入，无则创建新事务(从方法)
**REQUIRES_NEW**		需要新事务，无论有无，总是创建新事务







#### 写法

加载并读取文件的标准写法,用类加载器:
**InputStream in = this.getClass().getClassLoader().getResourceAsStream("user.txt");**