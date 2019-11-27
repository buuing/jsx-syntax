
## 创建组件
---

##### **普通`template`组件**

- 原先的vue组件无非就是老三样:
  - 在`<template> <script> <style>`中分别去写html, js, css

```html
<template>
  <div id="home">
    <h1 class="title">hello world</h1>
    <p>普通template语法</p>
  </div>
</template>

<script>
export default {
  name: 'home',
  data () {
    return {}
  }
}
</script>
```

<br />

##### **使用`jsx`书写的组件**

- 我们看到下面的代码, 已经没有了`<template>`标签, 取而代之的是在js里面多了一个`类似于生命周期`一样的`render函数`
- `style 标签`则没有任何变化, 还是跟以前一样书写

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

!> 需要注意, `template标签`和`render函数`不可以同时使用, 只能二选其一

<br />

## 标签书写
---

##### **单标签要闭合**

- 双标签没变化, 但是`单标签必须要闭合`

```jsx
<br />
<img src="./images.png" />
<input value="msg" />
```

<br />

##### **根标签**

- `return`只能返回一个dom元素, 所以多标签必须拥有一个`父元素`来包裹

```jsx
render () {
  return <div>
    <p>1</p>
    <p>2</p>
    <p>3</p>
  </div>
}
```

- 如果想换行书写, 则需要在`return`后面紧跟一个小括号

```jsx
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

<br />

## class与style
---

##### **`template`语法**

```html
<div>
  <p class="title"></p>
  <p :class="{ 'title': true, 'active': isShow }"></p>
  <p style="width: 100px; font-size: 18px"></p>
  <p :style="{ width: '100px', fontSize: '18px' }"></p>
</div>
```

<br />

##### **`jsx`语法**

```jsx
<div>
  <p class="title"></p>
  <p class={{ 'title': true, 'active': this.isShow }}></p>
  <p style="width: 100px; font-size: 18px"></p>
  <p style={{ width: '100px', fontSize: '18px' }}></p>
</div>
```

<br />

## 渲染变量
---

##### **`template`语法**

```html
<div>
  <p>{{ 1 + 1 }}</p>
  <p>{{ msg }}</p>
  <p v-html="msg"></p>
  <p v-model="msg"></p>
</div>
```

<br />

##### **`jsx`语法**

- 在jsx语法中, 所有的js都需要写在`{ }`单花括号中
- 如果想要使用`data`中的数据, 则需要通过`this`调用

```jsx
<div>
  <p>{ 1 + 1 }</p>
  <p>{ this.msg }</p>
  <p domPropsInnerHTML={ this.msg }></p>
  <p v-model={ this.msg }></p>
</div>
```

<br />

## 条件渲染
---

##### **`template`语法**

```html
<div>
  <p v-if="isShow">这是一个p标签</p>
</div>
```

<br />

##### **`jsx`语法**

- jsx里面不能使用类似于`v-if`这样的指令, 只能是通过`&& 逻辑运算符`或`三元运算符`

```jsx
<div>
  { this.isShow && <p>这是一个p标签</p> }
  { this.isShow ? <p>这是一个p标签</p> : null }
</div>
```

!> 把dom当成对象来看, 你就觉得很容易理解了

<br />

## 列表渲染
---

##### **`template`语法**

```html
<ul>
  <li v-for="(item, index) in list" :key="index">
    {{item}}
  </li>
</ul>
```

<br />

##### **`jsx`语法**

```jsx
// 普通写法
<ul>
  {
    this.list.map((item, index) => {
      return <li key={index}>{item}</li>
    })
  }
</ul>

// 换行的话必须加括号
<ul>
  {
    this.list.map((item, index) => {
      return (
        <li key={index}>{item}</li>
      )
    })
  }
</ul>

// 简写
<ul>
  { this.list.map((item, index) => <li key={index}>{item}</li>) }
</ul>
```

<br />

## 事件处理
---

##### **`template`语法**

```html
<div>
  <button @click="handleClick('参数')">点击事件</button>
</div>
```

<br />

##### **`jsx`语法**

- 可以使用`es5`的`bind`方法把this传递进去
- 也可以使用`es6`的箭头函数直接使用外界的this

```jsx
<div>
  <button onClick={ this.handleClick.bind(this, '参数') }>点击事件</button>
  <button onClick={() => this.handleClick('参数')}>点击事件</button>
</div>
```

!> jsx里面所有的事件都是以`on`开头, 然后让事件名`首字母大写`即可

<br />
