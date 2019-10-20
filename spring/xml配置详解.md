##xml中的配置详细解析哦

|标签名称|作用位置|功能|
|-----------------|-------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|beans|全局xml文件|整个配置文件的根节点，包含一个或多个Bean元素。在该标记中可配置命名空间与schema的装载路径，<br>default-init-method属性为该配置文件中的所有Bean实例统一指定初始化方法，<br>default-destroy-method属性为该配置文件中的所有Bean实例统一指定销毁方法，<br>default-lazy-init属性为该配置文件中的所有Bean实例统一指定是否进行迟延加载|
|Bean|一个Bean|用以指导Bean工厂正确地进行Bean的生产与装配。<br>由class属性指定类全名，由id（推荐使用）或name属性指定生成的Bean实例名称。<br>init-method属性指定初始化时要调用的方法，destroy-method属性实例销毁时要调用的方法。<br>Bean实例的依赖关系可通过property子标记（set方式注入）或constructor-arg子标记（构造方式注入）来定义。<br>bean标记中的scope属性用以设定Bean实例的生成方式，当scope设置为singleton或默认时以单例模式生成，当scope设置为prototype时则以原型（多例）模式生成。|
|constructor-arg|Bean的子标记|用以传入构造参数进行实例化，这也是注入依赖关系的一种常见方式。<br>index属性指定构造参数的序号（从0开始），当只有一个构造参数是通常省略不写；<br>type属性指定构造参数的类型，当为基本数据类型时亦可省略此属性；<br>参数值可通过ref属性或value属性直接指定，也可以通过ref或value子标记指定|
|parent|Bean的子标记|它用于定义继承在子类的bean中来加入parent="Bean中的id",也就是Bean中的那个用于命名的id|
|property|Bean的子标记|它是bean标记的子标记，用以调用Bean实例中的相关Set方法完成属性值的赋值，从而完成依赖关系的注入|
|name|Bean的子标记|用于指定bean的属性名称|
|ref|是constructor-arg,<br>property,list,set,entry的子标记|由bean属性指定一个Bean工程中某个Bean实例的引用|
|value标记|是constructor-arg,<br>property,list,set,entry的子标记|用于直接指定一个常量值|
|list标记|Bean的子标记|用以封装List或数组类型的依赖注入(也就是赋值)不过具体的袁术通过ref或者value子标记指定|
|set|Bean的子标记|用以封装set类型的依赖注入(也就是赋值)不过具体的元素通过ref或者value子标记指定|
|map|Bean的子标记|标记用以封装Map类型属性的以来注入,具体的元素是通多entry子标记来指定的|
|entry|map的子标记|用作map的子标记,用以设置键/值对.key属性接收字符串类型的"键"名称,"值"可以有ref或者value子标记来指定|


beans代码示例
```
<beansxmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"
default-init-method="init"
default-destroy-method="destroy">
```


