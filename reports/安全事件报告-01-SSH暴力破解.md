# 安全事件报告 01：SSH 暴力破解事件

- 报告编号：P1-IR-2026-0910-01
- 事件日期：2026-09-10
- 报告日期：2026-09-10
- 编制：实验演练（攻击方/防御方均由本人操作，隔离内网）
- 事件等级：中（已阻断，未造成入侵）

## 一、事件概述
2026-09-10 10:10，模拟外网攻击机 Kali（10.0.2.4）通过 pfSense WAN 端口映射（10.0.2.10:2222 → 192.168.20.100:22）对内网主机 `p1-lan-client` 的 SSH 服务发起密码暴力破解（8 次错误密码）。主机端 fail2ban 达到阈值后自动封禁攻击源 IP，Wazuh 同步产生检测告警。经核查未发现成功登录与主机篡改，事件被成功阻断。

## 二、时间线
| 时间（UTC） | 事件 |
|---|---|
| 10:10:06 | 攻击方使用 hydra 对 10.0.2.10:2222 发起 SSH 爆破（8 个密码，4 并发） |
| 10:10:10 - 10:10:12 | 目标主机 auth.log 记录 8 次 Failed password for lab from 10.0.2.4 |
| 10:10 左右 | Wazuh 触发告警：rule.id 5760，sshd: authentication failed，level 5，firedtimes 6 |
| 10:10 左右 | fail2ban 触发封禁：Total failed 8，Currently banned 1，封禁 10.0.2.4 |
| 10:12 | 安全分析：核查 Accepted 登录、last 登录记录、auditd 记录 |
| 10:12+ | 研判结论：攻击未成功，服务正常；按流程恢复 |

## 三、攻击路径
```
Kali 攻击机 (10.0.2.4，模拟外网)
   ↓ 访问 pfSense WAN 10.0.2.10:2222（NAT 端口映射）
pfSense 防火墙（NAT + WAN 放行规则）
   ↓ 转发到内网 192.168.20.100:22
p1-lan-client（SSH 服务）
   ↓ 认证失败日志写入 auth.log
Wazuh Agent → Wazuh Manager（规则匹配 → 告警）
fail2ban（读取 auth.log → 阈值封禁攻击源）
```

## 四、IOC（失陷指标）
| 类型 | 内容 |
|---|---|
| 攻击源 IP | 10.0.2.4（Kali 攻击机） |
| 攻击目标 | 10.0.2.10:2222（映射到 192.168.20.100:22） |
| 被尝试账号 | lab |
| 攻击特征 | 3 秒内 8 次 SSH 认证失败；hydra 并发连接特征 |
| 检测规则 | Wazuh rule.id 5760（sshd: authentication failed），level 5 |
| MITRE ATT&CK | T1110.001（Password Guessing）、T1021.004（SSH） |
| 合规关联 | PCI DSS 10.2.4/10.2.5、HIPAA 164.312.b、NIST 800-53 AU.14/AC.7 |

## 五、影响评估
- 攻击是否成功：**否**。无来自 10.0.2.4 的成功登录记录（Accepted），无异常会话。
- 系统完整性：auditd 未发现关键文件（/etc/shadow 等）被篡改。
- 服务可用性：SSH 服务正常，攻击被自动封禁，未影响正常管理访问。
- 风险等级：中（存在对外暴露的 SSH 入口 + 弱口令爆破尝试，但防护有效）。

## 六、处置动作
1. 检测：Wazuh 产生 level 5 告警；fail2ban 记录失败 8 次并自动封禁 10.0.2.4
2. 研判：核对攻击源、目标、失败次数、时间窗口，确认为 SSH 暴力破解
3. 处置（自动）：fail2ban 封禁攻击源 IP（bantime 600 秒）
4. 痕迹检查：`grep Accepted /var/log/auth.log`、`last`、`ausearch -k shadow_changes` 均未见入侵痕迹
5. 恢复：封禁到期自动解封（或人工解封），验证 SSH 正常登录与服务可用

## 七、复盘与改进建议
1. **减少暴露面**：生产环境不应将运维 SSH 直接暴露到外网；应通过 VPN/堡垒机访问，或限制来源 IP
2. **强化认证**：为 SSH 启用密钥认证并关闭密码登录（本项目可在后续整改中实施）
3. **保留自动封禁**：fail2ban 策略有效，建议结合企业级方案（如防火墙联动封禁、SOC 平台自动处置）
4. **完善集中审计**：将 pfSense 防火墙日志接入 Wazuh（R4），实现"边界+主机"双源告警关联
5. **完善检测规则**：Suricata ET 规则加载问题解决后，可从网络层同时检出扫描/爆破行为（R6）

## 八、附件（截图）
- screenshots/2026-09-10-演练-SSH爆破封禁.png（fail2ban 封禁状态）
- screenshots/2026-09-10-演练-SSH爆破日志.png（auth.log 失败记录）
- screenshots/2026-09-10-演练-Wazuh告警.png（Wazuh 告警详情）
