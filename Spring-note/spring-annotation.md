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
@Required 注解应用于 bean 属性的 setter 方法，表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常。  

## Spring @Autowired 注解  
**Setter 方法中的 @Autowired**  
当 Spring 遇到一个在 setter 方法中使用的 @Autowired 注解，它会在方法中试图执行 byType 自动连接。  

**属性中的 @Autowired**  
在属性中使用 @Autowired 注解来除去 setter 方法，Spring 会将这些传递过来的值或者引用自动分配给那些属性。  

**构造函数中的 @Autowired**  
一个构造函数 @Autowired 说明当创建 bean 时，即使在 XML 文件中没有使用元素配置 bean ，构造函数也会被自动连接。  

**@Autowired 的（required=false）选项**  
默认情况下，@Autowired 注解意味着依赖是必须的，它类似于 @Required 注解，然而，可以使用 @Autowired 的 （required=false） 选项关闭默认行为。  

## Spring @Qualifier 注解  
可能会有这样一种情况，当创建多个具有相同类型的 bean 时，并且想要为一个属性使用它们其中的一个进行装配，在这种情况下，可以使用 @Qualifier 注解和 @Autowired 注解通过指定哪一个真正的 bean 将会被装配来消除混乱。  

## Spring JSR-250 注解  
### @PostConstruct 和 @PreDestroy 注解  
类似于init-method 和 destroy-method，可以使用 @PostConstruct 注解作为初始化回调函数的一个替代，@PreDestroy 注解作为销毁回调函数的一个替代。

### @Resource 注解  
可以在属性中或者 setter 方法中使用 @Resource 注解，@Resource 注解使用一个 ‘name’ 属性，该属性以一个 bean 名称的形式被注入。可以说，它遵循 **by-name** 自动连接语义。  

如果没有明确地指定一个 ‘name’，默认名称源于字段名或者 setter 方法。在字段的情况下，它使用的是字段名；在一个 setter 方法情况下，它使用的是 bean 属性名称。
