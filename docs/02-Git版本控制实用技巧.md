# Git 版本控制实用技巧

## 前言

Git 是软件开发中最基本的工具之一。掌握 Git 不仅意味着会用 `add`、`commit`、`push`，更在于理解其背后的模型和高效的协作工作流。本文档整理了一套从基础到进阶的实用技巧。

---

## 1. Git 核心概念速览

```
工作区 (Working Directory)    暂存区 (Staging Area)      本地仓库 (Local Repo)      远程仓库 (Remote)
        │                              │                         │                       │
        │   git add <file>             │   git commit            │   git push            │
        │ ──────────────────▶         │ ───────────────▶        │ ────────────▶         │
        │                              │                         │                       │
        │                              │                         │   git pull/fetch      │
        │          git checkout/restore│     git reset           │ ◀────────────         │
        │ ◀──────────────────         │ ◀───────────────        │                       │
```

### 关键概念

| 概念 | 说明 |
|------|------|
| **Commit** | 代码的快照，记录了一次完整的变更 |
| **Branch** | 独立的开发线，可并行工作、互不干扰 |
| **Merge** | 将一个分支的变更合并到另一个分支 |
| **Rebase** | 将提交"移植"到新的基底上，保持历史线性 |
| **Stash** | 临时保存未完成的工作，快速切换上下文 |

---

## 2. 日常高频命令

### 2.1 分支管理

```bash
# 创建并切换到新分支
git checkout -b feature/user-login

# 查看所有分支
git branch -a

# 删除本地分支
git branch -d feature/old-branch

# 同步远程分支删除
git fetch --prune
```

### 2.2 暂存与提交

```bash
# 交互式暂存（推荐！比 git add . 更精细）
git add -p

# 提交并附加详细描述
git commit -m "feat: add user login module" -m "
- Implement JWT-based authentication
- Add login/logout API endpoints
- Include password hashing with bcrypt"

# 修改最近一次提交（不要对已推送的提交使用！）
git commit --amend
```

### 2.3 撤销操作速查表

```bash
# 撤销工作区的修改
git checkout -- <file>       # 旧语法
git restore <file>           # 新语法（推荐）

# 撤销暂存区的文件
git reset HEAD <file>        # 旧语法
git restore --staged <file>  # 新语法

# 回滚最近一次提交（保留工作区修改）
git reset --soft HEAD~1

# 完全回滚最近一次提交（丢弃所有修改）
git reset --hard HEAD~1

# 创建一个新的"反向"提交来撤销
git revert HEAD
```

---

## 3. 分支策略与工作流

### 3.1 Git Flow（适用于有版本发布周期的项目）

```
main     ●──────────●──────────────────────────●───
          \        /                          /
develop    ●──●──●──●──●──●──●──●──●──●──●──●
               \    /    \       /
feature/A       ●──●      \     /
                           \   /
feature/B                    ●──●
```

- `main`：生产就绪代码
- `develop`：集成开发分支
- `feature/*`：功能分支
- `release/*`：发布准备分支
- `hotfix/*`：紧急修复分支

### 3.2 Trunk-Based Development（适用于 CI/CD 和快速迭代）

```
main     ●──●──●──●──●──●──●──●──●──●
          \    /    \    /    \    /
short      ●──●      ●──●      ●──●
```

- 所有开发者直接在 `main` 上提交小粒度的变更
- 通过 Feature Flag 控制未完成功能的可见性
- 要求完善的自动化测试和 Code Review

---

## 4. 进阶技巧

### 4.1 交互式 Rebase — 整理提交历史

```bash
# 整理最近 3 个提交
git rebase -i HEAD~3

# 在编辑器中你会看到：
# pick abc1234 feat: add login
# pick def5678 fix: typo in login
# pick ghi9012 wip: half-done stuff
#
# 将后两个改为 squash/fixup 来合并为一个干净的提交：
# pick abc1234 feat: add login
# fixup def5678 fix: typo in login
# fixup ghi9012 wip: half-done stuff
```

### 4.2 Git Bisect — 二分查找 Bug 引入点

```bash
# 启动二分查找
git bisect start
git bisect bad HEAD        # 当前版本有问题
git bisect good v1.2.3     # v1.2.3 是好的

# Git 会自动切换到中间的提交，测试后标记：
git bisect good            # 或
git bisect bad

# 重复以上步骤，直到找到引入 Bug 的提交
git bisect reset           # 结束二分查找
```

### 4.3 Cherry-Pick — 选择性合并

```bash
# 将某个特定的提交应用到当前分支
git cherry-pick abc1234

# 挑选一段连续的提交
git cherry-pick abc1234..def5678
```

### 4.4 Stash 的高级用法

```bash
# 暂存并添加描述
git stash push -m "WIP: refactoring user service"

# 查看暂存列表
git stash list

# 应用但不删除暂存
git stash apply stash@{0}

# 只暂存已追踪的文件（忽略新建文件）
git stash --include-untracked

# 创建分支并应用暂存
git stash branch feature/stashed-work stash@{0}
```

---

## 5. 提交信息规范（Conventional Commits）

```
<type>(<scope>): <subject>

[body]

[footer]
```

### 常用 type

| Type | 说明 |
|------|------|
| `feat` | 新功能 |
| `fix` | Bug 修复 |
| `docs` | 文档更新 |
| `style` | 代码格式调整（不影响逻辑） |
| `refactor` | 重构（不增功能、不修 Bug） |
| `perf` | 性能优化 |
| `test` | 测试相关 |
| `chore` | 构建/工具等杂项 |

### 示例

```
feat(auth): add JWT-based authentication

Implement login and token refresh flow using JWT.
Tokens are stored in httpOnly cookies for security.

Closes #42
```

---

## 6. 常见问题排查

### 6.1 合并冲突

```bash
# 发生冲突时：
# 1. 查看冲突文件
git status

# 2. 手动编辑冲突文件，删除冲突标记（<<<<<<< ======= >>>>>>>）
# 3. 标记为已解决
git add <resolved-file>

# 4. 完成合并
git commit

# 如果后悔了，放弃合并：
git merge --abort
```

### 6.2 误提交到错误分支

```bash
# 1. 在当前分支记下提交的 hash
git log --oneline -1
# abc1234 feat: wrong branch commit

# 2. 切换到正确的分支
git checkout correct-branch

# 3. 挑拣提交
git cherry-pick abc1234

# 4. 回到错误分支删除提交
git checkout wrong-branch
git reset --hard HEAD~1
```

### 6.3 detached HEAD 状态

```bash
# 如果你发现自己处于 detached HEAD，创建分支保存工作：
git checkout -b new-branch-name

# 确认在新分支上
git branch
```

---

## 7. 安全注意事项

> ⚠️ **永远不要**将敏感信息（密码、API 密钥、证书）提交到 Git 仓库。
>
> ⚠️ **不要**对已经推送到远程的提交使用 `git commit --amend` 或 `git rebase`。

```bash
# 使用 .gitignore 排除敏感文件
echo ".env" >> .gitignore
echo "*.pem" >> .gitignore

# 使用 git-secrets 或 gitleaks 扫描敏感信息
# https://github.com/awslabs/git-secrets
```

---

## 结语

Git 是一门"先难后易"的工具 —— 前期投入时间理解它的模型，后期的工作效率会得到质的提升。建议在真实项目中多加练习，遇到不懂的操作用 `git --help <command>` 查阅文档，逐步从"会用"过渡到"精通"。
