# github

本仓库由本地定时任务自动同步到 GitHub，**无需手动执行 `git push`**。

- **本地路径**：`D:\ai voice\ai\github`
- **远程仓库**：https://github.com/CBGCL/github （`git@github.com:CBGCL/github.git`）
- **认证方式**：SSH 密钥 `~/.ssh/id_ed25519`
- **同步频率**：每 15 分钟一次
- **分支**：`main`

## 工作原理

Windows 计划任务 `GitHubAutoSync` 每 15 分钟调用一次：

```
C:\Users\Administrator\AppData\Local\GitHubAutoSync\GitHubAutoSync.ps1
```

脚本每一轮会依次执行：

1. `git fetch` —— 拉取远程最新状态
2. 若远程有新提交且本地有未提交改动，先提交再 `git pull --rebase`
3. `git add -A` + `git commit` —— 提交本地所有改动
4. `git push` —— 推送到 GitHub

也就是说：**你只管在这个目录里写代码，改动会自动出现在 GitHub 上。**

## 如果与远程冲突

脚本遇到 rebase 冲突时会**自动中止**并保留现场，绝不强推覆盖任何东西。
此时日志里会出现 `CONFLICT`，按普通 git 冲突流程手工解决即可：

```powershell
cd "D:\ai voice\ai\github"
git status          # 查看冲突文件
# 手工编辑解决冲突后：
git add -A
git commit -m "解决冲突"
```

## 常用操作

```powershell
# 查看同步日志
Get-Content "$env:LOCALAPPDATA\GitHubAutoSync\sync.log" -Tail 30

# 暂停自动同步
schtasks /change /tn "GitHubAutoSync" /disable

# 恢复自动同步
schtasks /change /tn "GitHubAutoSync" /enable

# 手动立即同步一次
schtasks /run /tn "GitHubAutoSync"

# 查看任务状态
schtasks /query /tn "GitHubAutoSync" /fo LIST /v
```

## 注意事项

- `.gitignore` 已排除依赖目录、构建产物、虚拟环境、日志以及**所有密钥类文件**
  （`*.key` / `*.pem` / `id_*` / `.env` / `credentials.json` 等），避免误提交敏感信息。
- 本仓库当前是 **公开（public）** 的。若代码涉及隐私或商业内容，请先改为私有。
- 大文件（模型权重 `*.pt` / `*.safetensors` / `*.ckpt` 等）已在 `.gitignore` 中排除。
  GitHub 单文件上限 100 MB，确实需要托管大文件请改用 Git LFS。
