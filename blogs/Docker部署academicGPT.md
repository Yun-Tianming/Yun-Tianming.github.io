# Docker部署academicGPT

**已有环境**：海外服务器，Docker。

主要参考链接： 

[海外云服务器(基于阿里云)Linux和公网解决方案 · Issue #386 · binary-husky/gpt_academic](https://github.com/binary-husky/gpt_academic/issues/386#issuecomment-1506789408)

- 创建指定目录并进入

```python
mkdir /mydynamicGPT
cd /mydynamicGPT
```

- clone仓库并进入

```python
git clone https://github.com/binary-husky/chatgpt_academic.git
cd chatgpt_academic
```

- 修改配置文件，更详细信息见项目主要介绍，主要内容如下：

```python
# Your Key是自己的api key，此处位于第二行
API_KEY = "Your Key"

# 由于我是国外的服务器，所以USE_PROXY设置的是false，即默认值未做修改
USER_PROXY = False

# 修改自己的暴露端口，我选的是443，此处位于37行
WEB_PROT = 443
```

- 拷贝配置文件，据说更好保护隐私

```python
cp config.py config_private.py
```

- **可选，修改镜像源** 国外服务器访问DockerFile里的阿里镜像可能会慢，所以我去掉了其对镜像源的指定，如果也需要取消，可参考以下步骤：首先确保自己位于chatgpt_academic目录下，执行命令`vim Dockerfile`，删除3~5行的内容，结果如下

```python
FROM python:3.11
  
WORKDIR /gpt
COPY requirements.txt .
RUN pip3 install -r requirements.txt

COPY . .

CMD ["python3", "-u", "main.py"]
```

- 打包Docker镜像，和master教程一致，命令为：

```python
docker build -t gpt-academic .
```

- 运行Docker镜像，使用以下命令：

```python
# --name可选，是给容器起的名字，-d是为了让他后台运行，443:443是为了把端口暴露出去
# 请确保暴露的端口telnet的通，同时此端口没有其他服务运行
docker run -itd --name chat -p 443:443 gpt-academic
```

- **可选 如果要更新怎么办** 比如说key要换，或者某个配置错了，可以修改文件之后，按照第六步打一个新包，然后执行以下三条命令：

```python
# 暂停原容器，如果没有设置名字，那这里的chat就用gpt-academic
docker stop chat

# 删除原容器，如果没有设置名字，那这里的chat就用gpt-academic
docker rm chat

# 重新执行第七步的命令
docker run -itd --name chat -p 443:443 gpt-academic
```


<br>


{% include article.html %}


<br>

<!--头像获取：https://github.com/walinejs/waline/issues/532-->

当前页阅读量为:
<span class="waline-pageview-count" data-path="/_includes/Waline.html" />