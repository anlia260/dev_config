# Nginx 简明配置

时间：`2017.7.21 16:00`

作者：`Akax Fed`



内容：

1. 需要nginx的场景
2. 简单介绍下nginx用法
3. 配置一个简单的web静态资源服务器

## 场景

1. 开发的时候需要代理第三方的服务去调试（联调）
2. SPA都是通过接口去挂载应用，当我们开发完成后单独部署一次模拟测试环境
3. 查看一些静态资源请求，模拟线上真实环境的压缩，请求，响应

## Nginx 的用法

* ```Nginx```反向代理发布多个域名80端口的WEB服务

``` javascript
server {
    listen       80;
        server_name  www.aaa.com;
        location / {
       　　proxy_pass   http://127.0.0.1:3000;
        }
　　　　location /static/ {
       　　proxy_pass   http://127.0.0.1:3000/static/;
　　　　}
}
server {
　　listen 80; 
　　server_name www.bbb.com; 
   location / {
    　　proxy_pass   http://127.0.0.1:4000;
    }
　　location /static/ {
    　　proxy_pass   http://127.0.0.1:4000/static/;
　　}
 }
```

> 配置了两个80的端口服务，绑定host之后，
>访问```www.aaa.com```相当于访问```3000```的本地服务，静态资源也是```3000```的资源
>访问```www.bbb.com```相当于访问```4000```的本地服务，静态资源也是```4000```的资源


* ```Nginx```绑定域名访问

``` javascript
server {
    listen       10086;
        server_name  www.aaa.com;
        location / {
       　　root   "F:/ST/project/kunyi/";
          index index.html;
        }
}
```

>访问```IP:10086```就会直接去访问项目了


## 配置一个简单的```web```服务器

>因为我们是```web```开发者，所有写出来的代码都会在```web放服务器```上运行



* 比如我们做了一个官网，想在本地部署一套环境给产品，设计观看预览
``` javascript
server {
        listen  80;  # 对外开放的端口号

        server_name  kunyi.stnts.com;  
 
        location / {
            
            # 通过域名或者端口号直接进来的一个目录
            root   "F:/ST/project/kunyi/";
            # 启用目录浏览 ，这个看需求（如果只想对外暴露index就不要用）
            autoindex on;   
        }

        # 配置静态资源的缓存时间
        location ~  .*\.(js|css|jpg)?$ 
        {
            expires 30d;
        }
}
```


* 我本地有一套开发环境，进入联调阶段了，现在我想代理到后端去做调试,当然我们也可以使用```webpack dev```去做代理调试，一般也是建议这么做，但是当我频繁切换代理服务，每次都去重启服务显然就不是很划算了。这个时候我们可以使用nginx代理去做了
``` javascript
server {
        listen  801;

        location /{
            proxy_pass http:# localhost:3001;
        }

        location ^~ /api/{
            proxy_pass http://192.168.35.153:8081;
        }
}
```
> 如果从ngixn做了代理的话，那么`webpack dev`的代理就不会起作用了

* 当我开发完成，想在本机或者是虚拟机上部署一个```web```服务
``` javascript
server {
        listen  1012;
        server_name  aa.com;

        # 指定项目root目录
        location /{
            root   "F:/AD/youtop-platform-ui/build/public";
            index index.html;
        }

        # 指定静态资源目录
        location  /front/ {
            alias    "F:/AD/youtop-platform-ui/build/public/";
        }

        # 反向代理配置
        location ^~ /api/{
            proxy_pass http://platform.youtop.auto;
        }
        # 反向代理配置
        location ^~ /service/{
            proxy_pass http://platform.youtop.auto;
        }
}
```




### 下载地址
[Nginx.zip](http://192.168.32.179:99/nginx/ "下载地址，包在我机器上如果下不了Q我") 
