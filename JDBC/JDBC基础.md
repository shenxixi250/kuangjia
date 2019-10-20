```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;
public class Test_JDBC {
    public static void main(String[] args) {
    //初始化驱动
        Connection c = null;
        Statement s = null;
        try {
        //驱动类com.mysql.jdbc.Driver
        //就在 mysql-connector-java-5.0.8-bin.jar中
        //如果忘记了第一个步骤的导包，就会抛出ClassNotFoundException
        Class.forName("com.mysql.jdbc.Driver");
            // 建立与数据库的Connection连接
            // 这里需要提供：
            // 数据库所处于的ip:127.0.0.1 (本机)
            // 数据库的端口号： 3306 （mysql专用端口号）
            // 数据库名称 how2java
            // 编码方式 UTF-8
            // 账号 root
            // 密码 admin
        //Connection
            c = DriverManager.getConnection(
                            "jdbc:mysql://127.0.0.1:3306/studentdata?characterEncoding=UTF-8",
                            "root", "shen");
        // 注意：使用的是 java.sql.Statement// 不要不小心使用到： com.mysql.jdbc.Statement;
            //Statement
            s = c.createStatement();
            String sql = "delete from student where student_id = 5";//这段是删除
            // String sql = "update hero set name = 'name 5' where id = 3";修改就是换了一条sql语句没有什么是不一样的
            s.execute(sql);
           //String sql ="INSERT INTO `studentdata`.`student`(`student_id`, `student_name`, `student_sex`, `student_age`, `student_dept`) VALUES ('5', 'k', 'k', '23', 'jj')";
           // s.execute(sql);  //这段语句是增加

            System.out.println("执行插入语句成功");
            String sql_show = "select * from student";
            s.execute(sql_show);
            ResultSet rs = s.executeQuery(sql_show);
            while (rs.next()) {
                int id = rs.getInt("student_age");// 可以使用字段名
                String name = rs.getString(2);// 也可以使用字段的顺
                System.out.printf("%d\t%s\t%n", id, name);
            }
            System.out.println("获取 Statement对象： " + s);
            System.out.println("连接成功，获取连接对象： " + c);
        System.out.println("数据库驱动加载成功 ！");

    } catch (ClassNotFoundException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }finally {
            // 数据库的连接时有限资源，相关操作结束后，养成关闭数据库的好习惯
            // 先关闭Statement
            if (s != null)
                try {
                    s.close();
                    System.out.println("s已经关闭");
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            // 后关闭Connection
            if (c != null)
                try {
                    c.close();
                    System.out.println("c已经关闭");
                } catch (SQLException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }

        }

}
}

```


**以上就是jdbc的全部是基础了  这包括了**

1. 首先是驱动的加载     使用Class.forName("com.mysql.jdbc.Driver")  来进行驱动的加载
2.  使用DriverManager类中的getConnection(url,user,password)来进行数据库的链接  此时的c 就相当于一个管道的角色 来接着你的程序和数据库
3. 在c中链接中创建一个 Statement来进行和数据库的通信问题  s.exectute 来执行sql语句
4. 使用ResultSet来接收 s.exectute语句执行后的结果    在使用while(rs.next()){rs.getInt("student_age")}   来对执行的结果进行处理操作

5. 学到如和使用try catch finally 来进行管道s 和查询c的进程的关闭  ok  jdbc的初级阶段结束了

