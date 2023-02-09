## ARTS - Share  使用docker部署python web工程

### 步骤

1. 创建一个flask app，并运行
2. 编写Dockerfile, 制作 docker image
3. 运行container

#### 创建一个flask app

首先根据官网安装python依赖的Flask。

之后参考官网：[Quickstart](https://flask.palletsprojects.com/en/2.2.x/quickstart/)

* 新建文件夹 ```first_py_docker```
* 在文件夹 first_py_docker下新建 ```src```目录
* 在src目录下新建```requirements.txt```，并写入 ```flask==2.2.2```
* 在src目录下新建```main.py```文件
* 在 main.py里输入flask官网示例:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<h1>hello world!</h1>"
```

* 运行flask

![image-20230129125306316](http://qiniu.honeywen.com/img/image-20230129125306316.png)

浏览器访问：

![image-20230129125340482](http://qiniu.honeywen.com/img/image-20230129125340482.png)

成功！



#### 编写Dockerfile, 制作 docker image

根据[docker官网Dockerfile文档](https://docs.docker.com/engine/reference/builder/) 来制作Dockerfile

在first-py_docker 目录下新建文件 Dockefile , 根据文档编写如下：

```dockerfile
FROM python:3.8-slim-buster 

WORKDIR /src

COPY src/requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt 

COPY . .

CMD [ "python", "-m", "flask", "--app=src/main", "run"]
```

接下来根据Dockerfile制作 docker image 

```docker build --tag first-py-docer .```



#### 运行container

根据上一步制作成功的image, 把它运行起来 , 参考 [docker run](https://docs.docker.com/engine/reference/run/)

```docker run -p 5000:5000 first-py-docer   ```



![image-20230129130621257](http://qiniu.honeywen.com/img/image-20230129130621257.png)

报错，说5000端口已经被使用了， 我们先改变端口为4000，先运行成功再回头看问题。

去flask官网找如何[改变端口的文档 ](https://flask.palletsprojects.com/en/2.2.x/cli/#setting-command-options)， 知道了使用 --port 4000 来改端口，我们重新运行 flask app测试成功，此时要修改Dockerfile的命令

``` CMD [ "python", "-m", "flask", "--app=src/main", "run", "--port=4000"]```，

重新build image (注意先删掉老的image或换个名字)

``````shell
docker run -p 4000:4000 first-py-docker
``````

运行成功， 浏览器访问 ，发现无法正常返回。

![image-20230129131732958](http://qiniu.honeywen.com/img/image-20230129131732958.png)

去查找问题，找到了这个答案： [Deploying a minimal flask app in docker - server connection issues](https://stackoverflow.com/questions/30323224/deploying-a-minimal-flask-app-in-docker-server-connection-issues)

回答说是这样：

![image-20230129131952618](http://qiniu.honeywen.com/img/image-20230129131952618.png)

问题是绑定了localhost接口，你如果想要docker外部访问容器，需要绑定到0.0.0.0, 改变方法是 flask 指定host ， 我们可以在启动flask时候指定，修改Dockerfile的CMD 命令如下：

```CMD [ "python", "-m", "flask", "--app=src/main", "run", "--port=4000", "--host=0.0.0.0"]```

重新build ,run ，启动成功，浏览器访问成功！。



源码地址github : https://github.com/wangweiomg/first-py-docker



### 错误分析

* 报错本地 5000端口占用，但是为何flask 本地起来能访问？
* 为什么需要改变flask host才能在外访问docker?







