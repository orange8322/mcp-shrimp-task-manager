---
description: 生成任务报告的提示词模板
globs: 
alwaysApply: false
---
!!! 主要目标 !!!
1. 根据 tasks.json 中的任务信息生成结构化的任务报告
2. 确保报告包含完整的任务执行链和时间线
3. 清晰展示任务完成状态和依赖关系
4. 将报告保存到 REPORTS_DIR 目录
!!! 主要目标结束 !!!

> 需要验证：
> 你，AI，必须回复"我已确认上述'主要目标'并将按顺序执行步骤"。

# 通用规则：
- 你可以运行任何所需的终端命令来完成任务，包括但不限于：
  - 查找项目根目录中的文件和目录
  - 检查代码库中的相关文件或目录
  - 使用终端运行任何完成任务所需的命令

# 处理规则：
1. 按照模板各部分有条不紊地处理任务
2. __切勿说__ "我看到问题了..."或"我找到问题了"之类的话
3. 如有任何不明确的方面，请求澄清
4. 对于任何缺失的上下文，使用`tree`搜索文件或请求特定信息
5. 在开始每个新部分（分析/解决方案/实现/等）之前：
   - 回顾到目前为止完成的工作
   - 列出即将进行的工作
6. 完成每个部分后：
   - 总结已完成的内容
   - 显示已完成的检查标记
   - 说明接下来的内容
7. 每个实施步骤之后：
   - 将步骤标记为完成 [x]
   - 显示带有进度的完整实施列表
   - 在继续前请求确认
8. 在继续验证前：
   - 显示完整的实施清单
   - 确认所有步骤都已完成
   - 获取用户确认以继续
9. 验证后：
    - 显示所有已完成的验证步骤
    - 获取最终确认

# 占位符定义：
- **[TASK]:** 正在处理的特定任务或问题（例如，"修复缓存管理器"）
- **[id]:**  任務的唯一標識符
- **[name]:** 簡潔明確的任務名稱
- **[TASK_DATE_AND_NUMBER]:** 任务文件的时间戳和顺序标识符（例如，"2025-01-14_1"）
- **[TASK FILE]:** 创建用于记录和跟踪任务进度的Markdown文件
- **[completedAt]:** 完成任务的时间
- **[COMPLETED_TASKS_LIST]:** 已完成的任务列表和摘要

# 终端命令：
- 使用以下命令检索填充以下占位符所需的信息：
  - **[completedAt]:** 当前日期和时间，通过运行`Get-Date -Format "yyyy-MM-dd_HH:mm:ss"`获取
  - **[DATE]:** 当前日期，通过运行`Get-Date -Format "yyyy-MM-dd"`获取
  - **[TIME]:** 当前时间，通过运行`Get-Date -Format "HH:mm:ss"`获取
  - **[USER_NAME]:** 当前用户，通过运行`whoami`获取

# 任务文件模板：
```markdown
# 上下文
任务文件名：[name]
创建时间：[completedAt]
创建者：[USER_NAME]

[COMPLETED_TASKS_LIST]

# 任务描述
[基于用户提供的[TASK]的详细描述。]

# 背景信息（可选）
[关于任务和相关上下文的详细背景信息。如果没有，添加"—"。]

# 任务分析
- [TASK]的目的。
- 已识别的问题，包括：
  - 造成的问题。
  - 为什么需要解决。
  - 实施细节和目标。
- 其他有用的参考详情。

# 要采取的步骤
[任务的可操作步骤列表]
- 在此部分底部添加"请勿删除"（以便在需要时参考）。

# 当前步骤：[当前步骤的编号]

# 原始任务模板
[完整未编辑的"任务文件模板"]
- 将完整未编辑的"任务文件模板"复制并粘贴到此部分，__包括所有详细信息__
- 在此部分底部添加"请勿删除"（以便在需要时参考）。

# 原始步骤
[完整未编辑的"步骤"部分]
- 将完整未编辑的"步骤"部分复制并粘贴到此部分，__包括每个步骤下的所有详细信息__
- 在此部分底部添加"请勿删除"（以便在需要时参考）。

# 注释
[任务期间的迭代注释。如果没有，添加"—"。]

# 任务进度
- 更新必须包括：
  - 必填：
    - [DATETIME]。
    - 成功/失败，在用户确认后
  - 可选：
    - 发现、解决方案、阻碍因素和结果。
    - 所有更新必须按时间顺序记录。

# 最终审查
[仅在任务完成后填写。]
```

# 步骤：

### **1. 任务分析**
1. 逐步检查[TASK]、相关代码和功能。  
2. 识别问题并在"任务分析"中记录发现。  
3. 在继续前获得用户确认。  
   - 在此合并任何迭代更改。

> 继续前：
> 1. 用你当前的进度更新[TASK FILE]
> 2. 显示更新后的[TASK FILE]内容
> 3. 等待用户确认你的分析已完成，如果没有，迭代此步骤

---

### **2. 任务文件创建**
1. 创建[TASK FILE]
   - 在`project_root/.tasks/[DIRECTORY_NAME]/[TASK_FILE_NAME]_[TASK_IDENTIFIER].md`创建任务文件
   - 要生成[TASK_FILE_NAME]，使用：
     ```bash
     echo "$(date +%Y-%m-%d)_$(($(ls .tasks/$(date +%Y-%m-%d)_* 2>/dev/null | wc -l) + 1))"
     ```
   - 要生成[DIRECTORY_NAME]，使用：
     ```bash
     echo "$(date +%Y-%m)"
     ```
2. 根据"任务文件模板"、步骤1（任务分析）的结果和用户提供的输入填写初始内容。  
3. 向用户确认文件名和内容。  

> 继续前：
> 1. 用你当前的进度更新[TASK FILE]
> 2. 显示更新后的[TASK FILE]内容
> 3. 等待用户确认[TASK FILE]内容

---

### **3. 迭代任务**
1. 在更改前全面分析代码上下文。  
2. 在[TASK FILE]的"任务进度"下记录所有进度。  
3. 对于每个更改：
   - 寻求用户对更新的确认。
   - 在日志中将更改标记为成功或失败。
4. 分析"任务进度"下的更新，确保你不重复以前的错误或失败的更改。

> 继续前：
> 1. 用你当前的进度更新[TASK FILE]
> 2. 显示更新后的[TASK FILE]内容
> 3. 如果你认为已完成任务，在继续下一步前咨询用户

---

### **4. 最终审查**
1. 完成[TASK FILE]中的"最终审查"。
2. 总结所有成功的更改。

> 继续前：
> 1. 用你当前的进度更新[TASK FILE]
> 2. 显示更新后的[TASK FILE]内容
> 3. 在结束任务前向用户确认

---

> # 用户输入：
> **[TASK]:** `<描述你的任务>`  
> **[BACKGROUND INFO]:** `<输入背景信息或包含详细信息的文件链接（可选）>`

# 任务报告模板：
```markdown
# 任务执行报告
生成时间：[completedAt]
报告ID：[id]

## 1. 任务背景
[从主任务的 description 和 analysisResult 中提取关键信息]

## 2. 主任务信息
- 任务名称：[name]
- 任务ID：[id]
- 任务状态：[status]
- 创建时间：[createdAt]
- 完成时间：[completedAt]
- 任务描述：[description]

## 3. 实施指南
[implementationGuide 的内容，如果有的话]

## 4. 子任务执行情况
### 4.1 任务依赖关系
[visualization of task dependencies]

### 4.2 任务执行时间线
[按时间顺序列出所有任务的创建和完成时间]

### 4.3 详细任务列表
[遍历所有相关任务，对每个任务：]
#### [Task Name] (ID: [task.id])
- 状态：[task.status]
- [x] 已完成 / [ ] 未完成
- 创建时间：[task.createdAt]
- 完成时间：[task.completedAt]
- 任务描述：[task.description]
- 实施步骤：[task.implementationGuide]
- 执行摘要：[task.summary]

## 5. 执行结果评估
[基于所有任务的完成情况和summary进行总结]

## 6. 遇到的问题和解决方案
[从任务记录中提取关键问题和解决方案]

## 7. 附录
### 7.1 相关文件
[列出所有任务中提到的相关文件]

### 7.2 验证标准
[列出所有任务的验证标准]
```

# 报告生成规则：
1. 报告文件命名：
   - 格式：task-report-[TASK_ID]-[TIMESTAMP].md
   - 保存位置：REPORTS_DIR 目录

2. 内容处理规则：
   - 使用任务的原始描述，保持信息的完整性
   - 保留任务间的依赖关系
   - 清晰标记任务状态（完成/未完成）
   - 按时间顺序组织信息

3. 格式要求：
   - 使用 Markdown 格式
   - 保持层级结构清晰
   - 使用适当的强调标记
   - 确保时间格式统一

4. 报告生成步骤：
   a) 收集主任务信息
   b) 整理相关子任务
   c) 分析任务依赖关系
   d) 生成时间线
   e) 汇总执行结果
   f) 输出到指定目录

5. 数据来源说明：
   - 主要数据来自 tasks.json
   - 补充信息来自任务的 analysisResult
   - 执行细节来自 taskReport 字段

6. 状态标记规则：
   - 已完成任务：[x]
   - 进行中任务：[~]
   - 未开始任务：[ ]
   - 被阻塞任务：[!]

7. 时间格式化：
   - 使用 ISO 8601 格式
   - 包含时区信息
   - 显示精确到秒

注意事项：
1. 确保报告的完整性和准确性
2. 保持信息的结构化和可读性
3. 突出重要信息和关键节点
4. 保留任务执行过程中的重要决策
5. 记录可能有助于未来参考的经验

验证清单：
- [ ] 报告结构完整
- [ ] 任务信息准确
- [ ] 时间线清晰
- [ ] 状态标记正确
- [ ] 依赖关系完整
- [ ] 保存位置正确

---

> # 用户输入：
> **[TASK]:** `<描述你的任务>`  
> **[BACKGROUND INFO]:** `<输入背景信息或包含详细信息的文件链接（可选）>` 

**现在开始创建文件，严禁不创建文件**