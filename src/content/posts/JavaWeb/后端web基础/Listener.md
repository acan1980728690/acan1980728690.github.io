---
title: Listener
published: 2025-08-02 19:10:19
tags: [JavaWeb,后端web基础]
category: 后端Web基础
draft: false
---
# Listener
---



## 概述

监听器:专门用于对域对象对象身上发生的事件或状态改变进行监听和相应处理的对象

- 监听器是GOF设计模式中,观察者模式的典型案例
- 观察者模式: 当被观察的对象发生某些改变时,观察者自动采取对应的行动的一种设计模式
- 监听器使用的感受类似JS中的事件,被观察的对象发生某些情况时,自动触发代码的执行
- 监听器并不监听web项目中的所有组件,仅仅是对三大域对象做相关的事件监听

监听器的分类

web中定义八个监听器接口作为监听器的规范,这八个接口按照不同的标准可以形成不同的分类

按监听的对象划分

-  application域监听器 ServletContextListener SeryletContextAttributeListener
-  session  域监听器 HttpSessionlistener HttpSessionAttributelistener HttpSessionBindingListener HttpsessionActivationlistener
-  request 域监听器  ServletRequestListener ServletRequestAttributeListener

按监听的事件分

- 域对象的创建和销毁监听器 ServletContextListener HttpSessionListener ServletRequestListener
- 域对象数据增删改事件监听器 ServletcontextAttributeListener HttpSessionAttributeListener  ServletRequestAtributeListener
- 其他监听器 HttpSessionBindingListener HttpSessionActivationListener

## 监听器的六个主要接口

监听器的配置方式

1. 在xml文件中

```xml
<listener>
     <listener-class>com.atguigu.listener.MyApplicationListener</listener-class>
</listener>
```

2. 在类前注解@WebListener

### application域监听器

1. ServletContextListener 监听ServletContext对象的创建和销毁

| 方法名                                    | 作用                     |
|----------------------------------------|------------------------|
| contextInitialized(ServletContextEvent sce) | ServletContext创建时调用 |
| contextDestroyed(ServletContextEvent sce)   | ServletContext销毁时调用 |

- ServletContextEvent对象代表从Servletcontext对象身上捕获到的事件，通过这个事件对象我们可以获取到ServletContext对象

2. ServletContextAttributeListener 监听ServletContext中属性的添加、移除和修改

| 方法名                                              | 作用                     |
|---------------------------------------------------|------------------------|
| attributeAdded(ServletContextAttributeEvent scab)    | 向ServletContext中添加属性时调用 |
| attributeRemoved(ServletContextAttributeEvent scab)  | 从ServletContext中移除属性时调用 |
| attributeReplaced(ServletContextAttributeEvent scab) | 当ServletContext中的属性被修改时调用 |


3. ServletContextAttributeEvent对象代表属性变化事件，它包含的方法如下 

| 方法名              | 作用                        |
|---------------------|-----------------------------|
| getName()           | 获取修改或添加的属性名      |
| getValue()          | 获取被修改或添加的属性值    |
| getServletContext() | 获取ServletContext对象      |

**示例**


```java
//通过访问路径来增删改ServletContext就不写了
@WebListener
public class MyApplicationListener implements ServletContextListener, ServletContextAttributeListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        ServletContext application =sce.getServletContext();
        System.out.println(application.hashCode()+"应用域初始化了");
    }
 
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        ServletContext application =sce.getServletContext();
        System.out.println(application.hashCode()+"应用域销毁了");
    }
 
    @Override
    public void attributeAdded(ServletContextAttributeEvent scae) {
        ServletContext application=scae.getServletContext();
        String key=scae.getName();
        Object value =scae.getValue();
        System.out.println(application.hashCode()+"应用域增加了"+key+":"+value);
    }
 
    @Override
    public void attributeRemoved(ServletContextAttributeEvent scae) {
        ServletContext application=scae.getServletContext();
        String key=scae.getName();
        Object value =scae.getValue();
        System.out.println(application.hashCode()+"应用域移除了"+key+":"+value);
    }
 
    @Override
    public void attributeReplaced(ServletContextAttributeEvent scae) {
        ServletContext application=scae.getServletContext();
        String key=scae.getName();
        Object value =scae.getValue(); //获取旧值
        Object newvalue =application.getAttribute(key);//获取新值
 
        System.out.println(application.hashCode()+"应用域修改了"+key+":"+value+"为"+newvalue);
    }
}
```

### session域监听器

```java
@WebListener
public class MySessionListener implements HttpSessionListener, HttpSessionAttributeListener {
    @Override
    public void attributeAdded(HttpSessionBindingEvent se) {
        // 任何一个session域中增加了数据都会触发该方法的执行
    }
 
    @Override
    public void attributeRemoved(HttpSessionBindingEvent se) {
        // 任何一个session域中移除了数据都会触发该方法的执行
    }
 
    @Override
    public void attributeReplaced(HttpSessionBindingEvent se) {
        // 任何一个session域中修改了数据都会触发该方法的执行
    }
 
    @Override
    public void sessionCreated(HttpSessionEvent se) {
        //任何一个session域对象的创建都会触发该方法的执行
    }
 
    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        //任何一个session域对象的销毁都会触发该方法的执行
 
    }
}
```

### request域监听器
```java
@WebListener
public class MyRequestListner implements ServletRequestListener, ServletRequestAttributeListener {
    @Override
    public void attributeAdded(ServletRequestAttributeEvent srae) {
        // 任何一个request域中增加了数据都会触发该方法的执行
 
    }
 
    @Override
    public void attributeRemoved(ServletRequestAttributeEvent srae) {
        // 任何一个request域中移除了数据都会触发该方法的执行
 
    }
 
    @Override
    public void attributeReplaced(ServletRequestAttributeEvent srae) {
        // 任何一个request域中修改了数据都会触发该方法的执行
 
    }
 
    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        // 任何一个request域销毁了都会触发该方法的执行
 
    }
 
    @Override
    public void requestInitialized(ServletRequestEvent sre) {
        // 任何一个request域初始化了都会触发该方法的执行
 
    }
}
```
## Session域的两个特殊监听器（基本不用）

### session绑定监听器

**HttpSessionBindingListener** 

监听当前监听器对象在Session域中的增加与移除

就是把这个监听器对象作为了session的一个value，绑进去的时候触发bound，解绑触发unbound

| 方法名                     | 作用                              |
|---------------------------|---------------------------------|
| valueBound(HttpSessionBindingEvent event)   | 该类的实例被放到Session域中时调用 |
| valueUnbound(HttpSessionBindingEvent event) | 该类的实例从Session中移除时调用 |

**HttpSessionBindingEvent**

对象代表属性变化事件，它包含的方法如下:

| 方法名       | 作用                       |
|--------------|----------------------------|
| getName()    | 获取当前事件涉及的属性名       |
| getValue()   | 获取当前事件涉及的属性值       |
| getSession() | 获取触发事件的HttpSession对象   |


### 钝化活化监听器

HttpSessionActivatonListener 监听某个对象在Session中的序列化与反序列化。 

| 方法名                                | 作用                                               |
|-------------------------------------|--------------------------------------------------|
| sessionWillPassivate(HttpSessionEvent se) | 该类实例和Session一起钝化到硬盘时调用                   |
| sessionDidActivate(HttpSessionEvent se)   | 该类实例和Session一起活化到内存时调用                     |

HttpSessionEvent对象代表事件对象，通过getsession()方法获取事件涉及的HttpSession对象。

**什么是钝化活化**

- session对象在服务端是以对象的形式存储于内存的,session过多,服务器的内存也是吃不消的
- 而且一旦服务器发生重启,所有的session对象都将被清除,也就意味着session中存储的不同客户端的登录状态丢失
- 为了分摊内存 压力并且为了保证session重启不丢失,我们可以设置将session进行钝化处理
- 在关闭服务器前或者到达了设定时间时,对session进行序列化到磁盘,这种情况叫做session的钝化
- 在服务器启动后或者再次获取某个session时,将磁盘上的session进行反序列化到内存,这种情况叫做session的活化

**如何配置**

懒得写了
