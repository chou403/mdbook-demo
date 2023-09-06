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



# Navicat导出ncx解析数据库密码

网址：https://tool.lu/coderunner/

代码

```php
<?php
class NavicatPassword
{
    protected $version = 0;
    protected $aesKey = 'libcckeylibcckey';
    protected $aesIv = 'libcciv libcciv ';
    protected $blowString = '3DC5CA39';
    protected $blowKey = null;
    protected $blowIv = null;
     
    public function __construct($version = 12)
    {
        $this->version = $version;
        $this->blowKey = sha1('3DC5CA39', true);
        $this->blowIv = hex2bin('d9c7c3c8870d64bd');
    }
     
    public function encrypt($string)
    {
        $result = FALSE;
        switch ($this->version) {
            case 11:
                $result = $this->encryptEleven($string);
                break;
            case 12:
                $result = $this->encryptTwelve($string);
                break;
            default:
                break;
        }
         
        return $result;
    }
     
    protected function encryptEleven($string)
    {
        $round = intval(floor(strlen($string) / 8));
        $leftLength = strlen($string) % 8;
        $result = '';
        $currentVector = $this->blowIv;
         
        for ($i = 0; $i < $round; $i++) {
            $temp = $this->encryptBlock($this->xorBytes(substr($string, 8 * $i, 8), $currentVector));
            $currentVector = $this->xorBytes($currentVector, $temp);
            $result .= $temp;
        }
         
        if ($leftLength) {
            $currentVector = $this->encryptBlock($currentVector);
            $result .= $this->xorBytes(substr($string, 8 * $i, $leftLength), $currentVector);
        }
         
        return strtoupper(bin2hex($result));
    }
     
    protected function encryptBlock($block)
    {
        return openssl_encrypt($block, 'BF-ECB', $this->blowKey, OPENSSL_RAW_DATA|OPENSSL_NO_PADDING);
    }
     
    protected function decryptBlock($block)
    {
        return openssl_decrypt($block, 'BF-ECB', $this->blowKey, OPENSSL_RAW_DATA|OPENSSL_NO_PADDING);
    }
     
    protected function xorBytes($str1, $str2)
    {
        $result = '';
        for ($i = 0; $i < strlen($str1); $i++) {
            $result .= chr(ord($str1[$i]) ^ ord($str2[$i]));
        }
         
        return $result;
    }
     
    protected function encryptTwelve($string)
    {
        $result = openssl_encrypt($string, 'AES-128-CBC', $this->aesKey, OPENSSL_RAW_DATA, $this->aesIv);
        return strtoupper(bin2hex($result));
    }
     
    public function decrypt($string)
    {
        $result = FALSE;
        switch ($this->version) {
            case 11:
                $result = $this->decryptEleven($string);
                break;
            case 12:
                $result = $this->decryptTwelve($string);
                break;
            default:
                break;
        }
         
        return $result;
    }
     
    protected function decryptEleven($upperString)
    {
        $string = hex2bin(strtolower($upperString));
         
        $round = intval(floor(strlen($string) / 8));
        $leftLength = strlen($string) % 8;
        $result = '';
        $currentVector = $this->blowIv;
         
        for ($i = 0; $i < $round; $i++) {
            $encryptedBlock = substr($string, 8 * $i, 8);
            $temp = $this->xorBytes($this->decryptBlock($encryptedBlock), $currentVector);
            $currentVector = $this->xorBytes($currentVector, $encryptedBlock);
            $result .= $temp;
        }
         
        if ($leftLength) {
            $currentVector = $this->encryptBlock($currentVector);
            $result .= $this->xorBytes(substr($string, 8 * $i, $leftLength), $currentVector);
        }
         
        return $result;
    }
     
    protected function decryptTwelve($upperString)
    {
        $string = hex2bin(strtolower($upperString));
        return openssl_decrypt($string, 'AES-128-CBC', $this->aesKey, OPENSSL_RAW_DATA, $this->aesIv);
    }
};
 
 
//需要指定版本两种，11或12
//$navicatPassword = new NavicatPassword(11);
//这里我指定的12的版本，原先指定的11，执行之后的密码是乱码
$navicatPassword = new NavicatPassword(12);
 
//解密
//$decode = $navicatPassword->decrypt('5658213B');
$decode = $navicatPassword->decrypt('复制出来的密码');
echo $decode."\n";
?>
```

