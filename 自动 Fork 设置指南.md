# 自动 Fork 设置指南

配置 GitHub Actions 自动 Fork 热门项目到你的 GitHub。

---

## 🔐 需要的权限

自动 Fork 需要 GitHub Token 具有以下权限：
- ✅ `repo` - 完全控制仓库
- ✅ `public_repo` - 访问公开仓库
- ✅ `workflow` - 管理 GitHub Actions

---

## 📋 设置步骤

### 第 1 步：创建 Personal Access Token

1. 打开 GitHub → **Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**

2. 点击 **Generate new token (classic)**

3. 填写信息：
   - **Note**: `Auto Fork Token`
   - **Expiration**: `No expiration` (或选择合适的时间)

4. 勾选权限：
   ```
   ✅ repo (Full control of private repositories)
   ✅ workflow (Update GitHub Action workflows)
   ```

5. 点击 **Generate token**

6. **复制 Token**（只显示一次，保存好！）

---

### 第 2 步：添加 GitHub Secret

1. 进入你的仓库 → **Settings** → **Secrets and variables** → **Actions**

2. 点击 **New repository secret**

3. 添加以下 Secret：

| Name | Value |
|------|-------|
| `FORK_TOKEN` | 第 1 步创建的 Token |

4. 点击 **Add secret**

---

### 第 3 步：初始化仓库并推送

```bash
# 进入项目目录
cd /workspaces/codespaces-blank/github 热点/每日推荐

# 初始化 Git
git init
git add .
git commit -m "✨ Auto fork setup"

# 替换为你的 GitHub 用户名
YOUR_USERNAME="your-username"

# 创建远程仓库并推送
git remote add origin https://github.com/$YOUR_USERNAME/github-daily-trending.git
git branch -M main
git push -u origin main
```

---

### 第 4 步：启用 GitHub Actions

1. 进入仓库 → **Actions** 标签

2. 找到 **Daily Trending + Auto Fork** workflow

3. 点击 **Enable workflow**

4. （可选）手动触发一次测试：
   - 点击 **Run workflow**
   - 选择分支
   - 点击 **Run workflow**

---

## 📊 运行结果

每次运行后会自动：

1. ✅ 获取 GitHub 热门项目
2. ✅ 生成推荐文档 (`每日推荐/YYYY-MM-DD.md`)
3. ✅ 自动 Fork 最多 3 个新项目
4. ✅ 记录已 Fork 项目 (`已 Fork 项目/list.json`)
5. ✅ 创建项目说明页面 (`已 Fork 项目/项目名/README.md`)
6. ✅ 提交并推送到仓库

---

## ⚙️ 自定义配置

### 修改每日 Fork 数量

编辑 `.github/workflows/daily-trending-auto-fork.yml`：

```yaml
workflow_dispatch:
  inputs:
    max_forks:
      description: 'Max projects to fork'
      required: false
      default: '3'  # 改成你喜欢的数字
```

### 修改最少 Stars 要求

```javascript
const minStars = process.env.INPUT_MIN_STARS || '500';
// 改成 '1000' 或 '100' 等
```

### 修改运行时间

```yaml
on:
  schedule:
    # 每天北京时间 14:00
    - cron: '0 6 * * *'
```

---

## 📁 生成的文件结构

```
github-daily-trending/
├── .github/
│   └── workflows/
│       └── daily-trending-auto-fork.yml
├── 每日推荐/
│   ├── README.md
│   ├── latest.md          # 最新推荐
│   ├── 2026-03-07.md      # 每日存档
│   └── ...
├── 已 Fork 项目/
│   ├── list.json          # Fork 列表
│   ├── project-name-1/
│   │   └── README.md
│   └── project-name-2/
│       └── README.md
└── README.md
```

---

## 🔍 查看你的 Fork

- **你的 Fork 列表**: `已 Fork 项目/list.json`
- **GitHub 上的 Fork**: https://github.com/YOUR_USERNAME?tab=repositories

---

## ⚠️ 注意事项

1. **GitHub API 限率**:
   - 未认证：60 次/小时
   - 已认证：5000 次/小时
   - 使用 Token 可避免限率问题

2. **Fork 数量限制**:
   - 建议每日 1-5 个，避免太多
   - GitHub 没有明确的 Fork 上限，但适度使用

3. **Token 安全**:
   - 不要泄露 Token
   - 定期更新 Token
   - 只给必要的权限

---

## 🎯 快速测试

设置完成后，手动触发一次：

```bash
# 进入仓库页面
# Actions → Daily Trending + Auto Fork → Run workflow → Run workflow
```

等待 2-5 分钟，查看：
- ✅ 是否生成了推荐文档
- ✅ 是否自动 Fork 了项目
- ✅ 提交记录是否正常

---

## 📞 遇到问题？

### Q: Actions 运行失败？
A: 检查 Token 权限是否正确，确保已添加到 Secrets

### Q: 没有自动 Fork？
A: 检查日志，可能是已达到每日上限或项目已 Fork 过

### Q: 想重新 Fork 某个项目？
A: 删除 `已 Fork 项目/list.json` 中的对应记录

---

*设置指南创建时间：2026-03-07*
