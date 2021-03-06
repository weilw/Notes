# Spring Beans 自动装配
Spring 容器可以在不使用<constructor-arg\>和<property\> 元素的情况下自动装配相互协作的 bean 之间的关系，这有助于减少编写一个大的基于 Spring 的应用程序的 XML 配置的数量。

**autowire 自动装配模式**  
|模式|描述|
|---|---|
|no|这是默认的设置，它意味着没有自动装配，应该使用显式的bean引用来连线。|
|byName|由属性名自动装配。Spring 容器看到在 XML 配置文件中 bean 的自动装配的属性设置为 byName。然后尝试匹配，并且将它的属性与在配置文件中被定义为相同名称的 beans 的属性进行连接。|
|byType|由属性数据类型自动装配。Spring 容器看到在 XML 配置文件中 bean 的自动装配的属性设置为 byType。然后如果它的类型匹配配置文件中的一个确切的 bean 名称，它将尝试匹配和连接属性的类型。如果存在不止一个这样的 bean，则一个致命的异常将会被抛出。|
|constructor|类似于 byType，但该类型适用于构造函数参数类型。如果在容器中没有一个构造函数参数类型的 bean，则一个致命错误将会发生。|
|autodetect|Spring首先尝试通过 constructor 使用自动装配来连接，如果它不执行，Spring 尝试通过 byType 来自动装配。|

**自动装配的局限性**  
|限制|描述|
|---|---|
|重写的可能性|你可以使用总是重写自动装配的 <constructor-arg>和 <property> 设置来指定依赖关系。|
|原始数据类型|你不能自动装配所谓的简单类型包括基本类型，字符串和类。|
|混乱的本质|自动装配不如显式装配精确，所以如果可能的话尽可能使用显式装配。|


## Spring 自动装配 ‘byName’  
在 XML 配置文件中 beans 的 auto-wire 属性设置为 byName。  
然后，Spring 尝试将它的属性与配置文件中定义为相同名称的 beans 进行匹配和连接。如果找到匹配项，它将注入这些 beans，否则，它将抛出异常。  

## Spring 自动装配 ‘byType’  
在 XML 配置文件中 beans 的 autowire 属性设置为 byType。  
如果它的 type 恰好与配置文件中 beans class 中的一个相匹配，Spring 将尝试匹配和连接它的属性。如果找到匹配项，它将注入这些 beans，否则，它将抛出异常。  
有多个匹配会抛出异常。

## Spring 由构造函数自动装配  
与byType类似，在 XML 配置文件中 beans 的 autowire 属性设置为 constructor。  
Spring 尝试把它的构造函数的参数与配置文件中 beans class 中的一个进行匹配和连线。如果找到匹配项，它会注入这些 bean，否则，它会抛出异常。  
有多个匹配一个。
