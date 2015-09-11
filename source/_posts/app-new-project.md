title: 新app开发流程梳理
date: 2015-09-09 15:48:12
tags:
 - 工作
---
app开发流程
===========

1.  项目在哪？
=============
App项目统一存放于[app-trade-f2e](http://code.dianpingoa.com/groups/app-trade-f2e)组中；
更早之前的一些项目建在了[apollo-static](http://code.dianpingoa.com/groups/apollo-static)组中。

2.  项目创建
============
新项目环境统一由前端组接口人搭建，各团队只需将项目名称发给前端组接口人，接口人将按照规范创建一个初始化项目。


3.  环境
========
确保本地已经安装了[nodejs](http://www.nodejs.org/)、cortex和anywhere；
*  前往nodejs官网下载安装nodejs；
*  推荐安装淘宝镜像:npm install -g cnpm --registry=https://registry.npm.taobao.org ；
*  执行cnpm install cortex -g 安装全局cortex项目构建工具；cortex文档请猛戳此链接：*[http://book.ctx.io/zh-cn/index.html](http://book.ctx.io/zh-cn/index.html)*;
*  执行cnpm install anywhere -g安装静态服务器，优点是进入任何目录执行anywhere都会以当前目录为主目录启动服务；

4.  依赖
=========
进入项目所在目录执行命令安装项目依赖；
*  cnpm install ;
*  cortex install;

*注：*可直接执行项目中的install.sh完成除nodejs之外的所有安装；  sh install.sh

5.  本地调试
===========
*  cortex build编译代码；
*  执行anywhere启动服务；
*  打开浏览器输入页面地址预览，在url上加上mock=1启动mock本地调试；
*  模拟器调试需先将native代码checkout至本地编译后在调试工具（类似一个小蜘蛛的图标，在正上方中间位置）中找到"打开url"，输入要调试的页面进行查看；
   url中的格式为2种：dpapp: dpcrm://web?url=encode(你的url)；efte：dpcrm://efte?unit=&path=&query={}；请一定要掌握正确的打开姿势。
   native项目地址为：
   1.  ios：[http://code.dianpingoa.com/crm/crm-apollo-iphone](http://code.dianpingoa.com/crm/crm-apollo-iphone);
   2.  android：[http://code.dianpingoa.com/crm/crm-apollo-android](http://code.dianpingoa.com/crm/crm-apollo-android);

6.  目录结构
============

>html\_entries                      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_html文件存放目录，目录中按模块继续划分子目录_
&nbsp;&nbsp;&nbsp;&nbsp;moduleName  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_子模块目录_
&nbsp;&nbsp;&nbsp;&nbsp;...
js\_entries                         &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_此目录存放编译打包后的js，提供给cortex使用_
src                                 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_存放js源文件_
&nbsp;&nbsp;&nbsp;&nbsp;actions     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_存放页面交互的动作;_
&nbsp;&nbsp;&nbsp;&nbsp;components  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_组件_
&nbsp;&nbsp;&nbsp;&nbsp;constants   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_常量_
&nbsp;&nbsp;&nbsp;&nbsp;containers  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_容器_
&nbsp;&nbsp;&nbsp;&nbsp;entries     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_主入口，模块划分_
&nbsp;&nbsp;&nbsp;&nbsp;reducers    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_store数据分发_
&nbsp;&nbsp;&nbsp;&nbsp;utils       &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_工具集合_
mocks                               &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_模拟服务端数据_
less                                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_css样式存放目录，里面按照模块划分_
&nbsp;&nbsp;&nbsp;&nbsp;moduleName
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;index.less
&nbsp;&nbsp;&nbsp;&nbsp;...
img  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_图片存放目录，里面按照模块划分_

7.  开发模式和文档
==================
采用redux+react+es6+dpapp的模式开发；可参照项目有[apollo-rotate-static](http://code.dianpingoa.com/app-trade-f2e/apollo-rotate-static)；资料请参照：
redux：[http://camsong.github.io/redux-in-chinese/index.html](http://camsong.github.io/redux-in-chinese/index.html);
react：[http://reactjs.cn/react/docs/getting-started.html](http://reactjs.cn/react/docs/getting-started.html);
es6：[http://es6.ruanyifeng.com/](http://es6.ruanyifeng.com/);
dpapp：[http://efte.github.io/dpapp/#概述](http://efte.github.io/dpapp/#概述);

*注：*以上提供的均为中文资料。

8.  数据交互
============
新建项目中，utils包内会包含一个名叫fetch.js的文件，actions中的js调用此文件中的fetch方法动态获取数据，数据来源分为两种，一种是mock数据，从本地mocks文件夹中获取；另一种是通过jsbrige的方式从服务端上获取。具体调用方式如下：

    import fetch from '../utils/fetch';
    import querystring from 'querystring';

    export const FETCH_MESSAGE_DETAIL = 'FETCH_MESSAGE_DETAIL';

    export function fetchMessageDetail(params) {
        return async (dispatch) => {
            try {
                let data = await fetch('/message-center/detail?' + querystring.stringify(params));
                dispatch({
                    type: FETCH_MESSAGE_DETAIL,
                    data: data
                })
            } catch (ex) {
                console.dir(ex);
            }
        }
    }


9. 公共样式库
============
目前移动端没有一个定制性自己的样式库和组件库；
不过可以暂时用开源的，比如[amazeui for react](http://amazeui.org/react/getting-started);

10.  其他注意事项
================
移动端并非像pc端有用强大的硬件环境，所以在开发时，逻辑处理、文件大小、请求量尽量精简，不易复杂化；

一定一定要遵守开发规范。
