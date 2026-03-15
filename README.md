# QSDK 编译

每个xml对应的修改放在对应的branch下

这个仓库对应 AU_LINUX_QSDK_NHSS.QSDK.12.5.R6_TARGET_ALL.12.5.6.2987.012.xml

这个仓库编译配置是 ipq53xx 64bit premium

编译过程中将此仓库中文件放到对应目录下

注意很多文件是编译过程中拉取、生成的，开始编译时放过去没用，会被覆盖掉。建议到报错时检查文件是否存在，存在就覆盖

**警告：** 本仓库不保证完全可用，为了能编译或解决报错，可能禁用了一些功能

**注意：** 由于编译问题，暂时禁用了CONFIG_PACKAGE_kmod-qca-nss-ecm-wifi-plugin，这会导致没有WiFi
