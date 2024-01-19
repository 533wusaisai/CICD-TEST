### Nginx安装
CentOS系统安装Nginx可以通过多种方式进行，包括使用官方的包管理器yum或者dnf（CentOS 8及以上版本），编译安装，或者使用第三方仓库如EPEL。以下是使用yum和EPEL仓库安装Nginx的步骤：

1. 添加EPEL仓库：
  > 如果你的系统中还没有EPEL仓库，你需要先添加它。EPEL（Extra Packages for Enterprise Linux）提供了许多在标准CentOS仓库中不可用的额外软件包。

  `sudo yum install epel-release`

2. 安装Nginx：
  >一旦EPEL仓库被添加，你可以使用yum来安装Nginx。

  `sudo yum install nginx`

3. 启动Nginx服务：
  > 安装完成后，你需要启动Nginx服务。

  `sudo systemctl start nginx`

  `sudo systemctl restart nginx` 
  or
  `sudo systemctl reload nginx`


4. 设置开机启动：
  > 如果你希望Nginx在系统启动时自动运行，可以使用以下命令来设置。

  `sudo systemctl enable nginx`


5. 调整防火墙设置：
> 如果你的系统运行着防火墙，你需要允许HTTP和HTTPS流量。

```js
sudo firewall-cmd --permanent --zone=public --add-service=http
sudo firewall-cmd --permanent --zone=public --add-service=https
sudo firewall-cmd --reload
```

6. 验证安装：
  >你可以通过访问你的服务器的公共IP地址或域名来验证Nginx是否成功安装。在Web浏览器中输入以下内容：

  `http://your_server_ip_or_domain/`


如果安装成功，你应该能看到Nginx的默认欢迎页面。

请注意，以上步骤适用于CentOS 7。如果你使用的是CentOS 8或更高版本，可能需要使用dnf代替yum，但是步骤基本相同。

如果你需要更高级的配置或者特定版本的Nginx，你可能需要考虑编译安装或者使用Nginx官方的仓库。编译安装会更复杂，但是它允许你自定义安装选项和模块。

### Nginx 配置
```js
server {
        listen 80; # 监听 80 端口

        server_name example.com; # 你的域名

        location / {
            proxy_pass http://localhost:3000; # Web 服务端地址和端口
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```
在这个配置中：

- listen 80; 指定 Nginx 监听 80 端口，这是 HTTP 的默认端口。
- server_name 指定了域名，你应该将其替换为你自己的域名或者 IP 地址。
- location / 定义了一个位置块，用于匹配所有进入的请求。
- proxy_pass 指令将请求转发到指定的地址和端口，这里应该替换为你的 Web 服务端的实际地址和端口。
- 其他 proxy_set_header 指令用于设置一些 HTTP 头信息，以确保请求在转发时保持正确的信息。
> 请注意，如果你的 Web 应用使用 WebSocket，proxy_set_header 指令中的 Upgrade 和 Connection 设置是必要的，以确保 WebSocket 连接能够正确建立。

```js
server {
    listen 80;
    server_name jenkins.example.com; # 替换为你的 Jenkins 域名

    location / {
        proxy_set_header        Host $host:$server_port;
        proxy_set_header        X-Real-IP $remote_addr;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header        X-Forwarded-Proto $scheme;
        proxy_redirect          http:// https://;
        proxy_pass              http://localhost:8080; # 假设 Jenkins 运行在同一台机器的 8080 端口
        # Required for new HTTP-based CLI
        proxy_http_version      1.1;
        proxy_request_buffering off;
        # workaround for Jenkins bug with HTTPS
        proxy_buffering         off;
    }
}
```

### Nginx 命令