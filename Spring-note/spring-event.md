# Spring 中的事件处理

## Spring 内置事件

ApplicationContext 负责管理 beans 的完整生命周期。当加载 beans 时，ApplicationContext 发布某些类型的事件。例如，当上下文启动时，ContextStartedEvent 发布，当上下文停止时，ContextStoppedEvent 发布。

通过 ApplicationEvent 类和 ApplicationListener 接口来提供在 ApplicationContext 中处理事件。如果一个 bean 实现 ApplicationListener，那么每次 ApplicationEvent 被发布到 ApplicationContext 上，那个 bean 会被通知。


|Spring 内置事件|描述|  
|---|---|  
|ContextRefreshedEvent|ApplicationContext 被初始化或刷新时，该事件被发布。这也可以在 ConfigurableApplicationContext 接口中使用 refresh() 方法来发生。|
|ContextStartedEvent|当使用 ConfigurableApplicationContext 接口中的 start() 方法启动 ApplicationContext 时，该事件被发布。你可以调查你的数据库，或者你可以在接受到这个事件后重启任何停止的应用程序。|
|ContextStoppedEvent|当使用 ConfigurableApplicationContext 接口中的 stop() 方法停止 ApplicationContext 时，发布这个事件。你可以在接受到这个事件后做必要的清理的工作。|
|ContextClosedEvent|当使用 ConfigurableApplicationContext 接口中的 close() 方法关闭 ApplicationContext 时，该事件被发布。一个已关闭的上下文到达生命周期末端；它不能被刷新或重启。|
|RequestHandledEvent|这是一个 web-specific 事件，告诉所有 bean HTTP 请求已经被服务。|

由于 Spring 的事件处理是单线程的，所以如果一个事件被发布，除非所有的接收者得到的该消息，该进程被阻塞并且流程将不会继续。因此，如果事件处理被使用，在设计应用程序时应注意。

**监听上下文事**  
为了监听上下文事件，一个 bean 应该实现只有一个方法 onApplicationEvent() 的 ApplicationListener 接口。
```
public class CStartEventHandler implements ApplicationListener<ContextStartedEvent> {
    @Override
    public void onApplicationEvent(ContextStartedEvent event) {
        System.out.println("ContextStartedEvent Received");
    }
}
```
```
public class CStopEventHandler implements ApplicationListener<ContextStoppedEvent> {
    @Override
    public void onApplicationEvent(ContextStoppedEvent event) {
        System.out.println("ContextStoppedEvent Received");
    }
}
```
```
<bean id="cStartEventHandler" class="com.weilw.exam.event.CStartEventHandler"></bean>
<bean id="cStopEventHandler" class="com.weilw.exam.event.CStopEventHandler"></bean>
```
```
ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("spring/beans-event.xml");
context.start();
HelloWorld helloWorld = (HelloWorld) context.getBean("helloWorld");
helloWorld.getMessage();
context.stop();
```

## Spring 中的自定义事件  

自定义事件类继承自 ApplicationEvent 并创建构造函数：
```
public class CustomEvent extends ApplicationEvent {
    /**
     * Create a new ApplicationEvent.
     *
     * @param source the object on which the event initially occurred (never {@code null})
     */
    public CustomEvent(Object source) {
        super(source);
    }

    @Override
    public String toString() {
        return "My Custom Event.";
    }
}
```
自定义事件发布类并实现接口 ApplicationEventPublisherAware
```
public class CustomEventPublisher implements ApplicationEventPublisherAware {
    private ApplicationEventPublisher publisher;

    @Override
    public void setApplicationEventPublisher(ApplicationEventPublisher publisher) {
        this.publisher = publisher;
    }

    public void publish(){
        CustomEvent ce = new CustomEvent(this);
        publisher.publishEvent(ce);
    }
}
```
定义事件处理器：
```
public class CustomEventHandler implements ApplicationListener<CustomEvent> {
    @Override
    public void onApplicationEvent(CustomEvent event) {
        System.out.println(event.toString());
    }
}
```
配置文件：
```
<bean id="customEventHandler" class="com.weilw.exam.event.CustomEventHandler"></bean>
<bean id="customEventPublisher" class="com.weilw.exam.event.CustomEventPublisher"></bean>
```
使用：
```
ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("spring/beans-event.xml");
CustomEventPublisher cep = (CustomEventPublisher) context.getBean("customEventPublisher");
cep.publish();
cep.publish();
```
