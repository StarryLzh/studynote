# Ajax

## 什么是Ajax

### Ajax的运行逻辑

1. 浏览器向服务器获取资源，浏览器渲染

2. B想获取更多的网页信息=>页面刷新，重新渲染

   1. 解决网页重新渲染问题
   2. 获取网页更多信息的需求通过操作页面的事件：滚动页面，点击页面按钮
      1. 事件通过JavaScript设置
      2. 触发事件，但是JavaScript没有网络通讯能力
         1. 通过浏览器实现网络通讯能力
         2. B的浏览器通讯规范`XMLHttpRequest` `(xhr)`，让JavaScript操作 xhr
         3. xhr是构造函数， 
            1. 创造新的实例对象
            2. 对象拥有自己的属性方法
      3. 即使用JavaScript来操控XHR以表示新的网页请求与服务器沟通
         1. XHR方法：
            1. `xhr.open(请求方法，url，是否异步)` :设置参数
            2. `ready State属性`
               1. `0`未调用`xhr.open`方法
               2. `1`调用了`xhr.open()`方法，但未调用`xhr.send()`方法
               3. `2`表示发送请求，还没收到请求
               4. `3`表示收到了部分响应
               5. `4`表示收到全部响应，触发`xhr.readyStatechange`事件
            3.  `xhr.onreadyStatechange`来监听`xhr.readystate`
            4. `xhr.send(请求体)` ：发送请求

   ## 如何创建Ajax

   ![创建过程](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20221208164416729.png)

```html
<!DOCTYPE html>
<html>
   <head>
       <link rel="stylesheet" type="text/css" href="index.css">
       <script src="index.js"></script>
    </head> 
    <body>
        <p>
            
        </p>
    </body>
</html>
```

js部分

```javascript
const p = document.querySelector('p');

//新建浏览器请求对象
let xhr = new XMLHttpRequest();
//设置请求
xhr.open('get','test.txt',true);
//监控readystate属性
xhr.onreadystatechange = () => {
    //接受到所有响应
    if(xhr.readyState === 4){
        //判断文件是否为正常接受
        if(xhr.status === 200){
            p.innerHTML = xhr.responseText;
        }
    }
};
//发送请求
//因为没有请求体，所以为null
xhr.send(null);
```

