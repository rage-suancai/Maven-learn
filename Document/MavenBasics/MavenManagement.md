## 使用Maven管理项目
注意: 开始之前 看看你C盘空间够不够 最好预留2GB空间以上

吐槽: 很多电脑预装系统C盘都给得巨少 就算不装软件 一些软件的缓存文件也能给你塞满 建议有时间重装一下系统重新分配一下磁盘空间

Maven翻译为"专家", "内行"是Apache下的一个纯Java开发的开源项目 基于项目对象模型(缩写: POM)概念 Maven利用一个中央信息片断能管理一个项目的构建, 
报告和文档等步骤 Maven是一个项目管理工具 可以对Java项目进行构建, 依赖管理Maven也可被用于构建和管理各种项目
例如 C#, Ruby, Scala和其他语言编写的项目 Maven曾是Jakarta项目的子项目 现为由Apache软件基金会主持的独立Apache项目

通过Maven 可以帮助我们做:
- 项目的自动构建 包括代码的编译, 测试, 打包, 安装, 部署等操作
- 依赖管理 项目使用到哪些依赖 可以快速完成导入

我们之前并没有讲解如何将我们的项目打包为Jar文件运行 同 我们导入依赖的时候 每次都要去下载对应的Jar包 这样其实是很麻烦的
并且还有可能一个Jar包依赖于另一个Jar包 就像之前使用JUnit一样 因此我们需要一个更加方便的包管理机制

Maven也需要安装环境 但是IDEA已经自带了Maven环境 因此我们不需要再去进行额外的环境安装
(无IDEA也能使用Maven 但是配置过程很麻烦 并且我们现在使用的都是IDEA的集成开发环境 所以这里就不讲解Maven命令行操作了) 我们直接创建一个新的Maven项目即可

### Maven项目结构
我们可以来看一下 一个Maven项目和我们普通的项目有什么区别:

<img src="https://image.itbaima.cn/markdown/2023/03/06/tYh7BGvZHu6ncdf.jpg"/>

那么首先 我们需要了解一下POM文件 它相当于是我们整个Maven项目的配置文件 它也是使用XML编写的:

```xml
                        <?xml version="1.0" encoding="UTF-8"?>
                        <project xmlns="http://maven.apache.org/POM/4.0.0"
                                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
                            <modelVersion>4.0.0</modelVersion>
                        
                            <groupId>org.example</groupId>
                            <artifactId>MavenStudy</artifactId>
                            <version>1.0-SNAPSHOT</version>
                        
                            <properties>
                                <maven.compiler.source>17</maven.compiler.source>
                                <maven.compiler.target>17</maven.compiler.target>
                                <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
                            </properties>
                        
                        </project>
```

我们可以看到 Maven的配置文件是以`project`为根节点 而`modelVersion`定义了当前模型的版本 一般是4.0.0 我们不用去修改

`groupId`, `artifactId`, `version`这三个元素合在一起 用于唯一区别每个项目 别人如果需要将我们编写的代码作为依赖 那么就必须通过这三个元素来定位我们的项目 我们称为一个项目的基本坐标
所有的项目一般都有自己的Maven坐标 因此我们通过Maven导入其他的依赖只需要填写这三个基本元素就可以了 无需再下载Jar文件 而是Maven自动帮助我们下载依赖并导入
- `groupId`: 一般用于指定组名称 命名规则一般和包名一致 比如我们这里使用的是`org.example` 一个组下面可以有很多个项目
- `artifactId`: 一般用于指定项目在当前组中的唯一名称 也就是说在组中用于区分于其他项目的标记
- `version`: 代表项目版本 随着我们项目的开发和改进 版本号也会不断更新 就像LOL一样 每次赛季更新都会有一个大版本更新 我们的Maven项目也是这样 我们可以手动指定当前项目的版本号
             其他人使用我们的项目作为依赖时 也可以根本版本号进行选择(这里的SNAPSHOT代表快照 一般表示这是一个处于开发中的项目 正式发布项目一般只带版本号)

`properties`中一般都是一些变量和选项的配置 我们这里指定了JDK的源代码和编译版本为1.8 无需进行修改

### Maven依赖导入
现在我们尝试使用Maven来帮助我们快速导入依赖 我们需要导入之前的JDBC驱动依赖, JUnit依赖, Mybatis依赖, Lombok依赖 那么如何使用Maven来管理依赖呢?

我们可以创建一个`dependencies`节点:

```xml
                        <dependencies>
                            // 里面填写的就是所有的依赖
                        </dependencies>
```

那么现在就可以向节点中填写依赖了 那么我们如何知道每个依赖的坐标呢? 我们可以在: https://mvnrepository.com/ 进行查询
(可能打不开 建议用流量 或是直接百度某个项目的Maven依赖) 我们直接搜索lombok即可 打开后可以看到已经给我们写出了依赖的坐标:

```xml
                        <dependency>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                            <version>1.18.28</version>
                            <scope>provided</scope>
                        </dependency>
```

我们直接将其添加到`dependencies`节点中即可 现在我们来编写一个测试用例看看依赖导入成功了没有:

```java
                        public class Main {
    
                            public static void main(String[] args) {
                                
                                Student student = new Student("小明", 18);
                                System.out.println(student);
                                
                            }
                            
                        }
```

```java
                        @AllArgsConstructor
                        @Data
                        public class Student {
                        
                            private String name;
                        
                            private int age;
                        
                        }
```

项目运行成功 表示成功导入了依赖 那么 Maven是如何进行依赖管理呢 以致于如此便捷的导入依赖 我们来看看Maven项目的依赖管理流程:

<img src="https://image.itbaima.cn/markdown/2023/03/06/XYNMU5WCrZv9cwy.jpg"/>

通过流程图我们得知 一个项目依赖一般是存储在中央仓库中 也有可能存储在一些其他的远程仓库(私服) 几乎所有的依赖都被放到了中央仓库中
因此 Maven可以直接从中央仓库中下载大部分的依赖(Maven第一次导入依赖是需要联网的) 远程仓库中下载之后 会暂时存储在本地仓库
我们会发现我们本地存在一个`.m2`文件夹 这就是Maven本地仓库文件夹 默认建立在C盘 如果你C盘空间不足 会出现问题

在下次导入依赖时 如果Maven发现本地仓库中就已经存在某个依赖 那么就不会再去远程仓库下载了

可能在导入依赖时 小小伙伴们会出现卡顿的问题 我们建议配置一下IDEA自带的Maven插件远程仓库地址 我们打开IDEA的安装目录
找到`安装根目录/plugins/maven/lib/maven3/conf`文件夹 找到`settings.xml`文件 打开编辑:

找到mirros标签 添加以下内容:

```xml
                        <mirror>
                              <id>nexus-aliyun</id>
                              <mirrorOf>*</mirrorOf>
                              <name>Nexus aliyun</name>
                              <url>http://maven.aliyun.com/nexus/content/groups/public</url>
                        </mirror>
```

这样 我们就将默认的远程仓库地址(国外) 配置为国内的阿里云仓库地址了(依赖的下载速度就会快起来了)

### Maven依赖作用域
除了三个基本的属性使用于定位坐标外 依赖还可以添加以下属性:
- type: 依赖的类型 对于项目坐标定义的packaging 大部分情况下 该元素不必声明 其默认值为jar
- scope: 依赖的范围(作用域 着重讲解)
- optional: 标记依赖是否可选
- exclusions: 用来排除传递性依赖(一个项目有可能依赖于其他项目 就像我们的项目 如果别人要用我们的项目作为依赖 那么就需要一起下载我们项目的依赖 如Lombok)

我们着重来讲解一下`scope`属性 它决定了依赖的作用域范围:
- compile: 为默认的依赖有效范围 如果在定义依赖关系的时候 没有明确指定依赖有效范围的话 则默认采用该依赖有效范围 此种依赖 在编译, 运行, 测试时均有效
- provided: 在编译, 测试时有效 但是在运行时无效 也就是说 项目在运行时 不需要此依赖 比如我们上面的Lombok 我们只需要在编译阶段使用它 编译完成后 实际上已经转换为对应的代码了 因此Lombok不需要在项目运行时也存在
- runtime: 在运行, 测试时有效 但是在编译代码时无效 比如我们如果需要自己写一个JDBC实现 那么肯定要用到JDK为我们指定的接口 但是实际上在运行时是不用自带JDK的依赖 因此只保留我们自己写的内容即可
- test: 只在测试时有效 例如: JUnit 我们一般只会在测试阶段使用JUnit 而实际项目运行时 我们就用不到测试了 那么我们来看看 导入JUnit的依赖

同样的 我们可以在网站上搜索Junit的依赖 我们这里导入最新的JUnit5作为依赖:

```xml
                        <dependency>
                            <groupId>org.junit.jupiter</groupId>
                            <artifactId>junit-jupiter</artifactId>
                            <version>5.8.1</version>
                            <scope>test</scope>
                        </dependency>
```

我们所有的测试用例全部编写到Maven项目给我们划分的test目录下 位于此目录下的内容不会在最后被打包到项目中 只用作开发阶段测试使用

```java
                        public class MainTest {

                            @Test
                            public void test() {
                                
                                System.out.println("测试");
                              	// Assert在JUnit5时名称发生了变化Assertions
                                Assertions.assertArrayEquals(new int[]{1, 2, 3}, new int[]{1, 2});
                                
                            }
                            
                        }
```

因此 一般仅用作测试的依赖如JUnit只保留在测试中即可 那么现在我们再来添加JDBC和Mybatis的依赖:

```xml
                        <dependency>
                            <groupId>mysql</groupId>
                            <artifactId>mysql-connector-java</artifactId>
                            <version>8.0.27</version>
                        </dependency>
                        <dependency>
                            <groupId>org.mybatis</groupId>
                            <artifactId>mybatis</artifactId>
                            <version>3.5.7</version>
                        </dependency>
```

我们发现 Maven还给我们提供了一个`resource`文件夹 我们可以将一些静态资源 比如配置文件 放入到这个文件夹中
项目在打包时会将资源文件夹中文件一起打包的Jar中 比如我们在这里编写一个Mybatis的配置文件:

```xml
                        <?xml version="1.0" encoding="UTF-8" ?>
                        <!DOCTYPE configuration
                                PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                                "http://mybatis.org/dtd/mybatis-3-config.dtd">

                        <configuration>
    
                            <settings>
                                <setting name="mapUnderscoreToCamelCase" value="true"/>
                                <setting name="cacheEnabled" value="true"/>
                                <setting name="logImpl" value="JDK_LOGGING" />
                            </settings>
    
                            <!-- 需要在environments的上方 -->
                            <typeAliases>
                                <package name="com.test.entity"/>
                            </typeAliases>
    
                            <environments default="development">
                                <environment id="development">
                                    <transactionManager type="JDBC"/>
                                    <dataSource type="POOLED">
                                        <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                                        <property name="url" value="jdbc:mysql://localhost:3306/study"/>
                                        <property name="username" value="test"/>
                                        <property name="password" value="123456"/>
                                    </dataSource>
                                </environment>
                            </environments>
    
                            <mappers>
                                <mapper class="com.test.mapper.TestMapper"/>
                            </mappers>
    
                        </configuration>
```

现在我们创建一下测试用例 顺便带大家了解一下Junit5的一些比较方便的地方:

```java
                        public class MainTest {

                            // 因为配置文件位于内部 我们需要使用Resources类的getResourceAsStream来获取内部的资源文件
                            private static SqlSessionFactory factory;
                        
                            // 在JUnit5中@Before被废弃 它被细分了:
                            @BeforeAll // 一次性开启所有测试案例只会执行一次(方法必须是static)
                            // @BeforeEach 一次性开启所有测试案例每个案例开始之前都会执行一次
                            @SneakyThrows
                            public static void before() {
                                
                                factory = new SqlSessionFactoryBuilder()
                                        .build(Resources.getResourceAsStream("mybatis.xml"));
                                
                            }
                            
                            @DisplayName("Mybatis数据库测试") // 自定义测试名称
                            @RepeatedTest(3) // 自动执行多次测试
                            public void test() {
                                
                                try (SqlSession sqlSession = factory.openSession(true)){
                                    TestMapper testMapper = sqlSession.getMapper(TestMapper.class);
                                    System.out.println(testMapper.getStudentBySid(1));
                                }
                                
                            }
                            
                        }
```

那么就有人提问了 如果我需要的依赖没有上传的远程仓库 而是只有一个Jar怎么办呢? 我们可以使用第四种作用域:
- system: 作用域和provided是一样的 但是它不是从远程仓库获取 而是直接导入本地Jar包:

```xml
                        <dependency>
                             <groupId>javax.jntm</groupId>
                             <artifactId>lbwnb</artifactId>
                             <version>2.0</version>
                             <scope>system</scope>
                             <systemPath>C://学习资料/4K高清无码/test.jar</systemPath>
                        </dependency>
```

比如上面的例子 如果scope为system 那么我们需要添加一个systemPath来指定jar文件的位置 这里就不再演示了

### Maven可选依赖
当项目中的某些依赖不希望被使用此项目作为依赖的项目使用时 我们可以给依赖添加optional标签表示此依赖是可选的 默认在导入依赖时 不会导入可选的依赖:

```xml
                        <optional>true</optional>
```

比如Mybatis的POM文件中 就存在大量的可选依赖:

```xml
                        <dependency>
                          <groupId>org.slf4j</groupId>
                          <artifactId>slf4j-api</artifactId>
                          <version>1.7.30</version>
                          <optional>true</optional>
                        </dependency>

                        <dependency>
                          <groupId>org.slf4j</groupId>
                          <artifactId>slf4j-log4j12</artifactId>
                          <version>1.7.30</version>
                          <optional>true</optional>
                        </dependency>

                        <dependency>
                          <groupId>log4j</groupId>
                          <artifactId>log4j</artifactId>
                          <version>1.2.17</version>
                          <optional>true</optional>
                        </dependency>

                         ...
```

由于Mybatis要支持多种类型的日志 需要用到很多种不同的日志框架 因此需要导入这些依赖来做兼容 但是我们项目中并不一定会使用这些日志框架作为Mybatis的日志打印器
因此这些日志框架仅Mybatis内部做兼容需要导入使用 而我们可以选择不使用这些框架或是选择其中一个即可 也就是说我们导入Mybatis之后想用什么日志框架再自己加就可以了

### Maven排除依赖
我们了解了可选依赖 现在我们可以让使用此项目作为依赖的项目默认不使用可选依赖 但是如果存在那种不是可选依赖
但是我们导入此项目有不希望使用此依赖该怎么办呢? 这个时候我们就可以通过排除依赖来防止添加不必要的依赖:

```xml
                        <dependency>
                            <groupId>org.junit.jupiter</groupId>
                            <artifactId>junit-jupiter</artifactId>
                            <version>5.8.1</version>
                            <scope>test</scope>
    
                            <exclusions>
                                <exclusion>
                                    <groupId>org.junit.jupiter</groupId>
                                    <artifactId>junit-jupiter-engine</artifactId>
                                </exclusion>
                            </exclusions>
                        </dependency>
```

我们这里演示了排除JUnit的一些依赖 我们可以在外部库中观察排除依赖之后和之前的效果

### Maven继承关系
一个Maven项目可以继承自另一个Maven项目 比如多个子项目都需要父项目的依赖 我们就可以使用继承关系来快速配置

我们右键左侧栏 新建一个模块 来创建一个子项目

```xml
                        <?xml version="1.0" encoding="UTF-8"?>
                        <project xmlns="http://maven.apache.org/POM/4.0.0"
                                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
                            <parent>
                                <artifactId>MavenTest</artifactId>
                                <groupId>org.example</groupId>
                                <version>1.0-SNAPSHOT</version>
                            </parent>
    
                            <modelVersion>4.0.0</modelVersion>
                        
                            <artifactId>ChildModel</artifactId>

                            <maven.compiler.source>17</maven.compiler.source>
                            <maven.compiler.target>17</maven.compiler.target>
                            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
                        
                        </project>
```

我们可以看到 IDEA默认给我们添加了一个parent节点 表示此Maven项目是父Maven项目的子项目 子项目直接继承父项目的`groupId`
子项目会直接继承父项目的所有依赖 除非依赖添加了optional标签，我们来编写一个测试用例尝试一下:

```java
                        import lombok.extern.java.Log;

                        @Log
                        public class Main {
                            
                            public static void main(String[] args) {
                                log.info("我是日志信息");
                            }
                            
                        }
```

可以看到 子项目也成功继承了Lombok依赖

我们还可以让父Maven项目统一管理所有的依赖 包括版本号等 子项目可以选取需要的作为依赖 而版本全由父项目管理
我们可以将`dependencies`全部放入`dependencyManagement`节点 这样父项目就完全作为依赖统一管理:

```xml
                        <dependencyManagement>
    
                            <dependencies>
                                <dependency>
                                    <groupId>org.projectlombok</groupId>
                                    <artifactId>lombok</artifactId>
                                    <version>1.18.22</version>
                                    <scope>provided</scope>
                                </dependency>
                                
                                <dependency>
                                    <groupId>org.junit.jupiter</groupId>
                                    <artifactId>junit-jupiter</artifactId>
                                    <version>5.8.1</version>
                                    <scope>test</scope>
                                </dependency>
                                
                                <dependency>
                                    <groupId>mysql</groupId>
                                    <artifactId>mysql-connector-java</artifactId>
                                    <version>8.0.27</version>
                                </dependency>
                                
                                <dependency>
                                    <groupId>org.mybatis</groupId>
                                    <artifactId>mybatis</artifactId>
                                    <version>3.5.7</version>
                                </dependency>
                            </dependencies>
    
                        </dependencyManagement>
```

我们发现 子项目的依赖失效了 因为现在父项目没有依赖 而是将所有的依赖进行集中管理 子项目需要什么再拿什么即可 同时子项目无需指定版本 所有的版本全部由父项目决定 子项目只需要使用即可:

```xml
                        <dependencies>
    
                            <dependency>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                                <scope>provided</scope>
                            </dependency>
    
                        </dependencies>
```

当然 父项目如果还存在dependencies节点的话 里面的内依赖依然是直接继承:

```xml
                        <dependencies>
    
                            <dependency>
                                <groupId>org.junit.jupiter</groupId>
                                <artifactId>junit-jupiter</artifactId>
                                <version>5.8.1</version>
                                <scope>test</scope>
                            </dependency>
    
                        </dependencies>
                        
                        <dependencyManagement>
                            <dependencies>
                              ...
```

### Maven常用命令
我们可以看到在IDEA右上角Maven板块中 每个Maven项目都有一个生命周期 实际上这些是Maven的一些插件 每个插件都有各自的功能 比如:
- `clean`命令 执行后会清理整个`target`文件夹 在之后编写Springboot项目时可以解决一些缓存没更新的问题
- `validate`命令 可以验证项目的可用性
- `compile`命令 可以将项目编译为.class文件
- `install`命令 可以将当前项目安装到本地仓库 以供其他项目导入作为依赖使用
- `verify`命令 可以按顺序执行每个默认生命周期阶段(`validate`, `compile`, `package`等)

### Maven测试项目
通过使用`test`命令 可以一键测试所有位于test目录下的测试案例 请注意有以下要求:
- 测试类的名称必须是以`Test`结尾 比如`MainTest`
- 测试方法上必须标注`@Test`注解 实测`@RepeatedTest`无效

这是由于JUnit5比较新 我们需要重新配置插件升级到高版本 才能完美的兼容Junit5:

```xml
                        <build>
                            <plugins>
                                <plugin>
                                    <groupId>org.apache.maven.plugins</groupId>
                                    <artifactId>maven-surefire-plugin</artifactId>
                                    <!-- JUnit 5 requires Surefire version 2.22.0 or higher -->
                                    <version>2.22.0</version>
                                </plugin>
                            </plugins>
                        </build>
```

现在`@RepeatedTest`, `@BeforeAll`也能使用了

### Maven打包项目
我们的项目在编写完成之后 要么作为Jar依赖 供其他模型使用 要么就作为一个可以执行的程序 在控制台运行 我们只需要直接执行package命令就可以直接对项目的代码进行打包 生成jar文件

当然 以上方式仅适用于作为Jar依赖的情况 如果我们需要打包一个可执行文件 那么我不仅需要将自己编写的类打包到Jar中
同时还需要将依赖也一并打包到Jar中 因为我们使用了别人为我们通过的框架 自然也需要运行别人的代码 我们需要使用另一个插件来实现一起打包:

```xml
                        <plugin>
                            <artifactId>maven-assembly-plugin</artifactId>
                            <version>3.1.0</version>
                            <configuration>
                                <descriptorRefs>
                                    <descriptorRef>jar-with-dependencies</descriptorRef>
                                </descriptorRefs>
                                <archive>
                                    <manifest>
                                        <addClasspath>true</addClasspath>
                                        <mainClass>com.test.Main</mainClass>
                                    </manifest>
                                </archive>
                            </configuration>
                            <executions>
                                <execution>
                                    <id>make-assembly</id>
                                    <phase>package</phase>
                                    <goals>
                                        <goal>single</goal>
                                    </goals>
                                </execution>
                            </executions>
                        </plugin>
```

在打包之前也会执行一次test命令 来保证项目能够正常运行 当测试出现问题时 打包将无法完成 我们也可以手动跳过 选择`执行Maven目标来`手动执行Maven命令 输入`mvn package -Dmaven.test.skip=true`来以跳过测试的方式进行打包

最后得到我们的Jar文件 在同级目录下输入`java -jar xxxx.jar`来运行我们打包好的Jar可执行程序(xxx代表文件名称)
- `deploy`命令用于发布项目到本地仓库和远程仓库 一般情况下用不到 这里就不做讲解了
- `site`命令用于生成当前项目的发布站点 暂时不需要了解

我们之前还讲解了多模块项目 那么多模块下父项目存在一个`packing`打包类型标签 所有的父级项目的packing都为pom packing默认是jar类型 如果不作配置 maven会将该项目打成jar包
作为父级项目 还有一个重要的属性 那就是modules 通过modules标签将项目的所有子项目引用进来 在build父级项目时 会根据子模块的相互依赖关系整理一个build顺序 然后依次build