---
title: Vue函数防抖和函数节流
date: 2021-03-18 17:40:22
tags:
  - 防抖
  - 节流

categories: [前端, Vue]
---

## 函数防抖（debounce）

### 应用场景

- 登录、发短信等按钮避免用户点击太快，以致于发送了多次请求，需要防抖
- 调整浏览器窗口大小时，resize 次数过于频繁，造成计算过多，此时需要一次到位，就用到了防抖
- 文本编辑器实时保存，当无任何更改操作一秒后进行保存

### 实现方法，防抖重在清零

```js
    function debounce(f, wait){
        let timer
        return (...args) => {
            clearTimeout(timer)
            timer = setTimeout(() => {
                f(..args)
            }, wait)
        }
    }
```

## 函数节流（throttle）

### 应用场景

- `scroll`事件，每隔一秒计算一次位置信息等
- 浏览器播放事件，每隔一秒计算一次进度信息等
- input 框实时搜索并发送请求展示下拉列表，每隔一秒发送一次请求

#### 实现方法，节流重在开关锁

```js
    function throttle(f, wait){
        let timer
        return (..args) => {
            if (timer) { return }
            timer = setTimeout(() => {
                f(..args)
                timer = null
            }, wait)
        }
    }
```

## 总结

- 防抖：防止抖动，单位时间内事件触发会被重置，避免事件被误触发多次。代码重在清零`clearTimeout`
- 节流：控制流量，单位时间内事件只能触发一次，如果服务器端的限流即 Rate Limit。代码实现重在开锁关锁`timer=timeout；timer=null`
