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

## 组件的生命周期

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