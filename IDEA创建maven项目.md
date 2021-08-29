### IDEA创建maven项目

##### 1、设置Maven环境变量及本地仓库

> * File->Setting，搜索框输入“Maven”，设置“Maven home path”，”User setting file“，“local repository”。
> * File->New Projects Setup->Setting for New Projects，设置“Maven home path”，”User setting file“，“local repository”（这里设置了以后新的项目都是该默认值）。

##### 2、新增空项目：

> File->New->Project，选择“Empty Project”，点“next”。输入“Project name”和“Project location”，点“Finish”。

##### 3、新增Maven模块：

> File->New->Module，选择“Maven”，点"next"。点击“Artifact Coordinates”下拉，填写“GroupId”，“ArtifactId”和“Version”（填写“ArtifactId”时“Name”会自动填写），最后填写“Location”，点“Finish”。

##### 4、设置Java版本：

> * File->Setting，在搜索框内输入“javac”，后选中“Java Compiler”，更改“Per-module bytecode version”中各个项目的属性为8。
> * File->Project Setting，选中“Project”，设置“Project SDK”为1.8（安装的java环境），选择“Project language level”为“8 - Lambdas, type annotations etc.”。
> * File->Project Setting，选中“Modules”，设置“Language level”为“8 - Lambdas, type annotations etc.”

