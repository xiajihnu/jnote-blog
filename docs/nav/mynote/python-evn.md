# Python环境搭建

Last updated August 20, 2025

---

本文主要记录Python环境配置相关技巧，每一次配置环境都在网上查找一些资料，非常繁琐。

## 更换镜像源

### 可用国内加速镜像源

### 命令行配置镜像源
这里以清华源为例，临时使用清华源安装numpy包
```
code here.
```
用命令行的方式，永久修改默认镜像源（以清华源为例）
```
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple/
```


## 环境创建与激活
对于较新的Python版本，不能再像以前一样直接安装各种包，然后开始运行，这回导致不同项目需要不同版本的package。从而产生package版本冲突。所以新版本Python在使用前，需要先创建python环境，每个项目都有自己的python环境，保持环境之间相互独立。

1. 创建Python环境
```
python3 -m venv ~/xxx-project/.venv
```

2. 激活Python环境
```
source ~/xxx-project/.venv/bin/activate
```

3. 安装pip包

```
code here.
```

