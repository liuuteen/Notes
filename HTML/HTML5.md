`HTML5` 新特性需要考虑兼容性问题，基本上 `IE9+` 以上支持。

# 1. 语义化标签

以前布局大多是用 `div` 做，对于搜索引擎没有语义（含义）。

```html
<div class="header"></div>
<div class="nav"></div>
<div class="content"></div>
<div class="footer"></div>
```

语义化标签：

- `<header>`
- `<nav>`
- `<article>` ：文章内容
- `<section>` ：文档区域
- `<aside>` ：侧边栏
- `<footer>`

这种语义化标准是针对 **搜索引擎** 的，在 `IE9` 中需要手动转为 **块级元素**

# 2. 多媒体标签

主要：

- 音频 `<audio>`
- 视频 `<video>`

**直接在页面中嵌入音频和视频**，不需要 `flash` 和浏览器插件了。

## 1. `<video>`

`HTML5` 原生支持的[视频格式](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Containers)文件有限（浏览器主要 mp4, Ogg, WebM 三种视频格式），所有浏览器支持 mp4

```html
<video src="./xx.mp4" controls></video>
```

`<source>` 可以为 `<picture>, <video>, <audio>` 提供多个源，提高浏览器兼容性

```html
<video src="./xx.mp4" controls>
  <source src="myVideo.mp4" type="video/mp4">
  <source src="myVideo.webm" type="video/webm">
  <p>Your browser doesn't support HTML5 video. Here is
     a <a href="myVideo.mp4">link to the video</a> instead.</p>
</video>
```

标签属性查询：https://www.runoob.com/tags/tag-video.html

`autoplay` 谷歌浏览器不会自动播放，需要加 `muted="muted"` 静音解决。

## 2. `<audio>`

`HTML5` 原生支持[音频格式](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/Audio_codecs)文件有限（MP3，Wav，Ogg），所有浏览器都支持 mp3

```html
<audio src="./xx.mp3" controls></audio>
```

```html
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
您的浏览器不支持 audio 元素。
</audio>
```

标签属性查询：https://www.runoob.com/tags/tag-audio.html

谷歌浏览器禁止了自动播放

# 3. 表单标签

`HTML5` 新增了 `input` 的类型

`type="email" 邮箱, type="number"` 数字, `type="search"` 搜索框

date 日期，time 时间， month/week 月/周，tel 电话，url 地址，color 拾色器

其他可查：https://www.runoob.com/tags/att-input-type.html

这些表单标签放在 `form` 中时自带验证。

## 新增表单属性

`input` 的属性：https://www.runoob.com/tags/tag-input.html

例如 `required` 必填 ，`placeholder` 提示文本

css 技巧例如修改提示文本颜色

```css
input::placeholder{ color: pink;}
```

`autofocus`, `autocomplete` 记录以前输入得文字信息

`multiple` 可以多选文件

