# github

本仓库由 DSH 配置的自动同步任务管理。

- 本地路径：`D:\ai voice\ai\github`
- 远程仓库：GitHub（SSH 认证）
- 自动同步：本地定时任务，自动 commit + push

## 说明

这个目录里的所有代码改动都会被定时任务自动提交并推送到 GitHub，
无需手动执行 `git push`。

如需临时暂停自动同步，可在任务计划程序中禁用名为
`GitHubAutoSync` 的计划任务。
