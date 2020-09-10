## CSS 阻塞
css 文件的下载和解析不会影响 DOM 的解析，但是会阻塞 DOM 的渲染。因为 CSSOM Tree 要和 DOM Tree 合成 Render Tree 才能绘制页面。下面的 test1 在 css 下载并解析完成前是默认样式， test2 在 css 下载并解析完成之前不会显示

```html
<button class="btn btn-primary">test1</button>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/css/bootstrap.min.css">
<div>test2</div>
```
<br>

css 文件没下载并解析完成之前，后续的 js 脚本不能执行。下面的 alert('ok') 在 css 下载并解析完成之前不会弹出来：
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/css/bootstrap.min.css">
<script>
    alert('ok')
</script>
```
<br>

css 文件的下载不会阻塞前面的 js 脚本执行。下面的 alert('ok') 会在 css 下载完成前弹出：
```html
<script>
    alert('ok')
</script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/css/bootstrap.min.css">
```

所以在需要提前执行不操作 dom 元素的 js 时，不妨把 js 放到 css 文件之前。
## js 阻塞
js 文件的下载和解析会阻塞 GUI 渲染进程，也就是会阻塞 DOM 和 CSS 的解析和渲染。

js 文件没下载并解析完成之前，后续的 HTML 和 CSS 无法解析：
```html
    <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
    <div>test</div>
```

复制代码
js 文件的下载不会阻塞前面 HTML 和 CSS 的解析：
```html
    <div>test</div>
    <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
```