# QSDK 编译

每个xml对应的修改放在对应的branch下

**注意：** 本仓库不保证完全可用，只是个人在探索过程中的记录，在此过程中出现任何问题可以开issue讨论，但是本人不承担任何责任

编译环境使用`Ubuntu 20.04`，编译过程中大量工具是长期不更新的工具，在新旧版本中都容易出现不兼容

编译过程参考[QSDK Wiki](https://wiki.codelinaro.org/en/clo/qsdk/overview)

[QSDK Repo](https://git.codelinaro.org/clo/qsdk/releases/manifest/qstak)提供的许多xml缺了好多东西，导致编译基本不现实，这个仓库只放经过尝试后至少能保证基础编译刷机的版本

拉取时注意`repo init -u https://git.codelinaro.org/clo/qsdk/releases/manifest/qstak -b release -m AU_LINUX_QSDK_<your version>.xml  --repo-url=https://git.codelinaro.org/clo/tools/repo  --repo-branch=qc-stable`命令中的`AU_LINUX_QSDK_<your version>.xml`替换成本仓库对应的branch名

大量数据要从某些网络连通条件不好的网站下载，请各显神通

**警告：** 本仓库适配的是[不死u-boot](https://mbd.pub/o/bread/YZWXlZhqaQ==)，这个版本似乎存在大量问题，包括硬编码和不兼容。比如`linux-6.1`和`linux-6.6`内核会在加载内核阶段卡在`Jumping to AARCH64 kernel via monitor`，加载FDT硬编码了`config@mi01.6`
