# Kaiwu Coding Skill
面向 AI 编程助手的 QBoson Kaiwu SDK 建模与求解技能，帮助将组合优化问题转化为 QUBO / Ising 模型，并生成、修改和验证相应的 Python 代码。

本仓库提供技能指令和参考资料，使用实际求解功能时仍需单独安装 Kaiwu SDK，并满足相应的授权条件。

## 适用场景

- **组合优化建模**：选址、调度、路径规划、覆盖、布尔规则、Max-Cut 等问题；
- **约束处理**：等式、不等式、松弛变量、硬约束罚系数与带权软规则；
- **SDK 代码开发与修复**：模型构建、QUBO / Ising 转换、求解器调用和候选解码；
- **版本迁移**：区分 Enterprise 1.3.1、1.4.0 与 Community 的接口和能力；
- **结果校验**：从候选决策重新计算业务目标和全部约束，区分业务成本与求解能量；
- **专题问题**：精度适配、高阶项降阶，以及离散决策与连续子问题的协同处理。

PyTorch 能量模型以及 RBM / BM / DBN、Q-VAE、Q-Diffusion 不属于 `kaiwu-coding-skill` 的适用范围，应使用独立的 `kpp-coding-skill`。

## 快速开始

### 1. 获取技能

```bash
git clone https://github.com/qboson/kaiwu-coding-skill.git
```

将完整仓库目录放入所用平台的技能目录，目录名保持为 `kaiwu-coding-skill`，保留 `SKILL.md` 与 `references/` 的相对位置。例如，项目使用 `.agents/skills/` 时，入口应为 `.agents/skills/kaiwu-coding-skill/SKILL.md`。

#### 加载与使用

安装或导入技能后，可通过以下两种方式使用：

- **直接描述任务**：在提示词中写明“请使用 kaiwu-coding-skill……”，由助手匹配并加载技能；
- **手动选择技能**：在输入框的技能菜单中搜索 `kaiwu-coding-skill`，选中后补充任务。常见入口为 `@` 或 `/`，部分平台使用 `$`，以当前平台提示为准。

后续提示词采用第一种写法。

### 2. 准备 SDK 环境

将以下提示词交给大模型：

> 请用 kaiwu-coding-skill 准备环境：检查 conda/Python 是否安装，按需安装并复用或创建环境；安装对应 Kaiwu SDK，验证建模和求解，报告版本、结果及授权缺项。

### 3. 描述任务

向助手提供业务目标、输入数据、全部约束、SDK 版本、运行环境和时间限制。例如：

> 请使用 kaiwu-coding-skill，基于 Kaiwu Enterprise 1.4.0，将带权顶点覆盖问题建模为 QUBO，并用本地模拟退火求解。请说明罚系数依据，遍历 SDK 返回的候选解，重新验证每条边是否被覆盖，并报告选中的顶点和原业务成本。

也可以用于已有代码修复：

> 请使用 kaiwu-coding-skill 检查现有 Kaiwu 代码，修复从 Enterprise 1.3.1 迁移到 1.4.0 后的导入、求解和解码问题，并保留原业务目标与约束。

## 工作流程

1. 确认解释器、SDK 发行版、版本和授权条件；
2. 明确决策变量、原业务目标与可行域，构造等价的 QUBO / Ising 模型；
3. 按变量规模、系数范围与执行预算设置求解参数；
4. 调用 SDK 求解，遍历候选并按正确的变量映射解码；
5. 重算原业务约束与目标，保留实际返回候选中的最佳可行结果；
6. 报告实际参数、耗时、验证结果及是否有最优性证明。

罚函数值为零或 SDK 约束检查通过，不能代替对原业务约束的验证。模拟退火等启发式方法找到可行解，也不代表已经证明全局最优。

## 仓库结构

```text
kaiwu-coding-skill/
├── README.md                    # 项目说明与使用入口
├── SKILL.md                     # 技能指令、建模规则和交付规范
└── references/
    ├── api_reference.md         # API 总览
    ├── kaiwu_enterprise.md      # Enterprise API 参考
    ├── kaiwu_community.md       # Community API 参考
    ├── examples.md              # SDK 历史示例
    ├── module_docs.md          # 模块文档索引
    ├── module_docs/             # 按发行版划分的模块资料
    └── docs/                    # 安装、建模、精度与应用专题资料
```

## 参考资料

| 内容 | 入口 |
| --- | --- |
| 技能规则与 Enterprise 1.4.0 调用流程 | [SKILL.md](SKILL.md) |
| Enterprise API | [kaiwu_enterprise.md](references/kaiwu_enterprise.md) |
| Community API | [kaiwu_community.md](references/kaiwu_community.md) |
| API 总览 | [api_reference.md](references/api_reference.md) |
| 模块索引 | [module_docs.md](references/module_docs.md) |
| SDK 示例 | [examples.md](references/examples.md) |
| 精度处理 | [precision.md](references/docs/precision.md) |

参考资料包含不同版本的接口和历史示例，使用前应核对目标 SDK 版本；从 1.3.x 迁移到 1.4.x 时，请按 `SKILL.md` 中的版本对照更新导入与调用。

## 维护与反馈

欢迎通过 [Issues](https://github.com/qboson/kaiwu-coding-skill/issues) 反馈问题，或提交 Pull Request 改进技能指令和参考资料。反馈 API 或求解问题时，请附上 SDK 发行版与版本、最小复现代码、错误信息及预期行为，避免包含密钥和授权凭据。
