# github

本仓库与 GitHub 手动同步，**没有自动同步**。

- **本地路径**：`D:\ai voice\ai\github`
- **远程仓库**：https://github.com/CBGCL/github （`git@github.com:CBGCL/github.git`）
- **认证方式**：SSH 密钥 `~/.ssh/id_ed25519`（免密）
- **分支**：`main`

## 怎么同步

**双击**这个文件即可拉取最新数据：

```
D:\ai voice\ai\GitHub手动同步.bat
```

它会自动：

1. 执行 `git pull` 从 GitHub 拉取最新代码
2. 显示当前工作区状态
3. 把窗口停在仓库目录里，方便你继续敲 git 命令

拉完之后想推送自己的改动，在同一个窗口里执行：

```powershell
git add -A
git commit -m "你的说明"
git push
```

## 也可以完全手动

不想用上面的 bat，就在仓库目录里自己敲：

```powershell
cd "D:\ai voice\ai\github"

git pull                      # 拉取远程最新数据
git status                    # 看当前状态

git add -A                    # 暂存所有改动
git commit -m "说明"           # 提交
git push                      # 推送到 GitHub
```

推荐用 **Git Bash**（开始菜单搜 Git Bash）执行这些命令，
它对路径里的空格和中文处理最省心。

## 关于之前的自动同步

之前配置过一个每 15 分钟自动提交推送的计划任务 `GitHubAutoSync`，
**现已禁用**（`Next Run Time: N/A`，状态 `Disabled`），不会再自动跑。

如果哪天想重新打开：

```powershell
schtasks /change /tn "GitHubAutoSync" /enable    # 重新启用
schtasks /change /tn "GitHubAutoSync" /disable   # 再次关闭
schtasks /run    /tn "GitHubAutoSync"            # 只手动触发一次
```

想彻底删除：

```powershell
schtasks /delete /tn "GitHubAutoSync" /f
```

同步脚本本身仍然保留在：

```
C:\Users\Administrator\AppData\Local\GitHubAutoSync\GitHubAutoSync.ps1
```

它不会自己运行，只有被计划任务或你手动调用时才执行。
该脚本会完整走一遍「pull → commit → push」，适合一键同步全部改动。

## 注意事项

- `.gitignore` 已排除依赖目录、构建产物、虚拟环境、日志以及**所有密钥类文件**
  （`*.key` / `*.pem` / `id_*` / `.env` / `credentials.json` 等），避免误提交敏感信息。
- 本仓库当前是 **公开（public）** 的。若代码涉及隐私或商业内容，请先改为私有：
  https://github.com/CBGCL/github/settings 底部 Danger Zone → Change visibility
- 大文件（模型权重 `*.pt` / `*.safetensors` / `*.ckpt` 等）已在 `.gitignore` 中排除。
  GitHub 单文件上限 100 MB，确实需要托管大文件请改用 Git LFS。
