
## vue指令

很遗憾, 在所有的原生指令当中只有`v-show`可用, 其余则需要你使用js来实现其逻辑

```jsx
<div v-show={this.isShow}>使用方法跟之前相同</div>
```

<br />

## 自定义指令

```jsx
{...{
  directives: [
    {
      name: 'my-custom-directive', // 指令名称
      value: '2', // 绑定的值
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ]
}}
```

<br />

- 示例: `element-ui`中的`v-loading`

```jsx
<div {...{
  directives: [ // 注意name字段只写`v-`后面的字符
    { name: 'loading', value: this.loading }
  ]
}}></div>
```
