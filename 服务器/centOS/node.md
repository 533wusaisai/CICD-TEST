### Nodejs 安装 
在CentOS上安装Node.js，你可以使用NodeSource仓库，这是一个提供最新版本Node.js二进制分发的仓库，或者使用官方的包管理器yum或dnf（CentOS 8及以上版本）来安装通过EPEL提供的版本。以下是使用NodeSource仓库安装Node.js的步骤：

  1. 导入NodeSource仓库：
  > 首先，你需要导入NodeSource仓库。NodeSource提供了一个安装脚本，可以自动添加仓库并导入GPG密钥。以下命令将为你安装Node.js 16.x版本，你可以根据需要安装其他版本，只需将setup_16.x中的数字更改为你想要安装的版本号。

  `curl -sL https://rpm.nodesource.com/setup_16.x | sudo bash -`


  2. 安装Node.js：
    - 使用yum或dnf安装Node.js。

    `sudo yum install -y nodejs`
    - 或者在CentOS 8及以上版本中，你可以使用dnf：

  `sudo dnf install -y nodejs`

  3.  验证安装：
    > 安装完成后，你可以通过检查Node.js和npm的版本来验证安装是否成功。

    `node --version`

    `npm --version`
    > 如果你想要使用EPEL仓库中的Node.js版本，你可以按照以下步骤操作：

    - 添加EPEL仓库（如果尚未添加）：

    `sudo yum install epel-release`

  4. 安装Node.js：

    `sudo yum install nodejs`

请注意，EPEL仓库中的Node.js版本可能不是最新的。如果你需要最新版本的Node.js或特定版本，建议使用NodeSource仓库。

### NVM 安装 
在CentOS上安装NVM（Node Version Manager）是一个相对直接的过程。NVM允许你在同一台机器上安装和使用多个版本的Node.js。以下是安装NVM的步骤：

1. 下载NVM安装脚本：

    - 你可以使用curl或wget来下载NVM的安装脚本。如果你的系统中没有这些工具，你可以使用yum来安装它们。以下是使用curl的命令：

  `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`

  或者使用wget：
    
  `wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash`


2. 运行安装脚本：

 > 执行上面的命令将下载并运行NVM的安装脚本。该脚本会克隆NVM的仓库到`~/.nvm`，并添加必要的环境变量到你的shell配置文件中，比如 `~/.bash_profile、~/.zshrc、~/.profile或~/.bashrc`。

3. 更新你的当前会话：
    - 安装完成后，你需要关闭并重新打开你的终端，或者运行以下命令来更新当前会话的环境变量：

```js
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" // This loads nvm
```

4. 验证NVM安装：
    - 为了确认NVM已经正确安装，你可以运行：

    `command -v nvm`

   - 如果安装成功，这个命令应该输出nvm。

5. 使用NVM安装Node.js：
    - 一旦NVM安装完成，你可以安装Node.js的特定版本。例如，要安装Node.js的最新版本，你可以使用：

    `nvm install node`

    或者，如果你想安装特定版本的Node.js，你可以指定版本号：

    `nvm install 14.17.0`


6. 切换Node.js版本：
    - 你可以随时切换到不同版本的Node.js。例如，要切换到已安装的14.17.0版本，你可以使用：

    `nvm use 14.17.0`


7. 查看已安装的Node.js版本：
    - 要查看所有已安装的Node.js版本，你可以运行：

    `nvm ls`

8. 设置默认Node.js版本：
    - 你可以设置一个默认的Node.js版本，这样每次打开一个新的终端时都会自动使用这个版本。例如：

    `nvm alias default 14.17.0`

这些步骤应该能够帮助你在CentOS上安装和使用NVM来管理Node.js版本。记得每次打开一个新的终端窗口时，都要运行nvm use命令来选择一个特定版本的Node.js，或者设置一个默认版本以避免这个步骤。


### Git安装

在CentOS上安装Git，你可以使用yum或dnf（CentOS 8及以上版本）包管理器。以下是使用yum在CentOS上安装Git的步骤：

1. 打开终端：
    - 打开你的CentOS系统的终端。

2. 安装Git：
    - 输入以下命令来安装Git：

  `sudo yum install git`

3. 验证安装：
    - 安装完成后，你可以通过检查Git的版本来验证安装是否成功：

  `git --version`

> 如果你使用的是CentOS 8或更高版本，你可以使用dnf来安装Git：

1. 打开终端：
    - 打开你的CentOS系统的终端。

2. 安装Git：
    - 输入以下命令来安装Git：

  `sudo dnf install git`

3. 验证安装：
    - 安装完成后，你可以通过检查Git的版本来验证安装是否成功：

  `git --version`

这些命令会从CentOS的默认仓库中安装Git。如果你需要安装最新版本的Git，你可能需要添加一个第三方仓库，如IUS（Inline with Upstream Stable），来获取最新版本的Git。但是，对于大多数用户来说，使用默认仓库中的Git版本应该就足够了。