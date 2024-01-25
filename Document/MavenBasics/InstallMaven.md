## Maven环境安装

### 下载Maven
直接前往Maven官方下载: https://maven.apache.org/download.cgi

<img src="https://image.itbaima.cn/markdown/2023/04/16/MW4bjtsyd1xcGKv.png"/>

看到这个.zip结尾的文件 直接下就完事

### 安装Maven

#### Windows
接着我们只需要将Maven解压到一个目录中(注意: 目录不要包含中文字符 空格也最好没有) 例如:

```shell
                        C:\Users\sweetan\opt\maven
```

<img src="https://image.itbaima.cn/markdown/2023/04/16/isqhVmkc3KCFgJ9.jpg"/>

然后我们需要进行一些配置 将其添加到PATH环境变量中 以win11为例子 右键此电脑 -> 点击属性 -> 再点击高级设置:

<img src="https://image.itbaima.cn/markdown/2023/04/16/hbqw6WKtD5TEXU2.jpg"/>

编辑环境变量的Path

<img src="https://image.itbaima.cn/markdown/2023/04/16/9uRjbtYek4mvZ1W.jpg"/>

添加bin路径到Path中:

<img src="https://image.itbaima.cn/markdown/2023/04/16/ntMwKPslo2WSpHI.jpg"/>

打开powershell或者cmd输入`mvn --version`:

<img src="https://image.itbaima.cn/markdown/2023/04/16/Xo8NJeRDQY1lWUT.jpg"/>

如果你希望IDEA也使用我们自己安装的Maven 那么可以进行配置:

<img src="https://image.itbaima.cn/markdown/2023/04/16/ReotGJ81ScUE4Zk.jpg"/>

修改为我们自己的Maven路径:

<img src="https://image.itbaima.cn/markdown/2023/04/16/9MZDeKpN13BdG8n.jpg"/>

点击应用即可生效(某些已经使用捆绑Maven的项目可能还需要重复进行一次设置)之后创建的项目都会优先考虑使用我们自己的Maven环境

#### MacOS
接着我们只需要将Maven直接拖到以下目录:

    /Library/Java/Extensions

<img src="https://image.itbaima.cn/markdown/2023/04/15/xsOTqb1ZU3JDg5A.png"/>

然后我们需要进行一些配置 将其添加到PATH环境变量中:

    sudo vim /etc/zshrc

在zsh配置中添加以下环境变量设置:

    export PATH=$PATH:/Library/Java/Extensions/Maven/bin

保存 然后刷新一下:

    source /etc/zshrc

刷新完成之后就可以查看版本了:

<img src="https://image.itbaima.cn/markdown/2023/04/15/Hxjn7kPVyrfJ1tg.png"/>

如果你希望IDEA也使用我们自己安装的Maven 那么可以进行配置:

<img src="https://image.itbaima.cn/markdown/2023/04/15/Hxjn7kPVyrfJ1tg.png"/>

修改为我们自己的Maven路径:

<img src="https://image.itbaima.cn/markdown/2023/04/15/zVgB1XUEs2fKlI9.png"/>

点击应用即可生效(某些已经使用捆绑Maven的项目可能还需要重复进行一次设置) 之后创建的项目都会优先考虑使用我们自己的Maven环境

#### Ubuntu
安装Maven前请确认java环境是否正确安装

```shell
    java --version
```

Ubuntu软件源中默认已经包含Maven 我们可以直接下载安装首先更新一下:

```shell
    sudo apt update
```

然后直接安装即可

```shell
    sudo apt install maven
```

安装完成之后就可以查看版本了:

```shell
    mvn --version
````

如果正常输出版本号就代表安装成功

#### CentOS
安装Maven前请确认java环境是否正确安装

```shell
    java --version
```

##### CentOS 7

安装前先确保系统已经更新

```shell
    sudo yum update
```

安装

```shell
    sudo yum install maven
```

##### CentOS 8 / stream
centos 8 / stream使用的是`dnf`包管理工具

确保系统已经更新

```shell
    sudo dnf update
```

安装

```shell
    sudo dnf install maven
````

安装完成之后就可以查看版本了:

```shell
    mvn --version
```

如果正常输出版本号就代表安装成功

### 修改Maven源到国内
在当前用户的家目录下建立文件夹`.m2` 并新建一个`settings.xml`文件

```shell
                        mkdir -p ~/.m2
                        vim ~/.m2/settings.xml
                        # 或者 nano ~/.m2/settings.xml
```

添加如下内容:

```xml
                        <?xml version="1.0" encoding="UTF-8"?>
                        <settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 http://maven.apache.org/xsd/settings-1.2.0.xsd">
                            <mirrors>
                                
                                <mirror>
                                    <id>aliyunmaven-public</id>
                                    <mirrorOf>*</mirrorOf>
                                    <name>阿里云公共仓库</name>
                                    <url>https://maven.aliyun.com/repository/public</url>
                                </mirror>
                                
                                <mirror>
                                    <id>maven-default-http-blocker</id>
                                    <mirrorOf>external:http:*</mirrorOf>
                                    <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
                                    <url>http://0.0.0.0/</url>
                                    <blocked>true</blocked>
                                </mirror>
                                
                          </mirrors>
    
                        </settings>
```

这样我们就完成了配置文件的配置了