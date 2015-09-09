title: 常见坑集合
date: 2015-09-01 20:06:09
tags:
 - 工作
---

1. Grid布局的fluid属性
====================
如果Grid不是最顶层，需要和父组件的宽度适应的时候，要在Grid的属性上设置fluid={true}才能保证想要的适应宽度。

    <Grid fluid={true} />

2. 关于reducer的清理值
===================
可以用双向绑定另外一个方法：this.manualChange(path,value)来做清空

3. cortex结合
===================
cortex.json中不要有main和entries的配置，直接配一个css空文件即可，但是要在‘dist/’里面有这个文件，可以不用他。

      "css":"dist/css/empty.css"