# IDEA使用git

## 配置忽略文件

将配置文件忽略掉可以屏蔽开发工具之间的差异

### 创建忽略规则文件xxx.ignore

便于被引用，该文件通常放在用户家目录下

```
# Complied class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
he_err_pid*

.classpath
.project
.settings
target
.idea
*.iml
```

### 在git config之中引用忽略配置文件

```
在gitconfig文件中添加代码:
[core]
	excludesfile = C:/Users/[自己账户用户名]/git.ignore
```

### 在IDEA中定位本地git程序

在IDEA的Setting中设置自己本机的git地址。

配置完成后，新建一个maven项目，并在VCS中开启git管理项目

## 使用IDEA克隆远程项目

在Git中选择clone，并将URL填入，等待Git操作即可