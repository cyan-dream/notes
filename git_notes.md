### 配置SSH key

```bash
ssh-keygen -t ed25519 -C "cyandream@foxmail.com"
```

- `~/.ssh/id_ed25519`：私钥

- `~/.ssh/id_ed25519.pub`：公钥，加到 GitHub -> SSH and GPG keys -> New SSH key

### 完整首次推送流程

```bash
touch .gitignore
git init
git add .
git commit -m "初始版本"
git branch -M main
git remote add origin git@github.com:cyan-dream/invision_rv1126b_ipc_livestream.git
git push -u origin main
```

之后日常只需要：

```bash
git add .
git commit -m "feat: xxx"
git push
```

### 创建新分支

```bash
# 1. 更新 main
git switch main
git fetch origin
git status
git pull --rebase

# 2. 新建开发分支
git switch -c dev

# 3. 开发并提交
git add .
git commit -m "feat: 新功能"

# 4. 推送到远程
git push -u origin dev

# 5. 之后继续开发
git add .
git commit -m "fix: 修复问题"
git push
```

### 分支合并

#### 方案一：直接重置main

让 `main` 直接指向 `dev` 的最新提交，历史也被覆盖

```bash
git switch main
git reset --hard dev
git push --force-with-lease
```

执行后：main 和 dev 完全一样，包括提交历史

#### 方案二：合并但用 dev 的内容

保留 `main` 的提交历史，但内容全部采用 `dev` 的版本 

```bash
git switch main
git merge dev -X theirs
```

`-X theirs` 表示：冲突时全部采用 `dev`（被合并分支）的版本

### 删除分支

```bash
git branch -d dev
git push origin --delete dev
```


