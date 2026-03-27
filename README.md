# 🚀 Private Reverse Proxy & Smart DNS Core

> 基于 **Cloudflare Workers + D1 + DNS API** 构建的 Serverless 私有反代与智能调度核心  
> 集成 **优选 IP 抓取 / 本地测速 / DNS 自动调度 / 反向代理管理**

---

![Cloudflare](https://img.shields.io/badge/Cloudflare-Workers-F38020?logo=cloudflare&logoColor=white)
![D1](https://img.shields.io/badge/Database-D1-blue)
![License](https://img.shields.io/badge/License-MIT-green)

---

# 🧠 系统架构

```
                ┌──────────────────────┐
                │   User Browser (UI)  │
                │  (测速 / 管理面板)     │
                └─────────┬────────────┘
                          │
                          ▼
               ┌──────────────────────┐
               │  Cloudflare Worker   │
               │  (核心控制逻辑层)      │
               └───────┬─────┬────────┘
                       │     │
        ┌──────────────┘     └──────────────┐
        ▼                                   ▼
┌──────────────────┐              ┌────────────────────┐
│   D1 Database     │              │ Cloudflare DNS API │
│ (节点/配置存储)     │              │ (自动写入 A/AAAA)   │
└──────────────────┘              └────────────────────┘
                       │
                       ▼
             ┌──────────────────┐
             │ Reverse Proxy     │
             │ (动态转发流量)      │
             └──────────────────┘
```

---

# ✨ 核心功能

### 🛡️ 安全鉴权
- Token 登录保护
- 无账号系统，轻量安全

### 🌐 优选 IP 抓取 + 本地测速
- 支持：电信 / 联通 / 移动 / 多线 / IPv6
- 浏览器本地真实延迟测试

### ☁️ 智能 DNS 调度
- 自动筛选最快节点
- 一键写入 Cloudflare DNS
- 支持 A / AAAA 记录

### 🔗 动态反向代理
- 路径前缀映射
- 多级安全策略：
  - IP 隐藏
  - 严格透传
  - 双重透传

### 📱 移动端优化
- 自适应 UI
- 防缩放
- 类原生体验

---

# 📊 工作流程

```
抓取 IP → 本地测速 → 排序 → 选出 Top3 → 写入 DNS → 用户访问 → 自动调度
```

---

# 🛠️ 部署准备

- Cloudflare 账号
- 已接入 Cloudflare 的域名

---

# 📦 部署步骤

## 1️⃣ 创建 D1 数据库

```
Storage & Databases → D1 → Create
```

示例：

```
proxy_db
```

---

## 2️⃣ 创建 Worker

```
Workers & Pages → Create Worker
```

示例：

```
dns-proxy-core
```

部署后：

- 编辑代码
- 覆盖 `worker.js`
- 点击 Deploy

---

## 3️⃣ 绑定数据库

```
Settings → Bindings → Add D1
```

| 变量名 | 值 |
|---|---|
| DB | proxy_db |

---

## 4️⃣ 环境变量

| 变量名 | 说明 |
|---|---|
| ADMIN_TOKEN | 登录密码 |
| CF_ZONE_ID | Cloudflare Zone ID |
| CF_DOMAIN | 调度域名 |
| CF_API_TOKEN | DNS API Token |

---

# 🔑 API Token 获取

路径：

```
Profile → API Tokens → Create Token
```

选择模板：

```
Edit zone DNS
```

权限：

```
Zone → DNS → Edit
```

---

# 🎮 使用方法

## 登录

```
https://你的Worker域名
```

输入：

```
ADMIN_TOKEN
```

---

## 🌐 节点测速

1. 获取节点
2. 本地测速
3. 点击：

```
更新 TOP3 至 DNS
```

---

## 🔗 添加反代

输入：

```
路径: /api
后端: http://1.1.1.1:8080
```

访问：

```
https://域名/api
```

---

# 📁 项目结构（推荐）

```
.
├── worker.js
├── README.md
├── assets/
│   ├── ui-preview.png
│   └── architecture.png
```

---

# 🎯 使用场景

- 🌍 流媒体加速（Emby / Plex）
- 🚀 代理节点调度
- 🌐 海外服务优化
- 🧠 智能 DNS 分流
- 🔐 私有反代网关

---

# ⚠️ 注意事项

### 🔐 安全

请保护：

- ADMIN_TOKEN
- CF_API_TOKEN

---

### ❗ DNS 覆盖风险

操作会：

- 删除所有 A/AAAA
- 写入新记录

⚠️ 请务必使用：

```
子域名（如 emby.xxx.com）
```

不要使用主域名！

---

### 📡 测速说明

测速基于浏览器环境：

- 受网络影响
- 非服务器级精度

---

# 🚀 未来规划

- [ ] 自动定时调度（Cron）
- [ ] 节点健康检测
- [ ] 多区域 DNS 策略
- [ ] UI 可视化图表
- [ ] 多用户系统

---

<p align="center">
Made with ❤️ by Cloudflare Workers
</p>
