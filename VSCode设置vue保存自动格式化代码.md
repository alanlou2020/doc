## VSCode设置vue保存自动格式化代码

### 1、安装VSCode eslint 插件。

### 2、依次选择“文件 -> 首选项 -> 设置”，搜索“setting.json”，点击“在setting.json中编辑”以打开setting.json文件。

### 3、拷入以下属性内容：

~~~json
// vscode默认启用了根据文件类型自动设置tabsize的选项
"editor.detectIndentation": false,
// 重新设定tabsize
"editor.tabSize": 4,
// #每次保存的时候自动格式化 
"editor.formatOnSave": true,
// #去掉代码结尾的分号 
"prettier.semi": false,
// #使用带引号替代双引号 
"prettier.singleQuote": true,
// #让函数(名)和后面的括号之间加个空格
"javascript.format.insertSpaceBeforeFunctionParenthesis": true,
// #这个按用户自身习惯选择 
"vetur.format.defaultFormatter.html": "js-beautify-html",
// #让vue中的js按编辑器自带的ts格式进行格式化 
"vetur.format.defaultFormatter.js": "vscode-typescript",
"vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
        "wrap_line_length": 120,
        "wrap_attributes": "auto"
        // #vue组件中html代码格式化样式
    }
},
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
},
"files.autoSave": "off",
"editor.suggestSelection": "first",
"vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
"diffEditor.ignoreTrimWhitespace": false,
"window.zoomLevel": 0,
"atomKeymap.promptV3Features": true,
"editor.multiCursorModifier": "ctrlCmd",
"guides.enabled": false,
"[json]": {

    "editor.quickSuggestions": {
        "strings": true
    },
    "editor.suggest.insertMode": "replace"
},
~~~

