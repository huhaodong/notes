# ananconda 指令总结

## 包安装

1. 搜索包: ananconda search -t <包类型> <包名称>
    
    例如: anaconda search -t conda tensorflow-gpu 搜索所有可用的tensorflow-gpu的conda类型的包。

2. 查询包安装方式: anaconda show <名称>

    名称对应搜索出来的所需要下载的包的名称。

## 环境管理

1. 克隆: conda create -n <名称> --clone <需要克隆的环境名称>