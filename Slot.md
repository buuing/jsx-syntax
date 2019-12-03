
## 作用域插槽

如果要用渲染函数向子组件中传递作用域插槽, 则可以使用`scopedSlots`字段

```jsx
scopedSlots: {
  default: props => {
    console.log(props)
    return <div>作用域插槽</div>
  }
}
```

在普通的vue模板中, 是通过`template`进行渲染, 在内部可以直接通过`scope`拿到当前item数据

```html
<el-table-column label="图片">
  <template slot-scope="scope">
    <img :src="scope.row.src" />
  </template>
</el-table-column>
```

使用jsx改写:

```jsx
<el-table-column label="图片" {...{
  scopedSlots: {
    default: scope => <img src={scope.row.src} />
  }
}}></el-table-column>
```
