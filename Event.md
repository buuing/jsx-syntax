
## 绑定事件

在jsx里面如果想dom绑定事件, 就不能使用`v-on:click`和`@click`, 取而代之的是以on开头, 然后让事件名首字母大写即可

<br />

- `click点击事件`

方法调用则可以使用`es5`的bind方法把this传递进去 (不推荐)

```jsx
<button onClick={ this.handleClick.bind(this) }>点击事件</button>
```

更推荐使用`es6`的箭头函数, 好处是可以直接使用外界的this

```jsx
<button onClick={e => this.handleClick('参数')}>点击事件</button>

<button onClick={e => {
  // 在里面也可以书写多行js代码, 或是调用其他方法
  console.log(e)
}}>点击事件</button>
```

<br />

- `.native原生事件`

```jsx
<button nativeOnClick={e => this.handleClick('参数')}>原生事件</button>
```

<br />

- `keyup/keydown键盘事件`

```jsx
<input onKeyup={e => this.handleEvent()} />
```

<br />

## 按键修饰符

直接判断事件参数里面的keyCode即可

| **按键修饰符** | **等价操作** |
| --- | --- |
| `.enter` | `e.keyCode === 13` |
| `.shift` | `e.keyCode === 16` 或 `!e.shiftKey` |
| `.ctrl` | `e.keyCode === 17` 或 `!e.ctrlKey` |
| `.alt` | `e.keyCode === 18` 或 `!e.altKey` |
| `.up` | `e.keyCode === 38` |
| `.down` | `e.keyCode === 40` |
| `.left` | `e.keyCode === 37` |
| `.right` | `e.keyCode === 39` |

- 例如:

```jsx
<input onKeyup={e => e.keyCode === 13 && this.handleEvent()} />
```

<br />

## 事件修饰符

对于 `.passive`、`.capture` 和 `.once`这三个修饰符, vue提供了相应的前缀

| **事件修饰符** | **前缀** |
| --- | --- |
| `.passive` | `&` |
| `.capture` | `!` |
| `.once` | `~` |
| `.once.capture` | `~!` |

- 例如:

```jsx
<button {...{
  on: {
    '!click': this.doThisInCapturingMode,
    '~keyup': this.doThisOnce,
    '~!mouseover': this.doThisOnceInCapturingMode
  }
}}></button>
```

如果是其他修饰符, 也可以通过js代码来代替

| **事件修饰符** | **处理函数中的等价操作** |
| --- | --- |
| `.stop` | `e.stopPropagation()` |
| `.prevent` | `e.preventDefault()` |
| `.self` | `if (e.target !== e.currentTarget) return` |
