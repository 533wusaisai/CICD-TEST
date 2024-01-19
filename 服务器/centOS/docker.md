### Docker 安装
  在CentOS上安装Docker，你可以遵循以下步骤。请注意，从CentOS 8开始，Docker已经被其上游企业版的Red Hat Enterprise Linux官方替换为Podman和Buildah。但是，你仍然可以在CentOS 7和CentOS 8上安装Docker CE（社区版）。

  #### 1. CentOS 7
  - 卸载旧版本的Docker（如果有的话）：
  ```js
  sudo yum remove docker \
               docker-client \
               docker-client-latest \
               docker-common \
               docker-latest \
               docker-latest-logrotate \
               docker-logrotate \
               docker-engine
  ```


  - 安装所需的包：
  `sudo yum install -y yum-utils`

  - 设置Docker仓库：
  `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

  - 安装Docker CE：
  `sudo yum install docker-ce docker-ce-cli containerd.io`

  - 启动Docker守护进程：
  `sudo systemctl start docker`

  - 验证Docker是否正确安装：
  `sudo docker run hello-world`

  -设置Docker开机自启：
  `sudo systemctl enable docker`

  #### 2. CentOS 8
  > 对于CentOS 8，Docker官方不再提供支持，但你可以使用以下方法来安装Docker CE：
  - 卸载旧版本的Docker（如果有的话）：

  ```js
  sudo dnf remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine
  ```

  -设置Docker仓库：
  `sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo`

  - 安装Docker CE：
  `sudo dnf install docker-ce docker-ce-cli containerd.io`

  - 启动Docker守护进程：
  `sudo systemctl start docker`

  - 验证Docker是否正确安装：
  `sudo docker run hello-world`

  - 设置Docker开机自启：
  `sudo systemctl enable docker`

  - 用户组
  >为了避免每次调用Docker命令时都需要使用sudo，你可以将你的用户添加到docker组中：
  `sudo usermod -aG docker your-user`
  > 替换your-user为你的用户名。之后，你需要注销并重新登录，或者重启系统以使这些更改生效。

  >注意事项
  >在CentOS 8上，可能会有一些与cgroup驱动的兼容性问题，因为CentOS 8默认使用cgroup v2，而Docker默认期望使用cgroup v1。如果遇到这个问题，你可能需要调整Docker的配置或系统的cgroup设置。
  >如果你在安装过程中遇到任何问题，确保你的系统是最新的，并且已经安装了所有必要的依赖项。可以通过运行sudo yum update（CentOS 7）或sudo dnf update（CentOS 8）来更新系统。





### Jenkins 镜像安装

在 CentOS 系统上使用 Docker 安装 Jenkins 的步骤如下：

1. 拉取 Jenkins Docker 镜像：

    从 Docker Hub 上拉取最新的 Jenkins 镜像：

  `sudo docker pull jenkins/jenkins:lts`

  这里使用的是 Jenkins 的长期支持版本（LTS）。

2. 运行 Jenkins 容器：

  使用以下命令运行 Jenkins 容器：

  `sudo docker run -d -p 8080:8080 -p 50000:50000 --name jenkins -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts`


>这个命令会创建一个名为 jenkins 的容器，并将容器的 8080 端口映射到主机的 8080 端口，这样你就可以通过浏览器访问 Jenkins。同时，将容器的 50000 端口映射到主机的 50000 端口，用于 Jenkins 代理的通信。-v jenkins_home:/var/jenkins_home 选项将 Jenkins 数据持久化到 Docker 卷 jenkins_home。

3. 获取初始管理员密码：

  Jenkins 首次启动时会生成一个初始的管理员密码，你需要这个密码来完成安装过程。使用以下命令获取密码：

  `sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword`


  > 复制显示的密码，你将在浏览器中设置 Jenkins 时使用它。

4. 访问 Jenkins：

    打开你的浏览器，访问 http://<你的服务器IP>:8080。你应该会看到 Jenkins 的安装向导。在要求输入初始管理员密码的地方，粘贴你之前复制的密码。

5. 完成安装：

    按照安装向导的指示完成安装。你可以选择安装推荐的插件集，或者自定义选择插件。之后，创建一个管理员用户，并配置 Jenkins 的实例。

6. 配置 Jenkins：
    安装完成后，你可以开始配置 Jenkins，包括创建新的构建作业、安装额外的插件等。

> 请确保你的服务器防火墙设置允许访问 8080 和 50000 端口。如果你在使用云服务，你可能还需要在云服务的安全组设置中允许这些端口的流量。



