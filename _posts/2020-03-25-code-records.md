---
layout: post
title: 代码记录
tags: 代码 Utils
categories: Utils
---

* TOC
{:toc}

### 重命名文件

```python
'''
修改某路径下（特定类型）文件的命名规则
'''
import os

def rename_file(file):
    new_name = file.replace("要替换内容", "替换目标内容")
    os.rename(file, new_name)

def list_dirs(path):
    dirs = os.listdir(path)
    for dir in dirs:
        dir_path = os.path.join(path, dir)
        if os.path.isdir(dir_path):
            list_dirs(dir_path)
        else:
            if dir_path.endswith(".mp4"):
                rename_file(dir_path)

def main():
    base_path = "E:\\videos\\Python"
    list_dirs(base_path)

if __name__ == '__main__':
    main()
```

### JDBC模板

```java
import java.sql.*;

public class TestJDBC {

    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try {
            // 加载数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 通过驱动管理类获取数据库连接
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/test?characterEncoding=utf-8", "root", "123456");
            // 定义sql语句
            String sql = "SELECT * FROM user WHERE username = ?";
            // 获取预处理statement
            preparedStatement = connection.prepareStatement(sql);
            // 设置参数，第一个参数为sql语句中参数位置（从1开始），第二个为参数值
            preparedStatement.setString(1, "admin");
            // 向数据库发送sql，查询结果集
            resultSet = preparedStatement.executeQuery();
            // 遍历结果集
            while (resultSet.next()) {
                System.out.println(resultSet.getString("id") + resultSet.getString("username"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            // 释放资源
            if (null != resultSet) {
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != preparedStatement) {
                try {
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (null != connection) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

