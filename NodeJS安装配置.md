## NodeJS安装配置

#### 1. 下载地址：https://nodejs.org/en/download/

#### 2. 配置方法（zip解压版）：

1. 添加解压文件的系统环境变量：在环境变量的系统变量的path末尾添加解压缩的文件夹名（如：D:\node-v12.18.4-win-x64）。

2. 执行命令，查看node版本：

   ```powershell
   node -v
   v10.16.0
   ```

3. 新版的node在安装时同时也安装了npm，执行命令，查看npm的版本：

   ~~~powershell
   npm -v
   6.9.0
   ~~~

#### 3. 修改全局依赖包下载路径:

默认情况下，我们在执行npm install -g XXXX下载全局包时，这个包的默认存放路径位C:\Users\Alan\AppData\Roaming\npm\node_modules下，可以通过CMD指令查看:

~~~powershell
npm root -g
C:\Users\Alan\AppData\Roaming\npm\node_modules
~~~

但是有时候我们不想让全局包放在这里，我们可以自定义存放目录,在CMD窗口执行以下两条命令修改默认路径：

~~~powershell
npm config set prefix "d:\node-v12.18.4-win-x64\node_global"
npm config set cache "d:\node-v12.18.4-win-x64\node_cache"
~~~

或者打开c:\node\node_modules\npm\.npmrc文件，修改如下：

prefix =d:\node-v12.18.4-win-x64\node_global
cache = d:\node-v12.18.4-win-x64\node_cache

以上操作表示，修改全局包下载目录为d:\node-v12.18.4-win-x64\node_global,缓存目录为d:\node-v12.18.4-win-x64\node_cache,并会自动创建node_global目录，而node_cache目录是缓存目录，会在你下载全局包时自动创建。

#### 4. 配置环境变量：

1. “环境变量” -> “系统变量”：新建一个变量名为 “NODE_PATH”， 值为“d:\node-v12.18.4-win-x64\node_global\node_modules”。
2. “环境变量” -> “系统变量”：编辑用户变量里的Path，在末尾添加“d:\node-v12.18.4-win-x64\node_global”。

#### 5. 测试：

1. 在cmd命令下执行 npm install webpack -g 安装webpack：

   ~~~powershell
   npm install webpack -g
   npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
   npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
   npm WARN deprecated chokidar@2.1.8: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
   npm WARN deprecated fsevents@1.2.13: fsevents 1 will break on node v14+ and could be using insecure binaries. Upgrade to fsevents 2.
   d:\node-v12.18.4-win-x64\node_global\webpack -> d:\node-v12.18.4-win-x64\node_global\node_modules\webpack\bin\webpack.js
   npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.1.2 (node_modules\webpack\node_modules\chokidar\node_modules\fsevents):
   npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.1.3: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
   npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.2.7 (node_modules\webpack\node_modules\watchpack-chokidar2\node_modules\chokidar\node_modules\fsevents):
   npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.13: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
   
   + webpack@4.44.2
   added 345 packages from 201 contributors in 195.034s
   ~~~

2. 查看"D:\node-v12.18.4-win-x64\node_cache"和"D:\node-v12.18.4-win-x64\node_global"，会有缓存和安装完成后的文件。

3. 在cmd命令下执行 webpack -v 查看webpack版本（需要先全局安装webpack-cli）：

   ~~~powershell
   npm install webpack-cli -g
   npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
   npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
   d:\node-v12.18.4-win-x64\node_global\webpack-cli -> d:\node-v12.18.4-win-x64\node_global\node_modules\webpack-cli\bin\cli.js
   npm WARN webpack-cli@3.3.12 requires a peer of webpack@4.x.x but none is installed. You must install peer dependencies yourself.
   
   + webpack-cli@3.3.12
   added 185 packages from 127 contributors in 6.927s
   ~~~

   ~~~powershell
   webpack -v
   4.44.2
   ~~~


#### 6. npm下载速度慢，更换镜像源地址：

1. 查看当前镜像源：

   ~~~powershell
   npm get registry
   https://registry.npmjs.org/
   ~~~

2. 设置为国内淘宝镜像：

   ~~~powershell
   npm config set registry https://registry.npm.taobao.org
   ~~~


#### 7. OK,大功告成！！！！！













