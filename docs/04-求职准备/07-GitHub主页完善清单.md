# GitHub 主页与仓库完善清单

## 一、当前检查结果

已经确认：

```text
GitHub 用户名：Diver725
主页：https://github.com/Diver725
公开仓库数量：3
默认分支：main
```

三个仓库可见性：

| 仓库 | 可见性 | 默认分支 | 当前描述 |
|---|---|---|---|
| p1-network-security-ops | Public | main | 已有描述 |
| p2-vuln-hardening | Public | main | 暂无描述 |
| p3-soc-blue-team | Public | main | 暂无描述 |

本地检查结果：

- 没有发现已跟踪的 `.vdi`、`.vbox`、`.ova`、`.iso` 等大文件
- 没有发现手机号、QQ 邮箱、私钥、Token 等敏感信息被提交
- 三个仓库工作区均与远端同步
- P3 文档中出现 `username=admin&password=password`，这是 DVWA 实验默认账号示例，需要在 README 或文档中明确标注为实验默认值
- P2 中保留 `PasswordAuthentication yes` 是为了 GVM 认证扫描，面试时需要解释这是实验设计取舍
- P1 文档中提到了本地密码文件路径，但不包含密码内容；如担心信息泄露，可以改成“本地密码文件”

## 二、你需要手动完成的事情

GitHub 网页端操作：

1. 设置个人资料
```text
Name：王一峰
Bio：网络空间安全 | Linux 运维 | 安全运维 | Wazuh SIEM
Location：南京 / 北京 / 上海 / 广州 / 深圳
```

2. 置顶三个仓库
```text
p1-network-security-ops
p2-vuln-hardening
p3-soc-blue-team
```

3. 给仓库添加描述
```text
P1：企业网络安全建设与等保整改：pfSense、Suricata、WAF、Wazuh、Kali
P2：漏洞管理与安全基线自动化加固：GVM、Lynis、Ansible
P3：SOC 安全运营与护网应急：Wazuh、自定义规则、IOC、事件报告
```

4. 给仓库添加 Topics
```text
P1：cybersecurity, pfsense, suricata, waf, wazuh, kali-linux, dengbao
P2：vulnerability-management, gvm, lynis, ansible, linux-hardening, security-baseline
P3：wazuh, siem, soc, blue-team, incident-response, fim, detection-engineering
```

5. 确认仓库公开，但不要放：
```text
手机号
QQ 邮箱
密码
Token
私钥
虚拟机磁盘
真实客户数据
```

## 三、建议新增 GitHub Profile README

需要在 GitHub 新建一个与用户名同名的仓库：

```text
仓库名：Diver725
可见性：Public
```

然后添加 `README.md`，示例：

```markdown
# 王一峰

东南大学网络空间安全专业，2027 届。

关注方向：Linux 运维、安全运维、SOC 安全运营、等保合规。

## 项目

- [企业网络安全建设与等保整改](https://github.com/Diver725/p1-network-security-ops)
- [漏洞管理与安全基线自动化加固](https://github.com/Diver725/p2-vuln-hardening)
- [SOC 安全运营与护网应急响应](https://github.com/Diver725/p3-soc-blue-team)

## 技术关键词

Linux · Networking · Wazuh · SIEM · GVM · Lynis · Ansible · UFW · Suricata · WAF · Incident Response
```

## 四、推荐处理顺序

```text
1. 设置 GitHub 名称和简介
2. 给 P2、P3 添加描述和 Topics
3. 置顶三个仓库
4. 确认 P1、P2、P3 的 README 首页展示完整
5. 创建 Diver725/Diver725 个人主页 README
6. 最后把 GitHub 链接写入简历
```

