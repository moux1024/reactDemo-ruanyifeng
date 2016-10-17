我的README

根据阮一峰的教程对demo中未作解释的细节进行补充

心得：react的组件封装极为简单，状态机的设定也让数据-模型的关系易于理解，只是html+js混合这种写代码的方式让人觉得不太优雅。

## demo01 了解babel
 - 提到babel,使用
 ```dash
 $babel src --out-dir build
 ```
 将src文件夹中的js转为es5格式并发布到build中,实现需要
  - babel-cli 根据需要,安装到本地或全局(安装到本地需要考虑修改package.json)
  - 创建babel的配置文件'.babelrc'到项目根目录,填写preset
  - 安装babel对react的支持babel-preset-react
  - 执行转码

## demo02 了解ReactDOM.render方法的参数
 - 提到jsx的解析语法，使用了ES6的map方法遍历数组并return，此时return的是一个什么对象？字符串？数组？DOM？
 - 尝试修改代码如下
 ```js
 var names = ['Alice', 'Emily', 'Kate'];

 function mkDom(arr){
   names.forEach(function(v,k){
     arr.push('<div>hello,'+v+'</div>')
   })
   return arr
 };
 // 最终可行的替代方法，但是会导致出现一个没有key的数组？？
 // function mkDom(arr){
 //   names.forEach(function(v,k){
 //     arr.push(<div>hello,{v}</div>)
 //   })
 //   return arr
 // };
 ReactDOM.render(
   <div>
   {
     mkDom([])
     // names.map(function (name) {
     //   return <div>Hello, {name}!</div>
     // })
   }
   </div>,
   document.getElementById('example')
 );
 ```
 得到
 ```html
 <div data-reactroot="">
   <!-- react-text: 2 -->
   &lt;div&gt;hello,Alice&lt;/div&gt;
   <!-- /react-text -->
   <!-- react-text: 3 -->
   &lt;div&gt;hello,Emily&lt;/div&gt;
   <!-- /react-text -->
   <!-- react-text: 4 -->
   &lt;div&gt;hello,Kate&lt;/div&gt;
   <!-- /react-text -->
 </div>
 ```
 此处要点
  - ReactDOM.render方法的第一个参数是干净的html文档结构，则其中{}包含的可解析js代码必须直接执行并返回干净的html结构。
  - {}中不接受‘;’、变量声明等代码，仅仅是一个执行环境，可以理解为angularJS中的html嵌入js，作用域非全局，打印this显示为undefined
  - map方法为何支持html文档的return？待测试,初步确定是jsx语法允许

## demo03
## demo04 了解React的Class
 - React的createClass方法是一个构造函数，调用并传入一个对象（含有属性render）会生成一个react对象，this指向这个对象，则此时this可以调用react对象各项属性
 - 顺序为：
  1. createClass=>new，添加render方法
  2. reactDOM=> 取html代码生成dom结构，获取dom属性，为1步里生成的对象添加对应属性
  3. render方法中所需的dom属性从react对象中取得，渲染

## demo05
## demo06 用propTypes【对象】和getDefaultProps【方法】设置createClass的props类型和默认值
 - 除了demo提供的createClass，可以用纯函数【非构造函数】来创建组件，如下
  ```js
  function Greeting(props) {
    return (
      <h1>Hello, {props.name}</h1>
    );
  }

  Greeting.propTypes = {
    name: React.PropTypes.string
  };

  Greeting.defaultProps = {
    name: 'John Doe'
  };
 ```
 - 关于纯函数组件的说明
  > https://facebook.github.io/react/docs/reusable-components.html

  > These components must not retain internal state, do not have backing instances, and do not have the component lifecycle methods. They are pure functional transforms of their input, with zero boilerplate.

 - 只有名无值的prop，默认已填写了boolean类型的值,true还是false，未确认


## demo07 获取真实的dom节点
 - react提供了handleEvent-事件回调处理属性：
  - 接受function值，用于在触发事件时执行对应的function
  - 使用this.refs来获取target，target是一个具有ref属性的标签，在react对象构造时被用这个属性的值命名，加入到refs属性中

## demo08 组件状态
 - react提供了state用于管理组件的状态：
  - this.state可以用于读取/修改组件的所有state
  - getInitialState，接受一个function，该function的返回值就是state对象，为组件的默认state
  - setState方法，用于更改state，demo中在handleEvent的function里使用

## demo09 表单
 - state的属性若设置为对象，则读取正常，修改时报错，待处理

## demo10 组件生命周期
 - 类似angularJS，但是以我目前水平，讲不出有内容的东西，暂放

## demo11 AJAX修改state
## demo12 从gitHub的搜索结果抓取数据并显示在页面上
## demo13 react项目完整前后端
