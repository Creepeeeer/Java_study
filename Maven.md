# Maven项目结构

![img](https://oss.itbaima.cn/internal/markdown/2024/08/28/iVXseYS6kcAzCnp.jpg)

其中src目录下存放我们的源代码和测试代码，分别位于main和test目录下，而test和main目录下又具有java、resources目录，它们分别用于存放Java源代码、静态资源（如配置文件、图片等）、很多JavaWeb项目可能还会用到webapp目录。

而下面的pom.xml则是Maven的核心配置，也是整个项目的所有依赖、插件、以及各种配置的集合，它也是使用XML格式编写的，一个标准的pom配置长这样：

xml复制代码

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.itbaima</groupId>
    <artifactId>BookManage</artifactId>
    <version>1.0</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

</project>
```

我们可以看到，Maven的配置文件是以`project`为根节点，而`modelVersion`定义了当前模型的版本，一般是4.0.0，我们不用去修改。

`properties`中一般都是一些变量和选项的配置，我们这里指定了JDK的源代码和编译版本为17，同时下面的源代码编码格式为UTF-8，无需进行修改。

# Maven基本概念

## 仓库

用于存储资源，包含各种jar包

**分类**

- 本地仓库：自己电脑上存储资源的仓库，连接远程仓库获取资源
- 远程仓库：非本机电脑上的仓库，为本地仓库提供资源 

- - 中央仓库，Maven团队维护，存储所有资源的仓库
  - 私服:部门/公司范围内存储资源的仓库，从中央仓库获取资源

**私服的作用**

- 保存具有版权的资源，包含购买或自主研发的jar
- 一定范围内共享资源，仅对内部开发，不对外共享



## 坐标

Maven中坐标用于描述仓库中资源的位置

`groupId`、`artifactId`、`version`这三个元素合在一起，用于唯一区别每个项目，别人如果需要将我们编写的代码作为依赖，那么就必须通过这三个元素来定位我们的项目，我们称为一个项目的基本坐标，所有的项目一般都有自己的Maven坐标，因此我们通过Maven导入其他的依赖只需要填写这三个基本元素就可以了，无需再下载Jar文件，而是Maven自动帮助我们下载依赖并导入：

- `groupId` 一般用于指定组名称，命名规则一般和包名一致，比如我们这里使用的是`org.example`，一个组下面可以有很多个项目。
- `artifactId` 一般用于指定项目在当前组中的唯一名称，也就是说在组中用于区分于其他项目的标记。
- `version` 代表项目版本
  - SNAPSHOT：功能不确定，尚处于开发中的版本，即快照版本
  - RELEASE：功能趋于稳定，当前更新停止，可以用于发行的版本





# 项目构建

```java
mvn compile #编译
mvn clean #清理
mvn test #测试
mvn package #打包
mvn install #安装到本地仓库
```



# 插件创建工程

**通用创建工程模板**

```java
mvn archetype:generate
    -DgroupId={project-packaging}
    -DartifactId={project-name}
    -DarchetypeArtifactId=maven-archetype-quickstart
    -DinteractiveMode=false
```

**创建Java工程**

```java
mvn archetype:generate -DgroupId=com.itheima -DartifactId=java-project -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=0.0.1-snapshot -DinteractiveMode=false
```

**创建Web工程**

```java
通用创建工程模板
mvn org.apache.maven.plugins:maven-archetype-plugin:3.4.1:generate `
  "-DgroupId={project-packaging}" `
  "-DartifactId={project-name}" `
  "-Dversion=0.0.1-SNAPSHOT" `
  "-Dpackage={project-packaging}" `
  "-DarchetypeGroupId=org.apache.maven.archetypes" `
  "-DarchetypeArtifactId={archetype-name}" `
  "-DarchetypeVersion=1.5" `
  "-DinteractiveMode=false"

说明：

{project-packaging}  项目包名，例如 com.creep
{project-name}       项目名，例如 java-project
{archetype-name}     项目骨架，例如 maven-archetype-quickstart

archetype:generate 的完整插件目标是 org.apache.maven.plugins:maven-archetype-plugin:3.4.1:generate；批量创建项目时需要提供 groupId、artifactId、version 以及 archetype 相关参数。

创建 Java 工程
mvn org.apache.maven.plugins:maven-archetype-plugin:3.4.1:generate `
  "-DgroupId=com.creep" `
  "-DartifactId=java-project" `
  "-Dversion=0.0.1-SNAPSHOT" `
  "-Dpackage=com.creep" `
  "-DarchetypeGroupId=org.apache.maven.archetypes" `
  "-DarchetypeArtifactId=maven-archetype-quickstart" `
  "-DarchetypeVersion=1.5" `
  "-DinteractiveMode=false"

maven-archetype-quickstart 是用来生成普通 Java Maven 项目的骨架，会生成 pom.xml、src/main/java、src/test/java 等结构。

创建 Web 工程
mvn org.apache.maven.plugins:maven-archetype-plugin:3.4.1:generate `
  "-DgroupId=com.creep" `
  "-DartifactId=web-project" `
  "-Dversion=0.0.1-SNAPSHOT" `
  "-Dpackage=com.creep" `
  "-DarchetypeGroupId=org.apache.maven.archetypes" `
  "-DarchetypeArtifactId=maven-archetype-webapp" `
  "-DarchetypeVersion=1.5" `
  "-DinteractiveMode=false"

maven-archetype-webapp 是用来生成传统 Java Web 项目的骨架，会生成 src/main/webapp、WEB-INF/web.xml、index.jsp 等结构。
```



# 依赖导入

Maven可以管理依赖

我们可以创建一个`dependencies`节点：

xml复制代码

```xml
<dependencies>
    //里面填写的就是所有的依赖
</dependencies>
```

那么现在就可以向节点中填写依赖了，那么我们如何知道每个依赖的坐标呢？我们可以在：[https://central.sonatype.com](https://central.sonatype.com/) 进行查询，我们直接搜索Lombok即可，打开后可以看到已经给我们写出了依赖的坐标：

xml复制代码

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.36</version>
</dependency>
```

我们直接将其添加到`dependencies`节点中即可

**加完如果报错，需要在Maven里同步所有Maven项目**



## 依赖管理流程

![img](https://oss.itbaima.cn/internal/markdown/2024/11/19/5s2HWYlJgSCOp1q.jpg)

通过流程图我们得知，一个项目依赖一般是存储在中央仓库中，也有可能存储在一些其他的远程仓库（可以自行搭建私服）几乎所有的依赖都被放到了中央仓库中，因此，Maven可以直接从中央仓库中下载大部分的依赖（因此Maven第一次导入依赖是需要联网的，否则无法下载）远程仓库中下载之后 ，会暂时存储在本地仓库，我们会发现我们本地存在一个`.m2`文件夹，这就是Maven本地仓库文件夹，默认建立在C盘。

在下次导入依赖时，如果Maven发现本地仓库中就已经存在某个依赖，那么就不会再去远程仓库下载了。

**注意：** 因为中心仓库服务器位于国外，下载速度缓慢，可能在导入依赖时会出现卡顿等问题，我们需要使用国内的镜像仓库服务器来加速访问（镜像仓库与中心仓库自动同步所有依赖，访问速度更快）有两种方式配置：

1. 可以配置IDEA自带的Maven插件远程仓库镜像地址，我们打开IDEA的安装目录，找到`安装根目录/plugins/maven/lib/maven3/conf`文件夹，找到`settings.xml`文件，打开编辑，找到mirror标签，把原来一串127.0.0.1的换成这个，空格也要补成和原来一样的：

   原来的

   ```java
   <mirror>
     <id>maven-default-http-blocker</id>
     <mirrorOf>external:http:*</mirrorOf>
     <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
     <url>http://0.0.0.0/</url>
     <blocked>true</blocked>
   </mirror>
   ```

   xml复制代码

   ```xml
   <mirror>
     <id>aliyunmaven</id>
     <mirrorOf>central</mirrorOf>
     <name>阿里云公共仓库</name>
     <url>https://maven.aliyun.com/repository/public</url>
   </mirror>
   ```

2. 自行前往Maven官网并下载最新版的Maven安装，然后将IDEA的Maven配置为我们自行安装的位置（好处是IDEA更新后不需要重新配置）可以一直使用，镜像配置方式同第1步。



## 排除依赖

```html
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.1.4</version>
            
            <!--排除依赖-->
            <exclusions>
                <exclusion>
                    <groupId>io.micrometer</groupId>
                    <artifactId>micrometer-observation</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
```





# 单元测试

使用Junit，在test/java目录创建测试类，在测试方法上声明@Test注解

单元测试类命名规范为:XxxxTest(规范)，Junit单元测试方法必须声明为public void



## 断言

Junit提供了一些辅助方法，用来帮我们确定被测试的方法是否按照预期的效果正常工作，这种方式称为断言

### Junit常见注解

| 注解               | 说明                                                         | 备注                              |
| ------------------ | ------------------------------------------------------------ | --------------------------------- |
| @Test              | 测试类中的方法用它修饰才能成为测试方法，才能启动执行         | 单元测试                          |
| @ParameterizedTest | 参数化测试的注解（可以让单个测试运行多次，每次运行时仅参数不同） | 用了该注解，就不需要 @Test 注解了 |
| @ValueSource       | 参数化测试的参数来源，赋予测试方法参数                       | 与参数化测试注解配合使用          |
| @DisplayName       | 指定测试类、测试方法显示的名称 （默认为类名、方法名）        |                                   |
| @BeforeEach        | 用来修饰一个实例方法，该方法会在**每一个**测试方法执行之前执行一次。 | 初始化资源（准备工作）            |
| @AfterEach         | 用来修饰一个实例方法，该方法会在**每一个**测试方法执行之后执行一次。 | 释放资源（清理工作）              |
| @BeforeAll         | 用来修饰一个静态方法，该方法会在所有测试方法之前**只执行一次**。 | 初始化资源（准备工作）            |
| @AfterAll          | 用来修饰一个静态方法，该方法会在所有测试方法之后**只执行一次**。 | 释放资源（清理工作）              |

```javascript
@BeforeAll
public static void beforeAll(){
    System.out.println("beforeAll");
}
@AfterAll
public static void afterAll(){
    System.out.println("afterAll");
}
@BeforeEach
public void beforeEach(){
    System.out.println("beforeEach");
}
@AfterEach
public void afterEach(){
    System.out.println("afterEach");
}
@ParameterizedTest
@ValueSource(ints = {1,3})
public void test(int num){
    System.out.println(num);
}
```





# 依赖范围

依赖的jar包默认情况下可以在任何地方使用，可以通过`<scope>...</scope>`设置其作用范围

作用范围：

- 主程序范围有效(main文件范围内)
- 测试程序范围有效(test文件夹范围内)
- 是否参与打包运行(package指令范围内)

| scope 值        | 主程序 | 测试程序 | 打包（运行） | 范例        |
| --------------- | ------ | -------- | ------------ | ----------- |
| compile（默认） | Y      | Y        | Y            | log4j       |
| test            | -      | Y        | -            | junit       |
| provided        | Y      | Y        | -            | servlet-api |
| runtime         | -      | Y        | Y            | jdbc 驱动   |

```javascript
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
```

