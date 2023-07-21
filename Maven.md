# Maven的功能

- 提供了一套标准的项目结构

  ```
  src
  	main
  		java
  		resources
  		webapp
  	test
  		java
  		resources
  pom.xml
  ```
- 提供了一套标准的构建流程（编译、测试、打包、发布）
- 提供了一套依赖管理机制

# 生命周期

同一生命周期内，执行后边的命令，前边的所有命令会自动执行

```
compile -> test -> package -> install
```

# 依赖范围

默认是compile

| 依赖范围 | 编译classpath            | 测试classpath            | 运行classpath(最后的jar或war包有无依赖包) | 例子                     |
| -------- | ------------------------ | ------------------------ | ----------------------------------------- | ------------------------ |
| compile  | Y                        | Y                        | Y                                         | logback                  |
| test     | -                        | Y                        | -                                         | junit                    |
| provided | Y                        | Y                        | -                                         | servlet-api              |
| runtime  | -                        | Y                        | Y                                         | jdbc驱动                 |
| system   | Y                        | Y                        | -                                         | 存在在本地的jar包        |
| import   | 引入DependencyManagement | 引入DependencyManagement | 引入DependencyManagement                  | 引入DependencyManagement |

