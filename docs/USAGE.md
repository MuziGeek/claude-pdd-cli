# Claude TDD CLI 使用指南

## 🚀 快速上手

### 安装
```bash
npm install -g claude-tdd-cli
```

### 初始化新项目
```bash
# 进入你的项目目录
cd my-project

# 完整安装（推荐）- 动态获取最新CCPM
cpdd init

# 快速安装
cpdd init --quick

# 离线安装
cpdd init --offline
```

## 📋 可用命令

### 核心命令

| 命令 | 描述 | 示例 |
|------|------|------|
| `cpdd init` | 初始化CCPM + TDD系统 | `cpdd init --online` |
| `cpdd status` | 显示安装状态 | `cpdd status` |
| `cpdd update` | 更新现有安装 | `cpdd update --force` |

### init 命令选项

```bash
cpdd init [options]
```

**可用选项**：
- `--mode <mode>` - 安装模式 (full/ccpm/tdd)
- `--online` - 强制在线模式（从GitHub获取最新CCPM）
- `--offline` - 强制离线模式（使用内置模板）
- `--quick` - 快速安装，使用检测到的默认设置
- `--force` - 覆盖现有安装
- `--github <repo>` - GitHub仓库集成 (owner/repo格式)
- `--framework <type>` - 指定框架类型

## 🎯 安装模式

### PDD模式 - 项目驱动开发 (推荐)
```bash
cpdd init --mode=pdd
```
**方法论**: Project-Driven Development - 项目需求优先，灵活集成多种开发方法
- ✅ 完整CCPM系统 (39个项目管理命令)
- ✅ TDD开发工具 (5个TDD命令)
- ✅ GitHub Issues集成
- ✅ 8个专业AI代理
- ✅ 支持PDD/BDD/TDD混合开发

### PM模式 - 项目管理驱动
```bash
cpdd init --mode=pm
```
**方法论**: Project Management Driven - 专注项目管理和团队协作
- ✅ CCPM项目管理功能 (39个PM命令)
- ✅ PRD和Epic管理
- ✅ GitHub Issues集成
- ✅ 团队协作工作流
- ❌ TDD开发工具

### TDD模式 - 测试驱动开发
```bash
cpdd init --mode=tdd
```
**方法论**: Test-Driven Development - 传统TDD红绿重构循环
- ✅ TDD开发命令 (5个TDD命令)
- ✅ 智能测试生成工具
- ✅ Red-Green-Refactor循环
- ✅ 代码质量保证
- ❌ 项目管理功能

## 🌐 在线/离线安装

### 在线模式（推荐）
```bash
cpdd init --online
```
- 从GitHub获取最新CCPM
- 自动集成TDD增强功能
- 始终保持最新特性

### 离线模式
```bash
cpdd init --offline
```
- 使用内置CCPM模板
- 无需网络连接
- 快速安装

### 智能模式（默认）
```bash
cpdd init
```
- 自动检测网络状态
- 在线失败时自动降级到离线
- 最佳用户体验

## 🛠️ 使用场景示例

### 1. 新团队项目
```bash
# 完整安装带GitHub集成
cpdd init --mode=pdd --github=myorg/project

# 查看安装状态
cpdd status
```

### 2. 个人开发项目
```bash
# 快速TDD开发环境
cpdd init --mode=tdd --quick

# 查看生成的结构
cpdd status
```

### 3. 现有项目添加功能
```bash
# 强制覆盖现有配置
cpdd init --force --online

# 更新到最新版本
cpdd update --force
```

## 🎯 生成的项目结构

运行 `cpdd init` 后，会在你的项目中创建：

```
.claude/
├── CLAUDE.md              # 统一规则和命令参考
├── config.json            # 系统配置
├── agents/                # AI代理 (根据模式而定)
├── commands/              # 可用命令
│   ├── pm/               # 项目管理命令 (39个，ccpm/full模式)
│   └── tdd/              # TDD命令 (5个，tdd/full模式)
├── workflows/            # 完整工作流
├── rules/                # 开发规则和最佳实践
├── epics/                # Epic模板和存储
└── prds/                 # PRD模板和存储
```

## 📊 工作流程

### 完整开发流程（full模式）
```bash
# 1. 初始化
cpdd init --mode=pdd --github=owner/repo

# 2. 在Claude Code中使用项目管理命令
/pm:prd-new feature-name
/pm:prd-parse feature-name
/pm:issue-start 123

# 3. 使用TDD开发命令
/tdd:spec-to-test
/tdd:cycle

# 4. 完成开发
/pm:issue-close 123
/pm:next
```

### 纯TDD开发流程（tdd模式）
```bash
# 1. 初始化
cpdd init --mode=tdd

# 2. 在Claude Code中使用TDD命令
/tdd:red          # 编写失败测试
/tdd:green        # 最小实现
/tdd:refactor     # 重构优化
/tdd:cycle        # 完整循环
```

## 📚 命令参考

安装完成后，所有功能都通过Claude Code中的命令访问：

### 项目管理命令（ccpm/full模式）
- `/pm:prd-*` - PRD管理 (5个命令)
- `/pm:epic-*` - Epic处理 (12个命令)
- `/pm:issue-*` - Issue管理 (8个命令)
- `/pm:*` - 工作流和协调 (14个命令)

### TDD开发命令（tdd/full模式）
- `/tdd:cycle` - 完整TDD循环
- `/tdd:red` - 红灯阶段（失败测试）
- `/tdd:green` - 绿灯阶段（最小实现）
- `/tdd:refactor` - 重构阶段（质量提升）
- `/tdd:spec-to-test` - 需求转测试用例

完整命令列表请查看生成的 `.claude/CLAUDE.md` 文件。

## 🔧 故障排除

### 安装失败
```bash
# 检查安装状态
cpdd status

# 强制重新安装
cpdd init --force --offline

# 更新到最新版本
cpdd update --force
```

### GitHub集成问题
```bash
# 重新配置GitHub集成
cpdd init --github=owner/repo --force
```

### 在线安装失败
```bash
# 使用离线模式作为备选
cpdd init --offline --force
```

## 📞 获取帮助

- **命令帮助**: `cpdd --help` 或 `cpdd init --help`
- **GitHub Issues**: [提交问题](https://github.com/MuziGeek/claude-pdd-cli/issues)
- **文档**: [项目文档](https://github.com/MuziGeek/claude-pdd-cli#readme)

## 💡 最佳实践

1. **推荐使用完整模式**: `cpdd init --mode=pdd` 获得最佳体验
2. **启用GitHub集成**: 便于团队协作和进度跟踪
3. **定期更新**: 使用 `cpdd update` 保持最新功能
4. **在线优先**: 让系统自动获取最新CCPM功能

---

**开始你的高效开发之旅！**

```bash
npm install -g claude-tdd-cli
cpdd init --mode=pdd --online
```

体验从需求到测试、从开发到部署的完整工作流程。🚀