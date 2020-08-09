学习笔记
## 浏览器工作原理
**URL** -> HTTP -> **HTML** -> parse -> **DOM** -> css computing -> **DOM with css** -> layout -> **DOM with position** -> render -> **Bitmap**

> 浏览器从URL到最终Bitmap图片。
自己实现最基本浏览器功能。
真实浏览器包含互动、性能、功能性（收藏、历史...）等。
### URL
从URL请求到HTTP响应，取出URL对应的HTML。
### HTML
对HTML超文本进行parse（文本分析），将HTML解析为DOM树。
### DOM
对只有DOM元素的树进行css规则计算。（重叠、覆盖）
### DOM with CSS
带有CSS属性的DOM树进行布局，将DOM所有产生的盒子的位置计算出来。
### DOM with position
通过render（渲染）将DOM with position 的内容（背景图、背景色...）滑到一张图片上，通过操作系统及硬件驱动提供的API最终展示出来。

## 有限状态机
### 每一个状态都是一个机器
> 有限状态机里的每个机器都是互相解耦的，它们互相独立。
- 在每个机器里，都可以做计算、存储、输出......
- 所有机器接受的输入一致
- 状态机里每一个机器本身没有状态（纯函数）
### 每一个机器都知道自己的下一个状态
- 每个机器都有确定的下一个状态（Moore）
- 每个机器根据输入决定下一个状态（Mealy）

## 不使用状态机处理字符串
```
// 不准使用正则表达式，纯粹用 JavaScript 的逻辑实现：在一个字符串中，找到字符“ab”
function match(string) {
  let foundA = false
  for (let c of string) {
    if (c=== 'a') {
      foundA = true
    } else if (foundA && c=== 'b') {
      return true
    } else {
      foundA = false
    }
  }
  return false
}
```

```
// 不准使用正则表达式，纯粹用 JavaScript 的逻辑实现：在一个字符串中，找到字符“abcdef”
function match(str) {
  let findA = false;
  let findB = false;
  let findC = false;
  let findD = false;
  let findE = false;
  for(let c of str) {
    if(c === 'a') {
      findA = true;
    } else if(findA && c === 'b') {
      findB = true;
    } else if(findB && c === 'c') {
      findC = true;
    } else if(findC && c === 'd') {
      findD = true;
    } else if(findD && c === 'e') {
      findE = true;
    } else if(findE && c === 'f') {
      return true;
    } else {
      findA = false;
      findB = false;
      findC = false;
      findD = false;
      findE = false;
    }
  }
  return false;
}
```

## 不使用状态机处理字符串
```
// 不准使用正则表达式，纯粹用 JavaScript 的逻辑实现：在一个字符串中，找到字符“abc”
function match(string) {
  let state = start;
  for (let c of string) {
    state = start(c);
  }
  return state === end;
}

function start(c) {
  if (c === 'a') {
    return foundA;
  } else {
    return start(c);
  }
}

function end(c) {
  return end;
}

function foundA(c) {
  if (c === 'b') {
    return foundB;
  } else {
    return start(c);
  }
}

function foundB(c) {
  if (c === 'c') {
    return end;
  } else {
    return start(c);
  }
}

console.log(match('ababc'));
```

```
// 用状态机实现：字符串“abcabx”的解析
function match(string) {
  let state = start;
  for (let c of string) {
    state = start(c);
  }
  return state === end;
}

function start(c) {
  if (c === 'a') {
    return foundA;
  } else {
    return start(c);
  }
}

function end(c) {
  return end;
}

function foundA(c) {
  if (c === 'b') {
    return foundB;
  } else {
    return start(c);
  }
}

function foundB(c) {
  if (c === 'c') {
    return foundC;
  } else {
    return start(c);
  }
}

function foundC(c) {
  if (c === 'a') {
    return foundAA;
  } else {
    return start(c);
  }
}

function foundAA(c) {
  if (c === 'b') {
    return foundBB;
  } else {
    return start(c);
  }
}

function foundBB(c) {
  if (c === 'x') {
    return end;
  } else {
    return foundB;
  }
}

console.log(match('ababcabxab'));
```

```
// 使用状态机完成”abababx”的处理
function match(string) {
  let state = start;
  for (let c of string) {
    state = start(c);
  }
  return state === end;
}

function start(c) {
  if (c === 'a') {
    return foundA;
  } else {
    return start(c);
  }
}

function end(c) {
  return end;
}

function foundA(c) {
  if (c === 'b') {
    return foundB;
  } else {
    return start(c);
  }
}

function foundB(c) {
  if (c === 'a') {
    return foundAA;
  } else {
    return foundB(c);
  }
}

function foundAA(c) {
  if (c === 'b') {
    return foundBB;
  } else {
    return foundA;
  }
}

function foundBB(c) {
  if (c === 'a') {
    return foundAAA;
  } else {
    return foundBB;
  }
}

function foundAAA(c) {
  if (c === 'b') {
    return foundBBB;
  } else {
    return foundBB;
  }
}

function foundBBB(c) {
  if (c === 'x') {
    return end;
  } else {
    return foundAA;
  }
}
```
## URL -> HTML
- 会话、表示、应用 -> HTTP
- 传输 -> TCP
- 网络 -> Internet
- 物理层、数据链路层 -> 4G/5G/WI-FI

HTTP 流、端口
TCP/IP  包/IP地址、libnet/libpcap

### HTTP
Request
Response
客户端发起请求，服务端才响应，一对一。

## Request
> HTTP协议是文本协议，内容都是字符串，
- Request line
  - POST/HTTP/1.1
- headers 
  - Host:127.0.0.1
  - Content-Type:application/x-www-form-urlencoded

- body
  - name=aaa&code=x%3D1

  ## Response
- status line
  - HTTP/1.1 200 ok
- headers
  - Content-Type:text/html
  - Date:Mon,23 Dec 2020 06:46:19 GMT
  - Connection:keep-alive
  - Transfer-Encoding:chunked

- body
  - 26
  - <html><body> Hello World<body></html>
  - 0

