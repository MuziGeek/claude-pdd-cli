---
description: 进入TDD RED阶段，编写失败的测试
allowed-tools: Bash, Read, Write, Edit
---

## 🔴 RED阶段：编写失败测试

切换到TDD RED阶段，专注编写会失败的测试用例。

### 1. 切换到RED阶段

首先更新TDD状态到RED阶段：

```bash
bash $CLAUDE_PROJECT_DIR/.claude/scripts/tdd/state-manager.sh update RED "$FEATURE_ID" "编写失败测试"
```

如果没有功能ID，系统会提示设置一个。

### 2. 查看当前状态

```bash
bash $CLAUDE_PROJECT_DIR/.claude/scripts/tdd/state-manager.sh status
```

### 3. RED阶段规则

在RED阶段，你只能：
- ✅ 编写测试文件 (`**/*.test.*`, `**/*.spec.*`, `tests/**/*`)
- ✅ 创建测试辅助工具
- ✅ 更新测试配置

被限制的操作：
- ❌ 修改源代码文件 (`src/**/*`, `lib/**/*`)
- ❌ 修改生产配置
- ❌ 实现业务逻辑

### 4. 测试编写原则

#### TDD三法则
1. 在写出能够失败的单元测试之前，不允许写任何产品代码
2. 只允许写出刚好能够失败的单元测试，不能编译也算失败
3. 只允许写出刚好能够通过当前失败测试的产品代码

#### 测试结构（AAA模式）
```javascript
describe('功能描述', () => {
  it('应该_期望结果_当_特定条件', () => {
    // Arrange (Given) - 准备测试数据
    const input = { /* test data */ };
    
    // Act (When) - 执行被测试操作
    const result = functionUnderTest(input);
    
    // Assert (Then) - 验证结果
    expect(result).toBe(expectedValue);
  });
});
```

### 5. 项目特定的测试模式

系统会根据检测到的项目类型提供对应的测试模板：

- **Java**: JUnit 5 + AssertJ
- **JavaScript/TypeScript**: Jest / Mocha
- **Python**: pytest / unittest
- **Go**: testing package
- **Rust**: built-in test framework

### 6. 验证测试失败

编写测试后，运行测试验证其正确失败：

```bash
# 运行测试（会自动调用test-runner hook）
bash $CLAUDE_PROJECT_DIR/.claude/hooks/test-runner.sh
```

### 7. RED阶段完成标志

- [ ] 测试按预期失败
- [ ] 失败原因清晰明确
- [ ] 测试表达了正确的需求
- [ ] 没有实现任何生产代码

### 8. 进入下一阶段

RED阶段完成后，使用 `/tdd:green` 进入GREEN阶段开始实现代码。

```bash
bash $CLAUDE_PROJECT_DIR/.claude/scripts/tdd/state-manager.sh record "RED phase completed"
```

---
**当前阶段**: 🔴 RED | **专注**: 编写失败测试 | **下一步**: `/tdd:green`