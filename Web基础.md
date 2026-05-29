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



