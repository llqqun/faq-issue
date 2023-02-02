# faq-issue
问题汇总

## nuxt2 开发环境下打开多个页面,导致连接卡死的问题

> 现象  

点击链接打开新页面，打开多个后发现页面一直在加载处于假死状态，但开一个新的浏览器又正常了。发现多个请求一直处于pending，分析原因大概是某个请求阻塞，但请求总数并不超过6，觉得奇怪，检查每个页面还有一个专门用于热更新的长链接 sse 与client

> 解决  

nuxt.config.js build属性下设置loadingScreen: false可以关掉sse，client似乎关闭不了。去掉之后这个阻塞现象没有出现

## nuxt2 开发环境下debugger触发时sourceMap文件不对

> 现象  

浏览器正常进入debugger模式,控制台显示的sourceMap代码文件不一致  
文件夹目录 pages > aBc > index.vue  

> 解决  

检查文件目录发现是因为驼峰命名导致的,将文件夹名称修改成非驼峰的就行  
