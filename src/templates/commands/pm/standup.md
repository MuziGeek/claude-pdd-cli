---
description: 自动生成团队站会报告，包含昨日完成、今日计划、阻塞问题
allowed-tools: Read, Bash, Grep, Glob
---

## 📊 团队站会报告

自动分析项目状态，生成结构化的每日站会报告，提升团队沟通效率。

### 执行方式

```bash
# 生成今日站会报告
/pm:standup

# 生成指定日期的报告  
/pm:standup --date "2024-01-15"

# 生成特定团队成员的报告
/pm:standup --member "developer1,developer2"

# 生成详细版本（包含技术细节）
/pm:standup --detailed

# 生成Slack格式的报告
/pm:standup --format "slack" --channel "#daily-standup"
```

### 报告内容结构

#### 标准站会报告格式

```markdown
# 📊 每日站会报告 - 2024年1月15日

## 🎯 项目整体状态
- **活跃Epic**: 3个
- **进行中任务**: 12个
- **今日计划完成**: 4个任务
- **团队开发效率**: 85% ⬆️ (+5% vs 昨日)

## 👥 团队成员状态

### 🧑‍💻 Alice Chen (@alice)
**昨日完成** ✅
- 完成用户注册API实现 (T001)
- 修复登录验证bug (T008)
- 代码评审: PR#23, PR#24

**今日计划** 🎯
- 实现密码重置功能 (T012)
- 重构用户服务层 (T015)
- 参与技术设计评审

**阻塞/风险** 🚨
- 无阻塞项

**状态**: 🟢 良好 | **负载**: 75% | **TDD阶段**: GREEN

---

### 🧑‍💻 Bob Wilson (@bob)
**昨日完成** ✅
- 完成支付API单元测试 (T019)
- 更新支付文档 (T021)

**今日计划** 🎯
- 集成第三方支付网关 (T020)
- 处理支付异常流程 (T022)

**阻塞/风险** 🚨
- ⚠️ 等待支付网关测试账号 (2天)
- 🔴 需要DevOps协助部署环境

**状态**: 🟡 有阻塞 | **负载**: 60% | **TDD阶段**: RED

---

## 🔥 重点关注

### 高优先级任务 (P0)
1. **用户认证系统** - 75% 完成，目标本周五交付
2. **支付集成** - 30% 完成，存在第三方依赖风险
3. **数据迁移** - 等待开始，依赖认证系统完成

### 阻塞问题汇总 🚨
1. **第三方依赖**: 支付网关测试账号申请延迟 (影响2个任务)
2. **环境问题**: 测试环境部署需要DevOps支持 (影响1个任务)
3. **技术难点**: Redis集成方案需要架构师确认 (影响1个任务)

### 今日会议/里程碑 📅
- 10:00 - 技术架构评审 (Alice, Bob, Tech Lead)
- 14:00 - 客户需求澄清会议 (Product Manager)
- 16:00 - 支付网关技术对接 (Bob, External Partner)

### 风险提醒 ⚠️
- 支付系统发布时间可能延后2-3天
- 团队整体负载偏高 (平均80%)，建议调整任务优先级

## 📈 趋势分析

### 效率指标
- **任务完成速度**: 1.8任务/人/天 (目标: 1.5)
- **代码审查效率**: 平均2小时内完成
- **测试通过率**: 94% (昨日: 91%)
- **部署成功率**: 98%

### 质量指标  
- **代码覆盖率**: 87% (⬆️ +2%)
- **Bug率**: 0.3 bugs/feature (⬇️ -0.1)
- **技术债务**: 稳定 (无新增)

## 🎯 明日预期
- 预计完成4个任务
- 支付网关集成如无阻塞将进入测试阶段
- 用户认证系统准备进入最终测试

---
**报告生成时间**: 2024-01-15 09:00:00 UTC  
**数据来源**: GitHub Issues, TDD状态, Git提交记录
```

### 报告生成逻辑

#### 数据源分析
```bash
# 1. Git提交分析 - 昨日工作内容
analyze_git_commits() {
    local yesterday=$(date -d "yesterday" +%Y-%m-%d)
    
    # 获取昨日提交
    git log --since="$yesterday 00:00:00" --until="$yesterday 23:59:59" \
        --pretty=format:"%an|%s|%H" | while IFS='|' read author subject hash; do
        echo "- $subject ($hash)"
    done
}

# 2. GitHub Issues状态分析
analyze_github_issues() {
    # 获取活跃Issues
    gh issue list --state open --assignee "@me" --json number,title,labels,assignees
    
    # 获取昨日关闭的Issues
    gh issue list --state closed --search "closed:>$(date -d yesterday +%Y-%m-%d)" --json number,title
}

# 3. TDD状态分析
analyze_tdd_status() {
    local tdd_state="$CLAUDE_PROJECT_DIR/.claude/tdd-state.json"
    
    if [[ -f "$tdd_state" ]]; then
        jq -r '.currentPhase, .testsPassing, .lastUpdate' "$tdd_state"
    fi
}
```

#### 成员活动识别
```bash
# 基于Git提交识别团队成员活动
get_member_activity() {
    local member="$1"
    local date="${2:-yesterday}"
    
    # 昨日完成的任务
    git log --author="$member" --since="$date 00:00:00" --until="$date 23:59:59" \
        --pretty=format:"- %s" | head -10
    
    # 当前分配的Issues
    gh issue list --assignee "$member" --state open --json number,title,labels
}
```

### 自定义报告模板

#### 简化版站会报告
```bash
/pm:standup --format "brief"
```

输出示例:
```
📊 站会速览 (2024-01-15)

✅ 昨日完成: 8个任务
🎯 今日计划: 6个任务  
🚨 阻塞问题: 2个
👥 活跃成员: 5人

重点关注: 支付网关集成延迟，需要优先解决
```

#### Slack通知格式
```bash
/pm:standup --format "slack" --webhook "$SLACK_WEBHOOK"
```

输出为Slack消息格式，可直接发送到频道。

#### 邮件报告格式
```bash
/pm:standup --format "email" --recipients "team@company.com"
```

### 智能分析功能

#### 1. 工作模式识别
```yaml
workPatterns:
  - pattern: "high-productivity"
    indicators:
      - 任务完成率 > 120%
      - 代码质量稳定
      - 无阻塞问题
    suggestion: "保持当前节奏，可考虑接受挑战性任务"
    
  - pattern: "blocked-heavy"  
    indicators:
      - 阻塞任务 > 30%
      - 任务完成率 < 70%
    suggestion: "优先解决阻塞问题，必要时重新分配任务"
```

#### 2. 风险预警
```yaml
riskDetection:
  - risk: "milestone-delay"
    triggers:
      - 关键路径任务延迟 > 1天
      - 依赖任务未按时完成
    alert: "里程碑可能延迟，建议调整计划"
    
  - risk: "team-overload"
    triggers:
      - 平均工作负载 > 85%
      - 连续3天高负载
    alert: "团队负载过重，考虑任务重新分配"
```

### 历史趋势分析

#### 生成周报
```bash
# 生成本周站会汇总
/pm:standup --period "weekly" --output "reports/weekly-standup.md"
```

#### 生成月报
```bash  
# 生成月度团队效率报告
/pm:standup --period "monthly" --include-metrics --output "reports/monthly-report.md"
```

### 集成配置

#### Slack集成
```json
{
  "standup": {
    "slack": {
      "enabled": true,
      "webhook": "https://hooks.slack.com/...",
      "channel": "#daily-standup",
      "schedule": "09:00",
      "timezone": "UTC+8",
      "mentions": {
        "blocked": ["@channel"],
        "critical": ["@tech-lead", "@product-owner"]
      }
    }
  }
}
```

#### 邮件集成
```json
{
  "standup": {
    "email": {
      "enabled": true,
      "smtp": {
        "host": "smtp.company.com",
        "port": 587,
        "secure": true
      },
      "recipients": [
        "team-lead@company.com",
        "product-owner@company.com"
      ],
      "schedule": "08:30"
    }
  }
}
```

### 使用场景

#### 1. 日常团队管理
```bash
# 晨会前生成报告
/pm:standup --format "presentation" --output "standup-slides.md"

# 发送给管理层
/pm:standup --format "executive" --recipients "management@company.com"
```

#### 2. 远程团队协作
```bash
# 异步站会 - 发送到Slack供团队成员查看
/pm:standup --format "slack" --async --channel "#async-standup"
```

#### 3. 项目健康监控
```bash
# 自动生成并存档每日报告
/pm:standup --auto --archive "reports/standups/"
```

### 最佳实践

#### 报告质量保证
- 数据来源多样化：Git + GitHub + 人工输入
- 信息筛选：突出重要信息，过滤噪音
- 行动导向：不仅报告问题，还提供建议

#### 团队协作优化
- 统一报告时间：每日固定时间生成
- 格式标准化：保持报告格式一致性  
- 互动反馈：支持团队成员添加补充信息

#### 自动化建议
- 设置定时任务：每日自动生成并发送
- 异常检测：自动识别异常模式并预警
- 持续改进：根据反馈优化报告内容

---
**下一步**: 结合 `/pm:blocked` 命令处理站会中识别的阻塞问题