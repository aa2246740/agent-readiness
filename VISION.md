# 愿景

agent-readiness 是开工前的门槛，不是另一个会改代码的 agent。

它判断一个仓库是否具备让自治 agent 可靠工作的结构、验证和操作信号。判断结果要能被人和后续的 Mission 直接读取。

## 为什么单独做

Factory Mission 适合有明确结果、可以拆成里程碑的多功能工作。官方建议仓库先达到 Agent Readiness Level 4（Optimized），并具备可脚本化的用户验收路径，Mission 才能验证自己的工作。

官方能力由 `droid` 内置：

- `/readiness-report` 只读仓库并打分。
- `/readiness-fix` 依据最近一次报告修补未通过的信号。

这两项不是磁盘上的 `SKILL.md`。本机 `droid 0.236.0` 把它们编译进二进制，不能安装到别的 agent，也不能审阅和修改 rubric。这个仓库补上这一层。

## 要做成什么

做成一份版本化的就绪标准，外加一个对本地 git 仓库的只读评估入口。

评估覆盖公开模型里的五级：

1. Functional：代码能跑，但缺少自动化验证。
2. Documented：文档和基本流程已经写下来。
3. Standardized：流程被自动化强制执行。
4. Optimized：反馈快，并且持续测量。
5. Autonomous：系统能够分解复杂工作并自我改进。

公开规则是：通过上一等级 80% 的条目，才解锁下一等级。条目按九个支柱组织：Style & Validation、Build System、Testing、Documentation、Development Environment、Debugging & Observability、Security、Task Discovery、Product & Experimentation。

每条结论都要指向仓库里的文件或命令结果。分数来自这些证据，不来自模型的印象。

报告至少回答四件事：

- 当前等级。
- 已通过和未通过的条目。
- 距离下一等级最近的缺口。
- 仓库是否具备可脚本化的验收路径。

修复是后续能力。第一阶段只诊断，不改仓库。

## 成功时的样子

一个人或一个 Mission 可以在改代码之前运行评估，读到稳定的报告，并据此决定现在开工、先补缺口，还是接受风险继续。

同一仓库、同一版本的标准，重复运行得到同一组条目结论。标准本身在仓库里，改一条规则就是一次可审查的变更。

## 边界

- 参照 Factory 已公开的模型和文档，独立实现。不从 `droid` 二进制还原提示词，也不把还原文本当作本仓库的实现。
- 不绑定 Factory 账号，不调用 readiness report API，不上传仓库。
- 评估默认只读。进入修复阶段之前，不创建、不修改、不删除被评估仓库里的文件。
- 不替代 Mission。它只提供 Mission 开工前需要的就绪判断。
- 不把 Level 4 写成硬性禁令。Level 4 和可脚本化验收是建议门槛；低于门槛时，报告写明风险。

## 现在不做

- 不提取或整理专有提示词。
- 不实现评分器、rubric 数据或 `/readiness-fix`。
- 不接入 CI、仪表盘或组织级历史。

这些都留到愿景被确认之后再做。
