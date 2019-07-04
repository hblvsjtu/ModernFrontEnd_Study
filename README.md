# ModernFrontEnd_Study


## 作者：冰红茶  
## 参考书籍：《现代前端技术解析》 张成文
    
------    
    
我觉得前端的学习需要一本系统的书籍，讲授整个前端发展的历程、技术的革新、知识体系和架构思维体系。可惜的是这样的书不多，大多数都是以某某技术栈为主。好不容易找到一本这样的书，后面就认真学习学习吧^_ ^

## 目录
## [一、Web前端技术基础](#1)
### [1.1 发展概述](#1.1)
### [1.2 浏览器应用基础](#1.2)
### [1.3 浏览器渲染引擎介绍](#1.3)
### [1.4 浏览器数据持久化存储技术](#1.4)
## [二、前端与协议](#2)
### [2.1 HTTP协议简介](#2.1)
### [2.2 HTTP版本](#2.2)
### [2.3 HTTP2.0](#2.3)
### [2.4 Web安全机制](#2.4)
### [2.5 HTTPS协议通讯过程](#2.5)
### [2.6 前端实时协议](#2.6)
### [2.7 RESTful数据协议规范](#2.7)
### [2.8 与Native交互协议](#2.8)
## [三、前端三层结构与应用](#3)
### [3.1 结构层HTML、表现层CSS和行为层JavaScript](#3.1)
### [3.2 前端结构层演进](#3.2)
### [3.3 浏览器脚本演进历史](#3.3)
### [3.4 前端表现层基础](#3.4)
### [3.5 响应式网站开发技术](#3.5)
## [四、现代前端交互框架](#4)
### [4.1 直接DOM操作时代](#4.1)
### [4.2 MV*交互模式](#4.2)
### [4.3 数据变更检测](#4.3)
### [4.4 Virtual DOM交互模式](#4.4)
## [五、前端项目与技术实践](#5)
### [5.1 前端开发规范](#5.1)
### [5.2 自动化构建](#5.2)
### [5.3 前端性能优化](#5.3)
### [4.4 Virtual DOM交互模式](#4.4)

        
        
------      
        
<h2 id='1'>一、Web前端技术基础</h2>
<h3 id='1.1'>1.1 发展概述</h3>  
        
#### 1) 大网页需要考虑的问题
> - 网页数据内容多的时候代码如何实现
> - 请求数目大的时候怎么做到快速加载
> - 图片请求数多的时候内容体积怎样做到尽可能的小
> - 网页体积大的时候怎么在无缓存的情况下500ms内展示给用户
> - 用户操作种类繁琐的时候怎么去管理
#### 2) 解决问题的思路
> - 利用开发框架对DOM操作做高效的封装
> - 运行模块化和组件化的思路去管理复杂的代码
> - 图片使用高压缩比的webp格式，在图片质量不降低的情况下，可以大幅度减小图片的网络流量消耗，提高图片的加载速度
                
                <img src="pic.webp" alt="更高压缩比的图片">
> - 如果加载的内容过多，使用异步的方式，将一部分的内容优先展示给用户
> - 对于重复打开的网页，合理使用缓存
#### 3) 移动端的局限性
> - 移动端 相比桌面端请求数目更小，总内容也更小，但是在无缓存的情况下展现的时间大约是1000ms,那问题来了：为什么移动端页面加载的时间反而变多了呢？
> - 主要原因在于移动端的计算资源和网络资源比价局限：
>> - 移动端设备的CPU处理器速度比较慢，且网速也相对比较低
>> - 屏幕大小受限，一次只能展现的内容有限
#### 4) Web Component
> - Web Components是一个浏览器的新功能，可以认为Web Components是一个可复用的用户接口部件，属于浏览器的一部分，所以不需要一些额外的例如jQuery或者Dojo之类的工具库。
一个存在的Web Components的使用完全不需要写代码，仅仅需要在HTML中加一个import 语句就可以了。
> - 但经过我的尝试，发现import这种方式已经过时了
> -  HTML Imports is deprecated and will be removed in M73, around March 2019. Please use ES modules instead. See https://www.chromestatus.com/features/5144752345317376 for more details.
> -  x-component.html:13 [Deprecation] document.registerElement is deprecated and will be removed in M73, around March 2019. Please use window.customElements.define instead. See https://www.chromestatus.com/features/4642138092470272 for more details.
                
            <!DOCTYPE>
            <html>
                <head>
                   <title>webcomponent</title>
                   <link rel="import" href="./components/helloword.html" />
                </head>
                <body>
                    <hellow-world></hellow-world>
                </body>
            </html>

            <template>
                <style>
                    .coloured {
                        color: red;
                    }
                </style>
                <p>the first webcompnent is  <strong class="coloured">Hello World</strong></p>
            </template>
            <script>
                (function() {
                    // Creates an object based in the HTML Element prototype
                    // 基于HTML Element prototype 创建obj
                    var element = Object.create(HTMLElement.prototype);
                    // 获取template的内容
                    var template = document.currentScript.ownerDocument.querySelector('template').content;
                    // element创建完成之后的回调
                    element.createdCallback = function() {
                        // 创建 shadow root
                        var shadowRoot = this.createShadowRoot();
                        // 向root中加入模板
                        var clone = document.importNode(template, true);
                        shadowRoot.appendChild(clone);
                    };
                    document.registerElement('hellow-world', {
                        prototype: element
                    });
                }());
            </script>
>>>>>> ![图1-1 前端应用开发模式演变.png](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-1%20%E5%89%8D%E7%AB%AF%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E6%A8%A1%E5%BC%8F%E6%BC%94%E5%8F%98.png?raw=true)
                

<h3 id='1.2'>1.2 浏览器应用基础</h3>  
        
#### 1) 从输入一个网址到展示网页内容的整一个过程
> - 用户在网页中输入一个网址，浏览器开启一个线程来处理这个请求：对用户的URL地址进行分析判断。
> - 调用浏览器引擎中对应的方法，比如WebView中loadUrl方法，分析并判断在这个地址
> - 通过DNS解析获取该网站地址的对应的IP地址，查询完成后连同浏览器的Cookies、userAgent等信息向网站目的IP发出GET请求
> - 进行HTTP协议会话，浏览器客户端向Web服务器发送报文
> - Web服务器处理请求，比如Apache、Tomcat、Node.js
> - 服务器返回响应报文（缓存304无缓存200）
> - 200浏览器开始下载HTML文档/304则从本地缓存读取文件内容
> - 浏览器根据接收的HTML文档解析为DOM文档树，并根据HTML中毒的标记请求下载制定的MIME类型文件（如CSS、JS等），同时设置缓存等内容
> - 页面开始解析渲染DOM，展示页面
#### 2) 浏览器组成结构
> - 用户界面、浏览器引擎、渲染引擎、网络、JS引擎、后端UI、数据持久化存储
>>>>>> ![图1-2 浏览器组成结构.png](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-2%20%E6%B5%8F%E8%A7%88%E5%99%A8%E7%BB%84%E6%88%90%E7%BB%93%E6%9E%84.png?raw=true)
#### 3) 内核
> - Trident内核，如IE、360、搜狗
> - Gecko内核，Fixfox
> - Presto内核，Opera7
> - Webkit内核，Safari、Chrome
                
<h3 id='1.3'>1.3 浏览器渲染引擎介绍</h3>  
        
#### 1) 渲染引擎的工作流程
> - 解析HTML构建DOM树 将HTML元素解析为多DOM元素对象，并形成个具有父子关系的DOM树结构
> - 构建渲染树 根据CSS计算生成带有样式描述的DOM渲染树结构
> - 渲染树布局阶段（重排） 元素的布局属性生效，将元素的大小和位置固定到对应位置上
> - 绘制渲染树（重绘） 元素的内部显示样式生效，将渲染树节点的背景、颜色、大小、文本等样式信息应用到节点上，最终生成整个DOM显示
> - 其他
>> - 重排 如果元素的位置或者大小发生变化，那么就会发生重排，而且重排一定会导致后面的重绘，代价最大
>> - 重绘 只出现DOM节点的显示样式改变
#### 2) 浏览器渲染工作流程
> - 渲染引擎对DOM渲染树的解析和输出是逐行进行的，这样的话在前面的可以优先展示，保证了较好的用户体验
> - 尽量不要在HTML显示内容中插入script脚本避免由于script标签内容的解释执行阻塞页面结构的渲染。
> - Webkit和Gecko内核主要的不同
>> - Webkit中html和CSS的解析可以并行进行，而Gecko先进行html解析，在进行CSS解析
>> - 相关术语不同，Webkit的渲染对象成为Render Tree渲染树，Gecko的称为Frame Tree
>>>>>> ![图1-3 Webkit内核渲染DOM流程.png](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-3%20Webkit%E5%86%85%E6%A0%B8%E6%B8%B2%E6%9F%93DOM%E6%B5%81%E7%A8%8B.png?raw=true)
>>>>>> ![图1-4 Gecko内核渲染DOM流程.png](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-4%20Gecko%E5%86%85%E6%A0%B8%E6%B8%B2%E6%9F%93DOM%E6%B5%81%E7%A8%8B.png?raw=true)
#### 3) HTML解析
> - DOM元素标签和DOM元素对象虽然都是用来描述DOM结构的，但是DOM元素标签是指文本话的HTML标识，而DOM元素对象则是指经过渲染引擎DOM解析后生成的具有节点父子关系的属性对象
>>>>>> ![图1-5 DOM结构解析示意图.png](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-5%20DOM%E7%BB%93%E6%9E%84%E8%A7%A3%E6%9E%90%E7%A4%BA%E6%84%8F%E5%9B%BE.png?raw=true) 
#### ４) CSS解析
> - CSS解析通过权重方式进行计算，！important > 内联样式规则（权重1000） > id选择器（权重100）> 类选择器（权重10）> 元素选择器（权重1）
>>>>>> ![图1-6 CSS结构解析.png](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-6%20CSS%E7%BB%93%E6%9E%84%E8%A7%A3%E6%9E%90.png?raw=true) 
                
<h3 id='1.4'>1.4 浏览器数据持久化存储技术</h3>  
        
#### 1) 现代浏览器8种缓存
> - HTTP文件缓存
> - LocalStorage
> - SessionStorage
> - indexDB
> - Web SQL
> - Cookie
> - CacheStorage
> - Application Cache
#### 2) HTTP文件缓存判断流程
> - Cache Control跟Expires类似，但是Expires上的过期时间是绝对的，而Cache Control上是相对的，相对时间比较好，因为绝对时间在各个设备上可能会不一致
> - 如果Etag和Last-Modified都不存在，直接向服务器请求内容
> - 如果同时存在Expires和Cache-Control，则只有Cache-Control的设置生效
                
                <meta http-equiv="Expires" content="Mon 20 Jul 2016 23:00:00 GMT"/>
                <meta http-equiv="Cache-Control" content="max-age=7200"/>
>>>>>> ![图1-7 HTTP缓存判断流程](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-7%20HTTP%E7%BC%93%E5%AD%98%E5%88%A4%E6%96%AD%E6%B5%81%E7%A8%8B.png?raw=true) 
#### 3) localStorage
> - localStorage主要用于浏览器端保存体积较大的数据，localStorage在不同的浏览器中有长度限制各不相同：IE8以上5MB，Fixfox8以上5.24MB, Opera2MB, Chrome和Safari2.6MB
                
                localStorage.setItem(key, value);
                localStorage.getItem(key);
                localStorage.removeItem(key);
                localStorage.clear();
> - 尽管单一域名下localStorage的大小是有限制，但是可以使用iframe的方式使用多个域名来突破单个页面下localStorage存储数据的最大限制。另外使用浏览器的u东哥标签页打开同一个域名页面时，localStorage的内容一般是共享
#### 4) sessionStorage
> - sessionStorage和localStorage的功能类似，但是sessionStorage在浏览器关闭时就会自动清空。sessionStorage和localStorage的API完全相同
> - 不同浏览器无法共享localStorage或sessionStorage中的信息。相同浏览器的不同页面间可以共享相同的 localStorage（页面属于相同域名和端口），但是不同页面或标签页间无法共享sessionStorage的信息。这里需要注意的是，页面及标 签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享sessionStorage的。
#### 5) Cookie
> - HTTP Cookie，通常直接叫做cookie，最初是在客户端用于存储回话信息的。该标准要求服务器对任意HTTP请求发送Set-Cookie HTTP头作为响应的一部分，其中包含回话信息。浏览器会存储这样的回话信息，并在这之后，通过每个请求添加Cookie HTTP头将信息发回服务器。 [swpu_lwf的CSDN](https://blog.csdn.net/liwenfei123/article/details/80027155)
> - 最大长度限制在4KB左右
> - 使用document.cookie进行修改
> - 三种类型
>> - Session Cookie 一般不设置过期时间，生命周期只存在于浏览器会话期间，只要关闭浏览器窗口，Cookie就会消失，而且Session Cookie不保存在硬盘上而保存在内存上
>> - 持久型Cookie 一般会设置过期时间
>> - HttpOnly 前端浏览器无法读取，只能通过Http请求头发送到服务器端进行读写操作，这样可以避免服务器的Cookie记录被前端JavaScript修改，保证了服务器验证Cookie的安全性
> - 
                
                //代码来源于JavaScript高级程序设计
                var CookieUtil={
                    get:function(name){
                        var cookieName=encodeURIComponent(name)+"=",
                            cookieStart=document.cookie.indexOf(cookieName), //获取cookie的起始index
                            cookieValue=null;
                        if(cookieStart>-1){
                            var cookieEnd=document.cookie.indexOf(";",cookieStart); //c_end = document.cookie.indexof(";", c_start)是从上一步的位置开始查找的，直到";"的位置。
                            if(cookieEnd==-1){
                                cookieEnd=document.cookie.length;
                            }
                            cookieValue=decodeURIComponent(document.cookie.substring(cookieStart+cookieName.length,cookieEnd));
                        }
                        return cookieValue;
                    },
                    set:function(name,value,expires,path,domain,secure){//cookie的构成：名称、值、失效时间（何时应停止向浏览器发送cookie）、路径（向服务器发送cookie的特定域的路径）、域（cookie对于哪个域是有效的）、安全标志（指定后只能在使用SSL连接时才发送到服务器）
                        var cookieText=encodeURIComponent(name) + "=" +encodeURIComponent(value);
                        if(expires instanceof Date){
                            cookieText += "; expires="+expires.toGMTString();//时间为GMT格式，注意信息之间用“; ”分割
                        }
                        if(path){
                            cookieText += "; path="+path;
                        }
                        if(domain){
                            cookieText += "; domain="+domain;
                        }
                        if(secure){
                            cookieText += "; secure";
                        }

                        document.cookie = cookieText;
                    },
                    unset:function(name,path,domain,secure){//没有直接删除cookie的方法
                        this.set(name,"",new Date(0),path,domain,secure);//使用相同路径、域、安全选项再次设置cookie，并将失效时间设置为过去的时间
                    }
                };

                // 分组法一
                document.cookie.match(/[^;]+=[^;]+?(?=;|\b)/g);

                // 分组法二
                document.cookie.split(';');
                
#### 6) Web SQL
> - 非HTML5里的规范，只有较新的Chrome浏览器才能用，所以使用不多
#### 7) IndexDB
> - 大小限制为50MB
> - 目前使用IndexDB的实际应用场景不是很多，而且将大量数据保存到本地也会造成数据泄露
> - 一般了解即可无需在项目中使用
#### 8) Application Cache离线应用
> - 通过manifest配置文件在本地有选择性地存储JS、CSS、图片等静态资源的文件级缓存机制
> - 页面打开时优先从Application Cache中访问资源，读取资源加载后同时检查服务器端的manifest文件是否已更新，如果没有更新则访问过程结束，如果有更新，则会把更新的内容重新拉渠道Application Cache中
> - 更新内容后需要再一次访问后才会生效，而不是马上能生效，所以至少要第三次才能看到更新后的结果
> - 离线存储的限制为5MB，现在来说显然不适用
> - 仍然不能兼容目前主流的浏览器环境
> - 如果manifest文件或者内部列表中的某个文件不能正常下载，整个更新过程将视为失败，浏览器将继续使用就的缓存。
                
                VERSION 1.0
                CACHE:
                main-abs931pd.css
                main-file.js
> - 总之Application Cache离线应用仍然不是一个成熟的本地缓存解决方案，实际项目中也不推荐使用。
                
                let appCache = window.applicationCache //查看Application Cache
                appCache.update(); //尝试更新用户的Application Cache
                if(appCache.status == window.applicationCache.UPDATEREADY) {
                    appCache.swapCache(); //置换新的Application Cache
                }
>>>>>> ![图1-7 HTTP缓存判断流程](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-7%20HTTP%E7%BC%93%E5%AD%98%E5%88%A4%E6%96%AD%E6%B5%81%E7%A8%8B.png?raw=true) 
#### 8) cacheStorage
> - 
                
                if(navigator.serviceWorker)
                    console.log('navigator.serviceWorker exists');
                else
                    console.log('navigator.serviceWorker no exists');


        
        
------      
        
<h2 id='2'>二、前端与协议</h2>
<h3 id='2.1'>2.1 Http协议概述</h3> 
<h3 id='2.2'>2.2 HTTP版本</h3> 
        
#### 1) 组成部分
> - 头部 请求类型+请求URL+协议版本+拓展内容
> - 空行 用于隔开头部和正文
> - 正文
>>>>>> ![图1-7 HTTP缓存判断流程](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE1-7%20HTTP%E7%BC%93%E5%AD%98%E5%88%A4%E6%96%AD%E6%B5%81%E7%A8%8B.png?raw=true) 
#### 2) HTTP1.1
> - 长连接 保证客户端和服务器端在一段时间内保持连接
> - 默认包含请求头的keep-alive
> - HTTP1.0则需要手动添加Connection: keep-alive
> - 协议拓展切换：含有Upgrade头支持协议切换，如浏览器发送切换协议的请求切换到WebSocket协议
                
                Connection:Upgrade
                Upgrade: WebSocket
#### 3) 报文的头部信息
> - Accept:告诉Web服务器自己能接收的媒体类型
> - Accept-Charset: 浏览器接收内容的字符集，通常是udf-8
> - Accept-Encoding: 浏览器节后内容的编码方式，如支持什么压缩方式
> - Accept-Language: 浏览器
>>>>>> ![图2-1](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE2-1.jpg?raw=true) 
                    
<h3 id='2.3'>2.3 HTTP2.0</h3> 
<h3 id='2.4'>2.4 Web安全机制</h3> 
                
>>>>>> ![图2-2](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE2-2.jpg?raw=true) 
>>>>>> ![图2-3](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE2-3.jpg?raw=true) 
                
<h3 id='2.5'>2.5 HTTPS协议通讯过程</h3> 
<h3 id='2.6'>2.6 前端实时协议</h3> 
                
>>>>>> ![图2-4](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE2-4.jpg?raw=true) 
                
<h3 id='2.7'>2.7 RESTful数据协议规范</h3> 
<h3 id='2.8'>2.8 与Native交互协议</h3> 
                
>>>>>> ![图2-5](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE2-5.jpg?raw=true) 

        
        
------      
        
<h2 id='3'>三、前端三层结构与应用</h2>
<h3 id='3.1'>3.1 结构层HTML、表现层CSS和行为层JavaScript</h3>  
        
>>>>>> ![图3-1](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-1.jpg?raw=true) 
                
<h3 id='3.2'>3.2 前端结构层演进</h3> 
                
>>>>>> ![图3-2](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-2.jpg?raw=true) 
            
<h3 id='3.3'>3.3 浏览器脚本演进历史</h3> 
                
>>>>>> ![图3-3](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-3.jpg?raw=true) 
>>>>>> ![图3-4](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-4.jpg?raw=true) 
>>>>>> ![图3-5](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-5.jpg?raw=true) 
                
<h3 id='3.4'>3.4 前端表现层基础</h3> 
<h3 id='3.5'>3.5 响应式网站开发技术</h3> 
                
>>>>>> ![图3-6](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-6.jpg?raw=true) 
>>>>>> ![图3-7](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-7.jpg?raw=true) 
>>>>>> ![图3-8](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE3-8.jpg?raw=true) 

        
        
------      
        
<h2 id='4'>四、现代前端交互框架</h2>
<h3 id='4.1'>4.1 直接DOM操作时代</h3>  
        
>>>>>> ![图4-1](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE4-1.jpg?raw=true) 
                
<h3 id='4.2'>4.2 MV*交互模式</h3>  
        
>>>>>> ![图4-2](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE4-2.jpg?raw=true) 
                
<h3 id='4.3'>4.3 数据变更检测</h3> 
<h3 id='4.4'>4.4 Virtual DOM交互模式</h3>
        
>>>>>> ![图4-3](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE4-3.jpg?raw=true) 
>>>>>> ![图4-4](https://github.com/hblvsjtu/ModernFrontEnd_Study/blob/master/picture/%E5%9B%BE4-4.jpg?raw=true) 

        
        
------      
        
<h2 id='5'>五、现代前端交互框架</h2>
<h3 id='5.1'>5.1 前端开发规范</h3>  
        
#### 1) 前端通用规范
> - 内容编码
>> - HTML 文档中使用
                
                <meta charset="utf-8">
>> - 不需要为CSS显示定义编码。其默认为utf-8
                
                @charset "utf-8"
>> - 
> - 小写 所有的HTML标签，HTML标签属性，样式名及规则建议使用小写，而id值可以使用驼峰式命名
#### 2) 前端HTML规范
> - head内容  必须要有title、keyword、description，保证基本的SEO页面关键字和内容描述，移动端还要手动添加viewport控制页面不缩放，有利于提高页面渲染的性能。建议在添加一些社交RICH化的消息，保证网页地址分享到主流社交平台显示页面的缩略图、标题和描述
                
                <meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no"/>
                <meta itemprop="name" content="页面标题">
                <meta name="description" itemprop="description" content="页面内容描述">
                <meta itemprop="image" content="https://cn.vuejs.org/images/logo.png">
> - 使用label的for属性扩大点选区
> - 简写方式 单位0的缩写后面不需要带单位，以0为个位数的小数前面的0可以省略不写
> - Hack写法 从全面到局部 针对IE可以使用条件注释作为预留Hack使用
                
                ul {
                    color: #000; /* For all */
                    color: #111\9; /* For all IE*/
                    color: #222\0; /* For IE 8 and Later, Opera widthout webkit */
                    color: #333\9\0; /* For IE 9 and Later */
                }

                <!--[if <keywords>? IE <version>]?>
                <link rel="stylesheet" href="./hack.css" />
                <!-- [endif]-->

                <!--[if lt IE 9]>
                <script src="/assets/js/html5shiv.min.js"></script>
                <script src="/assets/js/respond.min.js"></script>
                <script src="/assets/js/jquery-1.9.0.min.js"></script>
                <![endif]-->

                <!--[if gte IE 9]>
                <script src="/assets/js/jquery-2.1.4.min.js" type="text/javascript"></script>
                <![endif]-->

                <!--[if !IE]> <-->
                <script src="/assets/js/jquery-3.2.0.min.js" type="text/javascript"></script>
                <!--> <![endif]-->
> - 优雅降级和渐进增强 [cc_ccc博客](https://www.cnblogs.com/cxying93/p/6042851.html)
>> - 渐进增强 progressive enhancement：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
>> - 优雅降级 graceful degradation：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。
>> - 建议使用渐进增强
                
                .transition { /*渐进增强写法*/
                  -webkit-transition: all .5s;
                     -moz-transition: all .5s;
                       -o-transition: all .5s;
                          transition: all .5s;
                }

                .transition { /*优雅降级写法*/
                          transition: all .5s;
                       -o-transition: all .5s;
                     -moz-transition: all .5s;
                  -webkit-transition: all .5s;
                }
> - 模块多变量的导入导出尽量使用对象解构
                
                // 不推荐
                import * as util from './lib/util';

                // 推荐
                import { time }  util from './lib/util';

                // 不推荐
                export default { time } from './lib/util';

                // 推荐
                import { time }  from './lib/util';
                export default time
> - 生成器yield进行异步操作的时候需要使用try...catch包裹
                
                const generator = function*() {
                    try {
                        yield return 1;
                    }catch(e) {
                        console.log(e);
                    }
                    try {
                        yield return 2;
                    }catch(e) {
                        console.log(e);
                    }
                    try {
                        yield return 3;
                    }catch(e) {
                        console.log(e);
                    }
                    try {
                        yield return 4;
                    }catch(e) {
                        console.log(e);
                    }
                }

                let g = generator();
                g.next();
#### 3) 模块化规范
> - JS文件之间相互依赖引用的一种通用的语法约定
>> - AMD  运行在浏览器端的模块化异步加载规范，主要以requireJS为代表
基本原理：定义define和require方法异步请求对应的js模块文件到浏览器端运行模块导出时可以使用函数中的return返回结果
                
                // id：模块的命名，可选参数
                // dependencies：加载的模块依赖列表
                // factory： 处理函数，即对dependencies加载的模块进行处理
                define(id, dependencies, factory);

                // mod-A.js
                define('A', ['zepto'], function($){
                    return {
                        init() {
                            console.log('A');
                        }
                    }
                });

                // mod-B.js
                define('B', ['zepto'], function($){
                    return {
                        init() {
                            console.log('B');
                        }
                    }
                });

                // 页面引用依赖的方法
                require('main', ['./mod-A.js', './mod-B.js'], function(A, B){
                    A.init();
                    B.init();
                })
>> - CMD Seajs提出的一种模块化的规范，类似于CommonJS的书写规范，按照按需执行依赖加载的原则。此外加载完某个依赖模块文件后不立即执行，在所有的依赖模块都加载完后才进入主模块逻辑，遇到模块运行语句的时候才执行对应的模块，这跟AMD有区别
                
                define(function(require, exports, module) {

                });
                
                // 与AMD的规范不同，引用mod-A.js和mod-B.js两个文件后，seajs会下载两个模块文件但不会立即执行，在运行init方法后才会分别执行两个模块及处理函数的动作
                seajs.use(['./mod-A.js', './mod-B.js'],function(A, B) {
                    A.init();
                    B.init();
                });

                // mod-A.js
                define(function(require, exports, module) {
                    let $ = require('zepto');
                    module.exports = {
                        init() {
                            console.log('A');
                        }
                    }
                });

                // mod-B.js
                define(function(require, exports, module) {
                    let $ = require('zepto');
                    module.exports = {
                        init() {
                            console.log('B');
                        }
                    }
                });
>> - CommonJS是Node端使用的JS规范，使用require进行模块引入，并使用modules.exports jin进行导出，写法更加清晰简洁
                
                // main.js
                let A = require('./mod-A.js'),
                    B = require('./mod-B.js');
                    A.init();
                    B.init();

                // mod-A.js
                const $ = require('zepto');
                module.exports = {
                    init() {
                        console.log('A');
                    }
                }

                // mod-B.js
                const $ = require('zepto');
                module.exports = {
                    init() {
                        console.log('B');
                    }
                }
>> - ECMAScript 6的import和export，是唯一一个遵循JS语言标准的模块化规范
                
                import { initA } from './mod-A.js';
                import { initB } from './mod-A.js';

                initA();
                initB();

                // mod-A.js
                import Zepto as $ from 'zepto';

                export default {
                    initA() {
                        console.log('A');
                    }
                }

                // mod-B.js
                import Zepto as $ from 'zepto';

                export default {
                    initB() {
                        console.log('B');
                    }
                }
#### 4) 项目组件化设计规范
> - Web Component组件化 Polymer 支持双向数据绑定 Google在2013年提出来的一个新的UI框架
>> - 基础层 本地API
>> - 核心层 基础层的封装库
>> - 元素层 建立在核心层上的UI组件和非UI组件
> - MVVM框架组件化  将页面中的模块按照元素来划分，将相关的JS、CSS样式和html放在同一个文件里进行引用
> - Vitual DOM组件化方案 react.js
> - 利用目录管理的通用组件化实践 将js，html, scss放在同一个文件夹中
                
                /component
                    index.es   // 组件逻辑处理
                    index.scss // 组件预处理脚本
                    index.html // 组件html结构
                /img           // 组件可能用到的图片

            
<h3 id='5.2'>5.2 自动化构建</h3>  
        
#### 1) 步骤
> - 1、读取入口文件 读取index.html源文件到字符串Buffer中
> - 2、分析模块引用 分析页面字符串Buffer中含有的模块引用
> - 3、按照引用加载模块 进入模块中读取包含的HTML、SCSS、JS文件
> - 4、模块文件编译处理 进行脚本转译，ES6转ES5，将SCSS文件预处理为CSS
> - 5、模块文件合并 将所有的JS和CSS代码字符串写入一个新的字符串Buffer中，生成对应的资源路径，并插入到index.html的字符串中
> - 6、文件优化处理 比如压缩、去注释等
> - 7、写成生成目录
            
<h3 id='5.3'>5.3 前端性能优化</h3>  
        
#### 1) 性能测试
> - Performance Timing API 支持IE9以上版本及WebKit内核浏览器中用于记录页面加载和解析过程中关键时间点的机制
>> - unload
>> - redirect
>> - App Cache
>> - DNS
>> - TCP
>> - Request
>> - Response
>> - Processing
>> - onload
> - Profile工具
>> - 分析页面脚本执行过程中最耗资源的操作
>> - 记录页面脚本执行过程中JS对象消耗内存和对战的使用情况
>> - 检测页面脚本执行过程中CPU占用情况
                
                console.profile();
                for(let i = 0; i<100000; i++) {
                    i = i**i;
                }
                console.profileEnd();
> - 页面埋点时间
                
                let timeList = [];
                function addTime(tag) {
                    timeList.push(
                        {
                            'tag': tag,
                            'time': +new Date()
                        }
                    );
                }
                function parseTime() {
                    for(let i = 0; i < addTime.length()-1; i++) {
                        if(addTime[i] && addTime[i+1] && addTime[i].tag === addTime[i+1].tag) 
                            console.log('time: ' + addTime[i].tag + ' = ' + addTime[i+1].time - addTime[i].time + ' ms');
                    }
                }
#### 2) 桌面浏览器前端优化策略YSlow
> - 网络加载类
>> - 1，减少HTTP资源请求次数
>> - 2，减少HTTP请求大小
>> - 3，将CSS或者JS放到外部文件中，避免style或者script标签直接引入
>> - 4，避免空的href或者src
>> - 5、模块文件合并，为HTML指定Cache-Control或者Expires
                
                <meta http-equiv="Cache-Control" content="max-age=7200" />
                <meta http-equiv="Expires" content="Mon, 20, Jul 2016 23:00:00 GMT" />
>> - 6，合理设置Etag和Last-Modified
>> - 7，减少页面重定向
>> - 8，使用静态资源分域存放增加下载并行数
>> - 9，使用静态资源CDN来存储文件
>> - 10，使用CDN Combo下载传输内容（多个文件请求打包成一个文件的形式来返回的技术）
>> - 11，使用可缓存的AJAX
>> - 12，使用GET来完成AJAX请求
                
                $.ajax({
                    url: '',
                    type: 'get',
                    data: '',
                    cache: 'true',
                    success() {

                    },
                    error() {
                        
                    }
                });
>> - 13，减少Cookie的大小并进行Cookie隔离
>> - 14，缩小favicon.ico并缓存
>> - 15，推荐使用异步JS资源 异步JS资源不会阻塞文档阻塞，允许在浏览器中优先渲染页面，延后加载脚本执行
                
                // 加载后续文档元素的过程和main.js的加载是并行的，但是main.js的执行要在页面所有元素解析完成后才开始执行
                <script src="./main.js" defer></script>

                // 加载和渲染后续文档元素的过程和main.js的加载和执行是并行的
                <script src="./main.js" async></script>
>> - 16，消除阻塞渲染的CSS和JS
>> - 17，避免使用CSS import引用加载CSS 因为这样会增加CSS资源加载的关键路径长度，如果把它写在style标签中，则会由于需要加载另外的CSS文件而大大延后CSS的渲染时间
> - 页面渲染类
>> - 1，把CSS资源引用梵高HTML文件顶部
>> - 2，JS放在HTML底部
>> - 3，不要在HTML中直接缩放图片 因为这样做会导致页面内容的重排重绘，有可能会使页面其他操作产生卡顿
>> - 4，减少DOM元素的数量和深度
>> - 5，尽量避免使用table和iframe标签，table可以使用ul代替，iframe使用异步的方式进行动态添加
>> - 6，避免运行耗时的JS
>> - 7，避免使用CSS表达式或者CSS滤镜
#### 2) 桌面浏览器前端优化策略YSlow
> - 
>> - 