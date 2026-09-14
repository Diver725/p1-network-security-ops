# P1 第 3 周 A 记录：Wazuh 日志审计平台部署

日期：2026-09-06

## 目标
部署集中式日志审计平台（SIEM），让业务主机日志实时汇总并触发告警（对应等保"安全管理中心"）。

## 环境
- 新增虚拟机 p1-wazuh：固定 IP 192.168.20.101（netplan 静态），4 vCPU / 8GB 内存
- 磁盘：虚拟硬盘扩至 60GB 并扩展 LVM（根分区 58GB）
- 固定 IP 规划：pfSense=.1、客户端=.100、Wazuh=.101、DHCP 池 .110-.199
- 解决 IP 冲突：服务器全部改静态地址，DHCP 起始改为 .110

## 部署过程（Wazuh 4.14.7 单节点：indexer + server + dashboard）
1. 官方安装助手：curl wazuh-install.sh && sudo bash ./wazuh-install.sh -a
2. 遇到并解决两个问题：
   - 磁盘写满（No space left）→ 虚拟硬盘 20G→60G，LVM 在线扩容（growpart + pvresize + lvextend + resize2fs）
   - 首次失败导致 dpkg 数据库损坏，wazuh-manager 无法卸载 → 用空脚本替换 prerm/postrm + dpkg --purge --force-all 清除残留后重装（-o 覆盖）
3. 安装成功，记录 admin 初始密码（存本地密码文件，不入库）

## Agent 接入
- 在 p1-lan-client（192.168.20.100）安装 Wazuh Agent（DEB amd64），配置 Server 为 192.168.20.101
- 启动并设开机自启（systemctl enable/start wazuh-agent）
- 控制台 Endpoints 显示 p1-lan-client 状态 Active

## 验证结果
- Discover 能查到客户端上传的日志（auditd/AppArmor 事件被自动解析成结构化字段，level 3）
- 模拟 SSH 失败登录（对 127.0.0.1 用错误密码尝试 15 次）→ 触发告警 "sshd: authentication failed"（level 5）
- 日志采集 → 规则分析 → 实时告警 的完整链路打通

## 学习点
- SIEM 的价值：多台机器日志集中 + 自动解析 + 规则告警 + 可视化（区别于单机 auditd）
- Wazuh 架构：Agent（采集）→ Manager（分析）→ Indexer（存储）→ Dashboard（展示）
- 告警级别分档：0-2 忽略、3-5 低、6-8 中、9-12 高、13-15 严重
- 日志/索引类服务要预留充足磁盘（官方建议 50GB+）
- 软件包安装失败后 dpkg 可能进入坏状态，需要先修复再重装

## 待办（第 3 周 B / 后续）
- [ ] 雷池 WAF + Web 服务器部署（第 3 周 B）
- [ ] 更多自定义检测规则（第 3 周后半 / 第 5 周）
- [ ] 截图：Discover 日志事件页（可选补充）
