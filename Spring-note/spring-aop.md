# Spring 框架的 AOP  

Spring 框架的一个关键组件是面向方面的编程(AOP)框架。面向方面的编程需要把程序逻辑分解成不同的部分称为所谓的关注点。跨一个应用程序的多个点的功能被称为 **横切关注点**，这些横切关注点在概念上独立于应用程序的业务逻辑。有各种各样的常见的很好的方面的例子，如日志记录、审计、声明式事务、安全性和缓存等。

在 OOP 中，关键单元模块度是类，而在 AOP 中单元模块度是方面。依赖注入帮助你对应用程序对象相互解耦和 AOP 可以帮助你从它们所影响的对象中对横切关注点解耦。

Spring AOP 模块提供拦截器来拦截一个应用程序，例如，当执行一个方法时，你可以在方法执行之前或之后添加额外的功能。

## AOP术语  
|项|描述|
|---|---|
|Aspect|一个模块具有一组提供横切需求的 APIs。例如，一个日志模块为了记录日志将被 AOP 方面调用。应用程序可以拥有任意数量的方面，这取决于需求。|
|Join point|在应用程序中它代表一个点，可以在插件 AOP 方面。在实际的应用程序中，其中一个操作将使用 Spring AOP 框架。|
|Advice|这是实际行动之前或之后执行的方法。在程序执行期间通过 Spring AOP 框架实际被调用的代码。|
|Pointcut|这是一组一个或多个连接点，通知应该被执行。可以使用表达式或模式指定切入点。|
|Introduction|引用允许添加新方法或属性到现有的类中。|
|Target object|被一个或者多个方面所通知的对象，这个对象永远是一个被代理对象。也称为被通知对象。|
|Weaving|Weaving 把方面连接到其它的应用程序类型或者对象上，并创建一个被通知的对象。这些可以在编译时，类加载时和运行时完成。|

## 通知类型  
Spring 方面可以使用下面提到的五种通知工作：

|通知|描述|
|---|---|
|前置通知|在一个方法执行之前，执行通知。|
|后置通知|在一个方法执行之后，不考虑其结果，执行通知。|
|返回后通知|在一个方法执行之后，只有在方法成功完成时，才能执行通知。|
|抛出异常后通知|在一个方法执行之后，只有在方法退出抛出异常时，才能执行通知。|
|环绕通知|在建议方法调用之前和之后，执行通知。|

## 实现自定义方面  
Spring 支持 @AspectJ annotation style 的方法和基于模式的方法来实现自定义方面

### Spring 中基于 AOP 的 XML架构  
需要导入 spring-aop 架构：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">

   <!-- bean definition & AOP specific configuration -->

</beans>
```

#### 添加库文件  
在项目中添加 Spring AOP 指定的库文件
- aspectjrt.jar  
- aspectjweaver.jar  
- aspectj.jar  
- aopalliance.jar


#### 声明一个 aspect  
```
<aop:config>
   <aop:aspect id="myAspect" ref="aBean">
   ...
   </aop:aspect>
</aop:config>
<bean id="aBean" class="...">
...
</bean>
```

#### 声明一个切入点  
一个切入点有助于确定使用不同建议执行的感兴趣的连接点（即方法）。在处理基于配置的 XML 架构时，切入点将会按照如下所示定义：
```
<aop:config>
   <aop:aspect id="myAspect" ref="aBean">
   <aop:pointcut id="businessService"
      expression="execution(* com.xyz.myapp.service.*.*(..))"/>
   ...
   </aop:aspect>
</aop:config>
<bean id="aBean" class="...">
...
</bean>
```

#### 声明建议  
可以使用 <aop:{ADVICE NAME}> 元素在一个 中声明五个建议中的任何一个，如下所示：
```
<aop:config>
   <aop:aspect id="myAspect" ref="aBean">
      <aop:pointcut id="businessService"
         expression="execution(* com.xyz.myapp.service.*.*(..))"/>
      <!-- a before advice definition -->
      <aop:before pointcut-ref="businessService"
         method="doRequiredTask"/>
      <!-- an after advice definition -->
      <aop:after pointcut-ref="businessService"
         method="doRequiredTask"/>
      <!-- an after-returning advice definition -->
      <!--The doRequiredTask method must have parameter named retVal -->
      <aop:after-returning pointcut-ref="businessService"
         returning="retVal"
         method="doRequiredTask"/>
      <!-- an after-throwing advice definition -->
      <!--The doRequiredTask method must have parameter named ex -->
      <aop:after-throwing pointcut-ref="businessService"
         throwing="ex"
         method="doRequiredTask"/>
      <!-- an around advice definition -->
      <aop:around pointcut-ref="businessService"
         method="doRequiredTask"/>
   ...
   </aop:aspect>
</aop:config>
<bean id="aBean" class="...">
...
</bean>
```
可以对不同的建议使用相同的 **doRequiredTask** 或者不同的方法。这些方法将会作为 aspect 模块的一部分来定义。

### Spring 中基于 AOP 的 @AspectJ  
@AspectJ 作为通过 Java 5 注释注释的普通的 Java 类，它指的是声明 aspects 的一种风格。通过在 XML 配置文件中包含以下元素，@AspectJ 支持是可用的。
```
<aop:aspectj-autoproxy/>
```
#### 同样需要添加库文件
- aspectjrt.jar  
- aspectjweaver.jar  
- aspectj.jar  
- aopalliance.jar

#### 声明一个 aspect  
Aspects 类和其他任何正常的 bean 一样，除了它们将会用 @AspectJ 注释之外，它和其他类一样可能有方法和字段，如下所示：
```
@Aspect
public class AspectModule {
}
```
它们将在 XML 中按照如下进行配置，就和其他任何 bean 一样：
```
<bean id="myAspect" class="org.xyz.AspectModule">
   <!-- configure properties of aspect here as normal -->
</bean>
```

#### 声明一个切入点  
一个切入点有助于确定使用不同建议执行的感兴趣的连接点（即方法）。在处理基于配置的 XML 架构时，切入点的声明有两个部分：
1. 一个切入点表达式决定了我们感兴趣的哪个方法会真正被执行。
2. 一个切入点标签包含一个名称和任意数量的参数。方法的真正内容是不相干的，并且实际上它应该是空的。  

示例中定义了一个名为 ‘businessService’ 的切入点，该切入点将与 com.xyz.myapp.service 包下的类中可用的每一个方法相匹配：
```
import org.aspectj.lang.annotation.Pointcut;
@Pointcut("execution(* com.xyz.myapp.service.*.*(..))") // expression
private void businessService() {}  // signature
```

#### 声明建议  
可以使用 @{ADVICE-NAME} 注释声明五个建议中的任意一个，如下所示。假设已经定义了一个切入点标签方法 businessService()：
```
@Before("businessService()")
public void doBeforeTask(){
 ...
}
@After("businessService()")
public void doAfterTask(){
 ...
}
@AfterReturning(pointcut = "businessService()", returning="retVal")
public void doAfterReturnningTask(Object retVal){
  // you can intercept retVal here.
  ...
}
@AfterThrowing(pointcut = "businessService()", throwing="ex")
public void doAfterThrowingTask(Exception ex){
  // you can intercept thrown exception here.
  ...
}
@Around("businessService()")
public void doAroundTask(){
 ...
}
```

可以为任意一个建议定义切入点内联。下面是在建议之前定义内联切入点的一个示例：
```
@Before("execution(* com.xyz.myapp.service.*.*(..))")
public doBeforeTask(){
 ...
}
```
