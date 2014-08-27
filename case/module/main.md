## 模块的加载与解析时间
    - 本地模块define触发以下流程
    - 首先加载所有依赖资源
    - 依赖资源解析完成
    - 解析当前模块

## 每个模块只加载一次，并且只解析一次；解析的顺序不可控

    define('a', function () {
        console.log('a');
        return 'a';
    });

    define('e', function (require) {
        console.log('e');
    });

## 每个模块只能定义一次，重复定义无效

    define('a', function (require) {
        console.log('a start');
        return 'a';
    });

    define('a', function (require) {
        console.log('a start');
        return 'a';
    });

## 模块暴露方式

    define('a', function (require) {
        return 'a';
    });

    define('inst', function (require) {
        return { name: 'a' };
    });

    define('b', function (require, exports, module) {
        module.exports = 'b';
    });

    define('c', function (require, exports) {
        exports.name = 'c';
    });

    // 错误的接口暴露方式
    define('e', function (require, exports) {
        exports = 'e';
    });


## 全局 require对象与模块require对象

    define('a/b', function () {
        // 全局require 不允许使用相对路径
        console.log(require('./e')); // Exception
    });

## 解析顺序采用依赖倒序方式

    define('a/b', function (require) {
        console.log('b start');
        return 'a/b';
    });

    define('a/b/c', function (require) {
        console.log(require('./d'));
        console.log(require('a/b'));
        console.log('a/b/c start');
        console.log(require('./e'));
    });

    define('a/b/e', function (require) {
        console.log('e start');
        return 'a/b/e';
    });

    define('a/b/d', function (require) {
        console.log('d start');
        return 'a/b/d';
    });

## 依赖出现错误，模块不做解析
    define('error', function (require) {
        console.log('b start');
        require('./notfound');
    });


## 引用依赖

    define('b', function (require) {
        console.log('b');
        require('c');
        return 'b';
    });

    define('c', function (require) {
        console.log('c');
        require('b');
        return 'c';
    });

## 模块合并

    define(function () {
        require('');
    });