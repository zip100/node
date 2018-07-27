* Mac 系统下需要先创建 GoLang WorkSpace 

```sh
mkdir -p ~/GoLang/GoWorkspace/bin
mkdir -p ~/GoLang/GoWorkspace/pkb
mkdir -p ~/GoLang/GoWorkspace/src
```

* 设置 GOPATH 环境变量

> 在 ~/.bash\_profile 添加环境变量声明

```
export GOPATH=/Users/mike/GoWorkspace
export PATH=$PATH:${GOPATH//://bin:}/bin
export GOBIN=
```

* 安装 Glide

```
curl https://glide.sh/get | sh
```



![](/assets/WX20180727-111810.png)

