## Vue CLI（脚手架）安装

#### 1、需先安装NodeJS（需node8.9以上或更高版本，推荐 v10 以上）。

#### 2、检查NodeJS版本。

1. 默认情况下安装完NodeJS会自动安装Node和NPM。

2. Node环境要求8.9以上或者更高版本（推荐 v10 以上）。

   ~~~powershell
   PS C:\> node -v
   v10.16.0
   PS C:\> npm -v
   6.9.0
   ~~~

#### 3、Vue CLI使用前提。

1. WebPack的全局安装。

   ~~~powershell
   PS C:\> npm install webpack -g
   PS C:\> webpack -v
   4.44.2
   ~~~

2. 安装Vue脚手架。

   ~~~powershell
   PS C:\> npm install -g @vue/cli
   PS C:\> vue --version
   @vue/cli 4.5.6
   ~~~

   ==注意：上面安装的是Vue CLI3的版本，如果需要想按照Vue CLI2的方式初始化项目时不可以的。==

   ==拉取2.x模板（旧版本）==

   ~~~powershell
   PS C:\> npm install -g @vue/cli-init
   PS C:\> vue init webpack my-project
   ~~~

3. Vue CLI2初始化项目。

   ~~~powershell
   PS C:\> vue init webpack my-project
   ~~~

4. Vue CLI3初始化项目。

   ~~~powershell
   PS C:\> vue create my-project
   ~~~