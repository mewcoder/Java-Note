# 1.入门程序

```html
<body>
  
  <div id="app">
    {{ message }}
  </div>

  <!-- 开发环境版本，包含了有帮助的命令行警告 -->
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var app = new Vue({
      el:"#app",
      data:{
        message:" 你好 小黑! "
      }
    })
  </script>
</body>
```



el:挂载点：

- Vue会管理el选项命中的元素及其内部的后代元素
- 可以使用其他的选择器,但是建议使用ID选择器
- 可以使用其他的双标签,不能使用HTML和BODY

`v-text` 和 `v-html`：

- `v-text`和`{{}}`表达式渲染数据，不解析标签。

- `v-html`不仅可以渲染数据，而且可以解析标签。

`v-on`

- v-on指令的作用是:为元素绑定事件
- 指令可以简写为@
- 绑定的方法定义在methods属性中
-  方法内部通过this关键字可以访问定义在data中数据

```html
  <body>
    <div id="app">
      <input type="button" value="累加年龄" @click="addAge">
      <img v-show="isShow" src="./img/monkey.gif" alt="">
    </div>
    <!-- 1.开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
      var app = new Vue({
        el:"#app",
        data:{
          age:17
        },
        methods: {
           changeIsShow:function(){
            this.isShow = !this.isShow;
           },
           addAge:function(){
            this.age++;
          }
        },
      })
    </script>
  </body>
```

`v-show`

-  v-show 指令的作用是:根据真假切换元素的显示状
- 值为 true 元素显示,值为 false 元素隐藏
- 指令后面的内容,最终都会解析为布尔值



`v-if`

- v-if指令的作用是:根据表达式的真假切换元素的显示状态
- 本质是通过操纵 dom 元素来切换显示状态
- 表达式的值为 true,元素存在于 dom 树中,为false,从 dom 树中移除

