# 部署配置说明

## GitHub Pages + GitHub Actions 自动部署

### 工作流原理

```
推送代码到 main 分支
         ↓
触发 GitHub Actions
         ↓
运行 Hugo 构建
         ↓
生成静态网站 (public/)
         ↓
自动部署到 GitHub Pages
         ↓
网站上线 https://你的用户名.github.io/MrainTechBlog/
```

### 必须的步骤

#### 1️⃣ 创建 GitHub 仓库

```bash
# 在 GitHub 上创建仓库：MrainTechBlog
# (不要初始化 README, .gitignore 等)
```

#### 2️⃣ 配置仓库远端

```bash
cd /Users/ruima/Documents/MrainWeb/MrainTechBlog

# 添加远端
git remote add origin https://github.com/你的用户名/MrainTechBlog.git

# 改为 main 分支（如需要）
git branch -M main
```

#### 3️⃣ 提交并推送代码

```bash
git add .
git commit -m "Initial commit: Hugo blog setup with PaperMod theme"
git push -u origin main
```

#### 4️⃣ 配置 GitHub Pages

在 GitHub 仓库设置中：

1. 进入 **Settings** → **Pages**
2. **Build and deployment** 下的 **Source** 选择 **GitHub Actions**
3. 保存

#### 5️⃣ 配置 baseURL

编辑 `hugo.yaml`，设置正确的 baseURL：

```yaml
# 如果仓库名为 MrainTechBlog
baseURL: https://你的用户名.github.io/MrainTechBlog/

# 如果用户仓库（仓库名必须是 username.github.io）
baseURL: https://你的用户名.github.io/
```

### 文件说明

已为你创建了：

- `.github/workflows/deploy.yml` - 自动化部署工作流

### 工作流特性

✅ **自动触发**：每次 push 到 main 分支自动部署
✅ **Hugo 扩展版**：支持 Sass/SCSS
✅ **子模块支持**：自动处理 PaperMod 主题子模块
✅ **最小化输出**：`--minify` 压缩生成的 HTML/CSS/JS
✅ **完全自动化**：无需本地生成 public/ 再推送

### 部署后的网站结构

```
https://你的用户名.github.io/MrainTechBlog/
├── index.html (首页)
├── categories/ (分类列表)
├── tags/ (标签列表)
├── posts/ (文章列表)
└── assets/ (资源文件)
```

### 常见问题解决

**Q: 网站部署后样式错乱？**
A: 检查 `baseURL` 是否正确设置。PaperMod 的资源路径依赖于 baseURL。

**Q: 草稿文章也被发布了？**
A: 确保文章的 Front Matter 中 `draft: false`。

**Q: 想撤回已发布的内容？**
A: 删除或设置 `draft: true`，然后 push。GitHub Actions 会自动重新构建并更新网站。

### 修改文章后的自动流程

1. 本地编辑文章
2. `git add .`
3. `git commit -m "Add/update article"`
4. `git push origin main`
5. ✨ GitHub Actions 自动构建和部署（约 1-2 分钟）
6. 网站自动更新

### 查看部署状态

在 GitHub 仓库中：

- **Actions** 标签页 → 查看所有工作流运行
- 绿色 ✅ 表示部署成功
- 红色 ❌ 表示部署失败（点击查看错误日志）
