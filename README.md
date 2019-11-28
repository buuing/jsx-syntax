
# Jsx语法指北 (Vue版)

<br />

> 郑重声明: 本文档只记录vue版jsx语法, `请勿在react项目中使用`, 因为两者之间还是有些许的区别

vue@2.6的版本可以搭配render函数直接使用jsx动态渲染组件, 但是2.5及以前的版本必须加babel插件才可以使用

> 以下操作适用于vue@2.5及以下

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

## 创建组件的新方式

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

## 标签的注意事项

双标签没变化, 但是单标签必须要闭合

```html
<br />
<img src="./images.png" />
<input value="msg" />
```

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
```

- `v-model`则不变, 但是其中的变量必须使用`this.属性`的方式使用

```jsx
<p v-model={ this.msg }></p>
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

## 事件处理

在jsx里面如果想dom绑定事件, 就不能使用`v-on:click`和`@click`, 取而代之的是以on开头, 然后让事件名首字母大写即可

可以使用es5的bind方法把this传递进去, 但是更推荐使用es6的箭头函数直接使用外界的this

```jsx
<div>
  <button onClick={ this.handleClick.bind(this, '参数') }>点击事件</button>
  <button onClick={e => this.handleClick('参数')}>点击事件</button>
  <button onClick={e => {
    // 在里面也可以书写多行js函数, 或是调用其他方法
  }}>点击事件</button>
</div>
```
