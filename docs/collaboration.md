# Git 与双人协作

## 本地目录与分支

当前电脑继续使用 `D:\XIAO\P047Pro`。该目录接入已有 GitHub 仓库，原学习路线文件保留。

Windows 普通 PowerShell 的 PATH 需包含 Git 所在目录；Codex 环境能执行 Git 不代表其他终端也能找到它。本机使用 Codex 自带的 Git，临时添加到 PATH 的设置只对当前终端有效。

本机直连 GitHub 曾出现 443 连接超时，已在当前仓库的本地 Git 配置中设置 GitHub 专用代理 `http://127.0.0.1:7897`，并通过不带临时参数的 `git push --dry-run origin dev/xiao` 验证。该设置不写入版本控制，也不影响其他仓库。使用时保持本机代理客户端开启；换电脑或代理端口变化时，按实际环境调整。若以后可直接连接 GitHub，可用 `git config --local --unset http.https://github.com.proxy` 移除本项配置。

2026-11-16 已通过 Git Credential Manager 完成 `tito-xiao` 的本机命令行登录，并验证 `dev/xiao` 上的 `git push` 成功。日常可以直接使用 Git 命令提交和推送；若以后凭据失效，再按 Git Credential Manager 提示重新登录。不要把账号密码或令牌写进仓库、命令示例或文档。

- `main` 是共同基线；只有完成对应检查的修改才合入。
- `dev/xiao` 是 xiao 的工作分支。
- 同事加入后用 `dev/自己的英文标识`，不共用 GitHub 账号。
- 若同一人同时做多个独立任务，可从最新 `main` 建立 `feat/任务名` 或 `fix/问题名`，各自提交 PR。

初次准备的文档与配置作为基础提交放入 `main`；后续通过 PR 整合。个人分支也可包含尚未完成的工作，提交说明和进度记录需要准确。

## 每次开始工作

先执行以下只读检查：

```powershell
git status
git branch --show-current
git fetch origin
```

有未提交修改时，先检查并决定如何保留；切换分支或合并前不要丢弃不明修改。

在工作区干净时，切换到自己的分支，并取回远程同名分支的更新：

```powershell
git switch dev/xiao
git pull --ff-only
git merge origin/main
```

`pull --ff-only` 若因分叉失败，应检查双方提交再处理，不能用强制推送或硬重置覆盖历史。`merge origin/main` 若冲突，逐文件理解双方修改，解决后检查、暂存并完成合并提交；暂时无法处理时可用 `git merge --abort` 取消本次合并。

## 保存一次修改

先用 `git diff` 检查内容，显式选择文件。以仅更新进度文档为例：

```powershell
git diff
git add docs/progress.md
git diff --cached
git commit -m "docs: record OFDM loopback progress"
git push
```

其他文件也应逐项加入。提交说明描述实际完成的事，区分文档、功能和修复。保存文件、提交本地历史和推送远程是三个不同动作。

## 合并一个阶段成果

1. 推送个人分支，在 GitHub 创建目标为 `main` 的 Pull Request。
2. 按模板写清问题、改动、执行的验证、结果和未验证项。
3. 同事到位后由另一人查看差异；当前单人阶段自行检查并如实说明。
4. 解决冲突、完成对应软件或板上检查后再合并。文档检查通过只代表文档可用。
5. 长期个人分支的 PR 优先使用 **Create a merge commit**，保留共同祖先，便于继续使用该分支。若使用 squash，后续应从最新 `main` 新建任务分支。
6. 合并后重新获取 `origin/main` 并整合到个人分支，再继续下一项工作。

仓库当前没有配置强制审批或分支保护；以上是团队约定。阶段成功后可为经过验证的提交添加有明确含义的标签，并在实验记录中写明提交号。

## 同事账号到位后的接入

- [ ] xiao 在仓库 Settings → Collaborators 中邀请真实 GitHub 账号，同事接受邀请。
- [ ] 同事配置自己的 Git 提交姓名和邮箱，再克隆仓库。
- [ ] 同事从最新 `main` 创建个人分支，推送并设定上游。
- [ ] 双方约定当前任务边界、共用文件和板卡时间，提交一个小改动完成首次 PR。

同事在自己的电脑上执行以下命令，先将 `YOUR_NAME` 换成自己的英文标识：

```powershell
git clone https://github.com/tito-xiao/P047Pro.git
cd P047Pro
git switch -c dev/YOUR_NAME
git push -u origin dev/YOUR_NAME
```

目前没有同事账号，因此未创建占位账号分支或发送邀请。分支不绑定人员权限，不能替代协作者授权。

## 硬件与 AI 协作

每次用板前沟通占用时间、接线和当前加载的固件，用板后记录配置及结果。Git 不记录板卡此刻实际状态。

给 AI 指定本次目标，让其先阅读 README、路线和进度，确认分支与未提交修改。完成后报告实际改动和验证，更新进度，保留可复现的依据。避免仅凭生成代码就声称板上通过。

不要通过提交保存账号令牌、登录密码、组会原文或未经公开许可的资料。`.gitignore` 是文件筛选工具，提交前仍需检查差异。
