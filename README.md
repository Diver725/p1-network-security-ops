# P1 · 企业网络安全建设与等保整改

定位：安全设备运维 + 等保 2.0 整改闭环（就业导向，对标央国企网安运维岗位）。

## 技术栈
pfSense（防火墙分区）+ Suricata（IPS，装于 pfSense）+ 雷池 WAF + Wazuh（日志审计 / 安全管理中心）+ 跳板机（运维审计）+ Kali（攻击机，仅内网）

## 周期
约 4-5 周

## 目录
- architecture/   网络拓扑图
- docs/           部署文档、操作步骤、排错记录
- scripts/        部署与检查脚本（先读懂再执行）
- configs/        pfSense / Suricata / WAF / Wazuh 配置片段
- screenshots/    实验截图
- reports/        安全事件报告、等保差距分析与整改报告

## 当前状态
- [x] 第 1 周：网络分区 + pfSense + Suricata（Suricata 告警实测待第 4 周 Kali）
- [x] 第 2 周：业务主机安全配置（SSH/口令/服务/SUID/审计）
- [x] 第 3 周 A：Wazuh 日志审计（Agent+日志+告警验证）
- [x] 第 3 周 B：雷池 WAF + Web 服务器（SQL 注入拦截验证通过）
- [x] 第 4 周 B：等保差距分析 + 整改 + 复测（R1-R3 完成）
- [ ] 第 5 周：事件演练 + 收尾（可选 JumpServer）

## 技能点清单（项目完成后补，对齐岗位 JD 关键词）

## 简历描述（STAR，项目完成后补）

## 面试高频题（项目完成后补）






