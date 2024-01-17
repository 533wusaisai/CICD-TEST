## docker 安装 jenkins
  - 拉取镜像 最新版本
    `docker pull jenkins/jenkins:lts`
  - 运行容器  将jenkins 运行在 8080 端口 将jenkins 数据存储在主机的jenkins目录下
    `docker run -d -p 8080:8080 -p 50000:50000 -v /jenkins:/var/jenkins_home --name jenkins jenkins/jenkins:lts`
    or
    `docker run -p 8080:8088 -p 50000:50000 jenkins/jenkins`
  - 获取初始管理员密码
    `docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword`
    
启动 `docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home --name jenkins jenkins/jenkins:lts`