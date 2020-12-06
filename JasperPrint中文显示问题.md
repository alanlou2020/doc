## Jaspersoft中文显示问题

### 1、安装Jaspersoft studio

* 安装Jaspersoft studio完成后，在安装目录添加fontExtend文件夹。

### 2、下载包anytao-jasperreports-fonts-master.zip

* 解压后用把anytao-jasperreports-fonts-master\jasperreports-fonts\net\sf\jasperreports\fonts\dejavu\simsun.ttf字体拷贝到fontExtend文件夹中。

* 用maven编译jasperreports-fonts，进入jasperreports-fonts文件夹，命令为：

  ~~~ maven
  mvn clean package -Dmaven.test.skip=true
  ~~~

* 生成本地仓库maven包，命令为：

  ~~~maven
  mvn install:install-file -Dfile="F:\GoogleDownLoad\anytao-jasperreports-fonts-master\jasperreports-fonts\target\jasperreports-fonts-6.4.1.jar" -DgroupId=net.sf.jasperreports -DartifactId=jasperreports-fonts -Dversion=6.4.1 -Dpackaging=jar
  ~~~

### 3、在Jaspersoft studio中添加fontExtend文件夹中拷入的字体，命名为mysimsun（添加的字体为simsun.ttf）。

### 4、在项目中引入maven包，如下：

~~~pom
<dependency>
    <groupId>net.sf.jasperreports</groupId>
    <artifactId>jasperreports</artifactId>
    <version>6.10.0</version>
    <exclusions>
        <exclusion>
        <groupId>com.lowagie</groupId>
        <artifactId>itext</artifactId>
        </exclusion>
        <exclusion>
        <groupId>net.sf.jasperreports</groupId>
        <artifactId>jasperreports-fonts</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>com.lowagie</groupId>
    <artifactId>itext</artifactId>
    <version>2.1.7</version>
</dependency>
<dependency>
    <groupId>net.sf.jasperreports</groupId>
    <artifactId>jasperreports-fonts</artifactId>
    <version>6.4.1</version>
</dependency>
~~~





