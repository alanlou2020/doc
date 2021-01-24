## VSCode快速生成vue模版

### 1、插件库中搜索Vetur，点击安装，安装完成之后点击重新加载，即可使用。

### 2、新建代码片段：依次选择“文件 -> 首选项 -> 用户代码片段”，此时，会弹出一个搜索框，我们输入vue，选择vue后，VSCODE会自动打开一个名字为vue.json的文件，复制以下内容到这个文件中，如下：

~~~json
{   
    "Print to console": {
        "prefix": "vue",
        "body": [
            "<!-- $1 -->",
            "<template>",
            "  <div></div>",
            "</template>",
            "",
            "<script>",
            "  export default {",
            "    data() {",
            "      return {",
            "",
            "      }",
            "    },",
            "    //生命周期 - 创建完成（访问当前this实例）",
            "    created() {",
            "",
            "    },",
            "    //生命周期 - 挂载完成（访问DOM元素）",
            "    mounted() {",
            "",
            "    }",
            "  }",
            "</script>",
            "<style scoped>",
            "/* @import url(); 引入css类 */",
            "$4",
            "</style>"
        ],
        "description": "Log output to console"
    }
}
~~~

说明：$0 表示你希望生成代码后光标的位置 ; prefix 表示生成对应预设代码的命令（ 就是快捷键，vue名称可随意修改）。

### 3、新建vue文件，输入vue 按键盘的tab件生成vue模版。

