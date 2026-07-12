# HTTP协议

超文本传输协议，规定了浏览器和服务器之间数据传输的规则

**特点**：

- 基于TCP协议，面向连接，安全
- 基于请求-响应模型，一次请求对应一次响应
- HTTP协议是无状态的协议，对于事务处理没有记忆功能，每次请求-响应都是独立的
  - 缺点:多次请求间不能共享数据
  - 优点：速度快



## HTTP请求协议



### 请求数据格式

- 请求行：请求数据的第一行(请求方式，资源路径，协议)
- 请求头：第二行开始，格式key:value
- 请求体：POST请求，存放请求参数，与请求头之间隔了一个空行

**请求方式-GET：请求参数在请求行中，没有请求体，如：/brand/findAll?name=creep&status=1 GET请求大小在浏览器中有限制**

**请求方式-POST：请求参数在请求体中，POST请求的大小是没有限制的**

| 请求头字段      | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| Host            | 请求的主机名                                                 |
| User-Agent      | 浏览器版本，例如 Chrome 浏览器的标识类似`Mozilla/5.0 ... Chrome/79`，IE 浏览器的标识类似`Mozilla/5.0 (Windows NT ...) like Gecko` |
| Accept          | 表示浏览器能接收的资源类型，如`text/*`、`image/*`或者`*/*`表示所有； |
| Accept-Language | 表示浏览器偏好的语言，服务器可以据此返回不同语言的网页；     |
| Accept-Encoding | 表示浏览器可以支持的压缩类型，例如`gzip`、`deflate`等。      |
| Content-Type    | 请求主体的数据类型。                                         |
| Content-Length  | 请求主体的大小（单位：字节）。                               |



### 请求数据获取

Web服务器对HTTP协议的请求数据进行解析，并进行了封装(HttpServletRequest)，在调用Controller方法的时候传递给了该方法，这样，就使得程序员不必直接对协议进行操作，让Web开发更加便捷

```java
@RequestMapping("/request")
public String request(HttpServletRequest request){
    //1.获取请求方法
    String method =request.getMethod();//GET
    System.out.println("请求方式"+method);

    //2.获取请求的url地址
    String url=request.getRequestURL().toString();//https://localhost:8080/request
    System.out.println("请求的url地址"+url);
    String uri=request.getRequestURI();
    System.out.println("请求的uri地址为"+uri);

    //3.获取请求协议
    String protocol =request.getProtocol();//HTTP/1.1
    System.out.println("请求协议"+protocol);

    //4.获取请求参数-name
    String name=request.getParameter("name");
    System.out.println("name:"+name);

    //5.获取请求头：Accept
    String accept =request.getHeader("Accept");
    return "ok";
}
```





## HTTP响应协议

### 响应数据格式

- 响应行：响应数据第一行(协议，状态码，描述)
- 响应头：第二行开始，格式key:value
- 响应体：最后一部分，存放响应数据

**状态码**

| 状态码分类 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 1xx        | 响应中 - 临时状态码，表示请求已经接收，告诉客户端应该继续请求或者如果它已经完成则忽略它。 |
| 2xx        | 成功 - 表示请求已经被成功接收，处理已完成。                  |
| 3xx        | 重定向 - 重定向到其他地方；让客户端再发起一次请求以完成整个处理。 |
| 4xx        | 客户端错误 - 处理发生错误，责任在客户端。如：请求了不存在的资源、客户端未被授权、禁止访问等。 |
| 5xx        | 服务器错误 - 处理发生错误，责任在服务端。如：程序抛出异常等。 |

**HTTP 响应头字段**

| 响应头字段       | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| Content-Type     | 表示该响应内容的类型，例如`text/html`、`application/json`。  |
| Content-Length   | 表示该响应内容的长度（字节数）。                             |
| Content-Encoding | 表示该响应压缩算法，例如`gzip`。                             |
| Cache-Control    | 指示客户端应如何缓存，例如`max-age=300`表示可以最多缓存 300 秒。 |
| Set-Cookie       | 告诉浏览器为当前页面所在的域设置 cookie。                    |





### 响应数据设置

Web服务器对HTTP协议的响应数据进行了封装(HttpServletResponse)，并在调用Controller方法的时候传递给了该方法。这样就使得程序员不必直接对协议进行操作，让Web开发更加便捷

```java
@RestController
public class ResponseController {
    @RequestMapping("/response")//方式1 HttpServletResponse
    public void response(HttpServletResponse response) throws IOException {
        //1.设置响应码
        response.setStatus(404);
        //2.设置响应头
        response.setHeader("name","creep");
        //3.设置响应体
        response.getWriter().write("<h1>hello creep</h1>");
    }

    @RequestMapping("/response2")//方式2 ResponseEntity
    public ResponseEntity<String> response2(){
        return ResponseEntity
                .status(401).
                header("name","creep").
                body("<h1>hello</h1>");
    }
}
```





**ps:响应状态码和响应头如果没有特殊需求的话，通常不手动设定，服务器会根据请求处理的逻辑，自动设置响应码和响应头**





# 分层解耦

## 三层架构

- **controller**:控制层，接收前端发送的请求，对请求进行处理，并响应数据
- **service**：业务逻辑层，处理具体的业务逻辑
- **dao**：数据访问层(Data Access Object)(持久层)，负责数据访问操作，包括数据的增删改查

## 分层解耦

**控制反转**(IOC)：对象的创建控制权由程序自身转到外部(容器)，这种思想称为控制反转

**依赖注入**(DI):容器为应用程序提供运行时，所依赖的资源，称之为资源注入

**Bean对象**：IOC容器中创建，管理的对象，称之为Bean



## IOC

**将一个类交给IOC容器管理**

在实现类上加@Component，注意不是接口上

```java
@Component
public class UserDaoImpl  implements UserDao {

    @Override
    public List<String> findAll() {
        InputStream in=this.getClass().getClassLoader().getResourceAsStream("user.txt");
        ArrayList<String> lines= IoUtil.readLines(in, StandardCharsets.UTF_8,new ArrayList<>());
        return lines;
    }
}

```

| 注解          | 说明                   | 位置                                                |
| ------------- | ---------------------- | --------------------------------------------------- |
| `@Component`  | 声明 bean 的基础注解   | 不属于以下三类时，用此注解                          |
| `@Controller` | `@Component`的衍生注解 | 标注在控制层类上                                    |
| `@Service`    | `@Component`的衍生注解 | 标注在业务层类上                                    |
| `@Repository` | `@Component`的衍生注解 | 标注在数据访问层类上（由于与 mybatis 整合，用的少） |

**ps：声明bean的时候可以通过注解的value属性指定bean的名字，如果没有指定，默认为类名首字母小写**



四大注解要想生效，还需要被组件扫描注解@ComponentScan扫描

该注解虽然没有显性配置，但是实际上已经包含在了启动类声明注解@SpringBootApplication中，默认扫描的范围是**启动类所在包及其子包**





## DI

在bean上加@Autowired，完成依赖注入

```java
@Component
public class UserServiceImpl implements UserService {
    @Autowired
    UserDao userDao;
    @Override
    public List<User> findAll() {
        List<String>lines=userDao.findAll();
        List<User> list=lines.stream().map(line->{
            String []parts=line.split(",");
            Integer id=Integer.parseInt(parts[0]);
            String username=parts[1];
            String password=parts[2];
            String name=parts[3];
            Integer age=Integer.parseInt(parts[4]);
            LocalDateTime updateTime=LocalDateTime.parse(parts[5], DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
            return new User(id,username,password,name,age,updateTime);
        }).toList();
        return list;
    }
}
```



### **三种注入方式**

- 属性注入

  ```java
  @RestController
  public class UserController {
      @Autowired
      private UserService userService;
  }
  ```

- 构造函数注入

  ```java
  @RestController
  public class UserController {
      private UserService userService;
      @Autowired
      public UserController(UserService userService){
          this.userService=userService;
      }
  ```

  ps：如果只有一个构造函数，@Autowired注解可以省略

- setter注入

  ```java
  @RestController
  public class UserController {
      private UserService userService;
      @Autowired
      public void setUserController(UserService userService){
          this.userService=userService;
      }
  ```




### 多个相同类型bean

@Autowired注解，默认是按照类型进行注入的

如果存在多个相同类型的bean，将会报错

**解决方法**

- @Primary

  ```java
  @Primary
  @Service
  public class UserServiceImpl2 implements UserService {
      @Override
      public List<User> findAll() {
  ```

  在IOC的时候声明

- @Qualifier

  ```java
  @RestController
  public class UserController {
      @Autowired
      @Qualifier("userServiceImpl")//这里写的是Bean名
      private UserService userService;
  ```

- @Resource

  ```java
  @RestController
  public class UserController {
      @Resource(name="userServiceImpl")//这里写的是Bean名
      private UserService userService;
  ```



## controller

### 指定请求方式

如果不指定所有方式都可以

```java
@RequestMapping(value = "/depts",method = RequestMethod.GET)
//下面这种方式更方便
@GetMapping("/depts")
```

### 接收参数

GET /user?id=1&name=zhangsan

- 通过原始的HttpServletRequest对象接收参数

  ```java
  @DeleteMapping("/depts")
      public Result delete(HttpServletRequest httpServletRequest){
          String strId=httpServletRequest.getParameter("id");
          Integer id=Integer.parseInt(strId);
      }
  ```

- 通过Spring提供的@RequestParam注解，将请求参数绑定给方法形参

  ```java
      @DeleteMapping("/depts")
      public Result delete(@RequestParam("id")Integer deptId){
  
      }
  ```

  默认required为true，如果不传递会报错，可以设置required为false

  ```java
      public Result delete(@RequestParam(value = "id",required = false)Integer deptId){
  
      }
  
  ```

- 如果请求参数名和形参变量名相同，直接定义方法形参即可接收，可以省略@RequestParam,也可以使用类来接收

#### 数组

- 直接使用原始数组接收

  ```java
  @DeleteMapping
  public Result delete(Integer[]ids){
  
  }
  ```

- 使用list接收，此时RequestParam不能省略

  ```java
  @DeleteMapping
  public Result delete(@RequestParam List<Integer> ids){
      
  }
  ```

  

### 接收json文件

通常会使用一个实体对象进行接收

规则：JSON数据的键名与方法形参对象的属性名相同，并需要使用@RequestBody注解标识

```java
public Result save(@RequestBody Emp emp){

}
```



### 接收路径参数

路径参数：通过请求URL直接传递参数，使用{...}来标识该路径参数，需要使用@PathVariable获取路径

比如  /depts/1

```java
@GetMapping("/depts/{id}")
public Result selectById(@PathVariable("id") Integer deptId){

}
如果形参和参数名一致，@PathVariable 括号里的可以省略
@GetMapping("/depts/{id}")
public Result selectById(@PathVariable Integer id){

}
```



### 路径合并

一个完整的请求路径，应该是类上的@RequestMapping的value属性+方法上的@RequestMapping的value属性

```java
@RequestMapping("/depts")
@RestController
public class DeptController {
    @Autowired
    private DeptService deptService;


    @GetMapping
    public Result list(){
        List<Dept> list=deptService.findAll();
        return Result.success(list);
    }

    @DeleteMapping
    public Result delete(Integer id){
        deptService.deleteId(id);
        return Result.success();
    }

    @PostMapping
    public Result insert(@RequestBody Dept dept){
        deptService.insert(dept);
        return Result.success();
    }

    @GetMapping("/{id}")
    public Result selectById(@PathVariable Integer id){
        Dept dept=deptService.selectById(id);
        return Result.success(dept);
    }

    @PutMapping
    public Result update(@RequestBody Dept dept){
        deptService.update(dept);
        return Result.success();
    }
}
```






# Mybatis

```java
@Mapper //应用程序在运行时，会自动的为该接口创建一个实现类对象(代理对象),并且会自动将该实现类对象存入IOC容器-bean
public interface UserMapper {
    @Select("select id,username from user")
    public List<User> findAll();
}
```





在sql语句中可以使用`#{}`或者`${}`来传递参数

```java
@Delete("delete from user where id=#{id}")
public Integer deleteById(Integer id); 
```





| 符号     | 说明                                                   | 场景                       | 优缺点               |
| -------- | ------------------------------------------------------ | -------------------------- | -------------------- |
| `#{...}` | 占位符。执行时，会将`#{...}`替换为`?`，生成预编译 SQL  | 参数值传递                 | 安全、性能高（推荐） |
| `${...}` | 拼接符。直接将参数拼接在 SQL 语句中，存在 SQL 注入问题 | 表名、字段名动态设置时使用 | 不安全、性能低       |



如果传入的参数是类的属性，直接写属性名

```java
@Insert("insert into user(username, password, name, age) " +
        "values(#{username},#{password},#{name},#{age})")
public void insertUser(User user);
```





如果接口方法形参中，需要传递多个参数，需要通过@Param注解为参数起名字，

```java
@Select("select * from user where id=#{id} and password=#{password}")
public  void selectUser(@Param("id") Integer id,@Param("password") String pwd);
//注解里的参数名和上面的要对应
```





## XML映射配置

在Mybatis中，既可以通过注解配置SQLSQL语句，也可以通过XML配置文件配置SQL语句

**默认规则**

- XML映射文件的名称与Mapper接口名称一致，并且将XML映射文件和Mapper接口放置在相同包下
- XML映射文件的namespace属性为Mapper接口全限定名一致
- XML映射文件中sql语句id与Mapper接口中的方法一致，并保持返回类型一致
- <img src="C:\Users\LENOVO\AppData\Roaming\Typora\typora-user-images\image-20260528102748507.png" alt="image-20260528102748507" style="zoom:67%;left-margin=0px;" />

```java
@Mapper //应用程序在运行时，会自动的为该接口创建一个实现类对象(代理对象),并且会自动将该实现类对象存入IOC容器-bean
public interface UserMapper {
    public List<User> findAll();
    
    
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.creep.mapper.UserMapper">
    <select id="findAll" resultType="com.creep.pojo.User">
        select * from user
    </select>
</mapper>
```





## 获取生成的主键

```java
@Options(useGeneratedKeys = true,keyProperty = "id")//获取生成到的主键
    @Insert("insert into emp(username, name, gender, phone, job, salary, image, entry_date, dept_id, create_time, update_time) " +
            "values(#{username},#{name},#{gender},#{phone},#{job},#{salary},#{image},#{entryDate},#{deptId},#{createTime},#{updateTime})")
    void insert(Emp emp);
```



## resultMap

手动映射

如果字段名和属性名基本一致的时候可以直接用resultType直接映射，但如果一对一，一对多的复杂情况就需要用resultMap手动映射

- `<id>`表示主键字段
- `<result>`表示普通字段

- `<association>`一对一
- `<collection>`一对多

```java
    <resultMap id="empResultMap" type="com.creep.pojo.Emp">
        <id column="id" property="id"/>
        <result column="username" property="username"/>
        <result column="password" property="password"/>

        <collection property="exprList" ofType="com.creep.pojo.EmpExpr">
            <id column="ee_id" property="id"/>
            <result column="ee_emp_id" property="empId"/>
        </collection>
    </resultMap>
    <select id="getbyId" resultMap="empResultMap">
    </select>
```





# SpringBoot项目配置文件

SpringBoot项目提供了多种属性配置方式(properties,yaml,yml)

## yml

格式：

- 数值前边必须有空格，作为分隔符
- 使用缩进表示层级关系，缩进时，不允许使用Tab键，只能用空格(idea中会自动将Tab转换为空格)
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- #表示注释，从这个字符一直到行尾，都会被解析器忽略



定义对象/Map集合

```yaml
user :
  name: 张三
  age: 18
  password: 123456
```



定义数组/List/Set集合

```yaml
hobby:
  - java
  - game
  - sport
```



ps:在yml格式的配置文件中，如果配置项的值是以0开头的，值需要使用`''`引起来，因为以0开头在yml中表示8进制的数据





## 封装规则

**默认数据封装规则**

实体类属性名和数据库表的字段名一致，mybatis会自动封装

```java
//实体类
public class Dept {
    private Integer id;
    private String name;
    private LocalDateTime createTime;
    private LocalDateTime updateTime;
}
//数据库表
CREATE TABLE dept (
                      id int unsigned PRIMARY KEY AUTO_INCREMENT COMMENT 'ID, 主键',
                      name varchar(10) NOT NULL UNIQUE COMMENT '部门名称',
                      create_time datetime DEFAULT NULL COMMENT '创建时间',
                      update_time datetime DEFAULT NULL COMMENT '修改时间'
) COMMENT '部门表';
```



如果不同可以通过下面3种方式建立映射

- 手动结果映射：通过@Results及@Result

  ```java
  @Mapper
  public interface DeptMapper {
      @Results({
              @Result(column = "create_time",property = "createTime"),
              @Result(column = "update_time",property = "updateTime"),
      })
      @Select("select id, name, create_time, update_time from dept order by update_time desc;")
      List<Dept> findAll();
  }
  ```

- 起别名：在SQL语句中，对不一样的列名起别名，别名和实体类属性名一样

  ```java
  @Mapper
  public interface DeptMapper {
      @Results({
              @Result(column = "create_time",property = "createTime"),
              @Result(column = "update_time",property = "updateTime"),
      })
      @Select("select id, name, create_time, update_time from dept order by update_time desc;")
      List<Dept> findAll();
  }
  ```

- 开启驼峰命名：如果字段名与属性名符合驼峰命名规则，mybatis会自动通过驼峰命名规则映射

  ```java
  mybatis:
    configuration:
      map-underscore-to-camel-case: true
  ```

  

# 日志技术

准备工作：引入logback的依赖(springboot项目该依赖已传递)，配置文件logback.xml

记录日志：定义日志记录对象Logger，记录日志

```java
public class LogTest {
private static final Logger log= LoggerFactory.getLogger(LogTest.class);
}
//也可以在类上加注解@Slf4j
```

配置文件logback.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<!-- 控制台输出 -->
	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<!--格式化输出：%d 表示日期，%thread 表示线程名，%-5level表示级别从左显示5个字符宽度，%logger显示日志记录器的名称， %msg表示日志消息，%n表示换行符 -->
			<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50}-%msg%n</pattern>
		</encoder>
	</appender>

	<!-- 系统文件输出 -->
	<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
			<!-- 日志文件输出的文件名, %i表示序号 -->
			<FileNamePattern>E:/study/java_study/code/Web-02/tlias-web-management-%d{yyyy-MM-dd}-%i.log</FileNamePattern>
			<!-- 最多保留的历史日志文件数量 -->
			<MaxHistory>30</MaxHistory>
			<!-- 最大文件大小，超过这个大小会触发滚动到新文件，默认为 10MB -->
			<maxFileSize>10MB</maxFileSize>
		</rollingPolicy>

		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<!--格式化输出：%d 表示日期，%thread 表示线程名，%-5level表示级别从左显示5个字符宽度，%msg表示日志消息，%n表示换行符 -->
			<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50}-%msg%n</pattern>
		</encoder>
	</appender>

	<!-- 日志输出级别 -->
	<root level="ALL">
		<appender-ref ref="STDOUT" />
		<appender-ref ref="FILE" />
	</root>
</configuration>

```

## 日志级别

### 日志级别说明

日志级别指的是日志信息的类型，日志都会分级别，常见的日志级别如下（级别由低到高）：

| 日志级别 |                             说明                             | 记录方式           |
| -------- | :----------------------------------------------------------: | ------------------ |
| trace    |             追踪，记录程序运行轨迹 【使用很少】              | `log.trace("...")` |
| debug    | 调试，记录程序调试过程中的信息，实际应用中一般将其视为最低级别 【使用较多】 | `log.debug("...")` |
| info     | 记录一般信息，描述程序运行的关键事件，如：网络连接、io 操作 【使用较多】 | `log.info("...")`  |
| warn     |          警告信息，记录潜在有害的情况 【使用较多】           | `log.warn("...")`  |
| error    |                    错误信息 【使用较多】                     | `log.error("...")` |

可以在配置文件中，灵活的控制输出那些类型的日志

```xml
<root level="info">
    <appender-ref ref="STDOUT" />
    <appender-ref ref="FILE" />
</root>
```

此时只有日志级别为info，warn和error的会输出



# PageHelper分页查询插件

- 引入PageHelper的依赖

```java
        <!--分页插件PageHelper-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.4.7</version>
        </dependency>
```

- 定义Mapper接口的查询方法(无需考虑分页)

- 在Service方法中实现分页查询

  ```java
  PageHelper.startPage(empQueryParam.getPage(),empQueryParam.getPageSize());
  List<Emp>list=empMapper.list(empQueryParam);
  Page<Emp> p=(Page<Emp>) list;
  return new PageResult<Emp>(p.getTotal(),p.getResult());
  ```

  

# 动态SQL

随着用户的输入或外部条件变化而变化的SQL语句

## 动态条件

- `<if>`判断条件是否成立，如果条件为true，则拼接SQL
- `<where>`根据查询条件，来生成where关键字，并会自动去除条件前面多余的and或or

```java
<mapper namespace="com.creep.mapper.EmpMapper">
    <select id="list" resultType="com.creep.pojo.Emp">
        select emp.*,dept.name deptName from emp left outer join dept on dept.id=emp.dept_id
        <where>
            <if test="name!=null">
                emp.name like concat('%',#{name},'%')
            </if>
            <if test="gender !=null">
                and emp.gender=#{gender}
            </if>
            <if test="begin!=null and end !=null">
                and emp.entry_date between #{begin} and #{end}
            </if>
        </where>
        order by emp.update_time desc
    </select>
</mapper>
```

## 动态插入

```mysql
insert into emp(...) values(?,?,?,?,?),(?,?,?,?,?),(?,?,?,?,?) 
```

`<for each>`:

- collection：集合属性
- item：集合遍历出来的元素/项
- separator：每一次遍历使用的分隔符
- open：遍历开始前拼接的片段
- close：遍历结束后拼接的片段

```xml
    <insert id="insertBatch">
        insert into emp_expr(emp_id, begin, end, company, job) values
        <foreach collection="exprList" item="expr" separator=",">
            (#{expr.emp_id},#{expr.begin},#{expr.end},#{expr.company},#{expr.job})
        </foreach>
    </insert>
```





# Spring事务管理

## 控制事务

- 注解:@Transactional
- 作用：将当前方法交给spring进行事务管理，方法执行前，开启事务；成功执行后，提交事务；出现异常，回滚事务
- 位置：业务（Service）层的方法上，类上，接口上



## rollbackFor

rollbackFor属性用于控制出现何种异常类型，回滚事务（默认情况下，只有出现RuntimeException才会回滚）

```java
@Transactional(rollbackFor = {Exception.class})
```



## propagation

事务传播行为：指的就是当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行事务控制

| 属性值        | 含义                                                         |
| ------------- | ------------------------------------------------------------ |
| REQUIRED      | 【默认值】需要事务，有则加入，无则创建新事务                 |
| REQUIRES_NEW  | 需要新事务，无论有无，总是创建新事务                         |
| SUPPORTS      | 支持事务，有则加入，无则在无事务状态中运行                   |
| NOT_SUPPORTED | 不支持事务，在无事务状态下运行，如果当前存在已有事务，则挂起当前事务 |
| MANDATORY     | 必须有事务，否则抛异常                                       |
| NEVER         | 必须没事务，否则抛异常                                       |

```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
```





# 文件上传

前端：

- form method必须为post
- form enctype必须为multipart/form-data
- input type必须为file

```html
 <form action="/upload" method="post" enctype="multipart/form-data">
        姓名: <input type="text" name="name"><br>
        年龄: <input type="text" name="age"><br>
        头像: <input type="file" name="file"><br>
        <input type="submit" value="提交">
    </form>
```

后端：

接收文件的参数为MultipartFile

```java
public Result upload(String name, Integer age, MultipartFile file){
    log.info("接收参数{},{},{}",name,age,file);
    return Result.success();
}
```



## 本地存储

```java
@Slf4j
@RestController
public class UploadController {

    @PostMapping("/upload")
    public Result upload(String name, Integer age, MultipartFile file) throws IOException {
        log.info("接收参数{},{},{}",name,age,file);
        //获取原始文件名
        String originalFilename=file.getOriginalFilename();

        //新的文件名
        String extension=originalFilename.substring(originalFilename.lastIndexOf("."));
        String newFilename= UUID.randomUUID().toString()+extension;
        //保存文件
        file.transferTo(new File("E:\\study\\java_study\\code\\Web-02\\tlias-web-management\\src\\main\\resources\\static\\"+newFilename));
        return Result.success();
    }
}
```

配置文件限制最大上传文件大小

```yml
spring:
  servlet:
    multipart:
      #最大单个文件大小
      max-file-size: 10MB
      #最大请求大小(包括所有文件和表单数据)
      max-request-size: 100MB
```



## 阿里云OSS

```java
@Component
public class AliyunOSSOperator {

    @Value("${aliyun.oss.endpoint}")
    private String endpoint;
    @Value("${aliyun.oss.bucketName}")
    private String bucketName;
    @Value("${aliyun.oss.region}")
    private String region ;

    public String upload(byte[] content, String originalFilename) throws Exception {
        // 从环境变量中获取访问凭证。运行本代码示例之前，请确保已设置环境变量OSS_ACCESS_KEY_ID和OSS_ACCESS_KEY_SECRET。
        EnvironmentVariableCredentialsProvider credentialsProvider = CredentialsProviderFactory.newEnvironmentVariableCredentialsProvider();

        // 填写Object完整路径，例如202406/1.png。Object完整路径中不能包含Bucket名称。
        //获取当前系统日期的字符串,格式为 yyyy/MM
        String dir = LocalDate.now().format(DateTimeFormatter.ofPattern("yyyy/MM"));
        //生成一个新的不重复的文件名
        String newFileName = UUID.randomUUID() + originalFilename.substring(originalFilename.lastIndexOf("."));
        String objectName = dir + "/" + newFileName;

        // 创建OSSClient实例。
        ClientBuilderConfiguration clientBuilderConfiguration = new ClientBuilderConfiguration();
        clientBuilderConfiguration.setSignatureVersion(SignVersion.V4);
        OSS ossClient = OSSClientBuilder.create()
                .endpoint(endpoint)
                .credentialsProvider(credentialsProvider)
                .clientConfiguration(clientBuilderConfiguration)
                .region(region)
                .build();

        try {
            ossClient.putObject(bucketName, objectName, new ByteArrayInputStream(content));
        } finally {
            ossClient.shutdown();
        }

        return endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + objectName;
    }

}
```





### 参数配置化

#### @Value

将一些需要灵活变化的参数，配置在文件中然后通过@Value注解来注入外部配置的属性

application.yml

```yml
aliyun:
  oss:
    endpoint: https://oss-cn-beijing.aliyuncs.com
    bucketName: creep-java-web
    region: cn-beijing
```

AliyunOSSOperator

```java
@Component
public class AliyunOSSOperator {

    @Value("${aliyun.oss.endpoint}")
    private String endpoint;
    @Value("${aliyun.oss.bucketName}")
    private String bucketName;
    @Value("${aliyun.oss.region}")
    private String region ;
}
```

#### @ConfigurationProperties

application.yml

```yml
aliyun:
  oss:
    endpoint: https://oss-cn-beijing.aliyuncs.com
    bucketName: creep-java-web
    region: cn-beijing
```

AliyunOSSProperties

```java
@Data
@Component
@ConfigurationProperties(prefix="aliyun.oss")
public class AliyunOSSProperties {
    private String endpoint;
    private String bucketName;
    private String region;
}
```

AliyunOSSOperator

```java
@Autowired
private AliyunOSSProperties aliyunOSSProperties;
public String upload(byte[] content, String originalFilename) throws Exception {
    String endpoint=aliyunOSSProperties.getEndpoint();
    String bucketName= aliyunOSSProperties.getBucketName();
    String region=aliyunOSSProperties.getRegion();
```





# 全局异常处理器

```java
@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler
    public Result HandleException(Exception exception){
        log.error("程序出错了",exception);
        return Result.error("程序出错了，请联系管理员");
    }
}

```





# 登录认证

## 登录校验

### 会话技术

- 会话：用户打开浏览器，访问web服务器的资源，会话建立，直到有一方断开连接，会话结束，在一次会话中可以包干多次请求和响应
- 会话跟踪：一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同义词绘画的多次请求间共享数据
- 会话跟踪方案：
  - 客户端会话跟踪技术：Cookie
  - 服务端会话跟踪技术：Session
  - 令牌技术



#### Cookie

- 响应头：Set-Cookie    
- 请求头：Cookie
- 优点：HTTP协议中支持的技术
- 缺点：
  - 移动端APP无法使用Cookie
  - 不安全，用户可以自己禁用Cookie
  - Cookie不能跨域



#### Session

- Session底层是基于Cookie的
- 优点：存储在服务端，安全
- 缺点：
  - 服务器集群环境下无法直接使用Session
  - Cookie的缺点

#### 令牌技术

- 优点
  - 支持PC端，移动端
  - 解决集群环境下的认证问题
  - 减轻服务器端存储压力
- 缺点：需要自己实现





## JWT令牌

定义了一种简介的，自包含的格式，用于哎通信双方以json数据格式安全的传输信息

**组成**

- 第一部分：Header(头)，记录令牌类型，签名算法等。例如:{"alg":"HS256","type":"JWT"}
- 第二部分：Payload(有效载荷)，携带一些自定义信息，默认信息等。例如:{"id":"1","username":"Tom"}
- 第三部分：Signature(签名)，防止Token被篡改，保证安全性

​	

```java
    //生成Jwt密钥
    @Test
    public void testGenerateJwt(){
        Map<String,Object> dataMap=new HashMap<>();
        dataMap.put("id",1);
        dataMap.put("username","creep");
        String jwt=Jwts.builder().signWith(SignatureAlgorithm.HS256,"Y3JlZXA=")//指定加密算法，密钥
                .addClaims(dataMap)
                .setExpiration(new Date(System.currentTimeMillis()+3600*1000))//设置过期时间
                .compact();//生成令牌
        System.out.println(jwt);
    }

    //解析Jwt令牌
    @Test
    public void testParseJWT(){
        String token="eyJhbGciOiJIUzI1NiJ9.eyJpZCI6MSwidXNlcm5hbWUiOiJjcmVlcCIsImV4cCI6MTc4MDkxODE5MH0.GjBIdx8TA6XJ2ag9VTD5nQUcLmXNnaJ1bDb7fYZS8wk";
        Claims claims= Jwts.parser().setSigningKey("Y3JlZXA=").parseClaimsJws(token).getBody();
        System.out.println(claims);
    }
```

**ps:JWT校验时使用的签名密钥必须和生成JWT令牌时使用的密钥是配套的**



## 过滤器Filter

过滤器可以把对资源的请求拦下来，从而实现一些特殊的功能

过滤器一般完成一些通用的操作，比如：登录校验，统一编码处理，敏感字符处理等

### 实现

定义一个Filter：定义一个类，实现Filter接口，并实现其所有方法

配置Filter：Filter类上加上@WebFilter注解，配置拦截路径，引导类上加@ServletComponentScan组件支持

```java
@WebFilter(urlPatterns = "/*")//拦截所有请求
@Slf4j
public class DemoFilter implements Filter {

    //初始化方法，web服务器启动的时候执行，只执行一次
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("init 初始化方法");

    }
    //拦截到请求执行，会执行多次
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        log.info("拦截到了请求");
        //放行
        chain.doFilter(request,response);
    }
    //销毁方法，web服务器关闭的时候执行，只执行一次
    @Override
    public void destroy() {
        log.info("destory 销毁方法...");
    }
}

//启动类加
@ServletComponentScan
```

**ps:如果过滤器不执行放行操作，过滤器拦截到请求之后，就不会访问对应的资源**





### 拦截路径

```java
@WebFilter(urlPatterns = "/*")
public class TokenFilter implements Filter {
```

| 拦截路径     | urlPatterns值 | 含义                              |
| ------------ | ------------- | --------------------------------- |
| 拦截具体路径 | /login        | 只有访问/login路径时，才会被拦截  |
| 目录拦截     | /emp/*        | 访问/emps下的所有资源，都会被拦截 |
| 拦截所有     | /*            | 访问所有资源都会被拦截            |



### 过滤器链

一个web应用中，可以配置多个过滤器，这多个过滤器就形成了过滤器链

**顺序：**注解配置的Filter，优先级是按照过滤器类名(字符串)的自然排序





## 拦截器Interceptor

定义拦截器，实现HandlerInterceptor接口，并实现其所有方法

```java
@Component
public class DemoInterceptor implements HandlerInterceptor {
    //在目标资源方法运行之前运行
    //返回值 true放行，false不放行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        log.info("preHandle");
        return  true;
    }
    //在目标资源运行之后运行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
        log.info("postHandle");
    }
    //视图渲染完毕后执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
        log.info("afterCompletion");
    }
}
```

配置：定义一个配置类实现WebMvcConfigurer接口，注册拦截器

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Autowired
    private DemoInterceptor demoInterceptor;
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(demoInterceptor).addPathPatterns("/**");//拦截所有请求
    }
}
```



### 拦截路径

要拦截哪些路径和不拦截哪些路径

```java
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(tokenInterceptor).addPathPatterns("/**")//拦截所有请求
                .excludePathPatterns("/login");//放行登录请求
    }
```

| 拦截路径    | 含义                   | 举例                                                        |
| ----------- | ---------------------- | ----------------------------------------------------------- |
| `/*`        | 一级路径               | 能匹配`/depts`, `/emps`, `/login`，不能匹配 `/depts/1`      |
| `/**`       | 任意级路径             | 能匹配`/depts`, `/depts/1`, `/depts/1/2`                    |
| `/depts/*`  | `/depts`下的一级路径   | 能匹配`/depts/1`，不能匹配`/depts/1/2`, `/depts`            |
| `/depts/**` | `/depts`下的任意级路径 | 能匹配`/depts`, `/depts/1`, `/depts/1/2`，不能匹配`/emps/1` |



### 执行流程

如果同时存在过滤器和拦截器会先执行过滤器



**过滤器和拦截器的区别**

- 接口规范不同：过滤器需要实现Filter接口，而拦截器需要实现HandlerInterceptor接口

- 拦截范围不同：过滤器Filter会拦截所有的资源，而Interceptor只会拦截Spring环境中的资源







# AOP

面向特定方法编程

场景：案例中部分业务方法运行较慢，定位执行耗时较长的接口，此时需要统计每一个业务方法的执行耗时

优势：

- 减少重复代码
- 代码无侵入

## 入门程序

导入依赖

aop程序

```java
@Slf4j
@Aspect//标识当前是一个aop类
@Component
public class RecordTimeAspect {

    @Around("execution(* com.creep.service.impl.*.*(..))")
    public Object recordTime(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        //记录方法运行的开始时间
        long begin=System.currentTimeMillis();
        //执行原始的方法
        Object result =proceedingJoinPoint.proceed();
        //记录方法运行的结束时间，记录耗时
        long end=System.currentTimeMillis();
        log.info("方法{}执行耗时{}ms",proceedingJoinPoint.getSignature(),end-begin);
        return  result;
    }
}
```



## 核心概念

- **连接点**：JoinPoint，可以被AOP控制的方法（暗含方法执行时的相关信息）
- **通知**：Advice，指那些重复的逻辑，也就是共性功能(最终体现为一个方法)
- **切入点**：PointCut，匹配连接点的条件，通知仅会在切入点方法执行时被应用
- **切面：**Aspect，描述通知与切入点的对应关系(通知+切入点)
- **目标对象**：Target,通知所应用的对象



## 通知类型

根据通知方法执行时机的不同，将通知类型分为一下常见的五种

- @Around 环绕通知，此注解标注的通知方法在目标方法前后都被执行
- @Before 前置通知，此注解标注的通知方法在目标方法前被执行
- @After 后置通知，此注解标注的通知方法在目标方法后被执行，无论是否有异常都会执行
- @AfterReturning 返回后通知，此注解标注的通知方法在目标方法后被执行，有异常不会执行
- @AfterThrowing 异常后通知，此注解标注的通知方法发生异常后执行

**ps**

- @Around环绕需要自己调用ProceedingJoinPoint.proceed()来让原始方法执行，其他通知不需要考虑目标方法执行
- @Around环绕通知方法的返回值，必须指定为Object，来接收原始方法的返回值



### @PointCut

该注解的作用是将公共的切点表达式抽取出来，需要用到时引用该切点表达式即可

```java
@Pointcut("execution(* com.creep.service.impl.*.*())")
private void pt(){}
@Around("pt()")
public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
```



```java
@Aspect
@Component
@Slf4j
public class MyAspect1 {

    @Pointcut("execution(* com.creep.service.impl.*.*())")
    private void pt(){}


    //前置通知-目标方法运行之前执行
    @Before("pt()")
    public void before(){
        log.info("before.....");
    }

    //环绕通知-目标方法运行前，后执行
    @Around("pt()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before....");
        Object result=proceedingJoinPoint.proceed();
        log.info("around...after");
        return  result;
    }
    //后置通知
    @After("pt()")
    public void after(){
        log.info("after.....");
    }

    @AfterReturning("pt()")
    public void afterReturning(){
        log.info("afterReturning.....");
    }
    @AfterThrowing("pt()")
    public void afterThrowing(){
        log.info("afterThrowing.....");
    }
}

```



## 通知顺序

当有多个切面的切入点都匹配到了目标方法，目标方法运行时，多个通知方法都会被执行

执行顺序：

- 不同切面类中，默认按切面类的类名字母排序
  - 目标方法前的通知方法：字母排名靠前的先执行
  - 目标方法后的通知方法：字母排名靠前的后执行
- 用@Order(数字)加在切面类上来控制顺序
  - 目标方法前的通知方法：数字小的先执行
  - 目标方法后的通知方法：数字小的后执行

```java
@Slf4j
@Component
@Aspect
@Order(3)
public class MyAspect2 {
```



## 切入点表达式

描述切入点方法的一种表达式，用来决定项目中哪些方法需要加入通知

**常见形式：**

- execution(...)：根据方法的签名来匹配
- @annotation(...)：根据注解匹配

### execution

主要根据方法的返回值，包名，类名，方法名，方法参数等信息来匹配

```java
execution(访问修饰符 返回值 包名.类名.方法名(方法参数) throw 异常)
其中可以省略的：
    访问修饰符，比如(public protected)
    包名.类名
    throws异常
```



可以使用通配符描述切入点

- `*`：单个独立的任意符号，可以通配任意返回值，包名，类名，方法名，任意类型的一个参数，也可以通配包，类，方法名的一部分

  ```java
  execution(* com.*.service.*.update*(*))
  ```

- `..`：多个连续的任意符号，可以通配任意层级的包，或任意类型，任意个数的参数

  ```java
  execution(* com.itheima..DeptService.*(..))
  ```

**可以使用&&，||，！来组合比较复杂的切入点表达式**

```java
@Before("execution(* com.creep.service.impl.DeptServiceImpl.list(..)) ||execution(* com.creep.service.impl.DeptServiceImpl.delete(..)) ")
```



### annotation

切入点表达式，匹配标识有特定注解的方法

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface LogOperation {
}
//定义注解

@Slf4j
@Component
@Aspect
@Order(1)
public class MyAspect4 {
//前置通知
@Before("@annotation(com.creep.anno.LogOperation)")
public void before() {
    log.info("MyAspect4 -> before ...");
}
}
//定义切面


@LogOperation
@Override
public List<Dept> list() {
    List<Dept> deptList = deptMapper.list();
    return deptList;
}
//在方法上加上注解
```



## 连接点

在Spring中用JoinPoint抽象了连接点，用它可以获得方法执行时的相关信息，如目标类名，方法名，方法参数等

- 对于@Around通知，获取连接点信息只能使用ProceedingJoinPoint
- 对于其他四种通知，获取连接点信息只能使用JoinPoint，它是ProceedingJoinPoint的父类型

```java
@Before("execution(* com.creep.service.impl.*Impl.*(..))")
public void before(JoinPoint joinPoint){
    //获取目标对象
    Object target = joinPoint.getTarget();
    log.info("目标对象为{}",target);

    //获取目标类
    String className = joinPoint.getTarget().getClass().getName();
    log.info("获取目标类：{}",className);

    //获取目标方法
    String methodName= joinPoint.getSignature().getName();

    //获取目标方法形参
    Object[] args = joinPoint.getArgs();


}
```





## ThreadLocal

ThreadLocal并不是一个Thread，而是Thread的局部变量

ThreadLocal为每个线程提供了一份单独的存储空间，具有线程隔离的效果，不同线程之间不会相互干扰

常用方法：

- public void set(T value) 设置当前线程的线程局部变量的值
- public T get() 返回当前线程所对应的线程局部变量的值
- public void remove() 移除当前线程的线程局部变量







# SpringBoot原理

## 配置优先级

springBoot中支持三种格式的配置文件：application.properties，application.yml，application.yaml

SpringBoot除了支持配置文件属性配置，还支持Java系统属性和命令行参数的方式进行属性配置

- Java系统属性

  ```java
  -Dserver.port=9000
  ```

- 命令行参数

  ```java
  --server.port=10010
  ```

**优先级：命令行参数>java系统属性>application.properties>application.yml>application.yaml**

在命令行执行

1. 执行maven打包指令package

2. 执行java指令，运行jar包

   ```java
   java -Dserver.port=9000 -jar springboot-web-config-0.0.1-SNAPSHOT.jar --server.port=10010
   ```

   

## bean管理

### bean作用域

Spring支持五种作用域，后三种在Web环境才生效

| 作用域      | 说明                                              |
| ----------- | ------------------------------------------------- |
| singleton   | 容器内同名称的 bean 只有一个实例（单例）（默认）  |
| prototype   | 每次使用该 bean 时会创建新的实例（非单例 / 多例） |
| request     | 每个请求范围内会创建新的实例（web 环境中，了解）  |
| session     | 每个会话范围内会创建新的实例（web 环境中，了解）  |
| application | 每个应用范围内会创建新的实例（web 环境中，了解）  |

单例的bean：无状态的bean

多例的bean：有状态的bean

```java
/**
* 默认bean是单例的-singleton：默认单例的bean是在项目创建启动时创建的，创建完毕后，会将该bean存入IOC容器
*/
@Lazy//延迟初始化-->延迟到第一次使用的时候，再来创建这个bean
@RestController
public class DeptController {
    

@Scope("prototype")//非单例的
@RestController
public class DeptController {
    

//获取bean对象
@Autowired
private ApplicationContext applicationContext;
@Test
public void testScope(){
    for(int i=0;i<100;i++){
        Object deptController = applicationContext.getBean("deptController");
        System.out.println(deptController);
    }
}
```

### 第三方bean

如果要管理的bean对象来自于第三方（不是定义的），是无法用@Component及衍生注解声明bean的，就需要用到@Bean注解

若要管理第三方bean对象，建议对这些bean进行集中分类配置，可以通过@Configuration注解声明一个配置类

```java
@Configuration
public class CommonConfig {
    @Bean//将方法返回值交给IOC容器管理，成为IOC容器的bean对象
    public AliyunOSSOperator abc(AliyunOSSProperties aliyunOSSProperties){
        return new AliyunOSSOperator(aliyunOSSProperties);
    }
}
```

如果第三方bean需要依赖其他bean对象，直接在bean定义方法中设置形参即可，容器会根据类型进行装配

通过@Bean注解的name或value属性可以声明bean的名称，如果不指定，默认bean的名称就是方法名



## SpringBoot原理

### 起步依赖

Maven当中的依赖传递

### 自动配置

自动配置：当spring项目启动后，一些配置类，bean对象就自动存入到了IOC容器中，不需要我们手动去声明，从而简化了开发，省去了繁琐的配置操作

#### 方案1

第三方工具包：

```java
@Component//加上Component注解
public class TokenParser {
```

启动类

```java
@ComponentScan(basePackages = {"com.example","com.itheima"})
@SpringBootApplication//具有组件扫描的功能，但是默认扫描的是启动类所在包及其子包，如果加了ComponentScan之后将不会默认扫描所在包及其子包
public class SpringbootWebConfigApplication {
```

#### **方案2**

@Import导入，@Import导入的类会被Spring加载到IOC容器中，导入形式主要有以下几种

1. 导入普通类

   ```java
   @Import(TokenParser.class)//导入普通类
   @SpringBootApplication
   public class SpringbootWebConfigApplication {
   ```

   

2. 导入配置类

   ```java
   @Import(HeaderParser.class)//导入配置类
   @SpringBootApplication
   public class SpringbootWebConfigApplication {
    
   //配置类
   @Configuration
   public class HeaderConfig {
   
       @Bean
       public HeaderParser headerParser(){
           return new HeaderParser();
       }
   ```

   

3. 导入ImportSelector接口实现类

   ```java
   @Import(MyImportSelector.class)//ImportSelector接口实现类
   @SpringBootApplication
   public class SpringbootWebConfigApplication {
       
   
   public class MyImportSelector implements ImportSelector {
       public String[] selectImports(AnnotationMetadata importingClassMetadata) {
           return new String[]{"com.example.HeaderParser"};
       }
   }
   ```

4. @EnableXxxx注解，封装@Import注解



#### 源码跟踪

@SpringBootApplication注解标识在SpringBoot工程引导类上，该注解由三个部分组成

- @SpringBootConfiguration：该注解和@Configuration注解作用相同，用来声明当前也是一个配置类
- @ComponentScan：组件扫描，默认扫描当前引导类所在包及前子包
- @EnableAutoConfiguration：SpringBoot实现自动豪华配置的核心注解



#### @conditional

按照一定的条件进行判断，在满足给定条件后才会注册对应的bean对象到SpringIOC容器中

位置：方法，类

@Conditional本身是一个父注解，派生出大量的子注解

- @ConditionalOnClass：判断环境中是否有对应的字节码文件，才注册bean到IOC容器

  ```java
  @Bean
  @ConditionalOnClass(name="io.jsonwebtoken.Jwts")//判断环境中是否有对应的字节码文件，如果有就创建bean
  public HeaderParser headerParser(){
      return new HeaderParser();
  }
  ```

- @ConditionalOnMissingBean：判断环境中没有对应的bean（类型或名称），才注册到bean到IOC容器中

  ```java
  @Bean
      @ConditionalOnMissingBean//判断环境中有没有对应的bean，如果没有就创建bean
      public HeaderParser headerParser(){
          return new HeaderParser();
      }
  ```

  

- @ConditionalOnProperty：判断配置文件中有对应属性和值，才注册bean到IOC容器

  ```java
  @ConditionalOnProperty(name="myname",havingValue = "creep")//判断myname的属性值是否等于creep，是就创建bean
  public HeaderParser headerParser(){
      return new HeaderParser();
  }
  
  
  在application.yml中写：
      myname: creep
  ```

  



### 自定义starter

在实际开发中，经常会定义一些公共组件，提供给各个项目团队使用，而在SpringBoot的项目中，一般会将这些公共组件封装为SpringBoot的Starter（包含了起步依赖和自动配置的功能）

![image-20260707193325222](C:\Users\LENOVO\AppData\Roaming\Typora\typora-user-images\image-20260707193325222.png)

自定义Aliyun-oss-spring-boot-starter 

步骤:

1. 创建aliyun-oss-spring-boot-starter模块
2. 创建aliyun-oss-spring-boot-autoconfigure模块，在starter中引入该模块
3. 在aliyun-oss-spring-boot-autoconfigure模块中的定义自动配置功能，并定义自动配置文件 META-INF/spring/xxxx.imports

