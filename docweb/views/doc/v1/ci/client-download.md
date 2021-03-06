##使用 tce 客户端

#### **TenxCloud tce 客户端适合以下用户使用**
 1. 没有把代码托管到GitHub或者BitBucket等代码托管平台上，只有本地的代码或者可部署的应用，希望可以尝试和使用时速云提供的镜像和容器服务的用户
 2. 偏好通过客户端和TenxCloud container engine进行交互的用户
 注意：我们的客户端可以支持 Windows、Linux和 Mac 三种平台

#### **如何使用 tce 客户端**
1.   在 [www.tenxcloud.com](https://www.tenxcloud.com "时速云") 上注册用户
2.   登录并进入第二个功能块 “构建”，点击绿色按钮“下载客户端”，在后面的页面中选择客户端的运行平台下载即可
![tce1](../images/ci/tce-1.png)

3.   为了使用方便，下载后将tce的路径加入到系统的PATH下，以便在任意目录均可运行。可以参考以下步骤：</br>
 **Windows**: [添加到系统Path](http://jingyan.baidu.com/article/db55b6099d1e0d4ba30a2fc0.html)</br>
 **Linux**:  [添加到系统Path](http://zhidao.baidu.com/link?url=psqItfkdfNFruHE9WS-phqcjqyYyyzOPHbvIquTCib_EdSTRz1Xpp4BYs0zsBxYh8yZvE-w33BdKxLKEV9nyqK)</br>
 **Mac**:  修改 /etc/paths，将tce所在路径加上

#### **tce 用法说明书**

 **tce [选项]**<br/>
*   `exec <app_name> <command>...` 在应用内部执行Linux命令
*   `help`    查看帮助，列出所有tce命令<br/>
*   `login`   登录 TenxCloud 容器引擎<br/>
*   `logout`  退出 TenxCloud 容器引擎<br/>
*   `logs <project Name>`    查看指定构建镜像的最近一次构建日志<br/>
*   `projects`  查看使用tce部署的项目，使用参数 '-a'  查看TenxCloud上所有构建的项目<br>
*   `ps`                  查看当前账户下的应用
*   `push <project Name>:<tag>`    构建镜像，将本地的文件push到TenxCloud
 容器引擎并构建镜像<br/>
*   `run` 根据本地的json文件，创建一个时速云应用
*   `version`查看当前版本，如果有新版本，会提示更新



注：我们还在继续添加更多更酷的选项，让您更便捷的通过终端操作TenxCloud 容器引擎，尽请期待！

tce 使用详解：
 1. 通过终端进入代码目录，该目录下必须包含Dockerfile。时速云提供了中文版的 [如何编写Dokerfile](../faq/dockerfile.html)，您可以参考Docker英文官方文档 -> [编写Dockerfile](http://docs.docker.com/reference/builder/)。
 2. 输入<span style="color: #0000ff;">`tce login`</span>，填写用户、密码后完成登录
 3. 输入<span style="color: #0000ff;">`tce push <project Name>:<tag>`</span>，客户端会自动将Dockerfile及引用的本地文件打包成zip，并上传到TenxCloud，由我们的容器引擎创建Docker 项目。这个过程会持续输出Docker build 的相关日志，方便跟踪镜像构建进度。
 注意：在这个过程中，我们会解析Dockerfile里的ADD 和COPY 指令集，打包所依赖的文件和目录，并且指令的源路径必须使用相对路径，不支持使用绝对路径，暂不支持添加空目录，比如：<br/>
 `ADD . /opt` 将当前目录下所有文件一起打包<br/>
 `ADD testDir/. /opt` 将项目 testDir目录下所有文件打包<br/>
 `ADD testDir/ /opt` 将项目testDir目录下所有文件打包<br/>
 `ADD testDir/file*.txt /opt` * 匹配<br/>
 `ADD testDir/file?.txt /opt` ? 匹配

 5. 如果构建项目过程中连接出了问题，中断了日志输出，或者想查看某一项目的构建过程，输入<span style="color: #0000ff;">`tce logs <project name>`</span>，继续查看构建日志。
 6. 镜像创建成功后，你可以通过
 [镜像控制台](https://www.tenxcloud.com/console/docker-registry) -> “我的镜像” 查看；还可以定义该镜像的服务接口，比如容器端口、环境变量等。
![tce1](/doc/v1/images/samples/port_path.png)
 5. 接下来我们就可以根据这个镜像创建容器了，一种方法是进入到 “容器”
 控制台，令一种方法是通过`tce run`创建容器。这样我们就成功启动了一个容器服务啦！
![tce1](/doc/v1/images/samples/tce_start.png)
 4. 最后输入<span style="color: #0000ff;">`tce projects`</span>，查看通过tce构建的所有项目。或者输入<span style="color: #0000ff;">`tce projects -a`</span>，查看所有构建项目（包括GitHub等）
 8. 如果想对应用内部进行操作，可以使用`tce exec`命令来查询日志，调试问题等等。
