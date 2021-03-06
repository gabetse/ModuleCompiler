# ModuleCompiler

[![Build Status](https://secure.travis-ci.org/daxingplay/ModuleCompiler.png)](http://travis-ci.org/daxingplay/ModuleCompiler)

## 简介

Module Compiler是一个基于NodeJS的KISSY模块打包工具，目前适用于KISSY 1.2+

## 特点

- 基于NodeJS，相比于KISSY自带的Java工具，打包快速
- 参照浏览器端的KISSY的config进行配置，无需额外知识，只需要改一下包路径即能快速打包
- 支持混合编码打包，不同的包可以使用不同的编码
- 支持GBK输出

## 使用

### 安装
    npm install module-compiler

or

    git clone git://github.com/daxingplay/ModuleCompiler.git


### 编写你的打包脚本

    var ModuleCompiler = require('module-compiler');

    // 这里和KISSY.config一样，先配置包
    ModuleCompiler.config({
        packages: [{
            'name': 'sh',
            'path': '这里建议写绝对路径，即sh这个包所在的目录',
            'charset': 'gbk'
        }]
    });

    // 将xxx.js打包为xxx.combine.js，输出编码为GBK
    ModuleCompiler.build('xxx.js', 'xxx.combine.js', 'gbk');

    // 用node执行你这个打包脚本就ok啦～


### 高级使用指南

    var ModuleCompiler = require('module-compiler');

    ModuleCompiler.config({
        // 和KISSY一样，可以配置多个包
        packages: [{
            'name': 'app1',
            'path': 'app1这个包所在目录的绝对路径',
            // 这里是指app1这个包中的文件的编码，同一个包内的编码请保持一致
            'charset': 'gbk'
        }, {
            'name': 'app2',
            'path': 'app2这个包所在目录的绝对路径',
            // 这里是指app2这个包源码的编码
            'charset': 'utf-8'
        }],
        // 可以设置哪些模块不打包进来。注意，这里exclude的是具体的模块名，支持正则
        exclude: ['base', 'event'],
        // 如果是对一个目录下的所有文件进行打包，可以设置哪些文件不打包进来，支持正则。注意和上面的exclude的配置的区别。
        ignoreFiles: ['.combo.js', '-min.js'],
        // 输出的文件名后缀，不带.js，比如打包后你想输出为xxx.combine.js，那么这里就配置为：.combine
        suffix: '',
        // 类似于KISSY的map方法，可以自己定义把模块名中的路径进行替换
        map: [
            // 这样配置的话，那么，如果原先输出的app1的模块名中含有app1/2.0/字样的话，就会被替换成app1/19891014/
            ['app1/2.0/', 'app1/19891014/']
        ],
        // 这里设置的是最后打包出来的文件的编码，默认UTF-8，这里的设置相当于是全局设置，下面build中的设置是针对单一打包实例的
        charset: 'gbk'
    });

    /**
     * 打包一个文件/目录
     * @param inputPath {String} 源文件/目录的绝对路径.
     * @param outputPath {String} 打包出来的文件/目录的路径.
     * @param outputCharset {String} 输出编码，这里的设置会覆盖config.charset中的设置，默认UTF-8
     * @return {Object} 打包出来的文件信息
     */
    ModuleCompiler.build('xxx.js', 'xxx.combine.js', 'gbk');

### API汇总

* ModuleCompiler.config(cfg)：配置包，返回当前所有配置信息。如果不带参数，直接返回当前所有配置信息。
* ModuleCompiler.analyze(inputPath)：只分析该文件依赖，不打包。
* ModuleCompiler.build(inputPath, outputPath, outputCharset)：打包函数，具体见上面的说明
* ModuleCompiler.clean(): 可以清空config中的设置。因为ModuleCompiler是单例运行，所以如果出现一些特别情况，可以在config前执行clean方法清空之前的配置。

## CHANGELOG

[版本更新记录](https://github.com/daxingplay/ModuleCompiler/blob/master/HISTORY.md)

## License
遵守 "MIT"：https://github.com/daxingplay/ModuleCompiler/blob/master/LICENSE.md 协议