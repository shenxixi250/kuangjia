## Bean的作用域

在Spring中必须要有定义, Bean的定义域分为以下五种
+ **singleton**在spring Loc容器仅仅存在一个Bean实例,Bean以实例方式存在.
+ **prototype**每次容器中调用Bean时,都会返回一个新的实例,也就是相当于每次都会调用getBean()时都会执行newXxxBean()
+ **request**每次http请求都会建立一个新的Bean,这个作用域仅仅使用在WebApplicationContext环境中
+ **session** 	同一个HTTP Session共享一个Bean，不同Session使用不同的Bean，仅适用于WebApplicationContext环境
+ **global_-session**一般用于Portlet应用环境，该运用域仅适用于WebApplicationContext环境

[singleton]()  [prototype](pro) [requset](req) [session](ses) [global-session](glo) 
---  

1. singleton是默认的作用域 ,也就是说如果没有指定作用域的配置项的时候,它的作用域就是singleton

当一个bean的作用域是singleton,那么spring ioc 容器中只有一个存在的共享的bean实力 对于所有的对bean的请求,只要id和bean相匹配时候,在只会返回一个实例.
```
<!-- A bean definition with singleton scope -->
<bean id="..." class="..." scope="singleton">
    <!-- collaborators and configuration for this bean go here -->
</bean>
```
以上就是它的声明定义了 


**一个例子** 

| 步骤 | 描述                                                                       |
|------|----------------------------------------------------------------------------|
| 1    | 创建一个项目SpringExample的项目,并且在src文件中创建一个com.tutorialspoint |
| 2    | 添加项目必须用的jar包                                                      |
| 3    | 在com.tutorialsprint包中创建java类HelloWorld和MainApp                      |
| 4    | 创建Bean.xml文件中                                                         |
| 5    | 运行程序                                                                   |

**HelloWorld.java** 文件内容
```
package com.tutorialspoint;
public class HelloWorld {
   private String message;
   public void setMessage(String message){
      this.message  = message;
   }
   public void getMessage(){
      System.out.println("Your Message : " + message);
   }
}
```
MainApp.java中内容
```
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      HelloWorld objA = (HelloWorld) context.getBean("helloWorld");
      objA.setMessage("I'm object A");
      objA.getMessage();
      HelloWorld objB = (HelloWorld) context.getBean("helloWorld");
      objB.getMessage();
   }
}
```
下面的内容是Beans.xml
```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld" 
      scope="singleton">
   </bean>

</beans>
```

输出如图
```
Your Message : I'm object A
Your Message : I'm object A
```

可以看到我们虽然创建了两个obj 但是他们调用的getMessage方法中的message这个变量是同一个  


<A NAME="pro">prototype</A> 

当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例**。Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。**
```
<!-- A bean definition with singleton scope -->
<bean id="..." class="..." scope="prototype">
   <!-- collaborators and configuration for this bean go here -->
</bean>
```
还是使用我们的上一个例子来说
```
 <bean id="helloWorld" class="com.tutorialspoint.HelloWorld" 
      scope="prototype">
   </bean>

```

这个是我们在xml中变换的内容 就是把bean中的作用域 来换成prototype  然后我们的输出结果就会发生一些变化了
```
Your Message : I'm object A
Your Message : null
```
这个就可以看出来了 我们没有给第二个message赋值所以它的值就是null


