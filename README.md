##  使用手册

<code>mpvue</code> 继承自 <code>Vue.js</code>，其技术规范和语法特点与 <code>Vue.js</code> 保持一致。

本框架以mpvue为核心，进行二次开发，添加图像处理机制，集成集成Font Awesome字体图标，flyio数据连接，echar图表（待考虑）腾讯地图，以及原生weUI并将持续扩展

本文档适用于有一定 Vue.js 使用经验的开发者。

1.下载后

```
npm i
```
2.运行

```
npm run dev
```
3.你将在框架根目录下找到“dist”
使用web开发者工具（微信开发者工具）打开dist目录

## 框架原理
- 以==mpvue==为核心，保留了vue.runtime核心，无缝使用vue.js的基础能力
- 本框架 提供了将 vue 的模板语法转换到小程序的 wxml 语法的能力
- 修改了 vue 的建构配置，使之构建出符合小程序项目结构的代码格式： json/wxml/wxss/js 文件
## 实例生命周期
整体同vue，但是生命周期因为掺入了小程序一些独特的生命周期所以请**注！意！下！面！内！容！**
### 1.在编写时请使用vue生命周期
<ul>
<li>beforeCreate</li>
<li>created</li>
<li>beforeMount</li>
<li>mounted</li>
<li>beforeUpdate</li>
<li>updated</li>
<li>activated</li>
<li>deactivated</li>
<li>beforeDestroy</li>
<li>destroyed</li>
</ul>
详细见vue生命周期<a href="https://cn.vuejs.org/v2/api/#选项-生命周期钩子">生命周期钩子</a>
2.支持小程序生命周期但请不要轻易使用
除了 Vue 本身的生命周期外，mpvue
还兼容了小程序生命周期，这部分生命周期钩子的来源于<a href="https://mp.weixin.qq.com/debug/wxadoc/dev/framework/app-service/page.html">微信小程序的 Page</a>， 除特殊情况外

**不！建！议！使！用！小！程！序！的！生！命！周！期！钩！子！**。

下面说说支持的小程序生命周期

**app 部分：**
- onLaunch，初始化
- onShow，当小程序启动，或从后台进入前台显示
- onHide，当小程序从前台进入后台

**page 部分：**
- onLoad，监听页面加载
- onShow，监听页面显示
- onReady，监听页面初次渲染完成
- onHide，监听页面隐藏
- onUnload，监听页面卸载
- onPullDownRefresh，监听用户下拉动作
- onReachBottom，页面上拉触底事件的处理函数
- onShareAppMessage，用户点击右上角分享
- onPageScroll，页面滚动

![image](http://mpvue.com/assets/lifecycle.jpg)
## 模板语法

几乎全支持 官方文档：模板语法，下面讲下不支持的情况。

### 1.不支持 纯-HTML
也就是说 v-html 指令不能用。
### 2不支持部分复杂的 JavaScript 渲染表达式
框架会把 template 中的 <code>{{}}</code> 双花括号的部分，直接编码到 wxml 文件中，由于微信小程序的能力限制(<a href="https://mp.weixin.qq.com/debug/wxadoc/dev/framework/view/wxml/data.html">数据绑定</a>)，所以无法支持复杂的 JavaScript 表达式。

目前可以使用的有 + - * % ?: ! == === > < [] .，剩下的还待完善。

```
<!-- 这种就不支持，建议写 computed -->
<p>{{ message.split('').reverse().join('') }}</p>

<!-- 但写在 @event 里面的表达式是都支持的，因为这部分的计算放在了 vdom 里面 -->
<ul>
    <li v-for="item in list">
        <div @click="clickHandle(item, index, $event)">{{ item.value }}</p>
    </li>
</ul>
```
