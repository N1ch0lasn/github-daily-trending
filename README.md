# GitHub 每日自动推荐

每天定时获取 GitHub 热门项目，**自动 Fork 到你的 GitHub**，并生成推荐文档。

---

## ✨ 功能亮点

- 📅 **每日自动获取** - 北京时间 14:00 准时运行
- 🍴 **自动 Fork** - 每天自动 Fork 最多 3 个新项目
- 📄 **生成文档** - 自动生成推荐说明文档
- 📊 **历史记录** - 保存所有历史推荐和 Fork 记录
- 🎯 **智能筛选** - 只推荐适合部署到 Cloudflare 的项目

---

## 📁 文件说明

```
每日推荐/
├── README.md              # 本文件
├── .github/
│   └── workflows/
│       └── daily-trending.yml  # GitHub Actions 定时任务
├── latest.md              # 最新推荐（始终指向最新）
└── YYYY-MM-DD.md          # 每日推荐存档
```

---

## ⚙️ 设置步骤

### 方式一：使用 GitHub Actions（推荐）⭐

**优点：** 完全免费、无需服务器、自动运行、自动 Fork

#### 快速设置（5 分钟）

1. **创建 GitHub Token**
   - 打开 https://github.com/settings/tokens
   - 创建新 Token，勾选 `repo` 和 `workflow` 权限
   - 复制 Token

2. **添加到仓库 Secrets**
   - 你的仓库 → Settings → Secrets and variables → Actions
   - 新建 Secret：`FORK_TOKEN` = 你的 Token

3. **初始化并推送仓库**
   ```bash
   cd /workspaces/codespaces-blank/github 热点/每日推荐
   git init
   git add .
   git commit -m "✨ Auto fork setup"
   
   # 替换为你的 GitHub 用户名
   YOUR_USERNAME="your-username"
   git remote add origin https://github.com/$YOUR_USERNAME/github-daily-trending.git
   git branch -M main
   git push -u origin main
   ```

4. **启用 Actions**
   - 进入仓库 → Actions → 启用 workflow

5. **测试运行**
   - Actions → Daily Trending + Auto Fork → Run workflow

📖 **详细教程**: [自动 Fork 设置指南.md](./自动 Fork 设置指南.md)

---

### 方式二：本地 Cron 定时任务

**适用场景：** 有自己的服务器/VPS

1. **安装依赖**
   ```bash
   # 安装 gh CLI (推荐)
   brew install gh  # macOS
   # 或
   curl -fsSL https://github.com/github/hub/raw/master/script/get-github | bash
   
   # 或使用 apt
   sudo apt update && sudo apt install gh
   ```

2. **配置 GitHub Token**
   ```bash
   export GITHUB_TOKEN="your_token_here"
   ```

3. **添加 Cron 任务**
   ```bash
   crontab -e
   
   # 添加以下行（每天北京时间 14:00 运行）
   0 6 * * * /workspaces/codespaces-blank/github\ 热点/每日自动获取.sh
   ```

4. **测试脚本**
   ```bash
   chmod +x /workspaces/codespaces-blank/github\ 热点/每日自动获取.sh
   /workspaces/codespaces-blank/github\ 热点/每日自动获取.sh
   ```

---

## 📅 定时任务配置

### GitHub Actions 时间修改

编辑 `.github/workflows/daily-trending.yml`：

```yaml
on:
  schedule:
    # 每天 UTC 6:00 (北京时间 14:00)
    - cron: '0 6 * * *'
```

**常用时间：**

| Cron 表达式 | 北京时间 | 说明 |
|------------|----------|------|
| `0 0 * * *` | 08:00 | 每天早上 8 点 |
| `0 6 * * *` | 14:00 | 每天下午 2 点 |
| `0 12 * * *` | 20:00 | 每天晚上 8 点 |
| `0 16 * * *` | 00:00 | 每天凌晨 0 点 |

[Cron 表达式生成器](https://crontab.guru/)

---

## 📊 查看推荐

- **最新推荐：** `每日推荐/latest.md`
- **历史推荐：** `每日推荐/YYYY-MM-DD.md`

---

## 🔧 自定义配置

### 修改筛选条件

编辑 `daily-trending.yml` 中的搜索条件：

```javascript
// 搜索 > 1000 stars 的项目
path: '/search/repositories?q=stars:>1000+...'

// 搜索 > 500 stars 的项目
path: '/search/repositories?q=stars:>500+...'

// 搜索特定主题
path: '/search/repositories?q=topic:cloudflare+...'
```

### 修改推荐数量

```javascript
// 获取 15 个项目
per_page=15

// 获取 30 个项目
per_page=30
```

---

## 📝 输出示例

```markdown
# GitHub 每日推荐 - 2026 年 3 月 7 日

自动生成 | 精选适合部署到 Cloudflare 的有趣项目

---

## 🔥 今日热门项目

### 1. [owner/repo](https://github.com/owner/repo)

项目描述...

- ⭐ **1,234** stars | 🍴 **567** forks
- 🏷️ cloudflare · workers · pages

...
```

---

## 🎯 下一步

1. ✅ 初始化 Git 仓库
2. ✅ 推送到 GitHub
3. ✅ 启用 Actions
4. ✅ 等待第一次自动运行

需要我帮你初始化仓库并推送吗？ 😊

---

*创建于 2026-03-07*
