# Git 工作流操作指南

从开始开发到提交代码的完整命令流程。

## 1. 开始新功能开发

### 1.1 确保主分支是最新的
```bash
# 切换到主分支
git checkout main

# 拉取最新代码
git pull origin main
```

### 1.2 创建新的功能分支
```bash
# 创建并切换到新分支（推荐命名规范：feature/功能名、fix/问题名）
git checkout -b feature/your-feature-name

```

## 2. 开发过程中

### 2.1 查看当前状态
```bash
# 查看工作区状态（修改、新增、删除的文件）
git status

# 查看具体修改内容
git diff

# 查看已暂存的修改
git diff --staged
```

### 2.2 暂存文件
```bash
# 暂存指定文件
git add path/to/file.ts

# 暂存所有修改的文件
git add .

# 暂存多个文件
git add file1.ts file2.ts file3.ts

# 交互式暂存（选择性暂存）
git add -p
```

### 2.3 撤销修改（如果需要）
```bash
# 撤销工作区的修改（未暂存）
git checkout -- path/to/file.ts

# 或使用新版本命令
git restore path/to/file.ts

# 取消暂存（已经 git add 的文件）
git reset HEAD path/to/file.ts

# 或使用新版本命令
git restore --staged path/to/file.ts
```

## 3. 提交代码

### 3.1 提交规范
遵循约定式提交格式：
```
<type>: <description>

<optional body>
```

**类型（type）说明：**
- `feat`: 新功能
- `fix`: Bug 修复
- `refactor`: 重构（不改变功能）
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建/工具链相关
- `perf`: 性能优化
- `ci`: CI/CD 配置

### 3.2 执行提交
```bash
# 提交暂存的文件
git commit -m "feat: add user authentication feature"

# 多行提交信息
git commit -m "feat: add user authentication feature" -m "- Implement JWT token generation
- Add login/logout endpoints
- Add password encryption"

# 修改上一次提交（追加文件或修改提交信息）
git commit --amend

# 修改上一次提交信息
git commit --amend -m "新的提交信息"
```

### 3.3 提交示例
```bash
# 新功能
git commit -m "feat: implement user profile editing"

# Bug 修复
git commit -m "fix: resolve memory leak in data processing"

# 重构
git commit -m "refactor: extract authentication logic to separate module"

# 文档
git commit -m "docs: update API documentation for v2 endpoints"

# 测试
git commit -m "test: add unit tests for user service"
```

## 4. 查看提交历史

```bash
# 查看提交历史
git log

# 查看简洁的提交历史
git log --oneline

# 查看最近 5 次提交
git log -5

# 查看图形化分支历史
git log --graph --oneline --all

# 查看某个文件的修改历史
git log -p path/to/file.ts
```

## 5. 推送到远程仓库

### 5.1 首次推送新分支
```bash
# 推送并设置上游分支（-u 参数）
git push -u origin feature/your-feature-name
```

### 5.2 后续推送
```bash
# 推送到远程（已设置上游分支）
git push

# 强制推送（谨慎使用，仅在必要时）
git push --force

# 更安全的强制推送（不会覆盖他人提交）
git push --force-with-lease
```

## 6. 创建 Pull Request

### 6.1 使用 GitHub CLI（推荐）
```bash
# 创建 PR（会打开编辑器填写详情）
gh pr create

# 直接创建 PR（指定标题和描述）
gh pr create --title "feat: add user authentication" --body "## Summary
- Implemented JWT authentication
- Added login/logout endpoints

## Test plan
- [ ] Test login with valid credentials
- [ ] Test login with invalid credentials
- [ ] Test token expiration"

# 创建草稿 PR
gh pr create --draft

# 查看 PR 状态
gh pr status

# 查看 PR 列表
gh pr list
```

### 6.2 通过 Web 界面
推送代码后，访问 GitHub 仓库页面，点击 "Compare & pull request" 按钮。

## 7. 同步主分支的最新代码

### 7.1 在功能分支开发时
```bash
# 方法 1: 使用 rebase（保持线性历史）
git checkout main
git pull origin main
git checkout feature/your-feature-name
git rebase main

# 方法 2: 使用 merge
git checkout feature/your-feature-name
git pull origin main
# 或
git merge main
```

### 7.2 解决冲突
```bash
# 查看冲突文件
git status

# 手动编辑冲突文件，解决冲突后：
git add path/to/conflicted-file.ts

# 继续 rebase
git rebase --continue

# 或继续 merge
git commit

# 如果想放弃 rebase
git rebase --abort

# 如果想放弃 merge
git merge --abort
```

## 8. 常用查询命令

```bash
# 查看远程仓库信息
git remote -v

# 查看所有分支
git branch -a

# 查看远程分支
git branch -r

# 查看当前分支
git branch --show-current

# 查看文件的每一行最后修改信息
git blame path/to/file.ts

# 搜索提交信息
git log --grep="关键词"

# 查看某次提交的详细信息
git show <commit-hash>
```

## 9. 完整开发流程示例

```bash
# 1. 准备工作
git checkout main
git pull origin main
git checkout -b feature/user-dashboard

# 2. 开发代码
# ... 编写代码 ...

# 3. 查看修改
git status
git diff

# 4. 暂存并提交
git add src/components/Dashboard.tsx src/services/user.ts
git commit -m "feat: implement user dashboard with statistics"

# 5. 继续开发
# ... 更多修改 ...
git add .
git commit -m "test: add unit tests for dashboard component"

# 6. 推送到远程
git push -u origin feature/user-dashboard

# 7. 创建 PR
gh pr create --title "feat: implement user dashboard" --body "## Summary
- Added user dashboard component
- Display user statistics
- Added unit tests

## Test plan
- [ ] Dashboard displays correctly
- [ ] Statistics are accurate
- [ ] All tests pass"

# 8. 如果需要同步主分支
git checkout main
git pull origin main
git checkout feature/user-dashboard
git rebase main
git push --force-with-lease
```

## 10. 注意事项

1. **提交频率**：保持小而频繁的提交，每个提交只做一件事
2. **提交信息**：使用清晰、描述性的提交信息
3. **推送前检查**：推送前确保代码通过测试和构建
4. **不要提交敏感信息**：API 密钥、密码等应使用环境变量
5. **代码审查**：创建 PR 后等待代码审查，不要直接合并
6. **分支命名**：使用有意义的分支名，如 `feature/login`、`fix/memory-leak`
7. **保持同步**：定期从主分支同步代码，避免大量冲突

## 11. 紧急情况处理

### 11.1 提交到错误的分支
```bash
# 撤销最后一次提交（保留修改）
git reset --soft HEAD~1

# 切换到正确的分支
git checkout correct-branch

# 重新提交
git add .
git commit -m "your commit message"
```

### 11.2 需要临时切换分支
```bash
# 暂存当前工作（不提交）
git stash

# 切换到其他分支处理紧急问题
git checkout other-branch

# 处理完毕后，切回原分支
git checkout feature/your-feature-name

# 恢复暂存的工作
git stash pop
```

### 11.3 误删除未提交的代码
```bash
# 查看所有 reflog
git reflog

# 恢复到指定状态
git reset --hard HEAD@{n}
```
