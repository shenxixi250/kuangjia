# Hibernate框架学习

[简单的hibernate](#hib) 
[Hibernate的原理](#ylh) 
[增删改查](#zsg)  
[关系Hebernate](#gxh)  
[概念Hibernate](gnh)  
[注解](#zj) 

<A NAME="hib">简单的hibernate</A>  

1.创建一个数据库 


```
	
create database test;

use test;

CREATE TABLE product_ (
  id int(11) NOT NULL AUTO_INCREMENT,
  name varchar(30) ,
  price float ,
  PRIMARY KEY (id)
) DEFAULT CHARSET=UTF8;

```


2.创建一个项目导入相应的jar包

```xml
maven中应该导入的代码
```

3.创建实体类

```java
package com.how2java.pojo;
  
public class Product {
    int id;
    String name;
    float price;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public float getPrice() {
        return price;
    }
    public void setPrice(float price) {
        this.price = price;
    }
      
}
```

4.配置Product.hbm.xml 
早包com.how2java.pojo下新建一个配置文件Product.hbm.xml,用于映射Product类对应数据库中的product_表   
也就是
```
<class name="Product" table="product_">

```

接下来类中表示属性的id 映射到表中的字段id 
```
<property name="name"/>
```

整个的xml的配置就是下面的代码了
```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
 
<hibernate-mapping package="com.how2java.pojo">
    <class name="Product" table="product_">
        <id name="id" column="id">
            <generator class="native">
            </generator>
        </id>
        <property name="name" />
        <property name="price" />
    </class>
     
</hibernate-mapping>
```
<generator class="native"> 意味着id的自增长方式采用数据库的本地方式


5.配置hibernate.cfg.xml

在 kengkeng 目录下创建hibernate.cfg.xml 
配置访问数据库 的驱动,url,账号密码等  


在src目录下创建 hibernate.cfg.xml
配置访问数据库要用到的驱动，url，账号密码等等
其他配置及含义：

 ```
<property name="dialect">org.hibernate.dialect.MySQLDialect</property>
``` 

这表示使用MYSQL方言。 什么方言呢？ 因为在代码层面，开发人员不用关心底层到底用Oracle还是Mysql，写的代码都是一样的。 可是Oracle和Mysql所用的sql语句的语法是有所区别的，那么这件事就交给Hibernate来做了。这个时候就需要告诉Hibernate底层用的是什么数据库，它才知道应该用什么样的“方言” 去对话。
 ```
<property name="current_session_context_class">thread</property>
 ```

这是Hibernate事务管理方式，即每个线程一个事务

 ```
<property name="show_sql">true</property>
 
```
这表示是否在控制台显示执行的sql语句

```
<property name="hbm2ddl.auto">update</property>
 ```

这表示是否会自动更新数据库的表结构，有这句话，其实是不需要创建表的，因为Hibernate会自动去创建表结构

 ```
<mapping resource="com/how2java/pojo/Product.hbm.xml" />
 ```

这表示Hibernate会去识别Product这个实体类 

6.测试类
```
package com.how2java.test;
 
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
 
import com.how2java.pojo.Product;
 
public class TestHibernate {
    public static void main(String[] args) {
 
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
 
        Session s = sf.openSession();
        s.beginTransaction();
 
        Product p = new Product();
        p.setName("iphone7");
        p.setPrice(7000);
        s.save(p);
         
        s.getTransaction().commit();
        s.close();
        sf.close();
    }
 
}

```

hibernate的基本步骤是：
1. 获取SessionFactory
2. 通过SessionFactory 获取一个Session
3. 在Session基础上开启一个事务
4. 通过调用Session的save方法把对象保存到数据库
5. 提交事务
6. 关闭Session
7. 关闭SessionFactory


项目的结构图

![项目的结构图](kengkeng) 






<A NAME="ylh">原理介绍</A> 

上面是hibernate一个简单的例子  下面我来简单的介绍一下hibernate的原理

![原理图](kengkeng) 




#### Hibernate的对象状态

+ 瞬时 指的是没有和hibernate发生任何关系，在数据库中也没有对应的记录，一旦JVM结束，这个对象也就消失了
+ 持久 指得是一个对象和hibernate发生联系，有对应的session,并且在数据库中有对应的一条记录
+ 离线 指的是一个对象虽然在数据库中有对应的一条记录，但是它所对应的session已经关闭了

一张图来理解转化关系
![转化关系图](kengkeng) 

这里仅仅介绍最简单的概念  以及一个最简单的流程
```
package com.how2java.test;
  
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;
 
import com.how2java.pojo.Product;
  
public class TestHibernate {
    /**
     * @param args
     */
    public static void main(String[] args) {
  
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
  
        Session s = sf.openSession();
        s.beginTransaction();
        Product p = new Product();
        p.setName("p1");
        System.out.println("此时p是瞬时状态");
        s.save(p);
        System.out.println("此时p是持久状态");
        s.getTransaction().commit();
        s.close();
        System.out.println("此时p是脱管状态");
        sf.close();
    }
  
}
```
这里就是p的三种状态 
hibernate的一级缓存的机制后面会有

<A NAME="zsg">增删改查</A> 

插入就是上面s.save(p) 这个过程了  首先我们获得sf也就是事务工厂 然后从事务工厂中获取一个事务 
开启事务 新建插入对象 然后保存对象  在提交事务
```
public class TestHibernate {
    public static void main(String[] args) {
 
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
 
        Session s = sf.openSession();
        s.beginTransaction();
 
        for (int i = 0; i < 10; i++) {
            Product p = new Product();
            p.setName("iphone"+i);
            p.setPrice(i);
            s.save(p);         
        }
 
        s.getTransaction().commit();
        s.close();
        sf.close();
    }
 
}
```



关于删改的问题 

```
public class TestHibernate {
    public static void main(String[] args) {
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
  
        Session s = sf.openSession();
        s.beginTransaction();

//创建一个p对象来用事务s的get方法来找到它
  
        Product p =(Product) s.get(Product.class, 6);
          
        System.out.println("id=6的产品是: "+p.getName());
//修改
		p.setName("ipone");
		s.update(p);
 // 删除
		s.delect(p);


        s.getTransaction().commit();
        s.close();
        sf.close();
    }
  
}

```

复杂的查询问题  
三种方法  hql criteria 标准的sql查询

#### hql

HQL（Hibernate Query Language）是hibernate专门用于查询数据的语句，有别于SQL，HQL 更接近于面向对象的思维方式。

比如使用的是类的名字Product,而非表格的名字product_ 

 1. 首先根据hql创建一个Query对象
2. 设置参数(和基1的PreparedStatement不一样，Query是基0的)
3. 通过Query对象的list()方法即返回查询的结果了。

注： 使用hql的时候，用的是类名Product,而不是表名product_
注： 使用hql的时候，不需要在前面加 select *


#### criteria

使用Criteria进行数据查询。
与HQL和SQL的区别是Criteria 完全是 面向对象的方式在进行数据查询，将不再看到有sql语句的痕迹 
使用Criteria 查询数据
1. 通过session的createCriteria创建一个Criteria 对象
2. Criteria.add 增加约束。 在本例中增加一个对name的模糊查询(like)
3. 调用list()方法返回查询结果的集合

#### 标准的sql
 通过标准SQL语句进行查询
Hibernate依然保留了对标准SQL语句的支持，在一些场合，比如多表联合查询，并且有分组统计函数的情况下，标准SQL语句依然是效率较高的一种选择 
 使用Session的createSQLQuery方法执行标准SQL语句

因为标准SQL语句有可能返回各种各样的结果，比如多表查询，分组统计结果等等。 不能保证其查询结果能够装进一个Product对象中，所以返回的集合里的每一个元素是一个对象数组。 然后再通过下标把这个对象数组中的数据取出来。 

```
public class TestHibernate {
    public static void main(String[] args) {
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
 
        Session s = sf.openSession();
        s.beginTransaction();
 
        String name = "iphone";

		//这个是hql的关键代码
        Query q =s.createQuery("from Product p where p.name like ?");
        q.setString(0, "%"+name+"%");
        List<Product> ps= q.list();
        for (Product p : ps) {
            System.out.println(p.getName());
        }
		//criteria
		Criteria c= s.createCriteria(Product.class);
        c.add(Restrictions.like("name", "%"+name+"%"));
        List<Product> ps = c.list();
        for (Product p : ps) {
            System.out.println(p.getName());
        }
        //标准的sql
		 String sql = "select * from product_ p where p.name like '%"+name+"%'";

        Query q= s.createSQLQuery(sql);
        List<Object[]> list= q.list();
        for (Object[] os : list) {
            for (Object filed: os) {
                System.out.print(filed+"\t");
            }
            System.out.println();
        }
        s.getTransaction().commit();
        s.close();
        sf.close();
    }
 
}
```

<A NAME="gxh">hibernate中类别关系</A> 

### 一对多 多对一 多对多
[教程链接](https://how2j.cn/k/hibernate/hibernate-many-to-one/41.html#nowhere) 



<A NAME="gnh">hibernate概念也就是进阶其实</A> 

让我们来看看有那些要掌握的
+ 事务
+ 属性延迟加载
+ 关系延迟加载
+ 级联
+ 一级缓存
+ 二级缓存
+ 分页
+ Session两种方式
+ N+1
+ 查询总数
+ 锁
+ 连接池  


#### 事务
hibernate中的事务由s.beginTransaction();开始由s.getTransaction().commit();结束


####  属性延迟加载就是
当使用load的方式来获取对象的时候，只有访问了这个对象的属性，hibernate才会到数据库中进行查询。否则不会访问数据库 
```
public class TestHibernate {
    public static void main(String[] args) {
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
  
        Session s = sf.openSession();
        s.beginTransaction();
     
        Product p = (Product)s.load(Product.class, 1);
        System.out.println("log1");
        System.out.println(p.getName());
        System.out.println("log2");
 
        s.getTransaction().commit();
        s.close();
        sf.close();
    }
}
```
运行结果
![运行结果](kengkeng) 

#### 关系延迟加载

延迟加载又叫lazyload，在one-many many-many的时候都可以使用关系的延迟加载

```
public class TestHibernate {
    public static void main(String[] args) {
        SessionFactory sf = new Configuration().configure().buildSessionFactory();
 
        Session s = sf.openSession();
        s.beginTransaction();
        Category c = (Category) s.get(Category.class, 1);
 
        System.out.println("log1");
        System.out.println(c.getProducts());
        System.out.println("log1");
 
        s.getTransaction().commit();
        s.close();
        sf.close();
    }
}
```

<++>
