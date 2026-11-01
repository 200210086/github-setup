# Git ↔ GitHub 连接配置记录

本机环境配置备忘，供重装或换机时参考。

## 账号信息

| 项目 | 值 |
| --- | --- |
| GitHub 用户名 | 200210086 |
| 提交邮箱 | 200210086@users.noreply.github.com |
| 连接方式 | SSH（ed25519） |
| Git 版本 | 2.54.0 (Windows) |

noreply 邮箱是 GitHub 提供的隐私邮箱，提交记录里不会暴露真实邮箱。
需在 GitHub → Settings → Emails 勾选 *Keep my email addresses private* 才能完全生效。

## 全局配置

```bash
git config --global user.name "200210086"
git config --global user.email "200210086@users.noreply.github.com"
git config --global init.defaultBranch main
git config --global core.autocrlf false
git config --global core.longpaths true
git config --global core.quotepath false
git config --global push.autoSetupRemote true
git config --global credential.helper manager
```

- `core.quotepath false` —— 中文文件名在 `git status` 里正常显示，不转八进制
- `core.longpaths true` —— 避免 Windows 260 字符路径限制报错
- `push.autoSetupRemote true` —— 首次 push 不用手动加 `-u`

## SSH 配置

密钥：`~/.ssh/id_ed25519`（无密码短语）

`~/.ssh/config`：

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
    AddKeysToAgent yes
    ServerAliveInterval 60
```

### 换机后重新生成

```bash
ssh-keygen -t ed25519 -C "github" -f ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub   # 复制输出内容
```

粘贴到 https://github.com/settings/ssh/new

### 自检命令

```bash
ssh -T git@github.com
# 期望：Hi 200210086! You've successfully authenticated, but GitHub does not provide shell access.

git ls-remote git@github.com:octocat/Hello-World.git HEAD
# 期望：返回一串 commit hash
```

`Permission denied (publickey)` = 公钥没上传或 ssh-agent 里没加载密钥。

## 日常工作流

```bash
git init
git add .
git commit -m "提交说明"
git remote add origin git@github.com:200210086/仓库名.git
git push
```

新建仓库先在 https://github.com/new 创建，**不要**勾选初始化 README / .gitignore，
否则本地首次 push 会因历史不一致被拒。
