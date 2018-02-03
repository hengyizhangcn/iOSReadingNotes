在mac电脑部署spring应用

1.安装eclipse

a.官网下载

http://wiki.eclipse.org/Eclipse/Installation

根据自己电脑的jdk环境选择适合的版本

```
java -version #如果命令不能使用输入java -help命令查询即可
```

b.使用brew cast命令安装

brew cask search eclipse #查询可安装选项，如

```
eclipse-cpp                   eclipse-php
eclipse-ide                   eclipse-platform
eclipse-installer             eclipse-ptp
eclipse-java                  eclipse-rcp
eclipse-jee                   eclipse-smarthome-designer
eclipse-modeling              nodeclipse
```

brew cask install eclipse-ide

2.