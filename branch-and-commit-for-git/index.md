# Git 分支、提交命名规范


Git 对于开发者来说经常使用，由于没有标准规范，分支名和提交信息，显得杂乱无章，不利于项目维护，这里简单分享一下 Git 命名规范。

## 分支命名

### 格式
```bash
git branch <分类/标识/简短-描述> 
```

### 分类
常用类型 `test`，`release`，`hotfix`，`bugfix`，`feature`。

- `test`：用于测试环境，有且只有一个，用于验证新功能。
- `release`：用于生产环境，有且只有一个，默认主分支，并且是保护分支。
- `hotfix`：热修复，临时版本或紧急 bug 修复分支。
- `bugfix`：bug 修复分支。
- `feature`：用于新增、修改、删除、重构功能分支。

### 标识
标识应该用`/`与分类分割开，标识可能是 issue 编号，版本号，bug 编号。如果没有标识，可用`no-ref`进行标识。

### 描述
最后是描述，描述分支用途，描述之间用`-`进行分割，描述尽可能简短，例：`add-users-search`

### 举例

- 新增功能，优化，或者修改功能： `git branch feature/pc-v1.0.0/add-news-list `
- 修复 bug：`git branch bugfix/bug-012/user-page-fix`
- 尽快修复 bug：`git branch hotfix/no-ref/login-not-working`

## 提交命名

格式：`<type>(scope):<subject>`，`scope`是可选项。

### 举例
```bash
feat: add hat wobble
^--^  ^------------^
|     |
|     +-> 对提交内容的总结。
|
+-------> Type: chore, docs, feat, fix, refactor, style, or test.
```
### Type 类型的说明
- `chore`：琐事，比如修改配置文件，不改变生产代码。
- `docs`：修改文档。
- `feat`：新增功能，新增构建脚本不算。
- `fix`：修改 bug，修改构建脚本不算。
- `refactor`：重构生产代码，比如重命名变量。
- `style`：代码格式化，不修改生产代码。
- `test`：新增、重构测试用例，不修改生产代码。

### VS Code 插件推荐
https://marketplace.visualstudio.com/items?itemName=redjue.git-commit-plugin

### 参考
https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716

https://dev.to/varbsan/a-simplified-convention-for-naming-branches-and-commits-in-git-il4

