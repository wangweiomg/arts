## ARTS - Share 使用AI绘图

参考的这篇文章: [**低成本体验生成 AI 小姐姐照片**](https://medium.com/@croath/%E4%BD%8E%E6%88%90%E6%9C%AC%E4%BD%93%E9%AA%8C%E7%94%9F%E6%88%90-ai-%E5%B0%8F%E5%A7%90%E5%A7%90%E7%85%A7%E7%89%87-85ffa7c13cd7)

stable diffusion



### 等待安装

![image-20230221102407629](http://qiniu.honeywen.com/img/20230221102409.png)



但是报错了：

![image-20230221102805143](http://qiniu.honeywen.com/img/20230221102806.png)

提示 Read timed out .  就是下载pytorch 不成功，只能换个vpn节点再试试了。

这次安装pytorch成功，但是又报错了其他：

![image-20230221104630343](http://qiniu.honeywen.com/img/20230221104631.png)

在安装gfpgan 时候报错了，又是timed out 只能换换节点再试试.

经过切换好几个节点，终于运行成功：

![image-20230221113035921](C:\Users\Welto\AppData\Roaming\Typora\typora-user-images\image-20230221113035921.png)



根据提示，浏览器访问: http://127.0.0.1:7860  ,终于启动：



![image-20230221113320496](http://qiniu.honeywen.com/img/20230221113321.png)



### 安装model

根据提示，下载https://civitai.com/models/6424/chilloutmix ， 放到 models/Stable-diffusion/ 

安装扩展：https://github.com/civitai/sd_civitai_extension

![image-20230221115925766](http://qiniu.honeywen.com/img/20230221115926.png)

### 生成美女

![image-20230221121036581](http://qiniu.honeywen.com/img/20230221121037.png)



### 参数调优

根据已存在的来复制参数， 比如

![image-20230221200346386](http://qiniu.honeywen.com/img/20230221200348.png)

我们最终生成的：

![image-20230221200422567](http://qiniu.honeywen.com/img/20230221200424.png)





![image-20230221202839423](C:\Users\Welto\AppData\Roaming\Typora\typora-user-images\image-20230221202839423.png)



![image-20230221205843479](http://qiniu.honeywen.com/img/20230221205844.png)