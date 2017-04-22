---
layout: post
title: 前端搭建Nodejs+npm+gulp开发环境
category: Node.js
tags: Node.js
description: 前端搭建Nodejs+npm+gulp开发环境
---
### 一 安装NodeJS
NodeJS官网地址：https://nodejs.org/zh-cn/  
首先到NodeJS的官网下载安装文件，如果是windows的机子，会得到msi的安装文件，双击之后一路next就可以了。  
安装完成后，在命令行执行node -v(显示NodeJS版本)和npm -v(显示npm版本)查看NodeJS是否安装成功。  
NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题。

### 二 全局安装gulp
##### gulp简介
gulp是前端开发过程中对代码进行构建的工具，是自动化项目的构建利器；她不仅能对网站资源进行优化，而且在开发过程中很多重复的任务能够使用正确的工具自动完成；使用她，我们不仅可以很愉快的编写代码，而且大大提高我们的工作效率。 

gulp是基于Nodejs的自动任务运行器， 她能自动化地完成javascript/coffee/sass/less/html/image/css 等文件的的测试、检查、合并、压缩、格式化、浏览器自动刷新、部署文件生成，并监听文件在改动后重复指定的这些步骤。在实现上，她借鉴了Unix操作系统的管道（pipe）思想，前一级的输出，直接变成后一级的输入，使得在操作上非常简单。

##### 安装
在命令行执行npm install -g gulp,即可全局安装gulp，若只想在单个项目中使用gulp，只需将-g去掉即可，但一般情况下都会选择全局安装gulp工具。  
在命令行运行gulp -v 即可查看gulp的版本号，从而查看gulp是否全局安装成功。 

### 初始化项目
命令行中切换到项目的根目录，运行npm init 根据提示填写相关信息，最终会生成一个package.json，在package.json中填入
```
{
  "name": "chac",
  "version": "1.0.0",
  "description": "chac",
  "author": "chac",
  "license": "ISC",
  "devDependencies": {
    "browser-sync": "^2.18.8",
    "gulp": "^3.9.1",
    "gulp-concat": "^2.6.1",
    "gulp-jshint": "^2.0.4",
    "gulp-less": "^3.3.0",
    "gulp-minify-css": "^1.2.4",
    "gulp-rename": "^1.2.2",
    "gulp-uglify": "^2.0.0",
    "gulp-util": "^3.0.8",
    "jshint": "^2.9.4",
    "jshint-stylish": "^2.2.1"
  }
}
```
在此目录下运行，npm install ，npm就会根据package.json下载依赖到本地的node_modules文件夹下

### gulp 的使用
在项目根目录新建gulpfile.js用来写运行脚本。
```
var gulp = require('gulp');

var jshint = require('gulp-jshint'); //js代码校验
var stylish=require('jshint-stylish');
var less = require('gulp-less'); //less的编译
var concat = require('gulp-concat'); //合并文件
var uglify= require('gulp-uglify'); //压缩js代码
var rename= require('gulp-rename'); //文件重命名
var minifyCss=require('gulp-minify-css'); //压缩css
var gutil=require('gulp-util'); //进行错误处理，监听 error 事件进行错误处理从而避免 gulp 进程崩溃
var browserSync=require('browser-sync').create();
var reload=browserSync.reload;

//开发环境目录
var devPaths = {
    js: 'develop/javascripts/',
    css: 'develop/stylesheets/'
}

//生产环境目录
var productPaths={
    js:'public/javascripts/',
    css:'public/stylesheets/',
    pug:'views'
}

//检查脚本语法
gulp.task('lint',function(){
     gulp.src(devPaths.js+'*.js')
        .pipe(jshint())
        .pipe(jshint.reporter(stylish));
});


//编译Less，压缩css
gulp.task('less',function(){
    gulp.src(devPaths.css+'*.less')
        .pipe(less())
        .pipe(concat('all.css'))
       .pipe(gulp.dest(productPaths.css))      //开发时需要关注文件压缩顺序，生产环境可以注释
        .pipe(rename('all.min.css'))
        .pipe(minifyCss())
        .pipe(gulp.dest(productPaths.css));
});


//合并，压缩文件
gulp.task('scripts',function(){
    gulp.src([devPaths.js+'*.js',devPaths.js+'**/*.js'])

        .pipe(concat('all.js'))
        .on('error', function(err) {
            gutil.log('JS Error!', err.message);
            this.end();
        })
       .pipe(gulp.dest(productPaths.js))     //开发时需要关注文件压缩顺序，生产环境可以注释
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .on('error', function(err) {
            gutil.log('JS Error!', err.message);
            this.end();
        })
        .pipe(gulp.dest(productPaths.js));
});

gulp.task('default',function(){
 //   var server=livereload();
    gulp.start('lint','less','scripts');
    gulp.watch(devPaths.css+'**/*',['less']).on('change',reload);
    gulp.watch(devPaths.js+'**/*',['lint','scripts']).on('change',reload);
//用 './xx' 开头作为当前路径开始，会导致无法监测到新增文件，需省略掉 './'
    gulp.watch('views/**/*.*').on('change',reload);
    browserSync.init({
        proxy: 'http://localhost:3000',
        port: 3000,
        files: [productPaths.css,productPaths.js,productPaths.pug]
    })
    console.log('Watching files changes');

});

```
然后在项目根目录运行gulp即可检测开发环境的less文件，js文件，html文件。  
##### less文件变化
在less文件发生改动时，gulp会自动检测less文件的变动，并将其解释为css，并压缩，并刷新页面。
##### js文件变化
在js文件发生改动时，gulp会自动检测js文件的变动，并检查其语法，并重命名压缩，并刷新页面。
##### html文件发生变化
在html文件发生变化时，gulp会自动检测html文件的变动，并刷新页面。  

    就这样，一个简易的前端开发环境就搭建好啦 ^_^

