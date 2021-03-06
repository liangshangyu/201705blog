# 初始化项目
```
npm init -y
```

# 安装依赖的模块
```
npm install express mongoose body-parser express-session connect-mongo connect-flash multer debug ejs bootstrap -S
```

# 划分模块
- 用户管理
- 文章分类管理
- 文章管理

# 编写路由
路由是项目的骨架，是项目的核心
## 首页路由
|路由名称|请求方式|路由路径|
|:----|:----|:----|
|显示首页|GET|/|

## 用户路由
|路由名称|请求方式|路由路径|
|:----|:----|:----|
|获取用户注册表单|GET|/user/signup|
|提交用户注册表单|POST|/user/signup|
|获取用户登录表单|GET|/user/signin|
|提交用户登录表单|POST|/user/signin|
|用户退出|GET|/user/signout|

### 注册功能

## 消息提示
- 1. 引入connect-flash中间件
- 2. 在session后面使用flash中间件
- 3. 在注册和登录成功或失败都要写入消息
- 4. 在中间件里把消息读出来赋给模板
- 5. 在模板中读取并显示消息内容

## 上传头像
1. 在注册表单增加avatar字段，注意`type=file` `name="avatar"`
2. 如果要上传文件的话，一定在form增加属性 `enctype="multipart/form-data"`
3. 在服务器使用 `multer` 中间件
4. 使用multer中间件
```
let multer = require('multer');
let upload = multer({dest:'./upload'});
upload.single('avatar')
```
5.修改model.给User增加一个字段
```
avatar:String    //增加一个头像的字段
```
6.在保存用户的时候增加一个avatar字段
```
user.avatar = `/${req.file.filename}`;
```
7.增加一个静态文件根目录
```
app.use(express.static(path.resolve('upload')));
```
8.在页面上显示头像
```
   .avatar{
            width:23px;
            height:23px;
            border-radius: 5px;
        }

 <%if(user){%>
            <ul class="nav navbar-nav navbar-right">
                <li>
                    <a href="#">
                        <img class="avatar" src="<%=user.avatar%>">
                    </a>
                </li>
                <li>
                    <a href="#">
                        <%=user.username%>
                    </a>
                </li>
            </ul>
            <% }%>
```

## 发表文章
1. 编写文章模板
2. 编写一个Article的Model
```
{
title:String,
content:String,
createAt:{type:Date,default:Date.now},
user:{type:ObjectId,ref:'User'}
}
ref表示此外键引用的是User集合的主键
3. 编写 POST /article/add 路由，在路由里先得到请求体 req.body,然后再加入作者字段，就可以保存到数据库里了。

```

## 在首页显示文章列表
1. 在路由里要先查询所有的文章，然后渲染首页。
2. 在首页模板里显示所有文章
