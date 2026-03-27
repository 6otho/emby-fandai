# 🚀 私有反代与智能 DNS 调度核心

基于 **Cloudflare Workers** 构建的轻量级 Serverless 私有核心面板。  
集成 **动态反向代理管理 + 优选 IP 抓取 + 本地测速 + 自动 DNS 调度** 于一体，  
同时完美适配 **移动端 & PC 端** 使用。

---

# ✨ 核心功能

🛡️ **安全鉴权登录**  
使用 Token 保护控制面板访问。

🌐 **优选节点抓取与测速**

- 一键抓取全网优选 IP（电信 / 联通 / 移动 / 多线 / IPv6）
- 基于本地浏览器进行真实延迟与可用性测试

☁️ **智能 DNS 调度**

- 自动筛选最快节点
- 一键写入 **Top 3 IP → Cloudflare DNS（A / AAAA）**

🔗 **动态反向代理**

- 支持路径前缀映射
- 支持多种安全模式：
  - IP 抹除
  - 严格透传
  - 双重透传

📱 **移动端优化**

- 防缩放设计
- 自适应布局
- 接近原生 App 使用体验

---

# 🛠️ 部署前准备

请准备：

- 一个 **Cloudflare 账号**
- 一个托管在 Cloudflare 的 **域名**

---

# 📦 部署步骤 / Deployment Steps

## 第一步：创建 D1 数据库

进入：

```
Storage & Databases → D1
```

创建数据库，例如：

```
proxy_db
```

📌 说明：

- 系统会自动建表
- 无需手动编写 SQL

---

## 第二步：创建 Worker

进入：

```
Workers & Pages → Create Application → Create Worker
```

创建 Worker，例如：

```
dns-proxy-core
```

然后：

1. 点击 `Edit Code`
2. 用项目代码 **完全覆盖默认代码**
3. 点击 `Deploy`

---

## 第三步：绑定 D1 数据库

进入：

```
Settings → Variables and Secrets → Bindings
```

添加 D1 绑定：

| 变量名 | 数据库 |
|---|---|
| DB | proxy_db |

⚠️ 必须使用变量名：

```
DB
```

否则代码无法运行。

---

## 第四步：配置环境变量（必需）

进入：

```
Variables and Secrets → Environment Variables
```

添加以下变量：

| 变量名 | 说明 | 示例 |
|---|---|---|
| ADMIN_TOKEN | 面板登录密码 | MySecretPassword123! |
| CF_ZONE_ID | Cloudflare 区域 ID | 023e105f4ecef8ad9ca... |
| CF_DOMAIN | 需要调度的子域名 | emby.yourdomain.com |
| CF_API_TOKEN | Cloudflare API Token | vA_mXYZ... |

保存后点击：

```
Deploy
```

---

## 🔑 获取 CF_API_TOKEN

步骤：

1. 进入 Cloudflare 控制台
2. 点击头像 → **My Profile**
3. 进入 **API Tokens**
4. 点击 **Create Token**
5. 选择模板：

```
Edit zone DNS
```

6. 设置权限：

```
Zone → DNS → Edit
```

7. 选择：

```
Specific zone → 你的域名
```

生成并复制 Token。

---

# 🎮 使用方法

## 登录面板

访问：

```
https://你的Worker域名
```

输入：

```
ADMIN_TOKEN
```

---

## 🌐 节点测速

1. 选择线路类型
2. 点击：

```
获取节点并测速
```

3. 等待测速完成
4. 点击：

```
更新 TOP3 至 DNS
```

系统会自动：

- 选择最快 3 个 IP
- 写入 Cloudflare DNS

---

## 🔗 部署反代节点

填写：

- 路径前缀（例如：`api`）
- 后端地址（例如：`http://1.1.1.1:8080`）

访问方式：

```
https://你的域名/api
```

等同于访问：

```
http://1.1.1.1:8080
```

---

# ⚠️ 注意事项

## 🔐 安全

请妥善保管：

- `ADMIN_TOKEN`
- `CF_API_TOKEN`

---

## ⚠️ DNS 覆盖警告

执行以下操作时：

- 更新 DNS
- 设为唯一解析

系统会：

👉 删除该域名下所有 A / AAAA 记录  
👉 写入新的 IP

❗ 强烈建议：

- 仅使用 **子域名**
- **不要使用主域名（根域名）**

否则可能导致网站宕机。

---

## 📡 测速说明

测速基于：

```
浏览器本地请求（Web API）
```

因此结果会受到：

- 当前网络环境
- 浏览器性能

的影响。

---

<p align="center">
Made with ❤️ by Cloudflare Workers
</p>
