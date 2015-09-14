title: 商家平台及相关
date: 2015-09-09 15:40:07
tags:
 - 工作
---

商家平台方面目前有两套框架在用
=============

一：用的neuron.js.  简而言之js中看到DP的项目均为此类。
代表项目是闪惠账单，git地址：[git@code.dianpingoa.com:f2e/bc-trade-finance-static.git](git@code.dianpingoa.com:f2e/bc-trade-finance-static.git)
	二：新的cortex框架部分，依赖jquery。
代表项目是点付宝,git地址：[git@code.dianpingoa.com:f2e-merchant/app-merchant-tool.git](git@code.dianpingoa.com:f2e-merchant/app-merchant-tool.git)
   相比较而言新的项目相对后端更容易上手，jquery的语法，缺点在于目前可用组件较少，前端组正在解决。
老的项目neuron的语法与jquery类似但有不同短期不易掌握，优点在于经过前期的积累，组件比较成熟。
开发环境：
目前商家平台方面前端IDE没特别要求，webstorm，sublime均可
开发步骤：
	1）首先应该装node.js （下载安装包即可），通过npm -v查看是否成功。
    2） 全局安装npm和cortex，依次执行
				npm install  -g
       			npm install cortex -g(可能需要sudo权限)
    3） 拉静态项目，进入项目根目录依次执行
				npm intsall （可用 cnpm install代替。cnpm为淘宝npm镜像，国内速度快，但是有时没最新的版本时，换npm install  cnpm地址：http://npm.taobao.org/）
				cortex install  (如果报错可能需要sudo权限，遇到过这样的情况)
	4）可以编辑的部分：
			老项目：
            对应js 写在 js目录下
			css 写在 less 目录下：不可直接写在css目录下，less 编译后会覆盖掉原有的css
	        具体可以查看gulpfile.js  gulp相关文档：[http://www.gulpjs.com.cn/docs/getting-started/](http://www.gulpjs.com.cn/docs/getting-started/)。
            新项目目录一般如下：
             js ——>src/js
	         css ——> src/less
	5）调试：
	关于chrome下js的调试，可以参考[http://www.jb51.net/article/58570.htm](http://www.jb51.net/article/58570.htm)。
	6）关于代理，
	为了避免每次都要发beta调试，可以设置本地代理用把beta 环境的文件映射到本地文件，两种项目有不同的代理方式具体参考
*   [商家平台本地代理](/2015/09/09/localProxy/)。
	7）上线流程简介
*   [cortex及其他坑综合]( http://efte.github.io/dpapp/#)