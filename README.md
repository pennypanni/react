### [React 入门实例教程-阮一峰](http://www.ruanyifeng.com/blog/2015/03/react.html)

## JSX 语法  [demo02](./demo02)

```javascript
var names = ['Alice', 'Emily', 'Kate'];

ReactDOM.render(
  <div>
  {
    names.map(function (name) {
      return <h1>Hello, {name}!!</h1>
    })
  }
  </div>,
  document.getElementById('example')
);
```

JSX 的基本语法规则：遇到 HTML 标签（以 `<` 开头），就用 HTML 规则解析；遇到代码块（以 `{` 开头），就用 JavaScript 规则解析。

## 组件  [demo04](./demo04)

React.createClass 方法用于生成一个组件类。

```javascript
var HelloMessage = React.createClass({
  render: function() {
    return <h1 style={{color:this.props.color}}>Hello {this.props.name}</h1>;
  }
});

ReactDOM.render(
  <HelloMessage name="John" color="red"/>,
  document.getElementById('example')
);
```

变量 `HelloMessage` 就是一个组件类。所有组件类都必须有自己的 `render` 方法，用于输出组件。<br>
注意：组件类的第一个字母必须大写，否则会报错;另外，组件类只能包含一个顶层标签，否则也会报错。<br>
组件的属性可以在组件类的 `this.props` 对象上获取，比如 `name` 属性就可以通过 `this.props.name` 读取。<br>
添加组件属性，有一个地方需要注意，就是 `class` 属性需要写成 `className` ，`for` 属性需要写成 `htmlFor` ，这是因为 `class` 和 `for` 是 JavaScript 的保留字。

## this.props.children  [demo05](./demo05)

```javascript
var NotesList = React.createClass({
  render: function() {
    return (
      <ol>
      {
        React.Children.map(this.props.children, function (child) {
          return <li>{child}</li>;
        })
      }
      </ol>
    );
  }
});

ReactDOM.render(
  <NotesList>
    <span>hello</span>
    <span>world</span>
  </NotesList>,
  document.body
);
```

`this.props.children` 属性表示组件的所有子节点。<br>
React 提供一个工具方法 `React.Children` 来处理 `this.props.children` 。我们可以用 `React.Children.map` 来遍历子节点，而不用担心 `this.props.children` 的数据类型是 `undefined` 还是 `object`。

## propTypes

propTypes属性，就是用来验证组件实例的属性是否符合要求[demo0601](./demo06/demo0601.html)

```javascript
 var MyTitle = React.createClass({
   propTypes: {
     title: React.PropTypes.string.isRequired,   //验证title属性
   },
 
   render: function() {
      return <h1> {this.props.title} </h1>;
    }
 });
 
 var data = 123;    //设置title属性为一个数值，控制台会显示一行错误信息。
 
 ReactDOM.render(
   <MyTitle title={data} />,
   document.body
 );
```  

`getDefaultProps` 方法可以用来设置组件属性的默认值。[demo0602](./demo06/demo0602.html)

```javascript
var MyTitle = React.createClass({
  getDefaultProps : function () {
    return {
      title : 'Hello World'
    };
  },

  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});

ReactDOM.render(
  <MyTitle />,
  document.body
);
```

## 组件的生命周期 [demo10](./demo10/index.html)

```javascript
var Hello = React.createClass({
  getInitialState: function () {
    return {
      opacity: 1.0
    };
  },

  componentDidMount: function () {
    this.timer = setInterval(function () {
      var opacity = this.state.opacity;
      opacity -= .05;
      if (opacity < 0.1) {
        opacity = 1.0;
      }
      this.setState({
        opacity: opacity
      });
    }.bind(this), 100);     
  },

  render: function () {
    return (
      <div style={{opacity: this.state.opacity}}>
        Hello {this.props.name}
      </div>
    );
  }
});

ReactDOM.render(
  <Hello name="world"/>,
  document.body
);
```

[关于bind(this)](http://www.jb51.net/article/65850.htm)：

在javascript当中每一个function都是一个对象，所以在这个里var temp=this 指的是function当前的对象。this是Javascript语言的一个关键字。它代表函数运行时，自动生成的一个内部对象，只能在函数内部使用。

注意： `render()`函数应该是纯粹的，也就是说该函数不修改组件的 `state`，每次调用都返回相同的结果，不读写 DOM 信息，也不和浏览器交互。如果需要和浏览器交互，在 `componentDidMount()` 中或者其它生命周期方法中做这件事。保持 render() 纯粹，可以使服务器端渲染更加切实可行，也使组件更容易被理解。

***

### ECMAScript 6 
[React速学教程(下)中的ES6](http://blog.csdn.net/fengyuzhengfan/article/details/52233582)<br>
[ES6入门-阮一峰](http://es6.ruanyifeng.com/#docs/let)

* 模块

在ES6中，模块的功能主要由 export 和 import 组成。每一个模块都有自己单独的作用域，模块之间的相互调用关系是通过 export 来规定模块对外暴露的接口，通过import来引用其它模块提供的接口。同时还为模块创造了命名空间，防止函数的命名冲突。

* 箭头函数

`=>`不只是关键字function的简写，它还带来了其它好处。箭头函数与包围它的代码共享同一个`this`,能帮你很好的解决this的指向问题。有经验的JavaScript开发者都熟悉诸如`var self = this`;或`var that = this`这种引用外围this的模式。但借助=>，就不需要这种模式了。

箭头函数的箭头=>之前是一个空括号、单个的参数名、或用括号括起的多个参数名，而箭头之后可以是一个表达式（作为函数的返回值），或者是用花括号括起的函数体（需要自行通过return来返回值，否则返回的是undefined）。

```javascript
// 箭头函数的例子
()=>1
v=>v+1
(a,b)=>a+b
()=>{
    alert("foo");
}
e=>{
    if (e == 0){
        return 0;
    }
    return 1000/e;
}
```

#### ES5与ES6在导入与导出组件上的不同

`ES5`通过`require`导入

```javascript
var React = require("react");
```

`ES6`通过`import`导入

```javascript
import React from 'react';
```

另外，ES6支持将组件导入作为一个对象，使用`* as`修饰即可。

```javascript
//引入app目录下AboutPage组件作为一个对象，接下来就可使用“AboutPage.”来调用AboutPage的方法及属性了。  
import  * as AboutPage from './app/AboutPage' 
```

<br>
<br>

`ES5`要导出一个类给别的模块用，一般通过`module.exports`来导出

```javascript
var MyComponent = React.createClass({
    ...
});
module.exports = MyComponent;
```

`ES6`通常用`export default`来导出类

```javascript
export default class MyComponent extends Component{
    ...
}
```

#### let命令

ES6新增了`let`命令，用来声明变量。它所声明的变量，只在let命令所在的代码块内有效。

for循环的循环语句部分是一个父作用域，而循环体内部是一个单独的子作用域。

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```
上面代码输出了3次abc，这表明函数内部的变量i和外部的变量i是分离的。

ES6明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

`let不允许在相同作用域内，重复声明同一个变量。`

#### const命令
const声明一个只读的常量。一旦声明，常量的值就不能改变。const一旦声明变量，就必须立即初始化，不能留到以后赋值。

const的作用域与let命令相同：只在声明所在的块级作用域内有效。

