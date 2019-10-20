# Spring hello World实例
说明:建立一个简单的spring 输出helloworld
+  建立java项目
+ 添加必要的库
+ 创建源文件
+ 创建bean的配置文件
+ 运行程序 


### 1.建立java项目  
这个就比较简单的了**File>>New>>project** , 按照向导走下去 把项目命名为HelloSpring
### 2.添加必要的库
在**build path>>configure build path** 打开框架后在选择上面的**Libraries** 中的侧面找到**add external JARs** 按钮,添加一些必要的jar文件
###3. 创建源文件
首先要创建一个包 **com.shenxixi** 然后在**src** 中创建两个类 **HelloWorld.java** 和**MainApp.java** 文件

```
HelloWorld内容是
package com.tutorialspoint;
public class HelloWorld {
   private String message;
   public void setMessage(String message){
      this.message  = message;
   }
   public void getMessage(){
      System.out.println("Your Message : " + message);
   }
}//这就是一个最简单的类了只有一个私有的变量和他的访问set和get方法
```
**MainApp.java** 
```
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = 
             new ClassPathXmlApplicationContext("Beans.xml");
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
      obj.getMessage();
   }
} //这个就是一个main函数  显示了我们如何通过 ApplicationContext 这是spring中的工厂方法  context就是我们创建出来的工厂实例了
//然后我们通过在Bean.xml中配置好HelloWorld这个类 然后就可以直接调用 工厂实例中的getBean方法了  然后它就能够返回一个对象给
//我们,我们在通过创建一个HelloWorld来接手这个对象  当然也可以直接创建 Object 类运用多态来继承这个类了
```
值得注意的 使用**getBean** 方法所获得的bean后 后面的就可以直接使用这个Bean中的方法了  

### 4.创建bean的配置文件  也就是.xml文件了
1.  这个文件需要在**src** 文件夹下面创建了  
2.  这个文件名可以随便取但是必须要以XML来进行结尾
3.  **Bean.xml** 中用于给不同的bean来创建唯一的ID,控制不同值的对象的创建,但是不影响任何Spring的源文件

```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

   <bean id="helloWorld" class="com.tutorialspoint.HelloWorld">
       <property name="message" value="Hello World!"/>
   </bean>

</beans>

```
这个xml文件就可以看出  **id** 是我们想给**class** 分配的别名了.
下面的**property** 就是我们给其中的**name 变量** 赋值
**value具体值** 
### 运行程序  
编译和运行**Main** 

会输出
```
Your Message: Hello World!
```

#总结就是一句话这个HelloWorld就是把赋值和我创建类的权利交给类我们工厂类context然后通过工厂的赋值等操作后然后再把一个做好的类或者说是Bean 交给我们  我们用obj来接受然后调用一些 里面的方法这是我对这个的理解
