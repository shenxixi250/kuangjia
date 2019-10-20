## Spring ApplicationContext 容器
+ **Application Context** 是  *BeanFactory** 子接口
- Application Context是spring中比较高级的容器 他可以加载配置文件中定义的bean中的解析文本信息和将事件传递给所指定的监听器.
- 这个容器也被其他的三个接口所实现  

| 接口                            | 功能                                                                                                                                                     |
|---------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| FileSystemXmlApplicationContext | 该容器从xml中加载已经被定义的bean.在这需要传入xml的完整路径                                                                                              |
| ClasspathXmlApplicationContext  | 这个容器从xml文件中加载已经被定义的bean.在这里不需要提供xml文件的完整路径,只需要在CLASSPATH环境变量中加入xml的路径即可容器就可以自动寻找bean的配置文件了 |
| WebXmlApplicationContext        | 这个容器会在一个web的应用程序范围里加载XMl文件中已经被定义 的bean                                                                                        |

WebXmlApplicationContext 是在web应用中常用到的以后还会了解 



这里我就先举一个 FileSystemXmlApplicationContext的例子  

其实例子和之前那个helloworld是一样的不过就是在构造工厂实例的时候会有些差别 
 FileSystemXmlApplicationContext  context = new  FileSystemXmlApplicationContex("C:/user/nihao/work/Beans.xml");   注意这个路径就需要是全路径了  





