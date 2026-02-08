# 后端开发指南

本指南将介绍如何在本地运行 `srv.us` 的后端服务。

## 前提条件

- Go (推荐 1.20+)
- OpenSSL (用于生成证书)
- Make (可选，但推荐使用)

## 快速入门

1.  进入 `backend` 目录：
    ```bash
    cd backend
    ```

2.  生成必要的密钥和证书：
    ```bash
    make fullchain.pem
    make ssh_host_rsa_key
    make ssh_host_ecdsa_key
    make ssh_host_ed25519_key
    ```
    这将生成用于 HTTPS 的自签名证书和 SSH 的主机密钥。

3.  编译二进制文件：
    ```bash
    make srvus
    ```

4.  运行服务器：
    ```bash
    make run
    ```
    该命令将使用以下默认参数运行服务器（在 `Makefile` 中定义）：
    - `-domain srvtest`: 服务域名（生产环境默认为 `srv.us`，本地开发为 `srvtest`）。
    - `-https-port 4443`: HTTPS 端口。
    - `-ssh-port 2222`: SSH 端口。
    - `-https-chain-path fullchain.pem`: 证书链路径。
    - `-https-key-path privkey.pem`: 私钥路径。
    - `-ssh-host-keys-path .`: SSH 主机密钥所在路径。

## 测试

要测试隧道，可以运行：

```bash
ssh -p 2222 localhost -R 1:localhost:3000
```
(确保本地端口 3000 上有服务正在运行，或者更改端口)。

注意：如果想要测试子域名路由，可能需要将 `srvtest` 添加到 `/etc/hosts` 并指向 `127.0.0.1` 以便正确访问 HTTPS 端点。
