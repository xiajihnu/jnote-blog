# 互联网访问

Last updated August 20, 2025

---

在大陆访问国际互联网总是会收到各种限制，本文主要记录在绕开这些限制过程中，遇到的技术问题。编写本文的主要目的在于，供将来遇到相同或相似问题时，提供现成的解决方案，而不必重复使用搜索引擎和大模型寻求答案。

## 低价云服务器收集
本章的主要目的时要记录在网上冲浪过程中了解到的低价云服务器套餐和购买链接，为后期更换代理节点和VPN节点做准备。

1. [racknerd: 10.96美元一年](https://racknerd.com/specials/)

2. [aliyun国际站: 9.9美元一年](https://www.alibabacloud.com/zh?_p_lc=1)

3. [博客，收集了各种VPS](https://www.duangvps.com/archives/category/vps%e5%88%86%e4%ba%ab)

## 资源收集
本章主要收集一些学习资源或工具资源

[Xray论坛，扫盲文章很有用](https://xtls.github.io/document/)

## Openvpn服务搭建
本章主要记录Openvpn服务的搭建步骤，实现VPN隧道加密。Openvpn服务可用于家庭网络的访问，同时，其也可以在某些特定场景下实现国际互联网的访问，且带宽上限非常高，甚至在用网高峰期，也能稳定保持理论带宽上限的90%。

### OpenVPN服务器部署流程

#### 环境搭建——必备软件安装
需要安装easyrsa，openvpn

#### 生成证书和密钥等文件
使用脚本一键式生成所有必要证书和密钥

#### 启动OpenVPN服务器并测试服务器的连通性
启动服务器之后，确保服务器正常运行，同时尝试使用客户端连接服务器。

#### OpenVPN流量转发配置方法
如果成功在ubuntu上部署了openvpn，并且客户端也可以连接到vpn服务器。但无法通过vpn流量转发上网，应该是vpn服务器上的流量转发没有设置。要在Ubuntu上配置VPN服务器以允许流量转发，可以采取以下步骤：

1. 启用内核 IP 转发 编辑 /etc/sysctl.conf 文件，取消注释或添加以下行以启用 IP 转发：
```
sudo vim /etc/sysctl.conf
```
确保以下行没有注释（没有 # 开头）：
```
net.ipv4.ip_forward=1
```
保存并关闭文件后，执行以下命令使更改生效：
```
sudo sysctl -p
```

2. 配置iptables以进行流量转发 使用 iptables 工具配置流量转发。在我的服务器中，VPN接口是 tun0，外部接口是 eth0， 10.8.0.0/24 是实际使用的VPN IP 地址范围。。
```
# For ipv4
sudo iptables -A FORWARD -i tun0 -o eth0 -s 10.8.0.0/24 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE

# For ipv6
sudo ip6tables -A FORWARD -i tun0 -o eth0 -s fd00:1234:5678::/64 -m conntrack --ctstate NEW -j ACCEPT
sudo ip6tables -A FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
sudo ip6tables -t nat -A POSTROUTING -s fd00:1234:5678::/64 -o eth0 -j MASQUERADE
```

3. 保存这些规则并使其在每次系统启动时都生效： 
```
   sudo iptables-save | sudo tee /etc/iptables/rules.v4
```
持久保存iptables规则 确保在系统重新启动后，iptables 规则得以保留。在 Ubuntu 中，可以通过设置 iptables-persistent 包来实现。
```
sudo apt-get update
sudo apt-get install iptables-persistent
```
安装过程中会询问是否保存当前的 iptables 规则，选择是。这样就会在系统启动时加载之前保存的规则。

4. 重启 OpenVPN 服务 重新启动 OpenVPN 服务，以便应用所做的更改：
```
sudo systemctl restart openvpn@xx-vpn
```
完成以上步骤后，应该能够允许VPN服务器上的流量转发，并允许连接到VPN的客户端访问互联网。确保适当修改iptables规则以匹配你的网络配置。

