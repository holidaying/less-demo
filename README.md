# less-demo
关于less的一些实践
##客户端安装情况
* 1.建立index.html
* 2.下载less.js
* 3.引入less.js
* 4.需要在服务器里面访问(此文是在nginx里面配置的地址)
```
 server {
        listen       9004;#less访问地址
        server_name  localhost;
        index index.html index.htm index.php;
        autoindex on;
        ssi  on;
        limit_rate 2000k;
        client_max_body_size 2048m;

        location ~* .*\.(html|htm|gif|jpg|jpeg|bmp|png|ico|txt|js|css|less|exe|map|json)$ {
            root E:/less; #ÐÞ¸ÄÎª±¾µØµÄ¾²Ì¬ÎÄ¼þ·¢²¼Ä¿Â¼
            index index.html index.htm;
        }

        location ^~ /manage/ {
            proxy_pass http://192.168.60.10:8181/manage/;
            proxy_redirect  default;
            proxy_cookie_path   /vdtcc/ /;
            proxy_set_header    Host    $host:$server_port;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-Host $host:$server_port;
            proxy_set_header    X-Forwarded-Server $host:$server_port;
            proxy_set_header    X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
    }
    ```
###基本框架
* 1.页面目录
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2604175-1f895de8f743e4ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 2.首页基本情况
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2604175-72cfaa6e5bffab88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 3.less语法设置body的颜色
```
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

body {
  color: @light-blue;
}
```
* 4.页面显示情况
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2604175-750aebba7fabf3a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

