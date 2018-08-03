# Spring 基于注解的配置  
注解在默认情况下在 Spring 容器中不打开，在 Spring 配置文件中启用
```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd">

   <context:annotation-config/>
   <!-- bean definitions go here -->

</beans>
```
|注解|描述|
|---|---|
|@Required|应用于 bean 属性的 setter 方法。|
|@Autowired|可以应用到 bean 属性的 setter 方法，非 setter 方法，构造函数和属性。|
|@Qualifier|通过指定确切的将被连线的 bean，@Autowired 和 @Qualifier 注解可以用来删除混乱。|
|JSR-250 Annotations|Spring 支持 JSR-250 的基础的注解，其中包括了 @Resource，@PostConstruct 和 @PreDestroy 注解。|

## Spring @Required 注解  
表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常。  

## Spring @Autowired 注解  
**Setter 方法中的 @Autowired**  
当 Spring 遇到一个在 setter 方法中使用的 @Autowired 注解，它会在方法中试图执行 byType 自动连接。  

**属性中的 @Autowired**  
在属性中使用 @Autowired 注解来除去 setter 方法，Spring 会将这些传递过来的值或者引用自动分配给那些属性。  

**构造函数中的 @Autowired**  
一个构造函数 @Autowired 说明当创建 bean 时，即使在 XML 文件中没有使用 元素配置 bean ，构造函数也会被自动连接。  

**@Autowired 的（required=false）选项**  
默认情况下，@Autowired 注解意味着依赖是必须的，它类似于 @Required 注解，然而，可以使用 @Autowired 的 （required=false） 选项关闭默认行为。  

## Spring @Qualifier 注解
