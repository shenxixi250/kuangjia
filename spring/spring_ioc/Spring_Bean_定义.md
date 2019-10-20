## Spring bean 定义

bean是相应与对象的概念 bean对象是有工厂创造出来的 通过spring ioc对bean进行**实例化 组装 ** 来管理Bean的   这些bean是由用容器提供的配置元数据来创建的  

bean的定义包含如下配置元的数据信息 
1.  如何创建一个bean
2.  bean的生命周期和详细信息 
3. bean的依赖关系  

有如下列表  

| 属性               | 描述                                                                                     |
|--------------------|------------------------------------------------------------------------------------------|
| class              | 这个属性是强制性的,并且指定用来创建bean的bean类                                          |
| name               | 这个属性指定的bean标识符,再基于xml的配置元数据中你可以使用id和name属性来指定bean的标识符 |
| scope              | 这个属性指定有特定的bean定义创建的对象作用域,他将会学习bean的作用域的时候重点讨论        |
| constructor-arg    | 用来注入依赖关系的                                                                       |
| properties         | 用来注入依赖关系的                                                                       |
| autowiring mode    | 用来注入依赖关系的                                                                       |
| initialization方法 | 在bean重点所有必须的属性被容器设置之后,回调方法.而不是在启动的时候去创建一个bean实例     |
| destruction        | 当包含该方法的容器被销毁的时候.只用回调方法                                              |

![图片](https://github.com/shenxixi250/photo/blob/master/spring_photo/spring1.jpg) 

###  Spring 配置元数
Spring Ioc容器完全有实际编写的配置的元数据的格式解耦. 其中有三种方法 吧卑职的元数据提供给Spring 容器 :
+  基于XML的配置文件
+  基于注解的配置文件 
+  基于java的配置文件  

你一进看到了如何把基于xml的配置元数据提供给容器,但是让我们看看 

```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <!-- A simple bean definition -->
   <bean id="..." class="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- A bean definition with lazy init set on -->
   <bean id="..." class="..." lazy-init="true">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- A bean definition with initialization method -->
   <bean id="..." class="..." init-method="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- A bean definition with destruction method -->
   <bean id="..." class="..." destroy-method="...">
       <!-- collaborators and configuration for this bean go here -->
   </bean>

   <!-- more bean definitions go here -->

</beans>
```

这个是完整版的 不仅仅可以定义一些 基本的在一个bean配置的属性 也可以配置一些 初始化 和销毁的方法   
这一节的** 思维导图**
	![Spring_bean定义的思维导图]() 



