## ioc容器
+  Spring的核心容器 容器负责**创建对象 链接在一起 配置对象 管理生命周期从创建到销毁**   
+  我们通过配置xml文件来进行对象的**实例化 配置 组装 ** 当然我们也可以通过注解的方式来进行
- 控制反转的意思就是new实例工作不在由程序员来交给Spring容器来做. Spring中**BeanFactory** 是ioc容器的代表


### Spring的两种容器
| 容器 | 描述 |
|------|------|
| Spring BeanFactory|它是最简单的容器org.springframework.beans.factory.BeanFactory接口来定义. |
| Spring ApplicationContext | 容器中添加了很多企业特定的功能,该容器是由org.springframework.context.ApplicationContext 定义的 |
## 总结一下就是我们基本上都是用ApplicationContext来进行工厂的处理的    

