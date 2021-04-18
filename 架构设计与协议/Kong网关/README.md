# Kong 网关

Kong是一个云原生，快速，可扩展和分布式微服务抽象层（也称为API网关，API中间件或某些情况下的Service Mesh）。作为2015年的开源项目，其核心价值在于高性能和可扩展性。

由于对项目的积极维护，Kong被广泛用于从初创公司到全球5000强以及政府机构的生产中。

可以在[Kong - 官方文档](https://docs.konghq.com/gateway-oss/)中了解更多详情用法，中文阅读可参考[Github qianyugang/kong-docs-cn 中文翻译过来的文档](https://github.com/qianyugang/kong-docs-cn)。

## Kong Cli 命令，详可见[Cli 文档](https://docs.konghq.com/gateway-oss/2.4.x/cli/)

``` 
/ # kong --help
Usage: kong COMMAND [OPTIONS]

The available commands are:
 ·check：检查kong配置是否有效
---------------
# kong check /usr/local/kong/nginx-kong.conf 
configuration at /usr/local/kong/nginx.conf is valid
---------------

 ·config：使用声明的配置文件运行Kong

 ·health：检查Kong是否心跳正常（运行正常）
---------------
# kong health
nginx.......running
Kong is healthy at /usr/local/kong
---------------

 ·migrations：管理数据库迁移

 ·prepare：在配置的前缀目录中准备Kong前缀。这个命令可以用于从nginx二进制文件启动Kong而不使用`kong start`命令。

 ·quit：优雅地退出一个正在运行的Kong节点（Nginx和其他节点）在给定的前缀目录中配置的服务。

 ·reload：重新加载Kong节点（并启动其他已配置的服务）在给定的前缀目录中。此命令将HUP信号发送到Nginx，它将生成workers（告知account配置变更），当他们处理完成当前的请求后就停止旧的。

 ·restart：重新启动Kong节点（以及其他配置的服务，如Serf）在给定的前缀目录中。

 ·roar

 ·start：在配置中启动Kong（Nginx和其他配置的服务）。

 ·stop：停止给定的正在运行的Kong节点（Nginx和其他已配置的服务）在指定的前缀目录。此命令将SIGTERM信号发送到Nginx。

 ·version

Options:
 --v              verbose
 --vv             debug
```

## 如何使用 Kong 网关

### 方式一：单独安装并启动 Kong

以 CentOS 系统为例，有很多方式，可以[下载源码安装编译](https://github.com/qianyugang/kong-docs-cn/blob/master/INSTALL/source.md)，Github 有 Kong 的源码（source：https://github.com/Kong/kong）；或者通过系统直接安装`sudo yum install -y kong`，详见参考[教程 - CentOS 安装 Kong](https://github.com/qianyugang/kong-docs-cn/blob/master/INSTALL/centos.md)。

安装完成后，直接启动 Kong，便会自动监听对应的端口

### 方式二：[自定义Nginx模板和嵌入Kong](https://github.com/qianyugang/kong-docs-cn/blob/master/GUIDES&REFERENCES/configuration.md#%E8%87%AA%E5%AE%9A%E4%B9%89nginx%E6%A8%A1%E6%9D%BF%E5%92%8C%E5%B5%8C%E5%85%A5kong)

从技术角度来看，Kong 是一个在 Nginx 中运行的Lua应用程序，并且可以通过 lua-nginx 模块实现。Kong 不是用这个模块编译 Nginx，而是与 OpenResty 一起分发，[OpenResty](https://openresty.org/cn/) 已经包含了 lua-nginx-module。OpenResty 不是 Nginx 的分支，而是一组扩展其功能的模块。

因此，直接在[Nginx 配置](nginx-kong.conf)中使用 Lua 的语法直接引入 `Kong`,将路由转发到 Kong 模块来处理：
``` 
location / {
        default_type                     '';

        set $ctx_ref                     '';
        set $upstream_te                 '';
        set $upstream_host               '';
        set $upstream_upgrade            '';
        set $upstream_connection         '';
        set $upstream_scheme             '';
        set $upstream_uri                '';
        set $upstream_x_forwarded_for    '';
        set $upstream_x_forwarded_proto  '';
        set $upstream_x_forwarded_host   '';
        set $upstream_x_forwarded_port   '';
        set $kong_proxy_mode             'http';

        proxy_http_version 1.1;
        proxy_set_header   TE                $upstream_te;
        proxy_set_header   Host              $upstream_host;
        proxy_set_header   Upgrade           $upstream_upgrade;
        proxy_set_header   Connection        $upstream_connection;
        proxy_set_header   X-Forwarded-For   $upstream_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $upstream_x_forwarded_proto;
        proxy_set_header   X-Forwarded-Host  $upstream_x_forwarded_host;
        proxy_set_header   X-Forwarded-Port  $upstream_x_forwarded_port;
        proxy_set_header   X-Real-IP         $remote_addr;
        proxy_pass_header  Server;
        proxy_pass_header  Date;
        proxy_ssl_name     $upstream_host;
        proxy_pass         $upstream_scheme://kong_upstream$upstream_uri;
    }
```


