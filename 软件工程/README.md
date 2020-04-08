### 在django中增加默认数据库的属性

```python
# 进行一对一连接 CASCADE表示删除user，AuthUser中对应的也删除
user = models.OneToOneField(AuthUser, on_delete=models.CASCADE)
# 也可以进行外键连接，实现一对多，多对一
models.ForeignKey
```



### django中使用软连接

```python
# 首先需要在bbs/urls.py中配置路由的时候声明 name
path("", include("home.urls"), name='home'),  # 这一步不需要！声明为name是多余的，可以声明namespace
path("", include("home.urls",namespace='home-index'))  # 这里的是实列命名空间
# 然后在对应的app下urls下声明 app_name 作为应用程序命名空间
app_name = 'home'
path("", home, name='index'),
# 在模板中调用的时候，使用
{% url 'home:index' %}
# 这样就可以访问对应的链接了
```

```
应用程序命名空间
这描述了正在部署的程序名。单个应用的每个实例拥有相同的命名空间。比如，Django admin 应用有可预测的应用命名空间 'admin' 。

实例命名空间
这标识了应用程序的特定实例。实例命名空间应该是完整项目唯一的。但是实例命名空间可以和应用命名空间相同。这常用来指定应用的默认实例。比如，默认Django admin 实例拥有名为 'admin' 的实例命名空间。
被指定的命名空间 URL 使用 ':' 操作符。比如，使用 'admin:index' 引用admin 应用的首页。这表明命名空间为 'admin' ，命名 URL 为``'index'`` 。

命名空间也可以嵌套。命名 URL 'sports:polls:index' 将在命名空间 'polls' 中寻找命名为 'index' 的模式，该模式是在顶层命名空间 'sports' 中定义的。
```



### 扩展实现的数据库如何访问原数据库中的属性？

```python
class User(models.Model):
    """用户数据库"""
    user = models.OneToOneField(AuthUser, on_delete=models.CASCADE) 
    
## 访问的时候
User.objects.get(user__username) # 获取AuthUser中的用户名
```





### django POST数据中遇到特殊符号的时候后台显示不出来？

```javascript
// POST数据的时候进行加密
encodeURIComponent(value.trim())
```





### django模型中存在外键，应该如何赋值

首先应该先创建一个外键的内容的模型

然后再赋值给这个使用了外键的模型的对应属性





### django中对url中next参数进行配置，使得网站更加人性化

前端
```html
<input type="hidden" value="{{ request.GET.next }}" name="next">  <!--获取当前访问页中的next信息-->
```

后端
```python
if request.POST.get("next"):
    return redirect(request.POST.get("next"))
```





### 设置Media 目录

```python
# settings.py
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, "media/")
# urls.py
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```





### 使用Redis

```python
# settings.py
REDIS_HOST = "localhost"
REDIS_PORT = 6379
REDIS_DB = 0
# views.py
import redis
from django.conf import settings
r = redis.StrictRedis(host=settings.REDIS_HOST, port=settings.REDIS_PORT, db=settings.REDIS_DB)

# 每次访问的时候访问次数加一
view = r.inc(name)

```

### 如果要对文章浏览次数进行排序

```python
r.zincrby("article_ranking", 1, article.id)
article_ranking = r.zrange("article_ranking", 0, -1, desc=True)[:10]  # 比如选择前十个

```





### 自定义templatetags

[官方文档]( https://docs.djangoproject.com/en/3.0/howto/custom-template-tags/ )

```python
# 在某个app下新建一个templatetags 文件夹
# 需要确定在settings.py 中对该app 进行了注册
# 在templatetags中建立一个.py文件
from django import template

register = template.Library()
from ..models import Virus


# 自定义标签
@register.simple_tag(name='virus')
def virus():
    return Virus.objects.count()

# 自定义过滤函数
@register.filter
def virus(id):
    return Virus.objects.get(id=id).name


# .py 文件夹是{%load xxx %} 时候需要使用的对象
# name 对应是使用的时候写的对象 {% xxx %}
# 可以不指定name，函数名即为默认的name

```

**定义之后需要重启服务器，django不会自动重新加载这些**



### Python int too large to convert to C long

这是一次在进行django中数据库操作的时候遇到的问题，主要是因为`models.DateField` 支持的精确度为日期，但是似乎在pycharm中插入数据的时候使用 的精度超过了这个精度(插入的时候用时间戳)

为了避免这个错误，使用 `models.DateTimeField`





### 访问数据库中的DateTimeField 类型的属性的时候一直返回None

<p style='color: red'>使用pycharm添加数据的时候存在该问题，如果使用django前端的管理界面进行添加可以避免这个问题，另外如果是用户在前端进行操作也是没有问题的</p>


### django中删除migrate建立的表之后再次执行相应的操作的时候无法生成对应的表

- 删除对应app下migrations下除了`__init__.py` 文件外的所有文件
- 删除django_migrations中的`对应`的迁移信息(删除之前drop的那张表)
- 执行python manage.py makemigrations
- 执行python manage.py migrate
- 如果报错说某个表已经建立，可以在生成的那个`...initial.py` 中对其余的表进行删除
- 迁移成功之后最好可以将initial.py中的信息进行还原



### 模型中进行多个条件的过滤

```python
PostReply.objects.filter(user_id=i.id, post_id=id) 
```



### 在redirect 的时候如何传递参数，来提示用户信息

可以使用`session`

```python
# redirect 之前在session中设置对应的信息
# 在访问的时候取出这个信息，并将其从session删除 使用pop即可

# 在 view中，成功完成某事后
request.session["message"] = message 
# 在每次访问这个网站的时候进行判断
if request.session.pop("message"):
    do something
```





### radio设置的时候可以两个按钮都选上？？

单选的按钮需要指定同一个`name`





### 有两个css文件，中间有些内容是冲突的，但是有时又要用到其中的一些样式，但是之前的样式保持不变？

- 首先，第一种方式肯定就是将某些需要css样式抽取出来，再创建一个自定义的css，然后引入
- 第二种方式，试了一下，可以将主要的css样式放在后面一些引入，后面的样式能够覆盖前面的样式





### git修改上一次commit的记录中的内容

```
git commit --amend
```





### 如何在弹窗中进行文件的上传?

使用`layui`组件设置的时候，按照下面的方式！！！(花了好几个小时)

- 界面中有个这样的按钮，显示隐藏，之后再弹窗中显示

```html
<div id="file_upload_div" style="display: none" class="text-center">
    <div class="layui-upload-drag" id="test10">
        <i class="layui-icon"></i>
        <p>点击上传，或将文件拖拽到此处</p>
    </div>
    <div class="mt-2 mb-2" id="file_name"></div>
    <div class="layui-input-inline " style="width: 100%;">
        <div class="layui-input-inline">
            <label for="type" class="layui-form-label" style="width: 100px">实验类型: </label></div>
        <form action="." method="POST">
            {% csrf_token %}
            <input value="hello" name="test" id="test">
            <select id="type" class="layui-select" name="type">
            <option selected value="物理实验">物理实验</option>
            <option value="化学实验">化学实验</option>
            <option value="基电实验">基电实验</option>
            <option value="电子技术实验">电子技术实验</option>
            <option value="其他实验">其他实验</option>
        </select>

        </form>
    </div>
                   <button type="submit" class="layui-btn mt-2" id="upload" ></button>

    <div class="row w-100"></div>
</div>

```

- 有一个事件来触发弹窗

```html
    <button onclick="_open()">click me</button>  <!--可以使用其他的事件，一样-->
```

- 弹窗的事件,请确保导入`layui.js 和layer.css`

```js
   function uploadFile() {
        layui.use('layer', function () {
            var layer = layui.layer;

            layer.open({
                type: 1,
                area: ["500px", "360px"],
                title: "上传文件",
                content: $("#file_upload_div")
            })
        });

    }
```

- 文件的上传使用`layui`组件, 由于这里是`POST`请求，需要`csrftoken`，否则`forbidden`

```js
        var uploadInst = upload.render({
            elem: '#test10' //绑定元素
            , headers: {
                "X-CSRFToken": csrf
            }
            , auto: false
            , bindAction: "#upload"
            , url: '{% url "course:upload" %}' //上传接口
            , size: 100*1024 // 最大值
            , before:function () {  // 传入表单数据
                this.data = {
                    test: $('input[name="test"]').val()
                }
            }
            , choose: function (obj) {
                obj.preview(function (index, file, result) {
                    document.getElementById("file_name").innerHTML = file.name;
                })
            }
            , done: function (res) {
                console.log("fdsa")
                document.getElementsByClassName("layui-layer-close")[0].click()
                layer.msg("上传成功")
            }
            , error: function () {
                layer.msg("上传失败，请重新上传")
            }
        });

```

