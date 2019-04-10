**Recommend**
- [AngularJS依赖注入](http://www.yiibai.com/angularjs/angularjs_dependency_injection.html)
- [深入理解 AngularJS 的 Scope](http://www.lovelucy.info/understanding-scopes-in-angularjs.html)

#指令
- AngularJS 指令是扩展的 HTML 属性，带有前缀 ng-。
- ng-app 指令初始化一个 AngularJS 应用程序。
- ng-init 指令初始化应用程序数据。
- ng-model 指令把元素值（比如输入域的值）绑定到应用程序。

#表达式
- AngularJS 表达式写在花括号内：{{ expression }}。
- AngularJS 表达式把数据绑定到 HTML，这与 ng-bind 指令类似，但使用情况不一样。
- AngularJS 将在表达式书写的位置"输出"数据。
- AngularJS 表达式 很像 JavaScript 表达式：它们可以包含文字、运算符和变量。

#Scope(作用域)
- Scope(作用域) 是应用在 HTML (视图) 和 JavaScript (控制器)之间的纽带。
- Scope 是一个对象，有可用的方法和属性。
- Scope 可应用在视图和控制器上。

#控制器
- AngularJS 控制器 控制 AngularJS 应用程序的数据。
- AngularJS 控制器是常规的 JavaScript 对象，由标准的 JavaScript 对象的构造函数 创建。
- ng-controller 指令定义了应用程序控制器。

#过滤器
- 过滤器可以使用一个管道字符（|）添加到表达式和指令中。例：{{ abc | uppercase }}则是将abc转换成大写格式
- AngularJS 过滤器可用于转换数据：currency (格式化数字为货币格式)；filter (从数组项中选择一个子集)；lowercase (格式化字符串为小写)；orderBy (根据某个表达式排列数组)；uppercase (格式化字符串为大写)

#路由
- AngularJS 路由允许我们通过不同的 URL 访问不同的内容。
- 通过 AngularJS 可以实现多视图的单页Web应用（single page web application，SPA）。
- 通常我们的URL形式为http://runoob.com/first/page，但在单页Web应用中 AngularJS 通过 # + 标记 实现，例如：http://runoob.com/#/first

#App组件机制
- 关于module的定义为：angular.module(‘com.ngbook.demo’, [])，module函数可以传递3个参数
- name：模块定义的名称，它应该是一个唯一的必选参数，它会在后边被其他模块注入或者是在ngAPP指令中声明应用程序主模块；
- requires：模块的依赖，它是声明本模块需要依赖的其他模块的参数。特别注意：如果在这里没有声明模块的依赖，则我们是无法在模块中使用依赖模块的任何组件的；它是个可选参数。
- configFn： 模块的启动配置函数，在angular config阶段会调用该函数，对模块中的组件进行实例化对象实例之前的特定配置，如我们常见的对$routeProvider配置应用程序的路由信息。它等同于"module.config"函数，建议用”module.config“函数替换它。这也是个可选参数。

#Controller
- 创建应用中的控制器，特别需要注意的是控制器的匿名函数参数，这两个参数的名称不可以修改，在 AngularJs 中，参数的名称用来进行依赖注入。第一个参数$scope就是这个控制器的上下文对象，通过它我们将模型，视图，和处理逻辑粘合在一起。

#值
- 值是简单的JavaScript对象，它是用来将值传递过程中的配置相位控制器。
```
/define a module
var mainApp = angular.module("mainApp", []);
//create a value object as "defaultInput" and pass it a data.
mainApp.value("defaultInput", 5);
...
//inject the value in the controller using its name "defaultInput"
mainApp.controller('CalcController', function($scope, CalcService,defaultInput) {
  $scope.number = defaultInput;
  $scope.result = CalcService.square($scope.number);
  $scope.square = function() {
    $scope.result = CalcService.square($scope.number);
  }
})
```

#工厂
- 工厂是用于返回函数的值。它根据需求创造值，每当一个服务或控制器需要。它通常使用一个工厂函数来计算并返回对应值
```
/define a module
var mainApp = angular.module("mainApp", []);
//create a factory "MathService" which provides a method multiply to return multiplication of two numbers
mainApp.factory('MathService', function() {     
  var factory = {};  
  factory.multiply = function(a, b) {
    return a * b 
  }
  return factory;
}); 
//inject the factory "MathService" in a service to utilize the multiply method of factory.
mainApp.service('CalcService', function(MathService){
  this.square = function(a) { 
    return MathService.multiply(a,a); 
  }
})
```

#服务
- 服务是一个单一的JavaScript包含了一组函数对象来执行某些任务。服务使用service()函数，然后注入到控制器的定义。
```
/define a module
var mainApp = angular.module("mainApp", []);
...
//create a service which defines a method square to return square of a number.
mainApp.service('CalcService', function(MathService){
  this.square = function(a) { 
    return MathService.multiply(a,a); 
  }
});
//inject the service "CalcService" into the controller
mainApp.controller('CalcController', function($scope, CalcService, defaultInput) {
  $scope.number = defaultInput;
  $scope.result = CalcService.square($scope.number);
  $scope.square = function() {
    $scope.result = CalcService.square($scope.number);
  }
})
```

#提供者
- 提供者所使用的AngularJS内部创建过程中配置阶段的服务，工厂等(相AngularJS引导自身期间)。下面提到的脚本，可以用来创建，我们已经在前面创建MathService。提供者是一个特殊的工厂方法以及get()方法，用来返回值/服务/工厂。
```
/define a module
var mainApp = angular.module("mainApp", []);
...
//create a service using provider which defines a method square to return square of a number.
mainApp.config(function($provide) {
  $provide.provider('MathService', function() {
    this.$get = function() {
      var factory = {};  
      factory.multiply = function(a, b) {
        return a * b; 
      }
      return factory;
    };
  });
})
```

- *Attention* 
>- angular.js文件：建议把脚本放在  元素的底部。这会提高网页加载速度，因为 HTML 加载不受制于脚本加载。
>- ng-bind 和 {{ expression }} 的区别：这两种方式的效果都是一样的，其主要区别在于，使用花括号语法时，在AngularJS使用数据替换模板中的花括号时，第一个加载的页面，通常是应用中的index.html，其未被渲染的模板可能会被用户看到。而使用第二站方法的视图不会遇到这种问题。原因是，浏览器需要首先加载index.html页面，渲染它，然后AngularJS才能把它解析成你期望看到的内容。所以，对于index.html页面中的数据绑定操作，建议采用ng-bind。那么在数据加载完成之前用户就不会看到任何内容。

#依赖注入(dependency injection)
###*scope*
- $scope是angularJS中的作用域(其实就是存储数据的地方)，很类似javascript的原型链。 
- $scope和$rootScope的区别</br>-$scope是html和单个controller之间的桥梁，数据绑定就靠他了。rootscope是各个controller中scope的桥梁。用rootscope定义的值，可以在各个controller中使用
- *Reference:*
>- [参考](http://www.lovelucy.info/understanding-scopes-in-angularjs.html)

###*http*
- $http服务直接同外部进行通信。$http服务只是简单的封装了浏览器原生的XMLHttpRequest对象。
- 链式调用：$http服务是只能接受一个参数的函数，这个参数是一个对象，包含了用来生成HTTP请求的配置内容。这个函数返回一个promise对象，具有success和error两个方法。</br>-
```
http({
  url:'data.json',
  method:'GET'
}).success(function(data,header,config,status){
//响应成功
}).error(function(data,header,config,status){
//处理响应失败
})
```

###*resource*
- 与$http相同，angularjs还提供了另外一个可选的服务$resource,使用它可以非常方便的同支持restful的服务单进行数据交互。
- *Reference:*
>- [AngularJS中使用$resource](http://www.itnose.net/news/144/6289094)

