# MinIO Quickstart Guide
# MinIO 快速入门指南

[![Slack](https://slack.min.io/slack?type=svg)](https://slack.min.io) [![Docker Pulls](https://img.shields.io/docker/pulls/minio/minio.svg?maxAge=604800)](https://hub.docker.com/r/minio/minio/) [![license](https://img.shields.io/badge/license-AGPL%20V3-blue)](https://github.com/minio/minio/blob/master/LICENSE)

[![MinIO](https://raw.githubusercontent.com/minio/minio/master/.github/logo.svg?sanitize=true)](https://min.io)

MinIO is a High Performance Object Storage released under GNU Affero General Public License v3.0. It is API compatible with Amazon S3 cloud storage service. Use MinIO to build high performance infrastructure for machine learning, analytics and application data workloads.

MinIO 是根据 GNU Affero General Public License v3.0 发布的高性能对象存储。它的 API 与亚马逊 S3 云存储服务兼容。使用 MinIO 可为机器学习、分析和应用数据工作负载构建高性能基础架构。

This README provides quickstart instructions on running MinIO on bare metal hardware, including container-based installations. For Kubernetes environments, use the [MinIO Kubernetes Operator](https://github.com/minio/operator/blob/master/README.md).

本 README 提供了在裸机硬件（包括基于容器的安装）上运行 MinIO 的快速入门指南。对于 Kubernetes 环境，请使用 MinIO Kubernetes 操作器。

## Container Installation
## 容器安装

Use the following commands to run a standalone MinIO server as a container.
使用以下命令将独立 MinIO 服务器作为容器运行。

Standalone MinIO servers are best suited for early development and evaluation. Certain features such as versioning, object locking, and bucket replication
require distributed deploying MinIO with Erasure Coding. For extended development and production, deploy MinIO with Erasure Coding enabled - specifically,
with a *minimum* of 4 drives per MinIO server. See [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html)
for more complete documentation.
独立 MinIO 服务器最适合用于早期开发和评估。某些功能（如版本控制、对象锁定和桶复制）需要使用擦除编码分布式部署 MinIO。对于扩展开发和生产，部署 MinIO 时应启用 Erasure Coding，特别是每个 MinIO 服务器至少要有 4 个驱动器。有关更完整的文档，请参阅 MinIO Erasure Code Overview。

### Stable
### 稳定

Run the following command to run the latest stable image of MinIO as a container using an ephemeral data volume:
运行以下命令可使用短暂数据卷作为容器运行 MinIO 的最新稳定映像：

```sh
podman run -p 9000:9000 -p 9001:9001 \
  quay.io/minio/minio server /data --console-address ":9001"
```

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`. You can test the deployment using the MinIO Console, an embedded
object browser built into MinIO Server. Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the
root credentials. You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.
MinIO 部署使用默认根凭证 minioadmin:minioadmin 启动。您可以使用 MinIO 控制台（MinIO Server 内置的嵌入式对象浏览器）测试部署。将主机上运行的 Web 浏览器指向 http://127.0.0.1:9000，然后使用根凭证登录。您可以使用浏览器创建存储桶、上传对象并浏览 MinIO 服务器的内容。

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool. See
[Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool. For application developers,
see <https://min.io/docs/minio/linux/developers/minio-drivers.html> to view MinIO SDKs for supported languages.
您还可以使用任何 S3 兼容工具（如 MinIO Client mc 命令行工具）进行连接。有关使用 mc 命令行工具的更多信息，请参阅使用 MinIO Client mc 进行测试。对于应用程序开发人员，请参阅 https://min.io/docs/minio/linux/developers/minio-drivers.html 查看支持语言的 MinIO SDK。

> NOTE: To deploy MinIO on with persistent storage, you must map local persistent directories from the host OS to the container using the `podman -v` option. For example, `-v /mnt/data:/data` maps the host OS drive at `/mnt/data` to `/data` on the container.
> 注意：要在带有持久化存储的容器上部署 MinIO，必须使用 podman -v 选项将本地持久化目录从主机操作系统映射到容器。例如，-v /mnt/data:/data 会将主机操作系统驱动器 /mnt/data 映射到容器上的 /data。

## macOS

Use the following commands to run a standalone MinIO server on macOS.
使用以下命令在 macOS 上运行独立的 MinIO 服务器。

Standalone MinIO servers are best suited for early development and evaluation. Certain features such as versioning, object locking, and bucket replication require distributed deploying MinIO with Erasure Coding. For extended development and production, deploy MinIO with Erasure Coding enabled - specifically, with a *minimum* of 4 drives per MinIO server. See [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html) for more complete documentation.
独立 MinIO 服务器最适合用于早期开发和评估。某些功能（如版本控制、对象锁定和桶复制）需要分布式部署带有擦除编码的 MinIO。对于扩展开发和生产，部署 MinIO 时应启用 Erasure Coding，特别是每个 MinIO 服务器至少要有 4 个硬盘。有关更完整的文档，请参阅 MinIO Erasure Code Overview。

### Homebrew (recommended)
### 自制软件（推荐）

Run the following command to install the latest stable MinIO package using [Homebrew](https://brew.sh/). Replace ``/data`` with the path to the drive or directory in which you want MinIO to store data.
运行以下命令，使用 Homebrew 安装最新的稳定 MinIO 软件包。将 /data 替换为您希望 MinIO 存储数据的驱动器或目录的路径。

```sh
brew install minio/stable/minio
minio server /data
```

> NOTE: If you previously installed minio using `brew install minio` then it is recommended that you reinstall minio from `minio/stable/minio` official repo instead.
> 注意：如果您之前使用 brew install minio 安装了 minio，那么建议您从 minio/stable/minio 官方软件仓库重新安装 minio。

```sh
brew uninstall minio
brew install minio/stable/minio
```

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`. You can test the deployment using the MinIO Console, an embedded web-based object browser built into MinIO Server. Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the root credentials. You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.
MinIO 部署将使用默认根凭证 minioadmin:minioadmin 启动。您可以使用 MinIO 控制台（MinIO Server 内置的基于 Web 的嵌入式对象浏览器）测试部署。将主机上运行的 Web 浏览器指向 http://127.0.0.1:9000，然后使用根凭证登录。您可以使用浏览器创建存储桶、上传对象和浏览 MinIO 服务器的内容。

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool. See [Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool. For application developers, see <https://min.io/docs/minio/linux/developers/minio-drivers.html/> to view MinIO SDKs for supported languages.
您也可以使用任何 S3 兼容工具（如 MinIO Client mc 命令行工具）进行连接。有关使用 mc 命令行工具的更多信息，请参阅使用 MinIO Client mc 进行测试。对于应用程序开发人员，请参阅 https://min.io/docs/minio/linux/developers/minio-drivers.html/ 查看支持语言的 MinIO SDK。

### Binary Download
### 二进制文件下载

Use the following command to download and run a standalone MinIO server on macOS. Replace ``/data`` with the path to the drive or directory in which you want MinIO to store data.
用以下命令在 macOS 上下载并运行独立的 MinIO 服务器。将 /data 替换为您希望 MinIO 存储数据的驱动器或目录的路径。

```sh
wget https://dl.min.io/server/minio/release/darwin-amd64/minio
chmod +x minio
./minio server /data
```

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`. You can test the deployment using the MinIO Console, an embedded web-based object browser built into MinIO Server. Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the root credentials. You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.
MinIO 部署使用默认的根凭证 minioadmin:minioadmin 启动。您可以使用 MinIO 控制台（MinIO Server 内置的基于 Web 的嵌入式对象浏览器）测试部署。将主机上运行的 Web 浏览器指向 http://127.0.0.1:9000，然后使用根凭证登录。您可以使用浏览器创建存储桶、上传对象和浏览 MinIO 服务器的内容。

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool. See [Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool. For application developers, see <https://min.io/docs/minio/linux/developers/minio-drivers.html> to view MinIO SDKs for supported languages.
您还可以使用任何 S3 兼容工具（如 MinIO Client mc 命令行工具）进行连接。有关使用 mc 命令行工具的更多信息，请参阅使用 MinIO Client mc 进行测试。对于应用程序开发人员，请参阅 https://min.io/docs/minio/linux/developers/minio-drivers.html 查看支持语言的 MinIO SDK。

## GNU/Linux

Use the following command to run a standalone MinIO server on Linux hosts running 64-bit Intel/AMD architectures. Replace ``/data`` with the path to the drive or directory in which you want MinIO to store data.
使用以下命令在运行 64 位 Intel/AMD 架构的 Linux 主机上运行独立的 MinIO 服务器。将 /data 替换为您希望 MinIO 存储数据的驱动器或目录的路径。

```sh
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
./minio server /data
```

The following table lists supported architectures. Replace the `wget` URL with the architecture for your Linux host.
下表列出了支持的架构。请将 wget URL 替换为您的 Linux 主机的架构。

| Architecture                   | URL                                                        |
| --------                       | ------                                                     |
| 64-bit Intel/AMD               | <https://dl.min.io/server/minio/release/linux-amd64/minio>   |
| 64-bit ARM                     | <https://dl.min.io/server/minio/release/linux-arm64/minio>   |
| 64-bit PowerPC LE (ppc64le)    | <https://dl.min.io/server/minio/release/linux-ppc64le/minio> |

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`. You can test the deployment using the MinIO Console, an embedded web-based object browser built into MinIO Server. Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the root credentials. You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.
MinIO 部署使用默认根凭证 minioadmin:minioadmin 启动。您可以使用 MinIO 控制台（MinIO Server 内置的基于 Web 的嵌入式对象浏览器）测试部署。将主机上运行的 Web 浏览器指向 http://127.0.0.1:9000，然后使用根凭据登录。您可以使用浏览器创建存储桶、上传对象和浏览 MinIO 服务器的内容。

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool. See [Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool. For application developers, see <https://min.io/docs/minio/linux/developers/minio-drivers.html> to view MinIO SDKs for supported languages.
您还可以使用任何 S3 兼容工具（如 MinIO Client mc 命令行工具）进行连接。有关使用 mc 命令行工具的更多信息，请参阅使用 MinIO Client mc 进行测试。对于应用程序开发人员，请参阅 https://min.io/docs/minio/linux/developers/minio-drivers.html 查看支持语言的 MinIO SDK。

> NOTE: Standalone MinIO servers are best suited for early development and evaluation. Certain features such as versioning, object locking, and bucket replication require distributed deploying MinIO with Erasure Coding. For extended development and production, deploy MinIO with Erasure Coding enabled - specifically, with a *minimum* of 4 drives per MinIO server. See [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html#) for more complete documentation.
> 注意：独立 MinIO 服务器最适合早期开发和评估。某些功能（如版本控制、对象锁定和桶复制）需要分布式部署带有擦除编码的 MinIO。对于扩展开发和生产，部署 MinIO 时应启用 Erasure Coding，特别是每个 MinIO 服务器至少要有 4 个硬盘。有关更完整的文档，请参阅 MinIO Erasure Code Overview。

## Microsoft Windows
## 微软视窗

To run MinIO on 64-bit Windows hosts, download the MinIO executable from the following URL:
要在 64 位 Windows 主机上运行 MinIO，请从以下 URL 下载 MinIO 可执行文件：

```sh
https://dl.min.io/server/minio/release/windows-amd64/minio.exe
```

Use the following command to run a standalone MinIO server on the Windows host. Replace ``D:\`` with the path to the drive or directory in which you want MinIO to store data. You must change the terminal or powershell directory to the location of the ``minio.exe`` executable, *or* add the path to that directory to the system ``$PATH``:
使用以下命令在 Windows 主机上运行独立的 MinIO 服务器。将 D:\ 替换为希望 MinIO 存储数据的驱动器或目录的路径。您必须将终端或 powershell 目录更改为 minio.exe 可执行文件的位置，或将该目录的路径添加到系统 $PATH 中：

```sh
minio.exe server D:\
```

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`. You can test the deployment using the MinIO Console, an embedded web-based object browser built into MinIO Server. Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the root credentials. You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.
MinIO 部署使用默认根凭证 minioadmin:minioadmin 启动。您可以使用 MinIO 控制台（MinIO Server 内置的基于 Web 的嵌入式对象浏览器）测试部署。将主机上运行的 Web 浏览器指向 http://127.0.0.1:9000，然后使用根凭据登录。您可以使用浏览器创建存储桶、上传对象和浏览 MinIO 服务器的内容。

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool. See [Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool. For application developers, see <https://min.io/docs/minio/linux/developers/minio-drivers.html> to view MinIO SDKs for supported languages.
您还可以使用任何 S3 兼容工具（如 MinIO Client mc 命令行工具）进行连接。有关使用 mc 命令行工具的更多信息，请参阅使用 MinIO Client mc 进行测试。对于应用程序开发人员，请参阅 https://min.io/docs/minio/linux/developers/minio-drivers.html 查看支持语言的 MinIO SDK。

> NOTE: Standalone MinIO servers are best suited for early development and evaluation. Certain features such as versioning, object locking, and bucket replication require distributed deploying MinIO with Erasure Coding. For extended development and production, deploy MinIO with Erasure Coding enabled - specifically, with a *minimum* of 4 drives per MinIO server. See [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html#) for more complete documentation.
> 注意：独立 MinIO 服务器最适合早期开发和评估。某些功能（如版本控制、对象锁定和桶复制）需要分布式部署带有擦除编码的 MinIO。对于扩展开发和生产，部署 MinIO 时应启用 Erasure Coding，特别是每个 MinIO 服务器至少要有 4 个硬盘。有关更完整的文档，请参阅 MinIO Erasure Code Overview。

## Install from Source
## 从源代码安装

Use the following commands to compile and run a standalone MinIO server from source. Source installation is only intended for developers and advanced users. If you do not have a working Golang environment, please follow [How to install Golang](https://golang.org/doc/install). Minimum version required is [go1.21](https://golang.org/dl/#stable)
使用以下命令从源代码编译并运行独立的 MinIO 服务器。源代码安装仅适用于开发人员和高级用户。如果您没有可用的 Golang 环境，请遵循如何安装 Golang。所需的最低版本为 go1.21

```sh
go install github.com/minio/minio@latest
```

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`. You can test the deployment using the MinIO Console, an embedded web-based object browser built into MinIO Server. Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the root credentials. You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.
MinIO 部署使用默认的 root 凭据 minioadmin:minioadmin 启动。您可以使用 MinIO 控制台（MinIO Server 内置的基于 Web 的嵌入式对象浏览器）测试部署。将主机上运行的 Web 浏览器指向 http://127.0.0.1:9000，然后使用根凭证登录。您可以使用浏览器创建存储桶、上传对象和浏览 MinIO 服务器的内容。

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool. See [Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool. For application developers, see <https://min.io/docs/minio/linux/developers/minio-drivers.html> to view MinIO SDKs for supported languages.
您还可以使用任何 S3 兼容工具（如 MinIO Client mc 命令行工具）进行连接。有关使用 mc 命令行工具的更多信息，请参阅使用 MinIO Client mc 进行测试。对于应用程序开发人员，请参阅 https://min.io/docs/minio/linux/developers/minio-drivers.html 查看支持语言的 MinIO SDK。

> NOTE: Standalone MinIO servers are best suited for early development and evaluation. Certain features such as versioning, object locking, and bucket replication require distributed deploying MinIO with Erasure Coding. For extended development and production, deploy MinIO with Erasure Coding enabled - specifically, with a *minimum* of 4 drives per MinIO server. See [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html) for more complete documentation.
> 注意：独立 MinIO 服务器最适合早期开发和评估。某些功能（如版本控制、对象锁定和桶复制）需要分布式部署带有擦除编码的 MinIO。对于扩展开发和生产，部署 MinIO 时应启用 Erasure Coding，特别是每个 MinIO 服务器至少要有 4 个硬盘。有关更完整的文档，请参阅 MinIO Erasure Code Overview。

MinIO strongly recommends *against* using compiled-from-source MinIO servers for production environments.
MinIO 强烈建议不要在生产环境中使用从源代码编译的 MinIO 服务器。

## Deployment Recommendations
## 部署建议

### Allow port access for Firewalls
### 允许防火墙端口访问

By default MinIO uses the port 9000 to listen for incoming connections. If your platform blocks the port by default, you may need to enable access to the port.
默认情况下，MinIO 使用端口 9000 来侦听传入连接。如果您的平台默认阻止该端口，则可能需要启用对该端口的访问。

### ufw

For hosts with ufw enabled (Debian based distros), you can use `ufw` command to allow traffic to specific ports. Use below command to allow access to port 9000
对于已启用 ufw 的主机（基于 Debian 的发行版），您可以使用 ufw 命令允许特定端口的流量。使用以下命令允许访问端口 9000

```sh
ufw allow 9000
```

Below command enables all incoming traffic to ports ranging from 9000 to 9010.
下面的命令允许所有进入 9000 至 9010 端口的流量。

```sh
ufw allow 9000:9010/tcp
```

### firewall-cmd
### 防火墙命令

For hosts with firewall-cmd enabled (CentOS), you can use `firewall-cmd` command to allow traffic to specific ports. Use below commands to allow access to port 9000
对于启用了 firewall-cmd 的主机（CentOS），可以使用 firewall-cmd 命令来允许特定端口的流量。使用以下命令允许访问端口 9000


```sh
firewall-cmd --get-active-zones
```

This command gets the active zone(s). Now, apply port rules to the relevant zones returned above. For example if the zone is `public`, use
该命令获取活动区。现在，将端口规则应用到上面返回的相关区域。例如，如果区域是公共的，使用

```sh
firewall-cmd --zone=public --add-port=9000/tcp --permanent
```

Note that `permanent` makes sure the rules are persistent across firewall start, restart or reload. Finally reload the firewall for changes to take effect.
注意，permanent 可确保规则在防火墙启动、重启或重载时保持不变。最后重新加载防火墙，使更改生效。

```sh
firewall-cmd --reload
```

### iptables

For hosts with iptables enabled (RHEL, CentOS, etc), you can use `iptables` command to enable all traffic coming to specific ports. Use below command to allow
access to port 9000
对于启用了 iptables 的主机（RHEL、CentOS 等），可以使用 iptables 命令来启用进入特定端口的所有流量。使用以下命令允许访问端口 9000


```sh
iptables -A INPUT -p tcp --dport 9000 -j ACCEPT
service iptables restart
```

Below command enables all incoming traffic to ports ranging from 9000 to 9010.
服务 iptables 重启
下面的命令允许所有进入 9000 至 9010 端口的流量。


```sh
iptables -A INPUT -p tcp --dport 9000:9010 -j ACCEPT
service iptables restart
```
服务 iptables 重启

## Test MinIO Connectivity
## 测试 MinIO 的连接性

### Test using MinIO Console
### 使用 MinIO 控制台进行测试

MinIO Server comes with an embedded web based object browser. Point your web browser to <http://127.0.0.1:9000> to ensure your server has started successfully.
MinIO 服务器自带基于 Web 的嵌入式对象浏览器。将网络浏览器指向 http://127.0.0.1:9000，确保服务器已成功启动。

> NOTE: MinIO runs console on random port by default, if you wish to choose a specific port use `--console-address` to pick a specific interface and port.
> 注意：MinIO 默认在随机端口上运行控制台，如果要选择特定端口，请使用 --console-address 选择特定接口和端口。

### Things to consider
### 注意事项

MinIO redirects browser access requests to the configured server port (i.e. `127.0.0.1:9000`) to the configured Console port. MinIO uses the hostname or IP address specified in the request when building the redirect URL. The URL and port *must* be accessible by the client for the redirection to work.
MinIO 会将浏览器访问配置的服务器端口（即 127.0.0.1:9000）的请求重定向到配置的控制台端口。MinIO 在创建重定向 URL 时会使用请求中指定的主机名或 IP 地址。客户端必须能访问 URL 和端口，重定向才能生效。

For deployments behind a load balancer, proxy, or ingress rule where the MinIO host IP address or port is not public, use the `MINIO_BROWSER_REDIRECT_URL` environment variable to specify the external hostname for the redirect. The LB/Proxy must have rules for directing traffic to the Console port specifically.
如果部署在负载平衡器、代理或入口规则后面，而 MinIO 主机 IP 地址或端口不是公开的，则使用 MINIO_BROWSER_REDIRECT_URL 环境变量来指定重定向的外部主机名。LB/Proxy 必须有专门将流量导向 Console 端口的规则。

For example, consider a MinIO deployment behind a proxy `https://minio.example.net`, `https://console.minio.example.net` with rules for forwarding traffic on port :9000 and :9001 to MinIO and the MinIO Console respectively on the internal network. Set `MINIO_BROWSER_REDIRECT_URL` to `https://console.minio.example.net` to ensure the browser receives a valid reachable URL.
例如，考虑在代理 https://minio.example.net 后部署 MinIO，https://console.minio.example.net，其规则是将端口 :9000 和 :9001 上的流量分别转发到内部网络上的 MinIO 和 MinIO 控制台。将 MINIO_BROWSER_REDIRECT_URL 设置为 https://console.minio.example.net，以确保浏览器接收到有效的可到达 URL。

| Dashboard                                                                                   | Creating a bucket                                                                           |
| -------------                                                                               | -------------                                                                               |
| ![Dashboard](https://github.com/minio/minio/blob/master/docs/screenshots/pic1.png?raw=true) | ![Dashboard](https://github.com/minio/minio/blob/master/docs/screenshots/pic2.png?raw=true) |

## Test using MinIO Client `mc`

`mc` provides a modern alternative to UNIX commands like ls, cat, cp, mirror, diff etc. It supports filesystems and Amazon S3 compatible cloud storage services. Follow the MinIO Client [Quickstart Guide](https://min.io/docs/minio/linux/reference/minio-mc.html#quickstart) for further instructions.
使用 MinIO 客户端 mc 进行测试
mc 为 ls、cat、cp、mirror、diff 等 UNIX 命令提供了一个现代化的替代工具。它支持文件系统和亚马逊 S3 兼容云存储服务。请按照 MinIO 客户端快速入门指南获取进一步说明。

## Upgrading MinIO
## 升级 MinIO

Upgrades require zero downtime in MinIO, all upgrades are non-disruptive, all transactions on MinIO are atomic. So upgrading all the servers simultaneously is the recommended way to upgrade MinIO.
MinIO 的升级不需要停机时间，所有升级都是非中断性的，MinIO 上的所有事务都是原子性的。因此，同时升级所有服务器是升级 MinIO 的推荐方式。

> NOTE: requires internet access to update directly from <https://dl.min.io>, optionally you can host any mirrors at <https://my-artifactory.example.com/minio/>
> 注意：需要访问互联网才能直接从 https://dl.min.io 更新，也可以选择将任何镜像托管到 https://my-artifactory.example.com/minio/。

- For deployments that installed the MinIO server binary by hand, use [`mc admin update`](https://min.io/docs/minio/linux/reference/minio-mc-admin/mc-admin-update.html)
- 对于手工安装 MinIO 服务器二进制文件的部署，请使用 mc admin update

```sh
mc admin update <minio alias, e.g., myminio>
```

- For deployments without external internet access (e.g. airgapped environments), download the binary from <https://dl.min.io> and replace the existing MinIO binary let's say for example `/opt/bin/minio`, apply executable permissions `chmod +x /opt/bin/minio` and proceed to perform `mc admin service restart alias/`.
- 对于没有外部互联网访问的部署（如 airgapped 环境），从 https://dl.min.io 下载二进制文件并替换现有的 MinIO 二进制文件，例如 /opt/bin/minio，应用可执行权限 chmod +x /opt/bin/minio，然后执行 mc admin service restart alias/。

- For installations using Systemd MinIO service, upgrade via RPM/DEB packages **parallelly** on all servers or replace the binary lets say `/opt/bin/minio` on all nodes, apply executable permissions `chmod +x /opt/bin/minio` and process to perform `mc admin service restart alias/`.
- 对于使用 Systemd MinIO 服务的安装，可在所有服务器上通过 RPM/DEB 软件包并行升级，或在所有节点上替换二进制文件（例如 /opt/bin/minio），应用可执行权限 chmod +x /opt/bin/minio，然后执行 mc 管理服务重启别名/。

### Upgrade Checklist
### 升级清单

- Test all upgrades in a lower environment (DEV, QA, UAT) before applying to production. Performing blind upgrades in production environments carries significant risk.
- Read the release notes for MinIO *before* performing any upgrade, there is no forced requirement to upgrade to latest release upon every release. Some release may not be relevant to your setup, avoid upgrading production environments unnecessarily.
- If you plan to use `mc admin update`, MinIO process must have write access to the parent directory where the binary is present on the host system.
- `mc admin update` is not supported and should be avoided in kubernetes/container environments, please upgrade containers by upgrading relevant container images.
- **We do not recommend upgrading one MinIO server at a time, the product is designed to support parallel upgrades please follow our recommended guidelines.**
- 将所有升级应用到生产环境之前，先在较低的环境（开发环境、质量保证环境、用户测试环境）中进行测试。在生产环境中执行盲目升级会带来很大风险。
- 在执行任何升级之前，请阅读 MinIO 的发行说明，没有强制要求在每次发行时都升级到最新版本。某些版本可能与您的设置无关，请避免不必要地升级生产环境。
- 如果计划使用 mc admin update，MinIO 进程必须有写入主机系统上二进制文件所在父目录的权限。
kubernetes/ 容器环境不支持并应避免使用 mc admin update，请通过升级相关容器映像来升级容器。
- **我们不建议一次升级一个 MinIO 服务器，该产品旨在支持并行升级，请遵循我们推荐的指南。**

## Explore Further
## 进一步探索

- [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html)
- [Use `mc` with MinIO Server](https://min.io/docs/minio/linux/reference/minio-mc.html)
- [Use `minio-go` SDK with MinIO Server](https://min.io/docs/minio/linux/developers/go/minio-go.html)
- [The MinIO documentation website](https://min.io/docs/minio/linux/index.html)

## Contribute to MinIO Project
## 为 MinIO 项目做贡献

Please follow MinIO [Contributor's Guide](https://github.com/minio/minio/blob/master/CONTRIBUTING.md)

## License
## 许可证

- MinIO source is licensed under the [GNU AGPLv3](https://github.com/minio/minio/blob/master/LICENSE).
- MinIO [documentation](https://github.com/minio/minio/tree/master/docs) is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- [License Compliance](https://github.com/minio/minio/blob/master/COMPLIANCE.md)
- MinIO 源代码采用 GNU AGPLv3 许可。
- MinIO 文档采用 CC BY 4.0 许可。
- 许可证合规性
