# Maven

Apache Maven，是一个软件（特别是Java软件）项目管理及自动构建工具，由Apache软件基金会所提供。基于项目对象模型（缩写：POM）概念，Maven利用一个中央信息片断能管理一个项目的构建、报告和文档等步骤。

## 下载安装

我们推荐使用maven3.5+以上版本，因为其引入的revision功能可以让其子项目共享版本，这也是实现bom的主要手段之一。

- [下载地址](http://maven.apache.org/download.cgi)

下载Binary包后解压到自己的目录。

## 环境变量
### windows
新建环境变量名：MAVEN_HOME，变量值：解压的目录地址
PATH中末尾新增%MAVEN_HOME%/bin（如果前面没分号增加一个）

### macOS

打开终端，打开当前用户环境变量文件：

```
vim ~/.bash_profile
```

新增一行：

```
export PATH=$PATH:~/{maven解压路径}/bin
```

保存后：

```
source ~/.bash_profile
```

## 确认安装

终端中输入: mvn -v