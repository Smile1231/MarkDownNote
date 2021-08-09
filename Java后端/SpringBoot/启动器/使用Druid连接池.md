# SpringBoot中的连接池

## Spring Boot之默认连接池配置策略

注意：如果我们使用``spring-boot-starter-jdbc`` 或 ``spring-boot-starter-data-jpa`` ``“starters”``坐标,``Spring Boot``将自动配置``HikariCP``连接池，
　

**因为``HikariCP``在性能和并发性相比其他连接池都要好。**

1. 如果可以得到``HikariCP``连接池类，就优先配置``HikariCP``
2. 否则，如果``Tomcat pooling``类可以得到，则使用它
3. 如果上面两个都拿不到，则配置其他连接池，比如``Commons DBCP2``

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/heimdallr?useUnicode=true&characterEncoding=utf8&allowMultiQueries=true
    username: root
    password: 123
    driver-class-name: com.mysql.cj.jdbc.Driver
#   Spring Boot官方推荐的数据库连接池是Hikari，从一些第三方的评测结果看，Hikari的性能比Druid要好，但是Druid自带各种监控工具，背后又有阿里一直在为它背书
#   hikari数据源配置，
    hikari:
      connection-test-query: SELECT 1 FROM DUAL
      connection-timeout: 30000
      maximum-pool-size: 20
      max-lifetime: 1800000
      minimum-idle: 5
```

## 使用Druid连接池(有两种方式)

### 使用``starter``方式

> 在``pom.xml``中导入``Spring Boot Druid Starter``
```xml
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>druid-spring-boot-starter</artifactId>
<version>1.2.4</version>
</dependency>
```
> 在配置文件中进行配置
> 
只要配置了数据库连接的用户名、密码和URL，在不引入``Druid``数据库连接池配置``spring.datasource.druid``时，就已经可以使用``Druid``数据库连接池。

当然我们总是需要显式地配置Druid数据库连接池。
``application.properties``实例：
```properties
# JDBC配置
spring.datasource.url=jdbc:mysql://localhost:3306/miniProgramme?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
spring.datasource.username=springuser
spring.datasource.password=test123
spring.datasource.driver-class-name: com.mysql.cj.jdbc.Driver

# 连接池配置
# 初始化连接数
spring.datasource.druid.initial-size=1
# 最小空闲连接数，一般设置和initial-size一致
spring.datasource.druid.min-idle=1
# 最大活动连接数
spring.datasource.druid.max-active=20
# 从连接池获取连接超时
spring.datasource.druid.max-wait=60000
# 打开PSCache，并且指定每个连接上PSCache的大小
# Oracle等支持游标的数据库，打开此开关，会以数量级提升性能，具体查阅PSCache相关资料
spring.datasource.druid.pool-prepared-statements=true
spring.datasource.druid.max-open-prepared-statements=20
# 检验连接是否有效的查询语句
# 如果数据库Driver支持ping()方法，则优先使用ping()方法进行检查，否则使用validationQuery查询进行检查
spring.datasource.druid.validation-query=select 1 from dual
# 设置从连接池获取连接时是否检查连接有效性，true时，每次都检查;false时，不检查
spring.datasource.druid.test-on-borrow=false
# 设置往连接池归还连接时是否检查连接有效性，true时，每次都检查;false时，不检查
spring.datasource.druid.test-on-return=false
# 设置从连接池获取连接时是否检查连接有效性
# 为true时，如果连接空闲时间超过minEvictableIdleTimeMillis进行检查，否则不检查
# 为false时，不检查
spring.datasource.druid.test-while-idle=true
# 配置间隔多久启动一次DestroyThread，对连接池内的连接才进行一次检测，单位是毫秒。
# 1.如果连接空闲并且超过minIdle以外的连接，如果空闲时间超过minEvictableIdleTimeMillis设置的值则直接物理关闭。
# 2.在minIdle以内的不处理。
spring.datasource.druid.time-between-eviction-runs-millis=60000
# 配置一个连接在池中最大空闲时间，单位是毫秒
spring.datasource.druid.min-evictable-idle-time-millis=300000
# 打开后，增强timeBetweenEvictionRunsMillis的周期性连接检查，minIdle内的空闲连接
# 每次检查强制验证连接有效性
spring.datasource.druid.keep-alive=true
```
更为简洁的``applicaiton.yml``示例：
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/miniProgramme?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
    username: springuser
    password: test123
    driver-class-name: com.mysql.cj.jdbc.Driver
    druid:
      initial-size: 1
      min-idle: 1
      max-active: 20
      max-wait: 60000
      pool-prepared-statements: true
      max-open-prepared-statements: 20
      validation-query: select 1 from dual
      test-on-borrow: false
      test-on-return: false
      test-while-idle: true
      time-between-eviction-runs-millis: 60000
      min-evictable-idle-time-millis: 300000
      keep-alive: true
```

### 不使用``starter``:

引入``Druid``依赖：
```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.4</version>
</dependency>
```

``yaml``配置：
```yaml
spring:
    datasource:
        type: com.alibaba.druid.pool.DruidDataSource
        driver-class-name: com.mysql.cj.jdbc.Driver
        platform: mysql
        url: jdbc:mysql://localhost:3306/miniProgramme?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=GMT%2B8
        username: root
        password: root
        initialSize: 5
        minIdle: 5
        maxActive: 20
        maxWait: 60000
        timeBetweenEvictionRunsMillis: 60000
        minEvictableIdleTimeMillis: 300000
        validationQuery: SELECT1FROMDUAL
        testWhileIdle: true
        testOnBorrow: false
        testOnReturn: false
        filters: stat,wall,log4j
        logSlowSql: true
```
如果这种方式不生效，需要再配置一个配置类：
```java
@Configuration
public class DruidConfiguration {
    private static final Logger logger = LoggerFactory.getLogger(DruidConfiguration.class);

    private static final String DB_PREFIX = "spring.datasource";

    @Bean
    public ServletRegistrationBean druidServlet() {
        logger.info("init Druid Servlet Configuration ");
        ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        // IP白名单
        servletRegistrationBean.addInitParameter("allow", "*");
        // IP黑名单(共同存在时，deny优先于allow)
        servletRegistrationBean.addInitParameter("deny", "192.168.1.88");
        //控制台管理用户
        servletRegistrationBean.addInitParameter("loginUsername", "mydruid");
        servletRegistrationBean.addInitParameter("loginPassword", "123456");
        //是否能够重置数据 禁用HTML页面上的“Reset All”功能
        servletRegistrationBean.addInitParameter("resetEnable", "false");
        return servletRegistrationBean;
    }

    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(new WebStatFilter());
        filterRegistrationBean.addUrlPatterns("/*");
        filterRegistrationBean.addInitParameter("exclusions", "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        return filterRegistrationBean;
    }

    //解决 spring.datasource.filters=stat,wall,log4j 无法正常注册进去
    @Component
    @ConfigurationProperties(prefix = DB_PREFIX)
    class IDataSourceProperties {
        private String url;
        private String username;
        private String password;
        private String driverClassName;
        private int initialSize;
        private int minIdle;
        private int maxActive;
        private int maxWait;
        private int timeBetweenEvictionRunsMillis;
        private int minEvictableIdleTimeMillis;
        private String validationQuery;
        private boolean testWhileIdle;
        private boolean testOnBorrow;
        private boolean testOnReturn;
        private boolean poolPreparedStatements;
        private int maxPoolPreparedStatementPerConnectionSize;
        private String filters;
        private String connectionProperties;

        @Bean     //声明其为Bean实例
        @Primary  //在同样的DataSource中，首先使用被标注的DataSource
        public DataSource dataSource() {
            DruidDataSource datasource = new DruidDataSource();
            datasource.setUrl(url);
            datasource.setUsername(username);
            datasource.setPassword(password);
            datasource.setDriverClassName(driverClassName);

            //configuration
            datasource.setInitialSize(initialSize);
            datasource.setMinIdle(minIdle);
            datasource.setMaxActive(maxActive);
            datasource.setMaxWait(maxWait);
            datasource.setTimeBetweenEvictionRunsMillis(timeBetweenEvictionRunsMillis);
            datasource.setMinEvictableIdleTimeMillis(minEvictableIdleTimeMillis);
            datasource.setValidationQuery(validationQuery);
            datasource.setTestWhileIdle(testWhileIdle);
            datasource.setTestOnBorrow(testOnBorrow);
            datasource.setTestOnReturn(testOnReturn);
            datasource.setPoolPreparedStatements(poolPreparedStatements);
            datasource.setMaxPoolPreparedStatementPerConnectionSize(maxPoolPreparedStatementPerConnectionSize);
            try {
                datasource.setFilters(filters);
            } catch (SQLException e) {
                System.err.println("druid configuration initialization filter: " + e);
            }
            datasource.setConnectionProperties(connectionProperties);
            return datasource;
        }

        public String getUrl() {
            return url;
        }

        public void setUrl(String url) {
            this.url = url;
        }

        public String getUsername() {
            return username;
        }

        public void setUsername(String username) {
            this.username = username;
        }

        public String getPassword() {
            return password;
        }

        public void setPassword(String password) {
            this.password = password;
        }

        public String getDriverClassName() {
            return driverClassName;
        }

        public void setDriverClassName(String driverClassName) {
            this.driverClassName = driverClassName;
        }

        public int getInitialSize() {
            return initialSize;
        }

        public void setInitialSize(int initialSize) {
            this.initialSize = initialSize;
        }

        public int getMinIdle() {
            return minIdle;
        }

        public void setMinIdle(int minIdle) {
            this.minIdle = minIdle;
        }

        public int getMaxActive() {
            return maxActive;
        }

        public void setMaxActive(int maxActive) {
            this.maxActive = maxActive;
        }

        public int getMaxWait() {
            return maxWait;
        }

        public void setMaxWait(int maxWait) {
            this.maxWait = maxWait;
        }

        public int getTimeBetweenEvictionRunsMillis() {
            return timeBetweenEvictionRunsMillis;
        }

        public void setTimeBetweenEvictionRunsMillis(int timeBetweenEvictionRunsMillis) {
            this.timeBetweenEvictionRunsMillis = timeBetweenEvictionRunsMillis;
        }

        public int getMinEvictableIdleTimeMillis() {
            return minEvictableIdleTimeMillis;
        }

        public void setMinEvictableIdleTimeMillis(int minEvictableIdleTimeMillis) {
            this.minEvictableIdleTimeMillis = minEvictableIdleTimeMillis;
        }

        public String getValidationQuery() {
            return validationQuery;
        }

        public void setValidationQuery(String validationQuery) {
            this.validationQuery = validationQuery;
        }

        public boolean isTestWhileIdle() {
            return testWhileIdle;
        }

        public void setTestWhileIdle(boolean testWhileIdle) {
            this.testWhileIdle = testWhileIdle;
        }

        public boolean isTestOnBorrow() {
            return testOnBorrow;
        }

        public void setTestOnBorrow(boolean testOnBorrow) {
            this.testOnBorrow = testOnBorrow;
        }

        public boolean isTestOnReturn() {
            return testOnReturn;
        }

        public void setTestOnReturn(boolean testOnReturn) {
            this.testOnReturn = testOnReturn;
        }

        public boolean isPoolPreparedStatements() {
            return poolPreparedStatements;
        }

        public void setPoolPreparedStatements(boolean poolPreparedStatements) {
            this.poolPreparedStatements = poolPreparedStatements;
        }

        public int getMaxPoolPreparedStatementPerConnectionSize() {
            return maxPoolPreparedStatementPerConnectionSize;
        }

        public void setMaxPoolPreparedStatementPerConnectionSize(int maxPoolPreparedStatementPerConnectionSize) {
            this.maxPoolPreparedStatementPerConnectionSize = maxPoolPreparedStatementPerConnectionSize;
        }

        public String getFilters() {
            return filters;
        }

        public void setFilters(String filters) {
            this.filters = filters;
        }

        public String getConnectionProperties() {
            return connectionProperties;
        }

        public void setConnectionProperties(String connectionProperties) {
            this.connectionProperties = connectionProperties;
        }
    }

}
```

✨
**推荐使用``starter方式``**

## 加密数据库密码

为安全起见，不应该将数据库密码明文暴露在配置文件中，Druid支持对数据库密码进行加密。

在``druid jar``包所在目录，比如在本地Maven repository下的``com/alibaba/druid/1.2.4``运行：
```java
java -cp druid-1.2.4.jar com.alibaba.druid.filter.config.ConfigTools 你的密码 > druid.txt
```
主要是使用这个策略加密了你的密码

![1612360769748.png](img/1612360769748.png)

``druid.txt``中包括：

- ``privateKey``：私钥，注意千万不要泄露私钥！
- ``publicKey``：公钥
- ``password``：加密后的密码
- 
修改配置文件：

- 将``spring.datasource.password``或``spring.datasource.druid.password``的值设置为``${password}``
- 设置``spring.datasource.druid.filter.config.enabled=true``
- 配置``spring.datasource.druid.connection-properties``为``config``.``decrypt=true;config.decrypt.key=${publicKey}``。

加密之后的``yaml``为：
```yaml
spring:
  datasource:
    url: jdbc:mysql://${MYSQL_HOST:localhost}:3306/db_example?serverTimezone=Asia/Shanghai
    username: springuser
    password: dEHKETfxS5DsvJ5uUwudIqZ/YzWHMsZu6xuf83abbkQT3T4FfTFBS/t5/zgWMkEWTri40QPalwsN+RUJOqLgFg==
    driver-class-name: com.mysql.cj.jdbc.Driver
    druid:
      filter:
        config:
          enabled: true
      connection-properties: config.decrypt=true;config.decrypt.key=MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAIuy1eqmTz3JApgJ8jNa99fKBM4/hGslaU5mCkCQyOxg89nu58paObqrHp495AGurD9IOcJ2/lV4Gjm4phkwFG0CAwEAAQ==
      initial-size: 1
      min-idle: 1
      max-active: 20
      max-wait: 60000
      pool-prepared-statements: true
      max-open-prepared-statements: 20
      validation-query: select 1 from dual
      test-on-borrow: false
      test-on-return: false
      test-while-idle: true
      time-between-eviction-runs-millis: 60000
      min-evictable-idle-time-millis: 300000
      keep-alive: true
```
就okk啦🌟

## Druid使用``log4j2``进行日志输出

1. pom.xml中springboot版本依赖
   ```xml
   <!--Spring-boot中去掉logback的依赖-->
   <dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-web</artifactId>
   <exclusions>
   <exclusion>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-logging</artifactId>
   </exclusion>
   </exclusions>
   </dependency>

   <!--日志-->
   <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
   </dependency>

   <!--数据库连接池-->
   <dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>druid-spring-boot-starter</artifactId>
   <version>1.2.4</version>
   </dependency>

   <!--其他依赖-->
   ```
2. ``log4j2.xml``文件中的日志配置(完整，可直接拷贝使用)
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
    <configuration status="OFF">
    <appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <!--只接受程序中DEBUG级别的日志进行处理-->
            <ThresholdFilter level="DEBUG" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss.SSS}] %-5level %class{36} %L %M - %msg%xEx%n"/>
        </Console>

        <!--处理DEBUG级别的日志，并把该日志放到logs/debug.log文件中-->
        <!--打印出DEBUG级别日志，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档-->
        <RollingFile name="RollingFileDebug" fileName="./logs/debug.log"
                     filePattern="logs/$${date:yyyy-MM}/debug-%d{yyyy-MM-dd}-%i.log.gz">
            <Filters>
                <ThresholdFilter level="DEBUG"/>
                <ThresholdFilter level="INFO" onMatch="DENY" onMismatch="NEUTRAL"/>
            </Filters>
            <PatternLayout
                    pattern="[%d{yyyy-MM-dd HH:mm:ss}] %-5level %class{36} %L %M - %msg%xEx%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="500 MB"/>
                <TimeBasedTriggeringPolicy/>
            </Policies>
        </RollingFile>

        <!--处理INFO级别的日志，并把该日志放到logs/info.log文件中-->
        <RollingFile name="RollingFileInfo" fileName="./logs/info.log"
                     filePattern="logs/$${date:yyyy-MM}/info-%d{yyyy-MM-dd}-%i.log.gz">
            <Filters>
                <!--只接受INFO级别的日志，其余的全部拒绝处理-->
                <ThresholdFilter level="INFO"/>
                <ThresholdFilter level="WARN" onMatch="DENY" onMismatch="NEUTRAL"/>
            </Filters>
            <PatternLayout
                    pattern="[%d{yyyy-MM-dd HH:mm:ss}] %-5level %class{36} %L %M - %msg%xEx%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="500 MB"/>
                <TimeBasedTriggeringPolicy/>
            </Policies>
        </RollingFile>

        <!--处理WARN级别的日志，并把该日志放到logs/warn.log文件中-->
        <RollingFile name="RollingFileWarn" fileName="./logs/warn.log"
                     filePattern="logs/$${date:yyyy-MM}/warn-%d{yyyy-MM-dd}-%i.log.gz">
            <Filters>
                <ThresholdFilter level="WARN"/>
                <ThresholdFilter level="ERROR" onMatch="DENY" onMismatch="NEUTRAL"/>
            </Filters>
            <PatternLayout
                    pattern="[%d{yyyy-MM-dd HH:mm:ss}] %-5level %class{36} %L %M - %msg%xEx%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="500 MB"/>
                <TimeBasedTriggeringPolicy/>
            </Policies>
        </RollingFile>

        <!--处理error级别的日志，并把该日志放到logs/error.log文件中-->
        <RollingFile name="RollingFileError" fileName="./logs/error.log"
                     filePattern="logs/$${date:yyyy-MM}/error-%d{yyyy-MM-dd}-%i.log.gz">
            <ThresholdFilter level="ERROR"/>
            <PatternLayout
                    pattern="[%d{yyyy-MM-dd HH:mm:ss}] %-5level %class{36} %L %M - %msg%xEx%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="500 MB"/>
                <TimeBasedTriggeringPolicy/>
            </Policies>
        </RollingFile>

        <!--druid的日志记录追加器-->
        <RollingFile name="druidSqlRollingFile" fileName="./logs/druid-sql.log"
                     filePattern="logs/$${date:yyyy-MM}/api-%d{yyyy-MM-dd}-%i.log.gz">
            <PatternLayout pattern="[%d{yyyy-MM-dd HH:mm:ss}] %-5level %L %M - %msg%xEx%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="500 MB"/>
                <TimeBasedTriggeringPolicy/>
            </Policies>
        </RollingFile>
    </appenders>

    <loggers>
        <root level="DEBUG">
            <appender-ref ref="Console"/>
            <appender-ref ref="RollingFileInfo"/>
            <appender-ref ref="RollingFileWarn"/>
            <appender-ref ref="RollingFileError"/>
            <appender-ref ref="RollingFileDebug"/>
        </root>

        <!--记录druid-sql的记录-->
        <logger name="druid.sql.Statement" level="debug" additivity="false">
            <appender-ref ref="druidSqlRollingFile"/>
        </logger>
        <logger name="druid.sql.Statement" level="debug" additivity="false">
            <appender-ref ref="druidSqlRollingFile"/>
        </logger>

        <!--log4j2 自带过滤日志-->
        <Logger name="org.apache.catalina.startup.DigesterFactory" level="error" />
        <Logger name="org.apache.catalina.util.LifecycleBase" level="error" />
        <Logger name="org.apache.coyote.http11.Http11NioProtocol" level="warn" />
        <logger name="org.apache.sshd.common.util.SecurityUtils" level="warn"/>
        <Logger name="org.apache.tomcat.util.net.NioSelectorPool" level="warn" />
        <Logger name="org.crsh.plugin" level="warn" />
        <logger name="org.crsh.ssh" level="warn"/>
        <Logger name="org.eclipse.jetty.util.component.AbstractLifeCycle" level="error" />
        <Logger name="org.hibernate.validator.internal.util.Version" level="warn" />
        <logger name="org.springframework.boot.actuate.autoconfigure.CrshAutoConfiguration" level="warn"/>
        <logger name="org.springframework.boot.actuate.endpoint.jmx" level="warn"/>
        <logger name="org.thymeleaf" level="warn"/>
    </loggers>
    </configuration>
    ```
3. 配置``application.properties``
   ```properties
   # 配置日志输出
    spring.datasource.druid.filter.slf4j.enabled=true
    spring.datasource.druid.filter.slf4j.statement-create-after-log-enabled=false
    spring.datasource.druid.filter.slf4j.statement-close-after-log-enabled=false
    spring.datasource.druid.filter.slf4j.result-set-open-after-log-enabled=false
    spring.datasource.druid.filter.slf4j.result-set-close-after-log-enabled=false
   ```
4. 输出日志于``druid-sql.log``
   
    就okk啦

## 监控

为安全起见，``Druid``默认不开启数据库连接池监控。

在测试环境上开启``Druid``数据库连接池监控：
```properties
# 配置下面参数用于启动监控页面，考虑安全问题，默认是关闭的，按需开启
spring.datasource.druid.stat-view-servlet.enabled=true
spring.datasource.druid.filter.stat.enabled=true
spring.datasource.druid.web-stat-filter.enabled=true
```
访问``http://localhost:8080/druid``来访问Druid监控Web控制台。

**即使``Druid``支持设置授权用户才能访问和``IP``白名单，但为了安全起见，不推荐在生产环境开启连接池监控。**









