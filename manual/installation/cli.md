## 1 Cordys CRM Service

!!! Tip ""
    Cordys CRM 在安装的时候默认向系统中添加了相应的 Cordys CRM Service，支持的 Service 命令有：

    - start : 启动 Cordys CRM 服务
    - stop : 停止 Cordys CRM 服务，并删除相关的运行容器、docker 网络等资源
    - restart : 停止后启动 Cordys CRM 服务，相当于先执行 stop，再执行 start 命令
    - status : 查看 Cordys CRM 服务当前各容器运行状态
    - reload : 重载 Cordys CRM 服务配置文件
    - uninstall : 卸载 Cordys CRM 服务，相关文件和数据会保留

## 2 csctl

!!! Tip ""
    Cordys CRM 默认内置了命令行运维工具（csctl），通过执行 csctl help 命令，可以查看相关的帮助文档。

    ```
    Usage: 
        ./csctl [COMMAND] [ARGS...]
        ./csctl --help
    
    Commands:
        status                查看 Cordys CRM 服务运行状态
        start                 启动 Cordys CRM 服务
        stop                  停止 Cordys CRM 服务
        restart               重启 Cordys CRM 服务
        reload                重载 Cordys CRM 服务
        version               查看 Cordys CRM 版本
        uninstall             卸载 Cordys CRM 服务
    ```