# QSDK 编译

每个xml对应的修改放在对应的branch下

这个仓库对应 AU_LINUX_QSDK_NHSS.QSDK.12.5.R6_TARGET_ALL.12.5.6.2987.012.xml

这个仓库编译配置是 ipq53xx 64bit premium

编译过程中将此仓库中文件放到对应目录下

注意很多文件是编译过程中拉取、生成的，开始编译时放过去没用，会被覆盖掉。建议到报错时检查文件是否存在，存在就覆盖

**警告：** 本仓库不保证完全可用，为了能编译或解决报错，可能禁用了一些功能

**注意：** 由于编译问题，暂时禁用了CONFIG_PACKAGE_kmod-qca-nss-ecm-wifi-plugin，这会导致没有WiFi

## 环境

推荐`Ubuntu 20.04`，我尝试过`Ubuntu 22.04 24.04` `Debian 11 12 13`都有各种各样的问题

安装依赖

```bash
sudo apt update
sudo apt install lzop build-essential clang flex bison g++ gawk \
gcc-multilib g++-multilib gettext git libncurses-dev libssl-dev \
python3-distutils python3-setuptools rsync swig unzip zlib1g-dev file wget
# 调试工具
sudo apt install u-boot-tools binwalk device-tree-compiler
```

### 安装repo

QSDK源码使用repo拉取，由于Ubuntu 20.04 apt源中没有repo，所以不能简单的`apt install repo`（这一步可以找别的有repo的系统拉下来，再把代码发给编译机）

#### 手动安装repo

```bash
sudo curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/local/bin/repo
sudo chmod a+x /usr/local/bin/repo
```

## 编译

编译全过程务必**不要使用root用户**

以下假设在`$HOME`目录下执行，自行替换真实路径

### 拉取本仓库

```bash
git clone -b AU_LINUX_QSDK_NHSS.QSDK.12.5.R6_TARGET_ALL.12.5.6.2987.012.xml https://github.com/YK-Samgo/jdc-be65000-qsdk-compile.git
```

### 拉取源码

```bash
# 创建编译目录并进入
# 注意总路径太长可能在编译过程中触发bash参数长度限制
# 使用 sudo mkdir /qsdk && sudo mount --bind compile/qsdk /qsdk && cd qsdk 即可规避
mkdir compile
cd compile
# repo 拉取，最后的xml来源于QSDK Wiki的表格https://wiki.codelinaro.org/en/clo/qsdk/overview#clo-download-steps
# 注意这两个repo命令会下载大量数据，部分数据难以下载，请各显神通
# repo拉取luci.git时优先从openwrt官网下载，很慢，失败了会再从codelinaro拉取，所以这边失败一次没问题
repo init -u https://codelinaro.org/clo/qsdk/releases/manifest/qstak -b release -m AU_LINUX_QSDK_NHSS.QSDK.12.5.R6_TARGET_ALL.12.5.6.2987.012.xml
repo sync
# 进入qsdk目录
cd qsdk
# 京东云BE6500 选 5.ipq53xx → 2.64bit → 11.premium
# open 是开源固件，据说很差
# premium 使用高通闭源驱动
source qca/configs/qsdk/setup-environment
 Select ipq53xx
 Select 64bit
 Select premium
 Debug build  -  no
```

### 修补文件

在`qsdk`目录下执行

```bash
$HOME/jdc-be65000-qsdk-compile/cover
```

### 编译

```bash
make -j$(nproc) V=s
# 如果出现报错，单线程再执行一次，自行处理报错，重复直到编译完全成功，如有必要可再次执行$HOME/jdc-be65000-qsdk-compile/cover
make -j1 V=s
```

最终包在`$HOME/compile/qsdk/targets/ipq53xx/generic/目录下`

## 刷入

这个仓库的修改全部适配不死uboot，进入uboot刷入factory包即可

开发过程极易出现问题，建议最好能有串口调试能力
