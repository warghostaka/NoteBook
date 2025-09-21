# Linux重要的系统文件

## /etc 初始化系统重要文件

| `/etc/sysconfig/network-scripts/ifcfg-eth0 `                | 网卡配置文件                           |
| ----------------------------------------------------------- | -------------------------------------- |
| `/etc/resolv.conf`                                          | Linux系统DNS客户端配置文件             |
| `/etc/hostname(CentOS 7)  /etc/sysconfig/network(CentOS 6)` | 主机名配置文件                         |
| `/etc/hosts`                                                | 系统本地的DNS解析文件                  |
| `/etc/fstab`                                                | 配置开机设备自动挂载的文件             |
| `/etc/rc.local`                                             | 存放开机自启动程序命令的文件           |
| `/etc/inittab`                                              | 系统启动设定运行级别等配置的文件       |
| `/etc/profile    /etc/bashrc`                               | 配置系统的环境变量/别名等文件          |
| `/etc/profile.d`                                            | 用户登录后执行的脚本所在目录           |
| `/etc/issue     /etc/issue.net`                             | 配置在用户登录终端前显示信息的文件     |
| `/etc/init.d`                                               | 软件启动程序所在目录（CentOS 6）       |
| `/usr/lib/systemd/system`                                   | 软件启动程序所目录（CentOS 7）         |
| `/etc/motd`                                                 | 配置用户登录系统之后显示提示内容的文件 |
| `/etc/redhat-release`                                       | 声明RedHat版本号和名称信息的文件       |
| `/etc/sysctl.conf`                                          | Linux内核参数设置文件                  |

> 域名：通常对应一个或者多个ip地址，通过DNS服务器可以对域名进行解析，转换成ip地址后访问
>
> 如果需要设置一个假的域名作测试访问，需要在hosts文件添加本地域名与网址