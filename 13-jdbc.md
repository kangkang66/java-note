
# 声明依赖
注意到这里添加依赖的scope是runtime，因为编译Java程序并不需要MySQL的这个jar包，只有在运行期才需要使用。
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
    <scope>runtime</scope>
</dependency>
```

# JDBC操作

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class LinkDatabaseInsert {
    public static void main(String[] args) throws SQLException, ClassNotFoundException {
       //使用连接池
        HikariConfig config = new HikariConfig();
        config.setDriverClassName("com.mysql.jdbc.Driver");
        config.setJdbcUrl("jdbc:mysql://127.0.0.1:3306/ifshot?useSSL=false&characterEncoding=utf8");
        config.setUsername("root");
        config.setPassword("123456");
        config.addDataSourceProperty("connectionTimeout", "1000"); // 连接超时：1秒
        config.addDataSourceProperty("idleTimeout", "60000"); // 空闲超时：60秒
        config.addDataSourceProperty("maximumPoolSize", "10"); // 最大连接数：10
        DataSource ds = new HikariDataSource(config);

        try (Connection conn = ds.getConnection()) { // 在此获取连接
            try(PreparedStatement statement = conn.prepareCall("select * from ifshot_users where id = ?")){
                statement.setInt(1,1);
                //用完即自动关闭rs
                try(var rs = statement.executeQuery()){
                    while (rs.next()){
                        int id = rs.getInt("id");
                        String appId = rs.getString("app_id");
                        System.out.println(id);
                        System.out.println(appId);
                    }
                }
            }
        }
    }

    public static void jdbctest() throws ClassNotFoundException, SQLException {
        //1.注册数据库的驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取数据库连接（里面内容依次是："jdbc:mysql://主机名:端口号/数据库名","用户名","登录密码"）
        Connection connection = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/ifshot?useSSL=false&characterEncoding=utf8","root","123456");

        //插入
        try(PreparedStatement statement = connection.prepareCall("insert into ifshot_users(app_id,open_id,union_id) values(?,?,?)")){
            statement.setString(1,"apppid223"); //数据库字段类型是int，就是setInt；1代表第一个参数
            statement.setString(2,"小明2323");    //数据库字段类型是String，就是setString；2代表第二个参数
            statement.setString(3,"uuunid333"); //数据库字段类型是int，就是setInt；3代表第三个参数
            //5.执行sql语句（执行了几条记录，就返回几）
            int i = statement.executeUpdate();
            System.out.println(i);
        }


        //查询
        //用完即自动关闭statement
        try(PreparedStatement statement = connection.prepareCall("select * from ifshot_users where id = ?")){
            statement.setInt(1,1);
            //用完即自动关闭rs
            try(var rs = statement.executeQuery()){
                while (rs.next()){
                    int id = rs.getInt("id");
                    String appId = rs.getString("app_id");
                    System.out.println(id);
                    System.out.println(appId);
                }
            }
        }

        connection.close();
    }
}
```