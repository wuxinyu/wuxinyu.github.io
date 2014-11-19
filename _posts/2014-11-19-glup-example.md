---
title: gulp example
layout: post
category : js
tagline: js
tags : [dev] 
---

  <p>
  var gulp = require('gulp');<br />
<br />
&nbsp; // 引入组件<br />
&nbsp; var less = require('gulp-less'), &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;// less<br />
&nbsp; &nbsp; &nbsp; minifycss = require('gulp-minify-css'), // CSS压缩<br />
&nbsp; &nbsp; &nbsp; uglify = require('gulp-uglify'), &nbsp; &nbsp; &nbsp; &nbsp;// js压缩<br />
&nbsp; &nbsp; &nbsp; concat = require('gulp-concat'), &nbsp; &nbsp; &nbsp; &nbsp;// 合并文件<br />
&nbsp; &nbsp; &nbsp; rename = require('gulp-rename'), &nbsp; &nbsp; &nbsp; &nbsp;// 重命名<br />
&nbsp; &nbsp; &nbsp; clean = require('gulp-clean'); &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;//清空文件夹<br />
<br />
&nbsp; // less解析<br />
&nbsp; gulp.task('build-less', function(){<br />
&nbsp; &nbsp; gulp.src('./javis/static/less/lib/s-production.less')<br />
&nbsp; &nbsp; &nbsp; .pipe(less())<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/lib/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./javis/static/less/lib/s-skins.less')<br />
&nbsp; &nbsp; &nbsp; .pipe(less())<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/lib/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./javis/static/less/lib/s/s.less')<br />
&nbsp; &nbsp; &nbsp; .pipe(less())<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/lib/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./javis/static/less/*.less')<br />
&nbsp; &nbsp; &nbsp; .pipe(less())<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/'))<br />
&nbsp; });<br />
<br />
&nbsp; // 合并、压缩、重命名css<br />
&nbsp; gulp.task('stylesheets',['build-less'], function() {<br />
&nbsp; &nbsp; &nbsp; // 注意这里通过数组的方式写入两个地址,仔细看第一个地址是css目录下的全部css文件,第二个地址是css目录下的areaMap.css文件,但是它前面加了!,这个和.gitignore的写法类似,就是排除掉这个文件.<br />
&nbsp; &nbsp; gulp.src(['./javis/static/build/css/*.css','!./javis/static/build/css/areaMap.css'])<br />
&nbsp; &nbsp; &nbsp; .pipe(concat('all.css'))<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/'))<br />
&nbsp; &nbsp; &nbsp; .pipe(rename({ suffix: '.min' }))<br />
&nbsp; &nbsp; &nbsp; .pipe(minifycss())<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css'));<br />
&nbsp; });<br />
<br />
&nbsp; // 合并，压缩js文件<br />
&nbsp; gulp.task('javascripts', function() {<br />
&nbsp; &nbsp; gulp.src('./javis/static/js/*.js')<br />
&nbsp; &nbsp; &nbsp; .pipe(concat('all.js'))<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js'))<br />
&nbsp; &nbsp; &nbsp; .pipe(rename({ suffix: '.min' }))<br />
&nbsp; &nbsp; &nbsp; .pipe(uglify())<br />
&nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js'));<br />
&nbsp; });<br />
<br />
&nbsp; // 清空图片、样式、js<br />
&nbsp; gulp.task('clean', function() {<br />
&nbsp; &nbsp; return gulp.src(['./javis/static/build/css/all.css','./javis/static/build/css/all.min.css'], {read: false})<br />
&nbsp; &nbsp; &nbsp; .pipe(clean({force: true}));<br />
&nbsp; });<br />
<br />
&nbsp; // 将bower的库文件对应到指定位置<br />
&nbsp; gulp.task('buildlib',function(){<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/angular/angular.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/angular/angular.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/bootstrap/dist/js/bootstrap.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/jquery/dist/jquery.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/angular-route/angular-route.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/angular-animate/angular-animate.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/angular-bootstrap/ui-bootstrap.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/angular-bootstrap/ui-bootstrap-tpls.min.js')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/js/'))<br />
<br />
&nbsp; &nbsp; //--------------------------css-------------------------------------<br />
<br />
&nbsp; &nbsp; gulp.src('./javis/static/less/fonts/*')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/fonts/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/bootstrap/fonts/*')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/fonts/'))<br />
<br />
&nbsp; &nbsp; gulp.src('./bower_components/bootstrap/dist/css/bootstrap.min.css')<br />
&nbsp; &nbsp; &nbsp; &nbsp; .pipe(gulp.dest('./javis/static/build/css/lib'))<br />
<br />
&nbsp; });<br />
<br />
&nbsp; // 定义develop任务在日常开发中使用<br />
&nbsp; gulp.task('develop',function(){<br />
&nbsp; &nbsp; gulp.run('buildlib','build-less','javascripts','stylesheets');<br />
<br />
&nbsp; &nbsp; gulp.watch('./javis/static/less/*.less', ['build-less']);<br />
&nbsp; });<br />
<br />
&nbsp; // 定义一个prod任务作为发布或者运行时使用<br />
&nbsp; gulp.task('prod',function(){<br />
&nbsp; &nbsp; gulp.run('buildlib','build-less','stylesheets','javascripts');<br />
<br />
&nbsp; &nbsp; // 监听.less文件,一旦有变化,立刻调用build-less任务执行<br />
&nbsp; &nbsp; gulp.watch('./javis/static/less/*.less', ['build-less']);<br />
&nbsp; });<br />
<br />
&nbsp; // gulp命令默认启动的就是default认为,这里将clean任务作为依赖,也就是先执行一次clean任务,流程再继续.<br />
&nbsp; gulp.task('default',['clean'], function() {<br />
&nbsp; &nbsp; gulp.run('develop');<br />
&nbsp; });
</p>
<p>
  <br />
</p>