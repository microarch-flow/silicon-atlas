# 任务:为我构建一份芯片开发流程 Wiki

## 我的背景

我叫 Biao,是一名系统软件架构师,目前正在转型做芯片创业。我的技术轨迹是从应用层软件开发逐步向硬件发展:

- 7 年系统软件工具链开发和 AI 芯片架构建模经验
- 参与过 AI 芯片项目的"spec → C model → RTL"前端环节
  - 我写 C model 没问题
  - Verilog 我只有初步了解,我生成的 Verilog 都是让 agent 基于我开发的 C model 生成的
  - 我对 RTL 设计的工程实践(code review、lint、CDC、覆盖率)、前端验证(UVM、formal、emulation)了解很少
- 完全没有参与过 RTL 之后的环节:后端、流片、硅后验证、量产爬坡
- 完全没有完整地从零开发并量产过一款芯片
- 完全不了解芯片项目的商业维度:NRE、mask cost、IP 授权、Foundry 关系、封测合作等

我现在正在创办一家 AI 芯片公司,定位是"快速迭代的芯片公司"——通过架构探索框架 + RTL 自动生成 + AI Agent 编排,把芯片设计周期从传统的 18-24 个月压缩到数月。第一版芯片计划全栈开源(RTL + 编译器)。

## 这份 Wiki 的目的

帮我建立**对完整芯片开发流程的清晰认识**。读完后我应该能够:

1. 准确说出从立项到量产的所有阶段、各阶段的关键活动、典型时长、参与角色和交付物
2. 理解各阶段之间的衔接和接口,知道前一阶段的输出如何影响后一阶段
3. 理解每个阶段的关键决策点、决策依据、常见错误
4. 理解芯片项目的成本结构和商业维度
5. 知道作为创业公司,哪些环节可以外包/复用,哪些必须自研
6. 识别出我作为软件背景的人最可能踩的坑

这不是为了让我成为某个具体环节的专家,而是为了让我有**全局视野**,能与各环节的专业工程师做有效沟通,能做创业层面的资源分配和决策。

## Wiki 的结构

请生成一个多目录、多文件的 Wiki,结构如下:

```
chip_development_wiki/
├── README.md                              # 总览索引,含完整流程图、阅读建议、术语表入口
├── 00_overview/
│   ├── README.md                          # 本目录概述
│   ├── 01_full_lifecycle.md               # 完整芯片生命周期总览
│   ├── 02_roles_and_teams.md              # 各角色和团队职责
│   ├── 03_cost_structure.md               # 芯片项目成本结构总览
│   ├── 04_startup_vs_bigcompany.md        # 创业公司 vs 大公司的流程差异
│   └── 05_glossary.md                     # 关键术语表
├── 01_product_definition_and_architecture/
│   ├── README.md
│   ├── 01_market_and_workload_analysis.md
│   ├── 02_spec_definition.md
│   ├── 03_architecture_exploration.md
│   ├── 04_ip_strategy.md                  # IP 自研 vs 购买决策
│   ├── 05_process_node_selection.md       # 工艺节点选择
│   └── 06_milestones_and_signoffs.md
├── 02_frontend_design_and_verification/
│   ├── README.md
│   ├── 01_rtl_design_practices.md         # RTL 设计工程实践(对我特别重要)
│   ├── 02_microarchitecture_design.md
│   ├── 03_verification_methodology.md     # UVM / formal / emulation 等
│   ├── 04_synthesis_preparation.md
│   ├── 05_dft_introduction.md             # Design for Test 引入
│   ├── 06_cdc_and_rdc.md                  # 时钟域、复位域检查
│   ├── 07_low_power_design.md
│   ├── 08_signoff_criteria.md
│   └── 09_software_engineer_pitfalls.md   # 软件背景人在前端容易踩的坑
├── 03_backend_physical_design/
│   ├── README.md
│   ├── 01_floorplanning.md
│   ├── 02_placement.md
│   ├── 03_clock_tree_synthesis.md
│   ├── 04_routing.md
│   ├── 05_static_timing_analysis.md
│   ├── 06_power_analysis.md
│   ├── 07_drc_lvs_signoff.md
│   ├── 08_advanced_node_considerations.md # 7nm 及以下节点的特殊考虑
│   └── 09_backend_outsourcing.md          # 后端外包决策
├── 04_tapeout_and_post_silicon/
│   ├── README.md
│   ├── 01_tapeout_process.md
│   ├── 02_mask_making_and_wafer_fab.md
│   ├── 03_packaging.md
│   ├── 04_post_silicon_bringup.md
│   ├── 05_post_silicon_debug.md
│   ├── 06_characterization.md
│   ├── 07_yield_analysis.md
│   └── 08_silicon_revision.md             # 出 bug 怎么办,respin 流程
├── 05_production_and_lifecycle/
│   ├── README.md
│   ├── 01_qualification_and_reliability.md
│   ├── 02_volume_ramp.md
│   ├── 03_supply_chain.md
│   ├── 04_field_support.md
│   └── 05_end_of_life.md
├── 06_cross_cutting_topics/                # 横跨多阶段的主题
│   ├── README.md
│   ├── 01_eda_tools_landscape.md
│   ├── 02_foundry_relationships.md
│   ├── 03_open_source_silicon.md          # 开源芯片策略(对我特别重要)
│   ├── 04_mpw_shuttle_strategy.md         # MPW 多项目晶圆策略
│   ├── 05_turnkey_services.md             # 设计服务/Turnkey 服务
│   └── 06_ai_chip_specific.md             # AI 芯片特有的考虑
└── 07_for_software_background_founders/   # 专门给软件背景创业者
    ├── README.md
    ├── 01_common_misconceptions.md
    ├── 02_critical_mindset_shifts.md
    ├── 03_recruitment_priorities.md       # 创业初期招聘优先级
    ├── 04_first_chip_pragmatics.md        # 第一颗芯片的务实建议
    └── 05_fast_iteration_realities.md     # "快速迭代"的真实约束
```

## 内容要求

### 深度要求(中层)

每个文件大约 3000-8000 字。需要包含:

- 该主题的核心概念和定义
- 关键活动的具体内容和顺序
- 涉及的工具(具体名称,如 Synopsys Design Compiler、Cadence Innovus 等)
- 典型时长和参与角色
- 关键决策点和决策依据
- 常见错误和坑
- 与上下游阶段的接口
- 创业公司视角下的取舍

避免:

- 泛泛而谈的"原则性"内容(如"质量很重要"这种没有信息量的话)
- 教科书式的纯理论(不讲为什么工程师要做这件事)
- 虚构的数字和案例(宁可写"通常 X 量级",不要写"具体 7.3 个月"这种伪精确)

### 视角要求

以**流程视角**为主线,在每个阶段穿插:

- **角色视角**:这个活动谁负责?需要什么背景的人?
- **决策视角**:这个活动的关键决策是什么?决策依据是什么?
- **成本视角**:这个活动的成本结构是什么?哪里可以省钱、哪里不能省?

### 工艺节点

主线讲先进节点(7nm 及以下),在涉及具体差异时简要说明与成熟节点(28nm/16nm)的差异。

### 我作为软件背景人的特殊关注

在以下地方需要特别照顾我的背景:

1. **02_frontend_design_and_verification/01_rtl_design_practices.md**:
   不要假设我懂 Verilog/SystemVerilog 的工程实践。需要解释:
   - 为什么 RTL 设计有那些"看起来很啰嗦"的规则(如严格的 reset 策略、避免锁存器、明确的状态机编码)
   - RTL 设计与软件设计的根本思维差异(并行 vs 串行、时序 vs 顺序、综合后的物理含义)
   - 我用 agent 基于 C model 生成 Verilog 的方式,在工程实践上有什么风险

2. **02_frontend_design_and_verification/09_software_engineer_pitfalls.md**:
   专门讨论软件背景人做 RTL 时的典型问题。

3. **07_for_software_background_founders/** 整个目录:
   讨论我这种背景做芯片创业的现实情况、思维转换、招聘建议、第一颗芯片的策略。

4. 在涉及"自动化"、"快速迭代"的讨论中,要诚实说明真实的工程约束——哪些环节可以加速,哪些环节有物理限制(如 mask 制造、wafer fab、reliability qualification)。

### 内容可信度标注

每个 markdown 文件结尾加一个 "可信度声明" 部分:

```
## 内容可信度说明

- **公开信息(高可信)**:[列出基于公开教科书、行业标准、知名公司公开实践的部分]
- **行业惯例(中可信)**:[列出基于业界普遍做法但具体细节因公司而异的部分]
- **经验性观察(中低可信)**:[列出基于一般经验但可能因项目、团队、工艺而异的部分]
- **不确定/需向资深工程师确认(低可信)**:[列出 AI 助手知识边界外或可能过时的部分]
```

这个标注帮我识别哪些内容可以直接用,哪些需要找资深工程师确认。

### 交叉引用

每个文件应该:

- 开头有"前置知识"链接(应该先读哪些文件)
- 结尾有"后续阅读"链接(读完这个之后应该看什么)
- 文中提到的术语首次出现时,链接到 `00_overview/05_glossary.md` 的对应条目
- 文中提到的其他阶段活动,链接到对应文件

### 案例和场景

在以下地方需要提供具体场景或案例:

- 每个阶段的描述中,至少有一个"典型流程"的具体场景描述(可以是虚拟的项目,但要现实)
- 涉及决策点的地方,描述"如果决策错误会发生什么"的后果
- 涉及成本的地方,给出量级估算(如"7nm 一次 mask cost 量级在百万美元")

### 商业和创业维度

在以下文件中重点讨论商业维度:

- `00_overview/03_cost_structure.md`:NRE、mask cost、wafer cost、package cost、test cost、IP cost 的分解和典型量级
- `01_product_definition_and_architecture/04_ip_strategy.md`:IP 自研 vs 购买、各种 IP 授权模式
- `01_product_definition_and_architecture/05_process_node_selection.md`:工艺节点的成本和性能取舍
- `06_cross_cutting_topics/02_foundry_relationships.md`:Foundry 关系建立、PDK 获取、shuttle 机会
- `06_cross_cutting_topics/04_mpw_shuttle_strategy.md`:MPW 策略对创业公司的意义
- `06_cross_cutting_topics/05_turnkey_services.md`:可以外包的服务和外包决策

## 执行步骤

请按以下步骤执行,**不要跳过任何步骤**:

### Step 1: 规划阶段(必须先完成,等我确认后再开始写)

在开始写任何内容之前,请先输出:

1. 你对我背景和需求的理解(确认我们对齐)
2. 完整的目录树(允许在我给的结构基础上调整,如果你有更好的建议)
3. 每个文件的"内容大纲"(3-5 句话说明该文件会讲什么)
4. 你识别出的需要特别处理的地方(如某些主题你的知识边界、某些主题需要更深入等)
5. 预估总字数和你的写作策略

输出后**停下来等我确认**,不要立即开始写。

### Step 2: 分批生成(我确认后)

我确认 Step 1 后,按目录顺序逐个目录生成。每个目录生成完后,简要汇报:

- 这个目录写了哪些文件
- 字数总计
- 你不确定的地方(需要我后续核实的)

然后等我说"继续"再生成下一个目录。

### Step 3: 最终交付

所有目录完成后,生成总的 README.md,包含:

- Wiki 的整体说明
- 推荐的阅读顺序(对我这种背景的人)
- 完整的文件索引
- 全局术语表的入口
- 一个总流程图(用 mermaid 或 ASCII art)

## 质量约束(避免常见 AI 生成缺陷)

请严格遵守以下约束:

1. **不要泛泛而谈**:每个段落要有具体信息,而不是"质量很重要"这种正确但无用的话

2. **不要虚构数据**:涉及具体数字时(如成本、时间、面积),要么基于公开知识给出量级估算并说明,要么直接说"具体数字因项目而异,典型范围是 X 到 Y"

3. **不要回避不确定性**:你不知道的就明确说不知道,在"可信度声明"中标注

4. **不要重复**:跨文件之间不要重复大段内容,应该通过链接交叉引用

5. **不要堆术语**:首次出现的术语必须解释,后续可以使用

6. **保持工程师视角**:目标是让我"能与专业工程师有效沟通",不是让我成为外行学者。语言要工程化,不要学术化

7. **诚实面对"快速迭代"的边界**:在涉及流程压缩的讨论中,要诚实说明哪些可以压缩、哪些不能,不要因为知道我想做"快速迭代芯片"就盲目鼓励

## 开始

请从 Step 1 开始,先输出规划,等我确认后再开始生成内容。


