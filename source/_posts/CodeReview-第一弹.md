---
title: CodeReview 第一弹
date: 2019-08-28 14:27:18
tags:
---

>分享人：熊莎
## 重要类型枚举化
>与服务端的约定，统一管理，方便维护，且不能随意修改

bad
```
if (type === 'type1') {}
 
if (type === 'type2') {}
```

good
```
enum types = {
  type1 = 'type1',
  type2 = 'type2'
}

if (type === types.type1) {}
 
if (type === types.type2) {}
```

## 视图中函数传递
>尽量不使用箭头函数或bind，这两种方式在每次render的时候，都会重新生成一个新的引用，如果作为子组件的属性传递，会导致子组件会强制更新

bad
```
<Button onClick={() => this.handleClick()}>按钮</Button>
```

good
```
<Button onClick={this.handleClick}>按钮</Button>
```

## 复杂逻辑添加注释
>一个程序员的代码只有他自己和上帝看得懂，过了几个月，只有上帝能看得懂了。
## 组件提取
* 逻辑封装，方便后期维护
* 性能优化，子组件可避免不必要的渲染
* 代码整洁，组件内部代码，外部可按需关注
## Switch用Object替代
>尽量避免不必要的判断

bad
```
switch (val) {
  case 'a':
    ...
  case 'b':
    ...
}
```

```
good
var config = {
  a: ...
  b: ...
}

config(val)

```

## React元素绑定事件时传递参数 -- 内存优化
>如遇绑定事件需要传参，且可能触发高频渲染时，考虑使用更加节省内存的方式

bad
```
handleClick = (uuid) =>{
  // TODO your code
}
  
const uuid = 'asdfghj'
<Button onClick={() => this.handleClick(uuid)}>按钮</Button>
```

good
```
handleClick = e =>{
    // 可以正确拿到基本数据类型参数  --输出 ‘asdfghj’
    console.log('data-uuid:', e.target.getAttribute('data-uuid'));
    // 拿不到复杂数据类型，复杂元素属性被toString处理  --输出 ‘[object Object]’
    console.log('data-obj:', e.target.getAttribute('data-obj'));
    //TODO your code
}

const uuid = 'asdfghj';
const obj = {
  a: '123'
};
<Button data-uuid={uuid} data-obj={obj} onClick={this.handleClick}>按钮</Button>
```

## 一个值使用多次最好声明变量去保存，以免多次取，造成不必要的性能消耗
bad
```
 if (obj.a) {
   obj.b = obj.a
 }
 obj.a = xxx
```
good
```
const a = obj.a
if (a){
  b =a
}
a = xxx
```
## 相同作用函数合并
## 多种类型展示拆分子组件
## 函数逻辑清晰


