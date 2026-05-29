# 源码编译运行

!!! Tip ""

    请使用 v1.3.0 等已发布分支的代码运行 **Cordys CRM** 。请勿使用 main 分支，main 分支代码均处于开发或测试阶段，可能存在较明显功能缺陷。   

!!! Tip ""

    目前支持的源码运行环境有： Windows (x86)、Linux（x86 & arm64）、MacOS（x86 & arm64）。

## 1 项目结构

!!! Tip ""

    ``` 
    ├── backend                                  # 后端项目
    │   ├── app                                  # 应用程序模块
    │   ├── crm                                  # CRM 核心模块
    │   └── framework                            # 通用框架模块
    ├── frontend                                 # 前端项目
    │   ├── packages                             # 前端包管理
    │   │   ├── lib-shared                       # 公共库模块
    │   │   │   ├── api                          # API 封装
    │   │   │   ├── assets                       # 静态资源
    │   │   │   ├── enums                        # 枚举
    │   │   │   ├── hooks                        # 钩子函数
    │   │   │   ├── locale                       # 国际化封装
    │   │   │   ├── method                       # 工具函数
    │   │   │   ├── model                        # 数据模型
    │   │   │   ├── types                        # 全局类型声明
    │   │   ├── mobile                           # 移动端项目
    │   │   ├── web                              # WEB端项目
    ├── installer                                # 安装脚本
    ├── conf                                     # 配置文件
    │   ├── mysql                                # MySQL 配置
    │   └── redis                                # Redis 配置
    └── shells                                   # 脚本
    ``` 


## 2 安装基础 POM

!!! Tip ""

    该命令会将 `parent pom` 安装到本地 Maven 仓库，使其他外部子工程可以获取最新的 `<properties>` 配置。

    ```bash
    ./mvnw install -N
    ```


## 3 后端构建

!!! Tip ""

    📌 提示：确保已正确安装 JDK 21 和 Maven 3.8.6 及以上版本和环境。

    执行以下命令构建后端模块（如 `framework`、`crm`、`app` 等）并安装到本地仓库：

    ```bash
    ./mvnw clean install -DskipTests -DskipAntRunForJenkins --file backend/pom.xml
    ```
    参数说明：
    
    * `-DskipTests`: 跳过测试用例执行。
    * `-DskipAntRunForJenkins`: 跳过 Jenkins 使用的 Ant 任务。

### 3.1  后端参数配置说明

!!! Tip ""

    在本地运行 **Cordys CRM** 项目时，需要在本地创建配置文件：
    
    ```
    /opt/cordys/conf/cordys-crm.properties

    ```
    💡 **提示：**
    你也可以通过修改应用 ‘Application’ 启动参数，自定义配置文件的加载路径。

### 3.2 配置示例
    
    ```properties
    # ==============================
    # 数据库连接配置
    # ==============================
    # 数据库连接 URL，请根据实际环境修改 IP 与数据库名称
    spring.datasource.url=jdbc:mysql://你的 MySQL IP:3306/cordys-crm?autoReconnect=false&useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8&zeroDateTimeBehavior=convertToNull&useSSL=false
    
    # 数据库用户名
    spring.datasource.username=root
    
    # 数据库密码
    spring.datasource.password=CordysCRM@mysql
    
    
    # ==============================
    # Redis 缓存配置
    # ==============================
    # Redis 服务器地址
    spring.data.redis.host=你的 Redis IP
    
    # Redis 端口
    spring.data.redis.port=6379
    
    # Redis 密码
    spring.data.redis.password=CordysCRM@redis
    
    # Redis Session 存储方式（indexed 推荐）
    spring.session.redis.repository-type=indexed
    
    # Session 过期时间（单位：秒，示例为 12 小时）
    spring.session.timeout=43200s

    ```


## 4 前端构建

!!! Tip ""

     📌 提示：确保已正确安装 Node.js 和依赖环境。

### 4.1 工程初始化&运行
!!! Tip ""

    在`/packages`目录下运行依赖安装命令：
    
    ```node
    pnpm i -w
    ```
    
    统一构建工程：
    
    ```node
    npm run build
    ```


### 4.2 mobile 移动端工程包

!!! Tip ""

    移动端工程由 Vite+Vue3+TS+Vant-UI 基础框架组成。
    
    运行移动端项目：
    
    ```node
    cd package/mobile

    npm run dev
    ```
    
    在`package/mobile`下单独构建移动端项目：
    
    ```node
    npm run build
    ```
    


### 4.3 mobile 调试&开发
!!! Tip ""

    为了方便移动端项目的开发调试，可以通过模拟登录态：

        1. 启动 `web` 项目并完成登录，登录后打开浏览器控制台，复制 `localStorage` 中的 `sessionId` 和 `csrfToken` 两个属性值。
        2. 启动 `mobile` 项目，打开控制台，将第1步复制的 `localStorage` 属性值粘贴至控制台，刷新页面即可模拟登录。若登录过期，重新登录 `web` 项目并复制新的属性值替换即可。
        3. 在手机端调试时，进入页面并完成授权登录后，快速点击 10 次用户名区域，切换到 `我的` 菜单，即可唤起 `Eruda` 调试工具。
    

### 4.4 WEB 端工程包
!!! Tip ""
 
    WEB 端工程由 Vite+Vue3+TS+Naive-UI 基础框架组成。
    
    运行 WEB 端项目：
    
    ```node
    cd package/web

    npm run dev
    ```
    
    在`package/web`下单独构建 WEB 端项目：
    
    ```node
    npm run build
    ```

## 5 整体打包

!!! Tip ""

    使用以下命令进行完整的构建与打包：

    ```bash
    ./mvnw clean package
    ```
## 6 常见问题

!!! Tip ""

    **Q1：构建过程中遇到依赖下载失败怎么办？**

    A：如果出现以下错误：

        
        [ERROR] Failed to execute goal on project backend: Could not resolve dependencies
        [ERROR] dependency: cn.cordys:quartz-spring-boot-starter:jar:1.0.0 (compile)
        

    请在本地 Maven 仓库配置中添加以下镜像源：

        
        <repositories>
          <repository>
            <id>cordys</id>
            <url>https://repository.fit2cloud.com/repository/public</url>
            <releases>
              <enabled>true</enabled>
            </releases>
            <snapshots>
              <enabled>true</enabled>
            </snapshots>
          </repository>
        </repositories>


    **Q2：运行前端项目时报错缺少模块怎么办？**

    A：请确保已正确执行以下命令安装所有依赖：

        pnpm i -w

    如果问题仍然存在，可以尝试删除 `node_modules` 目录并重新安装依赖。


    **Q3：后端启动时报错数据库连接失败怎么办？**

    A：请检查 `cordys-crm.properties` 配置文件中的数据库连接信息是否正确，包括 IP 地址、端口、数据库名称、用户名和密码。


    **Q4：如何修改默认的服务器端口？**

    A：可以在 `cordys-crm.properties` 文件中添加或修改：

        server.port=8081
