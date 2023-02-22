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

## nuxt2 移动端返回按钮功能BUG

移动端右上角返回按钮在当前页面历史只有一个的情况下,调用`this.$router.back()`函数执行无响应(例如分享页面的右上角返回功能)

```js
export default function ({ app, store, route, redirect }, inject) {
// 移动端返回按钮
  inject('backPage', () => {
    if (store.state.global.backStart) {
      app.router.push('/mobile')
    } else {
      app.router.go(-1)
    }
    const origin = route.path
    setTimeout(() => {
      store.commit('global/setBackStart', false)
      if (origin === location.pathname) {
        app.router.push({ path: '/mobile' })
      }
    }, 600)
  })
  }
```

解决方案: 自定义客户端插件中添加如上代码,这里通过store中的`backStart`状态控制.当页面只有一个历史页面时,返回上一层就会跳转到首页  
当页面历史记录有多个,而当前页面不是最后一个,这时执行`back()`依然会失效所以添加一个定时器判断页面是否跳转,没有跳则执行跳转首页操作
