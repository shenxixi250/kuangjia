##Bean的生命周期

理解Spring Bean的生命周期很简单 

一个bean的生命周期可以简单的表述为**Bean的定义--Bean的初始化--Bean的使用--Bean的销毁 ** 


明白了 初始化和销毁的两个概念后  我们就引入两个函数 
+  init-method 这是bean中的一个属性 它会指向一个方法,在bean实例化的时候就会调用这个方法 
+ destroy-method指定一个方法 只有从容器中移除bean的时候,才能调用这个方法

#### 初始化回调及销毁回调
   总的来说是有4种方法进行实现的的
   1. 定义bean来实现initalizingBean 接口覆盖他的 afterPropertiesSet()方法 

   2. 在xml中我们定义init-method方法和destory-method方法 就可以了  

   3. 使用BeanPostProcessor接口(bean的后置处理器)
   4. 使用标记注解的方法  也就是@PostConstruct:在bean创建完成并且属性赋值完成来执行初始化方法  @preDestroy:在容器销毁之前通知我们进行清理工作



________________

这里我先介绍两种方法 也就是第一种和第二种   

第一种 

```




```

第二种就是
首先是**HelloWorld.java** 
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
   public void init(){
      System.out.println("Bean is going through init.");
   }
   public void destroy(){
      System.out.println("Bean will destroy now.");
   }
}
```

**MainApp.java** 
```
package com.tutorialspoint;
import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      AbstractApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
      obj.getMessage();
      context.registerShutdownHook();
   }
}
```
在这里，你需要注册一个在 AbstractApplicationContext 类中声明的关闭 hook 的 registerShutdownHook() 方法。它将确保正常关闭，并且调用相关的 destroy 方法。

**XML文件** 
```

<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld"
       class="com.tutorialspoint.HelloWorld"
       init-method="init" destroy-method="destroy">
       <property name="message" value="Hello World!"/>
   </bean>

</beans>
```

可以看到执行结果就是
```
Bean is going through init.
Your Message : Hello World!
Bean will destroy now.
```


仔细观察就可以看出两种不同的初始化和销毁方式的区别了  

以第二种为标准的话 第一种实际上就是不在xml中声明了  然后是我们要创建的Bean类实现接口就可以了 但是这里有一个问题我没有弄懂  为啥我们实现完 接口后   是不是destroy直接实现这个方法就可以了  但是inti方法确是实现的是afterPropertiesSet()   这个方法  ...为啥不是init呢...


