---
title: reactStart
date: 2016-04-11 17:15:05
tags:
---
React -start

##React
- Facebook
- MVC中的V：M -> V
- 和Flux搭配，可以做到MVVM
- 默认转义所有字符串，防止XSS攻击
- React认为HTML标签及生成这些标签的代码间存在着内在联系，React设计允许你在构建标签结构时充分利用JS的强大能力，而不必在笨拙的模板语言上浪费时间

##Why React?
- 简单：
  model改变后，react自动处理用户界面的更新；
- 声明式：
  更新界面时，react仅仅会更新变化的部分；
- 虚拟Dom
  React在内存中维护一个快速响应的DOM描述，并利用他来快速地计算出差异，然后更新浏览器中的DOM;

##React 最需要走心的工作
- 构建可组合的、可复用的组件;
- 组件的好处：代码复用、测试更加点多;

##React - hello world
    <!DOCTYPE html>
    <html>
        <head>
            <title>React Hello World</title>
            <script type="text/javascript" src="./react.js"></script>
        </head>
        <body>
            <div id="app"></div>

            <script type="text/javascript">
                function h1 (text, style) {
                    return '<h1>'' + text + '</h1>';
                }
                var el = React.createElement('h1', {style: {color: '##00f'}}, 'Hello World!');
                React.render(el, document.getElementById('app'));
            </script>
        </body>
    </html>


##React - 类似的工作
    <!DOCTYPE html>
    <html>
        <head>
            <title>React Hello World</title>
        </head>
        <body>
            <div id="app"></div>

            <script type="text/javascript">
                  function h1 (text, style) {
                    var result;
                    var styleStr;

                    if (style) {
                        styleStr = 'style="';
                        for (var name in style) {
                            styleStr += name + ':' + style[name] + ';'; 
                        }
                        styleStr += '"';
                    }

                    if (styleStr) {
                        result = '<h1 ' + styleStr + '>' + text + '</h1>';
                    } else {
                        result = '<h1>' + text + '</h1>';
                    }

                      return result;
                  }

                  function render(target, container) {
                    container.innerHTML = target;
                  }

                  render(h1('Hello World!', {color: '##00f'}), document.getElementById('app'));
            </script>
        </body>
    </html>

##JSX - Javascript XML
优势：
- 类HTML标记语言，都是熟悉的语法
- 抽象了React Element的创建过程
- 简单直观，可以提升效率且成本很低
劣势：
- 在浏览器运行会很慢，项目中需要预处理

##JSX - hello world
    <!DOCTYPE html>
    <html>
        <head>
            <title>React Hello World</title>
            <script type="text/javascript" src="./react.js"></script>
            <script type="text/javascript" src="./JSXTransformer.js"></script>
        </head>
        <body>
            <div id="app"></div>

            <script type="text/jsx">
                React.render(<h1>Hello World, JSX!</h1>, document.getElementById('app'));
            </script>
        </body>
    </html>

##JSX - 工作原理
1. 使用JSXTransformer通过监听windows的onload/DOMContentLoaded，来遍历所有"text/jsx"脚本标签
2. 获取每个标签的source code并把JSX代码转为react的虚拟dom代码
3. 最后把react代码添加到新建的script标签中
4. 预编译的插件是同理，只不过发生在编译阶段

##JSX Tips
- JSX设置动态属性时，用花括号包裹Javascript变量
- 也可以把属性设置为一个函数的调用返回结果
- 条件判断支持：三元运输符、&&、||
- key: 列表时会建议使用，重用组件提升渲染性能
- refs：获取创建dom的描述对象 or 获取创建dom的对象, this.refs.usernameInput.getDOMNode() x
- HTML关键词：for > htmlFor, class > className, style: {}
- 事件统一使用驼峰式

##React Tips
- React创建的class名称首字母必须大写，不然找不到
- JSX可以不换行，换行时必须使用（）包装，不然会报非法组件错误；
- 只能有根一个节点，多个节点会抛错
- 内联样式支持对象，名称使用驼峰式，否则不起作用，fontSize
- 返回：null、false、React组件

##组件的复合
- 复用那些接口定义良好的组件来开发新的模块化组件
- this.props.children X
- 组件支持 ref="refId"  this.refs.refId

##React - 组件数据流
- props
- state

##React - dom对象
- refs

##React - 组件生命周期

###组件类创建时
- getDefaultProps

###初始化
- getinitialState

###挂载
- componentWillMount
- render
- componentDidMount

###更新
- componentWillReceiveProps
- shouldComponentUpdate
- componentWillUpdate
- render
- componentDidUpdate

###卸载
- componentWillUnmount

###生命周期
- Class Create Pahses:
    + invoke while class created and be shared between instances.
- Initialization Pahses:
    + constructor: Initialization of state. The instance is now retained.
    + componentWillMount
    + render
    + [children's constructors]
        - [children's componentWillMount and render]
        - [children's componentDidMount]
    + componentDidMount
- Update Phases:
    + componentWillReceiveProps (only called if parent updated)
    + shouldComponentUpdate (default: always returns true to prevent subtle bugs)
        - componentWillUpdate
        - render
            + [children's constructors or receive props phases]
        - componentDidUpdate
- Unmount Phases:
    + componentWillUnmount
        - [children's componentWillUnmount]
        - [children destroyed]
    + (destroyed): The instance is now blank, released by React and ready for GC

##Mixin
- 独立于组件的
- 多个组件可共享的
- 对象类型的配置

##Mixin - DEMO
    var userMixin = {
        getDefaultProps: function () {
            return {
                username: '小红'
            };
        },
        renderUser: function (welcomeMsg) {
            return (
                    <div className="user-wrap">
                        <span ref="welcomeDesc" className="welcome-user">{welcomeMsg}</span>
                    </div>
                );
        },
        componentDidMount: function () {
            console.log('mount');
        }
    }


    var Header = React.createClass({
        mixins: [userMixin],
        getInitialState: function () {
            return {
                systemName: 'React平台Header'
            };
        },
        render: function () {
            var welcomeMsg;
            if (this.props.username) {
                welcomeMsg = '欢迎，' + this.props.username + '！'
            };
            return (
                    <div className="header-content">
                        <h1>{this.state.systemName}</h1>
                        {
                            welcomeMsg ? this.renderUser(welcomeMsg) : null
                        }
                    </div>
                );
        }
    });

##React - PureRenderMixin
- React组件的渲染函数是“纯粹的” - 同样的props和state，渲染出同样的效果
- 可以提升性能 - 在shouldComponentUpdate里检查props、state是否会发生变化
- shouldComponentUpdate的判断结果会影响整个组件子树
- 据说props、state比较是浅比较，不适合(特别)复杂的数据结构 x

##PureRenderMixin - shouldComponentUpdate: shallow check
    /**
     - Performs equality by iterating through keys on an object and returning false
     - when any key has values which are not strictly equal between the arguments.
     - Returns true when the values of all keys are strictly equal.
     */
    function shallowEqual(objA, objB) {
      if (objA === objB) {
        return true;
      }

      if (typeof objA !== 'object' || objA === null || typeof objB !== 'object' || objB === null) {
        return false;
      }

      var keysA = Object.keys(objA);
      var keysB = Object.keys(objB);

      if (keysA.length !== keysB.length) {
        return false;
      }

      // Test for A's keys different from B.
      var bHasOwnProperty = hasOwnProperty.bind(objB);
      for (var i = 0; i < keysA.length; i++) {
        if (!bHasOwnProperty(keysA[i]) || objA[keysA[i]] !== objB[keysA[i]]) {
          return false;
        }
      }

      return true;
    }

###React - Mixin原理
- Mixin必须是一个对象
- 在React.createClass（源码：ReactClass > createClass: function (spec) {...}会把Mixin对象融入react对象
- 融入过程是通过遍历Mixin的属性（for..in）逐个添加到react对象得prototype上（mixSpecIntoComponent）
- 添加之前需要判断是否已经有了相同属性，且此属性是否支持覆盖、是否支持多个、是否需要合并
- 添加的方式是：覆盖（override）、合并、多个函数链

###React - Mixin融合方式的标准
- 一些基础的属性是不允许覆盖的，比如：setProps、replaceProps、replaceState、isMounted
- 渲染的属性是不允许定义多次的，比如：render、shouldComponentUpdate
- 需要合并的是那些有返回值的，比如：getDefaultProps、getInitialState
- 其他的都支持多个函数链的形式（先执行已有的，在执行Mixin的），比如：willMount、didMount、willUpdate、didUpdate等等

##React - 表单组件

###受用户交互影响的属性
- value，用于 input、textarea 组件
- checked，用于类型为 checkbox 或者 radio 的 input 组件
- selected，用于 option 组件

###onChange 回调函数来监听组件变化
- input 或 textarea 的 value 发生变化时。
- input 的 checked 状态改变时。
- option 的 selected 状态改变时。

###Controlled Components - 受限组件
- 设置了 value 的 input 是一个受限组件

###受限组件 - DEMO
    var App = React.createClass({
        getInitialState: function () {
            return {
                phone: '15821466666'
            };
        },
        render: function () {
            return (
                    <div>
                        <label htmlFor="phone">手机号：</label>
                        <input type='text' id="phone" value={this.state.phone} onChange={this.phoneChangeHlr} />
                    </div>
                );
        },
        phoneChangeHlr: function (event) {
            this.setState({phone: event.target.value});
        }
    });

    ReactDOM.render(<App />, document.getElementById('app'));

##React - add-ons
- TransitionGroup和CSSTransitionGroup
- LinkedStateMixin - 用于简化用户表单输入数据和组件 state 之间的双向数据绑定
- classSet，用于更加干净简洁地操作 DOM 中的 class 字符串
- cloneWithProps，用于实现 React 组件浅复制，同时改变它们的 props 
- update，一个辅助方法，使得在 JavaScript 中处理不可变数据更加容易
- PureRenderMixin，在某些场景下的性能检测器

##LinkedStateMixin - DEMO
    var LinkedStateMixin = React.addons.LinkedStateMixin;
    var App = React.createClass({
        mixins: [LinkedStateMixin],
        getInitialState: function () {
            return {
                phone: '15821466666'
            };
        },
        render: function () {
            return (
                    <div>
                        <label htmlFor="phone">手机号：</label>
                        <input type='text' id="phone" valueLink={this.linkState('phone')}  />
                    </div>
                );
        }
    });

    ReactDOM.render(<App />, document.getElementById('app'));


##开发工具webpack

###webpack + react install
- npm init
- npm install --save react react
- npm install --save-dev webpack webpack-dev-server
- npm install --save-dev jsx-loader css-loader file-loader style-loader url-loader

###Configuring webpack
    var webpack = require('webpack');

    module.exports = {
        // 它定义了打包的入口文件，数组中的文件会按顺序进行，并且它会自行解决依赖问题。
        entry: {
            main: './pages/main.js',
        },
        // 它定义了输出文件的的位置，包括路径，文件名，还可能有运行时的访问路径
        output: {
            path: __dirname + '/build',
            publicPath: '/build/',
            filename: '[name].js'
        },
        // Webpack 是使用类似 Browserify 的方式在本地按目录对依赖进行查找。
        // resolve属性中的extensions数组中用于配置程序可以自行补全哪些后缀。
        // 比如 Hello.jsx 这样的文件就可以直接用 require(./Hello) 引用。
        resolve: {
            extensions: ['', '.js', '.jsx']
        },
        // webpack将所有的资源都看做是模块，而模块就需要加载器
        module: {
            loaders: [
                // loaders 指定 jsx-loader 编译后缀名为 .jsx 的文件，
                // 建议给含有 JSX 的文件添加 .jsx 后缀，
                // 当然你也可以直接使用 .js 后缀， 相应的 test 配置正则要修改匹配就是。
                { test: /\.js?$/, exclude: /node_modules/, loader: 'jsx?harmony' }, 
                { test: /\.(css)/, loader: 'style-loader!css-loader' },
                // inline base64 URLs for <=8k images, direct URLs for the rest
                {test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192' } 
                // loaders: ['react-hot', 'jsx?harmony']
                // loaders后面如果跟的不是数组，则会报错：LoadersList.js:81 r.forEach(function(r) 
            ]
        },
        // 我们可以在plugin参数中配置我们需要用到的各种各样的插件。
        plugins: [
            new webpack.NoErrorsPlugin(),
        ]
    }

###Webpack 命令
启动webpack命令
- webpack -d 提供SourceMaps，方便调试
- webpack -w 提供watch方法，实时进行打包更新
- webpack --colors 输出结果带彩色，比如：会用红色显示耗时较长的步骤
- webpack --progress 输出的接口带进度
- webpack --profile 输出性能数据，可以看到每一步的耗时
- webpack -p 对打包后的文件进行压缩
- webpack --config <filename> 支持指定实用的配置文件，处理release和debug不同的情况

###配置scripts
    "scripts": {
        "locDev": "webpack -d -w --progress --colors",
        "serDev": "webpack-dev-server --progress --colors --port 8082"
    }

###运行
- npm run locDev
- npm run serDev

###React - 热插拔
- npm install --save-dev react-hot-loader
- webpack.config.js: entry 使用数组，并添加 'webpack/hot/only-dev-server'
- webpack.config.js: loaders: ['react-hot', 'jsx?harmony']
- 入口文件添加 module.hot.accept()
- 启动：webpack-dev-server -d --port 8082 --hot --progress --colors

###React - 热拔插多个入口
- webpack.config.js: entry:
    entry: {
        main: ['./pages/main/index.js', 'webpack/hot/only-dev-server'],
        help: ['./pages/help/index.js', 'webpack/hot/only-dev-server']
    }

##React - render时机
###批量更新 - 事件回调函数
- 事件处理函数中的多次setState/replaceState会被标记为一次批量更新
- 事件处理函数本身在React事件机制中是作为一个回调函数被调用，在函数执行完成后才会触发render

###实时更新 - 其他
- 调用setState/replaceState后直接触发render，比如setInterval

###render流程
- setState/replaceState 修改状态后
- 组件添加等待更新的状态列表，并把新的状态压入列表（ReactUpdateQueue.js: enqueueSetState()）
- 标记组件为需要重新渲染的组件（dirty components list）ReactUpdates：enqueueUpdate()
- runBatchedUpdates来处理dirty components（调用对应的render）

##React - React事件系统
1. React初始化时会把事件监听 ReactEventListener 注入到 ReactBrowserEventEmitter
2. ReactBrowserEventEmitter 是浏览器事件的顶级委托，ReactEventListener 为用户绑定事件的DOM绑定自己的处理函数，并在处理函数中调用用户的事件处理函数，然后理render相关逻辑；
3. React 事件系统：
    - 浏览器事件的顶级委托（top-level delegation）用来 trap (捕获？) 大多数原生浏览器事件，可以注入事件处理器；
    - 主线程唯一的工作是注入的 ReactEventListener（是为可插拔事件源准备的事件监听器）；
    - 转发这些被 trap 原生浏览器事件至 EventPluginHub， EventPluginHub 在提取任何组合事件时会调用对应插件（比如 SimpleEventPlugin ）并返回需要处理的事件列表；
    - 事件插件包括：ResponderEventPlugin、SimpleEventPlugin、TapEventPlugin、EnterLeaveEventPlugin、ChangeEventPlugin、SelectEventPlugin、BeforeInputEventPlugin
    - EventPluginHub 同时会维护Dom、事件名称和事件处理器的Mapping；
    - ReactEventListener 监听 EventPluginHub 的事件，并处理事件派发及事件响应
    - ReactEventListener 的事件响应函数会处理用户的事件处理函数
4. React组件及组件树的渲染都是在ReactMount里面执行
5. 
mount组件时，初始化组件并注册事件监听（mountComponent）
6. 事件响应由ReactEventListener的dispatchEvent触发，通过EventPluginHub找到事件处理相关 
7. 我们的事件函数作为回调处理
