# P1 网络拓扑说明

## 文字拓扑

```
【外网 p1-wan】10.0.2.0/24（NAT 网络，可真实上网）
   出口 10.0.2.1
   pfSense WAN (em0) = 10.0.2.10
   （Kali 攻击机第 4 周加入本段，模拟外网攻击者）
            │
      ┌─────▼──────┐
      │  pfSense   │  防火墙/NAT/DHCP/Suricata(IPS)
      │  CE 2.8.1  │  LAN (em1) = 192.168.20.1
      └─────┬──────┘
            │
【内网】192.168.20.0/24（host-only #3）
   192.168.20.1    pfSense LAN（网关）
   192.168.20.254  Windows 主机（管理机）
   192.168.20.100  p1-lan-client（Ubuntu 24.04，DHCP 获取）
   （后续：办公主机、Wazuh 日志审计平台加入本段）

【DMZ 备用】192.168.10.0/24（host-only #2）— 后续放 Web 服务器 + WAF
```

## 待办
- [ ] 用 draw.io 绘制正式拓扑图并存为 topology.png（源文件一并保存）
