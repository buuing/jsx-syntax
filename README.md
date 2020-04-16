
# Jsx语法指北 (Vue版)

<br />

> 郑重声明: 本文档只记录vue版jsx语法, `请勿在react项目中使用`, 因为两者之间还是有些许的区别

`vue-cli@3.x以上的版本可以`搭配render函数`直接使用jsx`动态渲染组件, 但是`vue-cli@2.x`的版本必须加以下babel插件才可以使用

<br />

> 以下操作适用于`vue-cli@2.x`

- 首先安装一堆babel插件

`npm install babel-plugin-syntax-jsx babel-plugin-transform-vue-jsx babel-helper-vue-jsx-merge-props babel-preset-env babel-plugin-jsx-v-model --save-dev`

- 然后在`.babelrc`里面增加如下配置

```js
{
  "presets": ["env"],
  "plugins": ["jsx-v-model", "transform-vue-jsx"]
}
```

> 如果你还是配置不成功

- [参考1: vue官网提供的babel插件](https://github.com/vuejs/babel-plugin-transform-vue-jsx)
- [参考2: 专门处理render里面的v-model数据双向绑定指令](https://github.com/nickmessing/babel-plugin-jsx-v-model)

<br />

## 目录

- 语法篇
  - [render函数](#render函数)
  - [标签及注释](#标签及注释)
  - [渲染变量](#渲染变量)
  - [引用图片](#引用图片)
  - [class与style](#class与style)
  - [条件渲染](#条件渲染)
  - [列表渲染](#列表渲染)

- [事件篇](./Event.md)
  - [绑定事件](./Event.md#绑定事件)
  - [按键修饰符](./Event.md#按键修饰符)
  - [事件修饰符](./Event.md#事件修饰符)

- [插槽篇](./Slot.md)
  - [作用域插槽](./Slot.md#作用域插槽)

- [指令篇](./Directive.md)
  - [自定义指令](./Directive.md#自定义指令)

<br />

## render函数

原先的vue组件无非就是老三样: 在`<template>`里写html, `<script>`里写js, `<style>`去写css

但是我们看到下面的代码, 已经没有了`<template>`标签, 取而代之的是在js里面多了一个`类似于生命周期`一样的render函数, style样式则没有任何变化, 还是跟以前一样写在`<style>`里面

```jsx
<script>
export default {
  name: 'home',
  data () {
    return {}
  },
  render () {
    return <div id="home">
      <h1 class="title">hello world</h1>
      <p>普通template语法</p>
    </div>
  }
}
</script>
```

> 但是需要注意, `template标签`和`render函数`不可以同时使用, 只能二选其一

<br />

## 标签及注释

- 单标签必须要闭合

```html
<br />
<img src="./images.png" />
<input value="msg" />
```

- 注释写在`{/* */}`里面

```jsx
<div>
  {/* <p>单行注释</p> */}

  {/*
    <p>多行注释</p>
    <p>多行注释</p>
    <p>多行注释</p>
  */}
</div>
```

- 返回根标签

在render函数中, return只能返回一个dom元素, 所以多标签必须拥有一个父元素来包裹

```js
render () {
  return <div>
    <p>1</p>
    <p>2</p>
  </div>
}
```

如果想换行书写, 则需要在return后面紧跟一个小括号

```js
render () {
  return (
    <div>
      <p>1</p>
      <p>2</p>
      <p>3</p>
    </div>
  )
}
```

> 如果你把`dom标签`看成是一个一个的对象就容易理解多了

<br />

## 渲染变量

在jsx语法中, 所有的js都需要写在`{ }`单花括号中, 如果想要使用data中的数据, 则需要通过this调用

- 双花括号`{{}}`改成单花括号`{}`

```jsx
<div>
  <p>{ 1 + 1 }</p>
  <p>{ this.msg }</p>
</div>
```

- `v-html`改成`domPropsInnerHTML`

```jsx
<p domPropsInnerHTML={ this.msg }></p>

<p {...{ // 高级写法
  domProps: {
    innerHTML: this.msg
  }
}}></p>
```

- `v-model`则不变, 但是其中的变量必须使用`this.属性`的方式使用

```jsx
<p v-model={ this.msg }></p>
```

<br />

## 引用图片

- 通过`require`引入

```jsx
<img src={require('@/assets/img/logo.png')} />

<div style={{
  background: `url(${require('@/assets/img/banner.png')})`
}}></div>
```

- 通过`import`引入

```html
<script>
import logo from '@/assets/img/logo.png'

export default {
  render () {
    return <div>
      <img src={logo} />
    </div>
  }
}
</script>
```

<br />

## class与style

vue版jsx里面, 标签的class属性还是原样书写, 不必像react一样写成className

```jsx
<p class="title"></p>

<p style="width: 100px; font-size: 18px"></p>
```

如果需要动态绑定, 则需要在花括号里面写一个对象

```jsx
<p class={{ 'title': true, 'active': this.isShow }}></p>

<p style={{
  width: '100px',
  fontSize: '18px',
  background: this.active ? '#fff' : '#000'
}}></p>
```

<br />

## 条件渲染

jsx里面不能使用类似于`v-if`这样的指令, 只能是通过`&& 逻辑运算符`或`三元运算符`

```jsx
<div>
  { this.isShow && <p>这是一个p标签</p> }

  { this.isShow ? <p>这是一个p标签</p> : null }
</div>
```

<br />

## 列表渲染

同样`v-for`也不能使用了, 使用map方法进行代替

```jsx
<ul>
  {
    this.list.map((item, index) => {
      // key当成属性写在li标签中即可
      return <li key={index}>{item}</li>
    })
  }
</ul>
```

简写方式:

```jsx
<ul>
  { this.list.map((item, index) => <li key={index}>{item}</li>) }
</ul>
```

<br />
