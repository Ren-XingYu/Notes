# 核心概念

IoC(Inversion of Control) 控制反转

- 使用对象时候，由主动new产生对象转换为由**外部**提供对象，此过程中对象创建控制权由程序转移到**外部**。此思想称为控制反转。

Spring技术对Ioc思想进行了实现。

- Spring提供了一个容器，称为IoC容器，用来充当IoC思想中的外部。
- IoC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IoC容器中统称为**Bean**。

DI(Dependency Injection) 依赖注入

- 在容器中建立Bean和Bean之间的依赖关系的整个过程，称为依赖注入。

# 注解

```
@Component
@Controller
@Service
@Repository
@Configuration
@Bean

@Value

@ComponentScan

@Primary
@Qualifier

# JDK提供的注解
@Resource

# 由JSR-250提供
@PostConstruct
@PreDestory

@EnableAspectJAutoProxy		开启Spring对AspectJ代理的支持（类上）
@Aspect		声明一个切面（类上）
@Before 	在方法执行之前执行（方法上）
@After 		在方法执行之后执行（方法上）
@Around 	在方法执行之前与之后执行（方法上）
@PointCut 	声明切点

@EnableAsync
@Async

@EnableScheduling
@Scheduled
```

# 创建Bean的方式

## 构造方法

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {
    @Override
    public void save() {
        System.out.println("order dao save");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderDao orderDao = (OrderDao) context.getBean("orderDao");
        orderDao.save();
    }
}
```

## 静态工厂

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {
    @Override
    public void save() {
        System.out.println("order dao save");
    }
}
```

```java
public class OrderDaoFactory {
    public static OrderDao getOrderDao() {
        return new OrderDaoImpl();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orderDao" class="x.y.z.OrderDaoFactory" factory-method="getOrderDao"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderDao orderDao = (OrderDao) context.getBean("orderDao");
        orderDao.save();
    }
}
```

## 实例工厂

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {
    @Override
    public void save() {
        System.out.println("order dao save");
    }
}
```

```java
public class OrderDaoFactory {
    public OrderDao getOrderDao() {
        return new OrderDaoImpl();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 配合使用,无实际意义 -->
    <bean id="orderDaoFactory" class="x.y.z.OrderDaoFactory"/>
    <!-- factory-method,方法名不固定每次需要配置 -->
    <bean id="orderDao" factory-bean="orderDaoFactory" factory-method="getOrderDao"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderDao orderDao = (OrderDao) context.getBean("orderDao");
        orderDao.save();
    }
}
```

## FactoryBean

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoFactoryBean implements FactoryBean<OrderDao> {
    @Override
    public OrderDao getObject() throws Exception {
        return new OrderDaoImpl();
    }

    @Override
    public Class<?> getObjectType() {
        return OrderDao.class;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orderDao" class="x.y.z.OrderDaoFactoryBean"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderDao orderDao = (OrderDao) context.getBean("orderDao");
        orderDao.save();
    }
}
```

# Bean的生命周期

Bean生命周期控制：在Bean创建后和销毁前做一些事情。

- 初始化容器
  - 1、创建对象(分配内存)
  - 2、执行构造方法
  - 3、执行属性注入(set操作)
  - 4、执行bean初始化方法
- 使用Bean
  - 执行业务操作
- 关闭/销毁容器
  - 执行Bean销毁方法

## 配置方式

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {
    @Override
    public void save() {
        System.out.println("order dao save");
    }

    public void init() {
        System.out.println("order dao init");
    }

    public void destroy() {
        System.out.println("order dao destroy");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl" init-method="init" destroy-method="destroy"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        context.registerShutdownHook(); // 注册钩子函数,关闭JVM的时候先关闭Spring容器
        OrderDao orderDao = (OrderDao) context.getBean("orderDao");
        orderDao.save();
        //context.close(); // 关闭Spring容器
    }
}
```

## 钩子函数

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao, InitializingBean, DisposableBean {
    @Override
    public void save() {
        System.out.println("order dao save");
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("order dao destroy");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("order dao afterPropertiesSet");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        context.registerShutdownHook();
        OrderDao orderDao = (OrderDao) context.getBean("orderDao");
        orderDao.save();
    }
}
```

# Bean的依赖注入

## setter注入

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {
    private String orderName;
    private Integer orderNum;

    private int[] array;

    private List<String> list;

    private Set<String> set;

    private Map<String, String> map;

    private Properties properties;

    @Override
    public void save() {
        System.out.println("order dao save " + orderName + " " + orderNum);

        System.out.println("array:" + Arrays.toString(array));
        System.out.println("list:" + list);
        System.out.println("set:" + set);
        System.out.println("map:" + map);
        System.out.println("properties:" + properties);
    }

    public void setOrderName(String orderName) {
        this.orderName = orderName;
    }

    public void setOrderNum(Integer orderNum) {
        this.orderNum = orderNum;
    }

    public void setArray(int[] array) {
        this.array = array;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    public void setProperties(Properties properties) {
        this.properties = properties;
    }
}
```

```java
public interface OrderService {
    void save();
}
```

```java
public class OrderServiceImpl implements OrderService {

    private OrderDao orderDao;

    @Override
    public void save() {
        System.out.println("order service save");
        orderDao.save();
    }

    public void setOrderDao(OrderDao orderDao) {
        this.orderDao = orderDao;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl">
        <property name="orderName" value="phone"/>
        <property name="orderNum" value="1"/>
        <property name="array">
            <array>
                <value>100</value>
                <value>200</value>
                <value>300</value>
            </array>
        </property>
        <property name="list">
            <list>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </list>
        </property>
        <property name="set">
            <list>
                <value>xxx</value>
                <value>yyy</value>
                <value>zzz</value>
            </list>
        </property>
        <property name="map">
            <map>
                <entry key="country" value="China"/>
                <entry key="province" value="JiangSu"/>
                <entry key="city" value="NanJing"/>
            </map>
        </property>
        <property name="properties">
            <props>
                <prop key="country">China</prop>
                <prop key="province">Jiangsu</prop>
                <prop key="city">NanJing</prop>
            </props>
        </property>
    </bean>
    <bean id="orderService" class="x.y.z.service.impl.OrderServiceImpl">
        <property name="orderDao" ref="orderDao"/>
    </bean>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderService orderService = (OrderService) context.getBean("orderService");
        orderService.save();
    }
}
```

## 构造器注入

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {
    private String orderName;
    private Integer orderNum;

    public OrderDaoImpl(String orderName, Integer orderNum) {
        this.orderName = orderName;
        this.orderNum = orderNum;
    }

    @Override
    public void save() {
        System.out.println("order dao save "+ orderName + " "+ orderNum);
    }
}
```

```java
public interface OrderService {
    void save();
}
```

```java
public class OrderServiceImpl implements OrderService {

    private OrderDao orderDao;

    public OrderServiceImpl(OrderDao orderDao) {
        this.orderDao = orderDao;
    }

    @Override
    public void save() {
        System.out.println("order service save");
        orderDao.save();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 标准方式 -->
    <bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl">
        <constructor-arg name="orderName" value="phone"/>
        <constructor-arg name="orderNum" value="1"/>
    </bean>
    <!-- 解决形参名问题,与形参名不耦合 -->
    <!--<bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl">
        <constructor-arg type="java.lang.String" value="phone"/>
        <constructor-arg type="java.lang.Integer" value="1"/>
    </bean>-->
    <!-- 解决参数类型重复问题 -->
    <!--<bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl">
        <constructor-arg index="0" value="phone"/>
        <constructor-arg index="1" value="1"/>
    </bean>-->
    <bean id="orderService" class="x.y.z.service.impl.OrderServiceImpl">
        <constructor-arg name="orderDao" ref="orderDao"/>
    </bean>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderService orderService = (OrderService) context.getBean("orderService");
        orderService.save();
    }
}
```

# Bean的自动装配

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
public class OrderDaoImpl implements OrderDao {

    @Override
    public void save() {
        System.out.println("order dao save");
    }
}
```

```java
public interface OrderService {
    void save();
}
```

```java
public class OrderServiceImpl implements OrderService {

    private OrderDao orderDao;

    @Override
    public void save() {
        System.out.println("order service save");
        orderDao.save();
    }

    public void setOrderDao(OrderDao orderDao) {
        this.orderDao = orderDao;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 按类型装配 -->
    <!--<bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl"/>
    <bean id="orderService" class="x.y.z.service.impl.OrderServiceImpl" autowire="byType"/>-->

    <!-- 按名称装配 -->
    <bean id="orderDao" class="x.y.z.dao.impl.OrderDaoImpl"/>
    <bean id="orderDao1" class="x.y.z.dao.impl.OrderDaoImpl"/>
    <bean id="orderService" class="x.y.z.service.impl.OrderServiceImpl" autowire="byName"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderService orderService = (OrderService) context.getBean("orderService");
        orderService.save();
    }
}
```

# Spring加载Properties文件

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.21</version>
    </dependency>
</dependencies>
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm_db
jdbc.username=root
jdbc.password=root
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--
     classpath: 加载当前工程的文件
     classpath*: 加载当前工程及所依赖Jar包中的文件
     system-properties-mode="NEVER: 取消加载系统环境变量中的值
     -->
    <context:property-placeholder location="classpath:*.properties" system-properties-mode="NEVER"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //ApplicationContext context1 = new FileSystemXmlApplicationContext("absoluteFilePath");
        //DataSource dataSource = (DataSource) context.getBean("dataSource");
        //DataSource dataSource = context.getBean("dataSource", DataSource.class);
        DataSource dataSource = context.getBean(DataSource.class);
        System.out.println(dataSource);
    }
}
```

# 注解开发

## 半注解开发

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
@Component
public class OrderDaoImpl implements OrderDao {

    @Override
    public void save() {
        System.out.println("order dao save");
    }

}
```

```java
public interface OrderService {
    void save();
}
```

```java
@Component
public class OrderServiceImpl implements OrderService {

    @Autowired
    private OrderDao orderDao;

    @Override
    public void save() {
        System.out.println("order service save");
        orderDao.save();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="x.y.z"/>
</beans>
```

```java
public class App {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        OrderService orderService = context.getBean(OrderService.class);
        orderService.save();
    }
}
```

## 纯注解开发

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>6.1.3</version>
    </dependency>
    <dependency>
        <groupId>javax.annotation</groupId>
        <artifactId>javax.annotation-api</artifactId>
        <version>1.3.2</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.21</version>
    </dependency>
</dependencies>
```

```java
public interface OrderDao {
    void save();
}
```

```java
@Repository
//@Scope("singleton") // singleton|prototype
public class OrderDaoImpl implements OrderDao {


    @Value("${order.orderName}")
    private String orderName;

    @Value(("${order.orderNum}"))
    private String orderNum;

    @Override
    public void save() {
        System.out.println("order dao save " + orderName + " " + orderNum);
    }

    @PostConstruct
    public void init() {
        System.out.println("order dao init");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("order dao destroy");
    }
}
```

```java
public interface OrderService {
    void save();
}
```

```java
@Service
public class OrderServiceImpl implements OrderService {

    // @Autowired按照类型注入,如果同一类型有多个实例,则需要再加一个@Qualifier("orderDao")注解进行名称注入
    @Autowired
    private OrderDao orderDao;

    @Override
    public void save() {
        System.out.println("order service save");
        orderDao.save();
    }
}
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm_db
jdbc.username=root
jdbc.password=root

order.orderName=phone
order.orderNum=1
```

```java
//@Configuration
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    // 三方Bean依赖的引用类型通过形参注入
    @Bean
    public DataSource dataSource(OrderDao orderDao) {
        System.out.println(orderDao);
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

```java
@Configuration // 代表配置文件: applicationContext
@ComponentScan({"x.y.z"}) // 代表context:component-scan
//@ComponentScan({"x.y.z.config"}) // 精准扫描Jdbc的配置类(JdbcConfig(需要用@Configuration注解标识)),不推荐
@Import({JdbcConfig.class}) // 推荐
@PropertySource({"classpath:jdbc.properties"}) // 代表context:property-placeholder,不支持使用通配符
public class SpringConfig {

    // spring的配置和三方配置建议分开
    /*@Bean
    public DataSource dataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/ssm_db");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        return dataSource;
    }*/
}
```

```java
public class App {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        context.registerShutdownHook();
        OrderService orderService = context.getBean(OrderService.class);
        orderService.save();
        DataSource dataSource = context.getBean(DataSource.class);
        System.out.println(dataSource);
    }
}
```

# AOP

- 连接点（JoinPoint）：程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等
  - 在SpringAOP中，理解为方法的执行
- 切入点（PointCut）：匹配连接点的式子
  - 在SpringAOP中，一个切入点可以描述一个具体方法，也可以匹配多个方法
    - 一个具体方法：x.y.z.dao包下的BookDao接口中的无形参无返回值的save方法
    - 匹配多个方法：所有的save方法，所有的get开头的方法，所有以Dao结尾的接口中的任意方法，所有带一个参数的方法
- 通知（Advice）：在切入点执行处执行的操作，也就是共性功能
  - 在SpringAOP中，功能最终以方法的形式呈现
- 通知类：定义通知的类
- 切面（Aspect）：描述通知与切入点的对应关系

## AOP工作流程

1、Spring容器启动

2、读取所有切面配置中的切入点

3、初始化Bean，判定Bean对应的类中的方法是否匹配到任意切入点

- 匹配失败，创建对象
- 匹配成功，创建原始对象（目标对象）的代理对象

4、获取Bean执行方法

- 获取Bean，调用方法并执行，完成操作
- 获取的的Bean是代理对象时，根据代理对象的运行模式运行原始方法与增强的内容，完成操作

## AOP切入点表达式

- 切入点表达式标准格式：动作关键字 (访问修饰符 返回值 包名.类/接口名.方法名(参数) 异常名)

  ```
  execution(public User x.y.z.service.UserService.findById(int))
  ```

  - 动作关键字：描述切入点的行为动作，例如execution表示执行到指定切入点
  - 访问修饰符：public，private等，可以省略
  - 返回值
  - 包名
  - 类/接口名
  - 方法名
  - 参数
  - 异常名：方法定义中抛出指定异常，可以省略

- 可以使用通配符描述切入点，快速描述

  - *：单个独立的任意符号，可以独立出现，也可以作为前缀或者后缀的匹配符出现

    ```
    execution(public * x.y.z.*.UserService.find*(*))
    ```

    匹配x.y.z包下的任意包中的UserService类或接口中所有find开头的带有一个参数的方法

  - ..：多个连续的任意符号，可以独立出现，常用于简化包名与参数的书写

    ```
    execution(public User x..UserService.findById(..))
    ```

    匹配x包下的任意包中的UserService类或接口中所有名称为findById的方法

  - +：专用于匹配子类类型

    ```
    execution(* *..*Service+.*(..))
    ```

## AOP通知类型

- AOP通知描述了抽取的共性功能，根据共性功能抽取的位置不同，最终运行代码时要将其加入到合理位置
- AOP通知共分为5种类型
  - 前置通知
  - 后置通知
  - 环绕通知
  - 返回后通知
  - 抛出异常后通知

# Spring事务角色

- 事务管理员：发起事务方，在Spring中通常指代业务层开启事务的方法
- 事务协调员：加入事务方，在Spring中通常指代数据层方法，也可以是业务层方法
- 事务传播行为：事务协调员对事务管理员所携带事务的处理态度

事务默认只有在出现Error和RuntimeException的时候才会回滚。