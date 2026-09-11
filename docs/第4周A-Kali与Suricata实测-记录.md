# P1 第 4 周 A 记录：Kali 攻击机 + Suricata 告警实测（部分完成）

日期：2026-09-08

## 已完成
1. Kali 部署：官方预装 VirtualBox 镜像（kali-linux-2026.2），网卡接 NAT 网络 p1-wan（模拟外网攻击者），DHCP 获 10.0.2.4，默认账号 kali/kali
2. 攻击路径验证：Kali ping 通 pfSense WAN（10.0.2.10）
3. nmap 扫描：nmap -sS -Pn -p 1-2000 10.0.2.10
   - 结果：Host up，2000 端口全部 filtered（无回应）
   - 含义：pfSense 默认 WAN 策略阻断所有入站，攻击者扫不到开放端口 = 防火墙行为符合预期

## Suricata 状态
- Suricata 8.0.5 运行正常，在 WAN(em0) 抓包（pcap running，日志无异常）
- 问题：ET Open 规则已下载安装（/usr/local/share/suricata/rules，84 个文件），但引擎只加载 415 条内置规则，无任何 ET 扫描/攻击规则 → nmap 扫描不产生告警

## 排查结论（已通过 SSH 定位）
- 规则更新日志显示"下载+安装完成"，规则文件实际在 /usr/local/share/suricata/rules/
- 插件重建接口规则（suricata.rules）时只编入内置事件规则（415 条），未包含 ET 规则
- 尝试：手工合并追加（被重启重建覆盖）、复制到 /usr/local/etc/suricata/rules（GUI 可见类别但加载仍 415）
- 判定：pfSense 2.9.0 + Suricata 8 插件存在规则集成兼容问题，记待办

## 已验证的告警能力（不依赖 Suricata）
- Wazuh：SSH 爆破触发告警（第 3 周 A，level 5）
- 雷池 WAF：SQL 注入被拦截（第 3 周 B，403）

## 待办
- [ ] 解决 Suricata ET 规则加载（思路：SID Mgmt 批量启用 / 重装 suricata 插件 / 换 OPNsense 验证；不影响 P1 主体）
- [ ] 第 4 周 B：等保差距分析与整改闭环（不依赖 Suricata，可正常推进）
- [ ] 第 5 周：事件演练（可用 Wazuh/WAF 已有告警能力组织）

---

## 追加排查（2026-09-12）与最终结论

**本次尝试**
1. 将 84 个规则文件（含全部 emerging-*.rules）从 /usr/local/share/suricata/rules 复制到插件预期目录 /usr/local/etc/suricata/rules
2. 重启 WAN 实例：引擎仍只加载 415 条内置规则（suricata.log 显示 1 rule files processed）
3. 删除 /usr/local/etc/suricata/emerging.rules.tar.gz.md5 强制重新下载规则并再次 Update：更新日志显示下载与安装成功
4. 再次检查 WAN Rules 的 Available Rule Categories：仍无 emerging-* 类别（仅有内置事件规则类别）

**结论**
- 现象：ET Open 规则文件已正确安装（/usr/local/share/suricata/rules，84 个文件），但 pfSense 2.9.0 内置的 Suricata 8.0.5 插件在重建接口规则时未将 ET 规则注册为可用类别，导致引擎仅加载 415 条内置规则。
- 判定：pfSense 2.9 + Suricata 8 插件组合的规则集成缺陷，非配置错误。
- 决定（按时间预算）：中止该项调试，记为已知限制。P1 的检测能力已由 Wazuh（主机日志告警）与雷池 WAF（Web 攻击拦截）两条链路验证；Suricata 的入侵检测告警将使用替代方案实现（P1 收尾或后续项目单独部署独立 Suricata 演示）。

**后续可选方案**
- 方案一：在 Ubuntu 虚拟机上单独部署 Suricata，对镜像/转发流量或抓包文件（pcap）进行检测演示
- 方案二：P1 收尾时尝试重装 suricata 插件或调整规则注册方式
- 方案三：在 P3（SOC）中直接使用 Wazuh 作为主要检测源，Suricata 作为可选补充
