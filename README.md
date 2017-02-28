# less-demo
关于less的一些实践(:red_car:[查看demo点这里](https://holidaying.github.io/less-demo/index.html))
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
### 基本框架
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

##项目中用到的基本语法
* 1.变量，请注意 LESS 中的变量为完全的 ‘常量’ ，所以只能定义一次。有助于项目公用一些颜色，字体大小，边距等，更有利于主题的制作。用@定义
```
@fargglad: #722872;
@morkfarg: #592059;
@morkgra: #888;
@fontColor:#666;
body {
    margin: 0;
    padding: 0;
    color: @fontColo;
    background: @morkgra;
    font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    font-size: 20px;
}
```
* 2.嵌套,我们可以在一个选择器中嵌套另一个选择器来实现继承，这样很大程度减少了代码量，并且代码看起来更加的清晰。与html标签相同的嵌套，逻辑更加清晰有助于阅读和样式污染。变量作用于明显。
```
#banner {
    background-color: @fargglad !important;
    height: 80px;
    box-shadow: 0px 2px 2px 1px rgba(0, 0, 0, 0.2);
    border-color: @morkfarg;
    .container {
        width: 95%;
        max-width: 1024px;
        justify-content: space-between;
        align-items: center;
        &::before,
        &::after {
            display: none
        }
    }
    img {
        height: 50px;
        margin: 15px 0;
    }
}
```
* 3.混合，变量允许我们单独定义一系列通用的样式，然后在需要的时候去调用。所以在做全局样式调整的时候我们可能只需要修改几行代码就可以了。
```
.border-radius(@radius) {
    border-radius: @radius;
    -moz-border-radius: @radius;
    -webkit-border-radius: @radius;
}
img {
            .border-radius(percentage(0.5));
            margin: 30px 0 10px;
            max-width: 80%;
            height: auto;
            width: 250px;
        }
```
* 4.&串联选择器，而不是写后代选择器，就可以用到&了. 这点对伪类尤其有用如 :hover 和 :focus.
```
.button {
    color: #fff;
    border: solid 2px #fff;
    .border-radius(percentage(0.5));
    display: inline-block;
    width: 50px;
    height: 50px;
    text-align: center;
    font-size: 20px;
    line-height: 48px;
    transition: all .3s ease-in-out;
    &:hover {
        border: solid 2px #fff;
        color: @fargglad;
        background: #fff;
    }
}
&就代表.button
```
* Math 函数（项目中很少用到）
LESS提供了一组方便的数学函数，你可以使用它们处理一些数字类型的值:
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`
如果你想将一个值转化为百分比，你可以使用percentage 函数:
percentage(0.5); // returns `50%`
命名空间
* Color 函数（有10个颜色处理的函数，LESS 提供了一系列的颜色运算函数. 颜色会先被转化成 HSL 色彩空间, 然后在通道级别操作。怎么计算的正在研究）
```
lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
darken(@color, 10%);      // return a color which is 10% *darker* than @color
saturate(@color, 10%);    // return a color 10% *more* saturated than @color
desaturate(@color, 10%);  // return a color 10% *less* saturated than @color
fadein(@color, 10%);      // return a color 10% *less* transparent than @color
fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
fade(@color, 50%);        // return @color with 50% transparency
spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
spin(@color, -10);        // return a color with a 10 degree smaller hue than @color
mix(@color1, @color2);    // return a mix of @color1 and @color2
```
  

