
## Spring之前
### EJB概念

    商务软件的核心部分是它的业务逻辑。业务逻辑抽象了整个商务过程的流程，并使用计算机语言将他们实现。
    J2EE 对于这个问题的处理方法是将业务逻辑从客户端软件中抽取出来，封装在一个组件中。这个组件运行在一个独立的服务器上，客户端软件通过网络调用组件提供的服务以实现业务逻辑，而客户端软件的功能单纯到只负责发送调用请求和显示处理结果。在J2EE 中，这个运行在一个独立的服务器上，并封装了业务逻辑的组件就是EJB（Enterprise JavaBean）组件。

### EJB实现技术
#### 对象的序列化

    对象的序列化过程就是将对象状态转换成字节流和从字节流恢复对象。

#### 分布式计算与RPC Remote Procedure Call

#### RMI Remote Method Invocation

# Spring IOC容器

### Spring BeanFactory容器
这是一个最简单的容器，它主要的功能是为依赖注入 （DI） 提供支持，这个容器接口在 org.springframework.beans.factory.BeanFactor 中被定义。 BeanFactory 和相关的接口，比如BeanFactoryAware、 DisposableBean、InitializingBean，仍旧保留在 Spring 中，主要目的是向后兼容已经存在的和那些 Spring 整合在一起的第三方框架。

在 Spring 中，有大量对 BeanFactory 接口的实现。其中，最常被使用的是 **XmlBeanFactory** 类。这个容器从一个 XML 文件中读取配置元数据，由这些元数据来生成一个被配置化的系统或者应用。

在资源宝贵的移动设备或者基于 applet 的应用当中， BeanFactory 会被优先选择。否则，一般使用的是 ApplicationContext，除非你有更好的理由选择 BeanFactory。

    XmlBeanFactory factory = new XmlBeanFactory
                             (new ClassPathResource("Beans.xml"));

### Spring ApplicationContext 容器

ApplicationContext 包含 BeanFactory 所有的功能，一般情况下，相对于 BeanFactory，ApplicationContext 会更加优秀。当然，BeanFactory 仍可以在轻量级应用中使用，比如移动设备或者基于 applet 的应用程序。

## Spring Bean定义
### Bean配置元数据
|属性|描述|
|---|---|
|class|这个属性是强制性的，并且指定用来创建 bean 的 bean 类。|
|name|这个属性指定唯一的 bean 标识符。在基于 XML 的配置元数据中，你可以使用 ID 和/或 name 属性来指定 bean 标识符。|
|scope|这个属性指定由特定的 bean 定义创建的对象的作用域。|
|constructor-arg|它是用来注入依赖关系的。|
|properties|它是用来注入依赖关系的。|
|autowiring mode|它是用来注入依赖关系的，并会在接下来的章节中进行讨论。|
|lazy-initialization mode|延迟初始化的 bean 告诉 IoC 容器在它第一次被请求时，而不是在启动时去创建一个 bean 实例。|
|initialization 方法|在 bean 的所有必需的属性被容器设置之后，调用回调方法。|
|destruction 方法|当包含该 bean 的容器被销毁时，使用回调方法。|

### 三个重要的方法把配置元数据提供给 Spring 容器：
- 基于 XML 的配置文件
- 基于注解的配置
- 基于 Java 的配置

## Spring Bean作用域
|作用域|描述|
|---|---|
|singleton|在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，默认值|
|prototype|每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean()|
|request|每次HTTP请求都会创建一个新的Bean，该作用域仅适用于WebApplicationContext环境|
|session|同一个HTTP Session共享一个Bean，不同Session使用不同的Bean，仅适用于WebApplicationContext环境|
|global-session|一般用于Portlet应用环境，改作用于仅适用于WebApplicationContext环境|

### singleton 作用域
Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。
```
<!-- A bean definition with singleton scope -->
<bean id="..." class="..." scope="singleton">
    <!-- collaborators and configuration for this bean go here -->
</bean>
```

### prototype 作用域
Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。
```
<!-- A bean definition with prototype scope -->
<bean id="..." class="..." scope="prototype">
   <!-- collaborators and configuration for this bean go here -->
</bean>
```

### 如何选择
对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域

## Spring Bean生命周期
**init-method**、**destroy-method**：init-method 属性指定一个方法，在实例化 bean 时，立即调用该方法。同样，destroy-method 指定一个方法，只有从容器中移除 bean 之后，才能调用该方法。

需要注册一个在 AbstractApplicationContext 类中声明的关闭 hook 的 registerShutdownHook() 方法。它将确保正常关闭，并且调用相关的 destroy 方法。

实现InitializingBean接口afterPropertiesSet()方法或DisposableBean接口destroy()方法达到初始化和销毁的目的。

**默认的初始化和销毁方法**
```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"
    default-init-method="init"
    default-destroy-method="destroy">

   <bean id="..." class="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

</beans>
```

## Spring Bean后置处理器
**BeanPostProcessor** 接口定义回调方法，可以实现该方法来提供自己的实例化逻辑，依赖解析逻辑等。也可以在 Spring 容器通过插入一个或多个 BeanPostProcessor 的实现来完成实例化，配置和初始化一个bean之后实现一些自定义逻辑回调方法。

可以配置多个 BeanPostProcessor接口，通过设置 BeanPostProcessor 实现的 Ordered 接口提供的 order 属性来控制这些 BeanPostProcessor 接口的执行顺序。

BeanPostProcessor 可以对 bean（或对象）实例进行操作，这意味着 Spring IoC 容器实例化一个 bean 实例，然后 BeanPostProcessor 接口进行它们的工作。

ApplicationContext 会自动检测由 BeanPostProcessor 接口的实现定义的 bean，注册这些 bean 为后置处理器，然后通过在容器中创建 bean，在适当的时候调用它。  
`<bean class="com.weilw.exam.BeanPostProcessor.InitHelloWorld"/>`

## Spring Bean定义继承和模板
子 bean 的定义继承父定义的配置数据。子定义可以根据需要重写一些值，或者添加其他值。

可以创建一个 Bean 定义模板，不需要花太多功夫它就可以被其他子 bean 定义使用。在定义一个 Bean 定义模板时，你不应该指定类的属性，而应该指定带 true 值的抽象属性
