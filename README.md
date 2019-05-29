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
### [2.1 HTTP协议](#2.1)
### [2.2 Web安全机制](#2.2)
### [2.3 垃圾收集算](#2.3)

        
        
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
>>>>>> ![图1-1 前端应用开发模式演变.png](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true)
                

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
>>>>>> ![图1-2 浏览器组成结构.png](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true)
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
>>>>>> ![图1-3 Webkit内核渲染DOM流程.png](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true)
>>>>>> ![图1-4 Gecko内核渲染DOM流程.png](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true)
#### 3) HTML解析
> - DOM元素标签和DOM元素对象虽然都是用来描述DOM结构的，但是DOM元素标签是指文本话的HTML标识，而DOM元素对象则是指经过渲染引擎DOM解析后生成的具有节点父子关系的属性对象
>>>>>> ![图1-5 DOM结构解析示意图.png](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true) 
#### ４) CSS解析
> - CSS解析通过权重方式进行计算，！important > 内联样式规则（权重1000） > id选择器（权重100）> 类选择器（权重10）> 元素选择器（权重1）
>>>>>> ![图1-5 DOM结构解析示意图.png](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true) 
                
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
>>>>>> ![图1-7 HTTP缓存判断流程](https://github.com/hblvsjtu/JVM_Study/blob/master/picture/%E5%9B%BE1-1%20%E8%BF%90%E8%A1%8C%E6%97%B6%E6%95%B0%E6%8D%AE%E5%8C%BA.png?raw=true) 
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
> - 最大长度限制在4KB左右
> - 使用document.cookie进行修改
#### 6) Web SQL
> - 非HTML5里的规范，只有较新的Chrome浏览器才能