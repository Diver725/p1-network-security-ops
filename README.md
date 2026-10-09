# P1 · 企业网络安全建设与等保整改

定位：安全设备运维 + 等保 2.0 整改闭环（就业导向，对标央国企网安运维岗位）。

## 技术栈
pfSense（防火墙分区）+ Suricata（IDS/IPS）+ 雷池 SafeLine WAF + Wazuh（SIEM 日志审计 / 安全管理中心）+ nginx（Web 业务）+ fail2ban（登录失败封禁）+ Kali（模拟外网攻击机）

## 项目环境（全隔离内网）
| 设备 | 地址 | 作用 |
|---|---|---|
| pfSense | WAN 10.0.2.10 / LAN 192.168.20.1 | 边界防火墙、NAT、DHCP、Suricata |
| p1-lan-client | 192.168.20.100 | 业务主机（已加固、已接入审计） |
| p1-wazuh | 192.168.20.101 | SIEM：日志集中、告警、仪表盘 |
| p1-web | 192.168.20.102 | Web 服务器（nginx） |
| p1-waf | 192.168.20.103 | 雷池 WAF |
| Kali | 10.0.2.4 | 模拟外网攻击机 |

## 当前状态
- [x] 第 1 周：网络分区 + pfSense + Suricata
- [x] 第 2 周：业务主机安全配置（SSH/口令/服务/SUID/审计）
- [x] 第 3 周 A：Wazuh 日志审计（Agent 接入、日志与告警验证）
- [x] 第 3 周 B：雷池 WAF + Web 服务器（SQL 注入拦截验证）
- [x] 第 4 周：Kali 攻击机、等保差距分析与整改（R1-R3 完成并复测）
- [x] 第 5 周：安全事件演练（SSH 暴力破解、Web SQL 注入）与事件报告
- [x] R4 防火墙日志接入 Wazuh（边界日志集中审计）
- [ ] 收尾补充：ClamAV（R5）、Suricata ET 规则（R6）、可选 JumpServer

## 项目成果

- 完成企业网络分区、防火墙、Suricata IDS、雷池 WAF 和 Wazuh SIEM 部署。
- 完成业务主机 SSH、口令、服务、SUID、auditd 和 fail2ban 加固。
- 按等保 2.0 三级要求完成整改、复测和报告。
- 完成 SSH 暴破与 SQL 注入两类安全事件演练。

