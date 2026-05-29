
## 1. 安装时找不到镜像报错


!!! Tip ""

       执行以下命令时：
      ```bash
      docker run -d \
        --name cordys-crm \
        --restart unless-stopped \
        -p 8081:8081 \
        -p 8082:8082 \
        -v ~/cordys:/opt/cordys \
        1panel/cordys-crm
      ```
      
      报错信息：
      
      ```
      Unable to find image '1panel/cordys-crm:latest' locally
      docker: Error response from daemon: Get "https://registry-1.docker.io/v2/": 
      net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers).
      See 'docker run --help'.
      ```


!!! Tip ""

    ✅ 解决方案
      **原因**：Docker 默认从官方仓库拉取镜像，网络可能无法直连。
      
      **处理方法**：配置镜像加速器。
      编辑 `/etc/docker/daemon.json`：
      
      ```bash
      sudo mkdir -p /etc/docker
      sudo tee /etc/docker/daemon.json <<-'EOF'
      {
        "registry-mirrors": [
          "https://hub.1panel.dev",
          "https://docker.1ms.run",
          "https://docker.1panel.live",
          "https://docker.m.ixdev.cn",
          "https://hub.rat.dev",
          "https://dockerproxy.net",
          "https://image.cloudlayer.icu",
          "https://docker-registry.nmqu.com",
          "https://hub.amingg.com",
          "https://docker.amingg.com",
          "https://docker.hlmirror.com",
          "https://hub1.nat.tf",
          "https://docker.m.daocloud.io",
          "https://docker.kejilion.pro",
          "https://docker.367231.xyz",
          "https://hub.1panel.dev",
          "https://dockerproxy.cool",
          "https://docker.apiba.cn",
          "https://proxy.vvvv.ee"
        ]
      }
      EOF
      ```
      
      重启 Docker 服务：
      
      ```bash
      sudo systemctl daemon-reload
      sudo systemctl restart docker
      ```
      
      如果依旧无法解决，可选择以下方式安装：
      
      * [离线包安装](../offline_installtion)
      
      * [1Panel 应用商店安装](../1panel_installtion)

---

## 2. 成功安装后无法访问


!!! Tip ""
    
    ✅ 排查思路
      1. 确认容器是否正常运行：
      
         ```bash
         docker ps
         ```
      
         应该能看到 `cordys-crm` 容器处于 **Up** 状态。
      
      2. 确认端口映射是否生效：
   
         ```bash
         docker port cordys-crm
         ```
   
         应该显示 `8081/tcp -> 0.0.0.0:8081`。
      
      3. 如果是云服务器：
   
          * 检查 **安全组** 或 **防火墙** 是否放行 `8081` 端口。
          * 本地访问时请确保使用 `http://<服务器公网IP>:8081/`。

---

## 3. 镜像兼容性说明
!!! Tip ""
      
      ✅ 支持：amd64 (x86_64)
      
      ❌ 暂不支持：ARM（包括 Apple M1/M2/M3、树莓派等）
      
      ARM 用户请关注后续镜像更新计划。

