# Spring 依赖注入

## Spring 基于构造函数的依赖注入
```
<!-- Definition for textEditor bean -->
<bean id="textEditor" class="com.tutorialspoint.TextEditor">
    <constructor-arg ref="spellChecker"/>
</bean>

<!-- Definition for spellChecker bean -->
    <bean id="spellChecker" class="com.tutorialspoint.SpellChecker">
</bean>
```

**构造函数参数解析**
- 构造函数的参数在 bean 定义中的顺序即把这些参数提供给适当的构造函数的顺序
- 如果使用 type 属性显式的指定了构造函数参数的类型，容器也可以使用与简单类型匹配的类型
- 使用 index 属性来显式的指定构造函数参数的索引

## Spring 基于setter的依赖注入
基于构造函数注入中，使用的是〈bean〉标签中的〈constructor-arg〉元素，而在基于设值函数的注入中，使用的是〈bean〉标签中的〈property〉元素
要把一个引用传递给一个对象，那么需要使用标签的 ref 属性，而如果要直接传递一个值，那么应该使用 value 属性

**使用 p-namespace 实现XML配置**
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <bean id="john" class="com.weilw.exam.dibysetter.Person"
        p:name="John Doe"
        p:person-ref="jane">
    </bean>
    <bean id="jane" class="com.weilw.exam.dibysetter.Person"
        p:name="Jane">
    </bean>
</beans>
```
## 注入内部 Beans
**inner beans** 是在其他 bean 的范围内定义的 bean
```
<bean id="outerBean" class="...">
  <property name="target">
    <bean id="innerBean" class="..."/>
  </property>
</bean>
```
## Spring 注入集合
|元素|描述|
|---|---|
|<list\>|注入一列值，允许重复。|
|<set\>|注入一组值，但不能重复。|
|<map\>|注入名称-值对的集合，其中名称和值可以是任何类型。|
|<props\>|注入名称-值对的集合，其中名称和值都是字符串类型。|

可以使用<list\>或<set\>来连接任何 java.util.Collection 的实现或数组

两种情况：  
（a）传递集合中直接的值  
（b）传递一个 bean 的引用作为集合的元素  

（a）和（b）混合使用  

**注入 null 和空字符串的值**
```
<bean id="..." class="exampleBean">
   <property name="email" value=""/>
</bean>

<bean id="..." class="exampleBean">
   <property name="email"><null/></property>
</bean>
```
