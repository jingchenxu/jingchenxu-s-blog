# 居中布局

> 居中布局通常会在web端的登录页中使用，使登陆框始终保持在页面中部。

````css
.login {
  width: 500px;
  height: 400px;
  margin: auto;
  position: absolute;
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
}
````

### 注意事项

有时候会在登录框的顶部添加系统的logo,对于此图片的定位方式为：

````css
.login-title {
  position: fixed;
  width: 500px;
  margin-left: -250px;
  margin-top: -80px; // logo的高度
}
````