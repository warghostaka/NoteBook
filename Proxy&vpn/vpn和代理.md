# vpn和代理

工作时发现对于这两个常用的东西还不太了解，包括他们客户端工作原理，一些模式等

vpn我使用的是easy connect客户端，代理使用的是clash客户端，后面将会以此为例

## vpn

**VPN** 全称 **Virtual Private Network（虚拟专用网络）**，本质是一种在公网上建立加密隧道的技术

vpn工作示意图如下：

![image-20250819202728305](C:/Users/WarghostAKA/AppData/Roaming/Typora/typora-user-images/image-20250819202728305.png)

**VPN的核心作用是**：在公网上建立一个加密隧道，将本机的所有流量全部安全传输到VPN远程服务器当中，此时外部看到的本机ip是VPN服务器的ip

**VPN工作过程：**使用easy connect等软件输入服务器地址并连接上vpn后，此时系统会修改路由表或者默认网关，所有的流量（或者部分流量）会先发往VPN的虚拟网卡，同时分配给你一个内网ip，VPN客户端会将这些流量加密并发送到VPN服务器，再由服务器进行解密，并替你访问目标网站

**网络层：**

从访问表现来看，VPN会让目标服务器认为你在VPN所在的网络

---

## 代理 Proxy

简单来说，代理意味着中间人，客户端先把请求发给代理，接着通过代理来访问目标服务器，最后将结果返回给客户端

![image-20250819222031788](C:/Users/WarghostAKA/AppData/Roaming/Typora/typora-user-images/image-20250819222031788.png)

特定应用和端口，或者设置了代理的程序才会走代理

代理通常分为2类：

- **正向代理（Forward Proxy）：**在浏览器和一些应用配置代理的情况下，客户端知道代理地址并向其发送请求，用于访问外网，我们常说的梯子也指的是这种类型
- **反向代理（Reverse Proxy）：**部署在服务器端，外部客户端访问反向代理由他把请求分发到后端真实服务器，常见于负载均衡，TLS终止，缓存等

常见的代理类型也分为2类：

- **HTTP 代理**：只能转发HTTP/ HTTPS协议，应用层代理，客户端将完整 HTTP 请求发给代理。HTTPS 常用 `CONNECT` 建立隧道。只对 HTTP/HTTPS 有良好支持。
- **SOCKS5 代理**：更底层，传输层，支持 TCP（和 UDP 转发）但不理解 HTTP，适用于几乎任何 TCP 应用。

从访问表现上来看，应用认为你通过一个中间人访问互联网

### TUN模式

TUN模式是 **虚拟网络接口模式 （Virtual Network Interface）**，他会在操作系统里创建一个虚拟网卡，然后把发送到网卡的所有IP流量都捕获起来，然后发送到代理客户端进行处理

 ![image-20250819225341884](C:/Users/WarghostAKA/AppData/Roaming/Typora/typora-user-images/image-20250819225341884.png)

TUN模式下的流量捕获范围，是系统所有的的IP层流量，因此可以实现全局代理，覆盖所有应用程序的流量

### 系统代理模式 System Proxy Mode

与TUN模式相对，Clash 在本地启动 **HTTP/HTTPS/SOCKS 代理端口**（如 7890）

**依赖应用**：必须遵循系统代理设置或手动配置代理端口

### 全局模式 Global Mode

**全局模式（Global mode）**：客户端或代理软件把 **所有**（或几乎所有）出站流量都走代理/隧道。等价于把默认路由或系统代理设置为“全量代理”。

全局模式工作原理图如下（非TUN）：

![image-20250819222104349](C:/Users/WarghostAKA/AppData/Roaming/Typora/typora-user-images/image-20250819222104349.png)

值得注意的是，系统代理模式下的全局模式，只能够捕获使用 **HTTP/HTTPS/SOCKS** 协议并配置了代理的应用。这意味着有一些不是这些协议的流量不会被捕获

此外，还有一些不遵循系统代理设置的程序会直接绕过代理，例如：git会直接走本地流量

因此，只有在TUN模式下的全局模式，才是真正意义上的全量代理

> **DNS泄露问题：**在浏览器访问某个网站时，域名解析请求，也就是DNS查询没有走代理，而是直接发给了你本地的DNS服务器，此时网站通过DNS日志能够发现你的域名解析来源于中国ip

### 规则模式 Rules Mode

**规则模式（Rule / Rule-based / PAC / Split mode）**：核心目标是 **分流** 仅当流量匹配某些规则（域名、IP 段、端口、进程、国家/地区、黑白名单等）时才走代理，否则直接访问（直连 Direct）

规则模式工作原理如图（非TUN）：

![image-20250819224921008](C:/Users/WarghostAKA/AppData/Roaming/Typora/typora-user-images/image-20250819224921008.png)

## clash

**clash**是一个基于规则的代理客户端，支持多种协议，并能通过规则决定那些流量走代理，哪些直连
本质上是一个本地运行的流量分流器 + 代理客户端，他会在本地开一个监听端口，配置代理的软件和浏览器流量先到clash，再由其决定走代理还是直连