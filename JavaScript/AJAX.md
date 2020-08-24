## 什么是AJAX
Asynchronous JavaScript And XML（异步的js和xml）是一种网页开发技术

## 优点
* $\color{red}页面无刷新更新数据$, 用户交互体验好
* $\color{red}前端和后端负载平衡$，把以前一些服务器负担的工作转嫁到客户端，减轻服务器负担；AJAX的原则是“按需取数据”，可以最大程度的减少冗余请求和响应对服务器造成的负担，提升站点性能。
* $\color{red}基于标准被广泛支持$，不需要下载浏览器插件
* $\color{red}异步与服务器通信$，不需要打断用户的操作，具有更加迅速的响应能力

## 缺点
* $\color{red}AJAX 干掉了Back和History功能$，即对浏览器机制的破坏
* $\color{red}安全问题$
* $\color{red}seo支持较弱$
* $\color{red}违背URL和资源定位的初衷$：我给你一个URL地址，如果采用了Ajax技术，也许你在该URL地址下面看到的和我在这个URL地址下看到的内容是不同的。这个和资源定位的初衷是相背离的


## readystate
| readystate |               状态               |                                  描述                                   |
| :--------: | :------------------------------: | :---------------------------------------------------------------------: |
|     0      |         UNSENT（未打开）         |                    open()方法还$\color{red}未被调用$                    |
|     1      |         OPENED（未发送）         |                    open()方法$\color{red}已经被调用$                    |
|     2      | HEADERS_RECEIVED（已获取响应头） |       send()方法已经$\color{red}被调用，响应头和响应状态已经返回$       |
|     3      |    LOADING（正在下载响应体）     | $\color{red}响应体下载中$;responseText中$\color{red}已经获取了部分数据$ |
|     4      |         DONE（请求完成）         |                          整个请求过程已经完毕                           |


<br><br><br><br>

## 编写ajax
```js
function http (url, data) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    xhr.onreadystatechange = () => {
      if (xhr.readystate === 4) {
        if (xhr.status === 200) {
          resolve(xhr.responseText)
        }
      }
    }
    xhr.open('post', url, true)
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')
    xhr.send(data)
  })
}

```