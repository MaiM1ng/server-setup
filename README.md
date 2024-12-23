# server-setup

需要安装的

1. zsh & oh my zsh
2. neovim & lazyvim
3. yazi
4. nerdfont
5. clash
6. tmux & oh my tmux

## clash

### 借用

clash可以选择直接走本地的代理

通过

```bash
function host_proxy_set() {
    HOST_NAME=172.31.80.1
    # HOST_NAME=$(cat /etc/resolv.conf | grep "nameserver" | awk {'print $2'})
    echo server is $HOST_NAME
    export https_proxy="$HOST_NAME:7890"
    export http_proxy="$HOST_NAME:7890"
}

alias host_proxy_set="host_proxy_set"
alias host_proxy_unset="proxy_unset"
```

### 本机配置Clash

直接复用项目: [clash for linux](https://github.com/wnlen/clash-for-linux)

但是该项目用在`zsh`需要修改一下默认脚本, 根据系统版本, 下面修改了rocky和ubuntu
`scripts/get_cpu_arch.sh`

```diff
```diff
29c29,30
<             CpuArch=$(get_cpu_arch "dpkg-architecture -qDEB_HOST_ARCH_CPU" "dpkg-architecture -qDEB_BUILD_ARCH_CPU" "uname -m")
---
>             # CpuArch=$(get_cpu_arch "dpkg-architecture -qDEB_HOST_ARCH_CPU" "dpkg-architecture -qDEB_BUILD_ARCH_CPU" "uname -m")
>           CpuArch=$(amd64)
31c32
<         "centos"|"fedora"|"rhel")
---
>         "centos"|"fedora"|"rhel"|"rocky")
33c34
<             CpuArch=$(get_cpu_arch "uname -m" "arch" "uname")
---
>             CpuArch=$(get_cpu_arch "arch" "uname")
```

项目clone下来以后修改`.env`中的订阅链接

`zsh`还得修改默认的环境变量, 将start.sh脚本复制进入zshrc的最后即可

```Bash
# 开启系统代理
function proxy_set() {
        local PROXY_PORT=17890
        export http_proxy=http://127.0.0.1:$PROXY_PORT
        export https_proxy=http://127.0.0.1:$PROXY_PORT
        export no_proxy=127.0.0.1,localhost:$PROXY_PORT
        export HTTP_PROXY=http://127.0.0.1:$PROXY_PORT
        export HTTPS_PROXY=http://127.0.0.1:$PROXY_PROT
        export NO_PROXY=127.0.0.1,localhost
        echo -e "\033[32m[√] 已开启代理 PORT: $PROXY_PORT\033[0m"
}

# 关闭系统代理
function proxy_unset(){
        unset http_proxy
        unset https_proxy
        unset no_proxy
        unset HTTP_PROXY
        unset HTTPS_PROXY
        unset NO_PROXY
        echo -e "\033[31m[×] 已关闭代理\033[0m"
}
```

## zsh & oh my zsh

zsh直接用包管理器安装

```bash
sudo apt install zsh
```

然后设置为默认shell, 这里不能加sudo，否则会更换掉`root`用户的默认shell

```bash
chsh -s $(which zsh)
```

然后安装`oh my zsh`, omz是zsh的插件管理框架

```Bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

然后更改主题为`Powerlevel10K`

```Bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

修改`~/.zshrc`

```zsh
ZSH_THEME="Powerlevel10K/Powerlevel10K"
```

## nerdfont

在官网下载字体

```bash
wget -c $URL
```

然后解压缩

```Bash
unzip $FILE -d /usr/share/fonts/$(FILE_DIR)
```

更新字体缓存

```bash
cd /usr/share/fonts/$(FILE_DIR)
sudo mkfontscale # 生成核心字体信息
sudo mkfontdir # 生成字体文件夹
sudo fc-cache -fv # 刷新系统字体缓存
```

## Neovim & lazyvim

下载已经安装好的包

```Bash
wget $(NVIM)
```

解压缩

```Bash
tar -zxvf $(NVIM)
```

然后配置环境变量
