# Homebrew

mac 标准安装脚本

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

mac 卸载脚本

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/HomebrewUninstall.sh)"
```

如果遇到报错中含有errno **54** / **443** / 的问题，这种一般切换源以后没有问题，因为都是公益服务器，不稳定性很大。

使用中科大国内源

```bash
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

> brew help查看帮助
>
> brew -v查看版本
>
> brew update更新brew
>
> brew install <包>安装
>
> brew uninstall <包名>卸载
>
> brew outdated查询可更新的包
>
> brew upgrade 全部更新包
> brew upgrade 包名 指定包更新包
>
> brew cleanup清理旧版本
>
> brew info 包名查看包信息
>
> brew list查看安装列表
>
> brew search <包名>查询可用包

# nvm

nvm 安装命令，版本号可以从官方最新看到。

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.39.1/install.sh | bash
```

安装中提示

```
 Failed to connect to raw.githubusercontent.com port 443 after 23 ms: Couldn't connect to server
```

可以使用 

```bash
brew install nvm
```

首先要保证之前没有安装过node，如果之前安装过，就先 brew uninstall node。

安装成功之后，环境配置文件 .zshrc 中添加

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```



# node

```bash
nvm install node
```

如果出现卡住无法下载，改国内源。

mac 只需要执行下面这句话就行了

```bash
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node/
```

windows 需要在nvm的安装路径下，找到settings.txt打开，在后面加加上

```
node_mirror: https://npm.taobao.org/mirrors/node/ 
npm_mirror: https://npm.taobao.org/mirrors/npm/
```

