## `layui` 的使用



#### 表单数据的上传

```html
        <form class="layui-form" action="/user/login" method="POST">
            <div class="layui-form-item">

                <label for="username" class="layui-form-label layui-icon layui-icon-username" style="color:#009688"></label>
                <div class="layui-input-block">
                    <input id="username" class="layui-input" name="username">
                </div>
            </div>
            <div class="layui-form-item">
                <label for="password" class="layui-icon layui-icon-password layui-form-label" style="color:#009688"></label>
                <div class="layui-input-block">
                    <input id="password" class="layui-input" name="password">
                </div>
            </div>
        <button class="layui-btn" lay-submit lay-filter="login">提交</button>
        </form>
<!--lay-submit 的标签不能少-->
```

```js
   layui.use(["form"],function() { // lay-submit 标签不能少
        let form = layui.form
            ,layer = layui.layer;

        form.on('submit(login)', function(data){
            $.ajax({
                url: "/user/login",
                type: "POST",
                data: {data:JSON.stringify(data.field)},
                success: function (data) {
                    console.log(JSON.parse(data))
                },
                error: function () {
                    console.log("error")
                }
            });

            return false;
        });
    })

```

