---
title: angularJS 1.5 component与ui-router的结合
date: 2016-08-28T03:19:22+08:00
slug: angular-component-ui-router
tags:
  - angularJS
type: page
---
AngularJS team将Component从2.0反向移植到1.5版本已经有一阵子了。在实际项目中由于主要采用MVVM模式，导致封装性欠佳。近日，脑洞大开，研究了以后决定局部实验。本文主要记录在angular-ui-router中渐进引入1.5版本component元件的一些过程。
<!--more-->
## 准备工作
为了正常使用component的新特性和顺利结合进angular-ui-router，先检查依赖。（angular-ui-router版本太低会导致传参数入component在$resolve的时候无效，哎，都是泪T_T，本宝宝以为只要angular版本高于1.5就行了，结果折腾了半天发现angular-ui-router也要对应更新）

主要的依赖需求是：
```javascript
"angular": "^1.5.8",    
"angular-ui-router": "^1.0.0"
```

## 引入component
首先假如定义如下的元件：
```javascript
app.component('myComponent', {
    templateUrl: 'myComponent.html',
    controller: 'MyController',
    bindings: { input1: '<', input2: '<' }
});
```
### 传统写法

使用template，通过$resolve传递数据。（本宝宝喜欢这个方法，在component实例化之前用promise的方式取数据，很强大！）
```javascript
$stateProvider.state('foo', {
  template: '<my-component input1="$resolve.foo" input2="$resolve.bar"></my-component>',
  url: '/foo/:fooId/:barId',
  resolve: {
    foo: ($stateParams, FooService) => FooService.get($stateParams.fooId),
    bar: ($stateParams, BarService) => BarService.get($stateParams.barId)
  } 
});
```
### component参数指定
```javascript
$stateProvider.state('foo', {
  component: 'myComponent',
  url: '/foo/:fooId/:barId',
  resolve: {
    // input1 and input2 are same names as myComponent `bindings`
    input1: ($stateParams, FooService) => FooService.get($stateParams.fooId),
    input2: ($stateParams, BarService) => BarService.get($stateParams.barId)
  } 
});
```

### name:bindings对象的形式
```javascript
$stateProvider.state('foo', {
  component: 'myComponent',
  bindings: { input1: "foo", input2: "bar" },
  url: '/foo/:fooId/:barId',
  resolve: {
    foo: ($stateParams, FooService) => FooService.get($stateParams.fooId),
    bar: ($stateParams, BarService) => BarService.get($stateParams.barId)
  } 
});
```
----
## 参考文献
https://github.com/angular-ui/ui-router/issues/2627