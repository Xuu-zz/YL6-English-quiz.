# 译林版六年级英语 AI 刷题与错题平台

> **一套「AI 技能 + 单文件网页」的小学英语刷题方案**：Hermes 负责出题、错题归因、聚类讲评；浏览器负责刷题、错题本和报表。没有后端、没有数据库、不需要联网，双击 `index.html` 就能用。

课程作业提交版 · 面向译林版（2026 新版）小学六年级英语

---

## 目录

- [一、项目简介](#一项目简介)
- [二、功能一览](#二功能一览)
- [三、全部 Skill 清单](#三全部-skill-清单)
- [四、目录结构](#四目录结构)
- [五、快速开始](#五快速开始)
- [六、演示操作步骤](#六演示操作步骤)
- [七、数据格式说明](#七数据格式说明)
- [八、使用方法](#八使用方法)
- [九、技术实现](#九技术实现)
- [十、验证记录](#十验证记录)
- [十一、常见问题](#十一常见问题)
- [十二、课程作业说明](#十二课程作业说明)

---

## 一、项目简介

### 1.1 要解决的问题

小学六年级英语复习阶段有三个反复出现的麻烦：

| 问题 | 现在的做法 | 问题在哪 |
|---|---|---|
| 题目不够用 | 老师手编、网上找 | 质量参差，考点不齐，改一道题要改选项、答案、解析四处 |
| 错题留不住 | 抄错题本、贴便利贴 | 只会记「这题错了」，记不下「为什么错」；一个学期后无法归类 |
| 讲评没抓手 | 整卷从头讲到尾 | 45 分钟讲 40 道题，最该补的三个点反而没讲透 |

### 1.2 解决思路

把「老师脑子里的经验」拆成**可复用的技能**，把「学生的练习」做成**可离线运行的网页**：

```
┌──────────────────────── AI 技能层（Hermes） ────────────────────────┐
│ 出题 → 变式 → 错题编码解析 → 语法标签抽取 → 错题聚类 → 报表导出      │
│        （6 个 Skill，输入输出都是可校验的 JSON）                    │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ 复制 JSON
┌──────────────────────────────▼──────────────────────────────────────┐
│                    前端层（单文件 index.html）                       │
│  登录 · 专题刷题 · 计时 · 虚拟键盘 · 四层解析 · 错题本 · 变式训练     │
│  语法手册 · 排行榜 · PK 挑战 · 个人中心 · JSON/文本导出               │
│              数据全部存浏览器 localStorage，无后端                    │
└─────────────────────────────────────────────────────────────────────┘
```

### 1.3 项目现状

| 指标 | 数量 |
|---|---|
| 内置题库 | 40 题 / 6 个语法考点（一般过去时 10、to do·doing 6、词族变形 6、序数词 6、特殊疑问句 6、主谓一致 6） |
| 网页模块 | 7 个（登录、刷题、错题、变式、手册、排行榜、个人中心） |
| AI 技能 | 6 个，全部带 YAML front-matter，可直接导入 Hermes |
| 网页体积 | 单文件 88 KB，零外链、零依赖 |
| 自动化验收 | 32 项断言，全部通过 |

### 1.4 适用对象

- **教师**：备课时出题、批卷后做错题归因、讲评课前生成讲评材料与讲义
- **学生**：在电脑/平板/手机上刷题、看四层解析、重做错题
- **师范生 / 教育技术方向学生**：作为「AI 辅助教学工具链」的完整案例

---

## 二、功能一览

| 模块 | 能做什么 | 实现要点 |
|---|---|---|
| ① 登录 | 昵称 + 班级 + 访问码（默认 `LY6A`） | 纯前端门禁，记录归属到人；不是认证，清缓存即失效 |
| ② 个人中心 | 做题总数、正确率、累计用时、错题数、最近 40 条记录 | 全部从 localStorage 现算，可导出 JSON 备份 |
| ③ 语法手册 | 10 条折叠面板：公式 + 例句 + 易错点 | 含序数词口诀、be born 只用 was/were、介词后接 doing |
| ④ 专题刷题 | 6 个考点任选；40 秒倒计时；A–D 选项完整渲染；填空题弹出英文虚拟键盘；提交判对错；解析四层折叠 | 到点自动判错；答题后停表，方便看解析 |
| ⑤ 错题整理 | 答错自动收录、按考点分组、整组重做、导出 `.txt` | 答对一次自动移出错题本 |
| ⑥ 举一反三 | 粘贴变式题 JSON，同考点再练 2–3 题 | 另有「载入示例」按钮，先看格式再粘贴 |
| ⑦ 排行榜 | 正确率榜 / 完成题数榜 / PK 最高分榜；60 秒 PK 连击计分 | 正确率榜做满 5 题才上榜，避免一题全对霸榜 |

![桌面端](docs/screen-desktop.png)

<p align="center"><img src="docs/screen-mobile.png" width="320" alt="手机端"></p>

---

## 三、全部 Skill 清单

6 个技能位于 `skills/` 目录，复制到 Hermes 的 `~/.hermes/skills/yl6/` 下即可被识别（便携版为 `data/hermes/state/skills/yl6/`）。**文件夹名必须与 `name:` 字段一致。**

| # | Skill | 一句话作用 | 输入 | 输出 |
|---|---|---|---|---|
| 1 | `yl6-question-generator` | 按单元 + 语法点同步出题 | `unit` / `grammar_topic` / `count` / `level` | `yl6.item/1` 题库 JSON |
| 2 | `yl6-variant-generator` | 错一道，练三道（考点不变，换外壳） | 一道原题 JSON | `yl6.variant/1` 变式 JSON |
| 3 | `error-code-parser` | 解析老师人工编码的错题文本 | 编码表格文本 | `yl6.error/1` 结构化错题 |
| 4 | `eng-grammar-tag-extract-yl6` | 给错题打语法标签，附常规用法 + 特例 | 错题 JSON | `yl6.tag/1` 标签 JSON |
| 5 | `error-cluster-yl6` | 按标签聚类，输出备课讲评材料 | `yl6.error/1` 的 records | `yl6.cluster/1` 讲评 JSON |
| 6 | `error-report-export` | 班级高频错题统计 + 可打印讲义 | cluster JSON | `yl6.report/1` + 讲义文本 |

### 3.1 调用链

```
                 ┌─ error-code-parser ─┐
出题/考试 ───────┤                     ├─ eng-grammar-tag-extract-yl6 ─ error-cluster-yl6 ─ error-report-export
                 └─ yl6-variant-generator（错题之后，同考点巩固）
      ↑
yl6-question-generator（考前/课后补题）
```

### 3.2 调用示例（斜杠命令）

```bash
# 1. 出题：六上 Unit 2，一般过去时，10 题，中档，只要 JSON
/yl6-question-generator unit=2 grammar_topic=一般过去时 count=10 level=中档

# 2. 变式：把一道错题的完整 JSON 粘在指令后面
/yl6-variant-generator {"id":"6A-U2-C-01","tag":"past_form","stem":"It ____ hard yesterday...","answer":"C"}

# 3. 错题编码解析：粘贴老师填好的编码表格
/error-code-parser
E01 | 六上 | Unit 2 | 单选 | It ___ hard yesterday. (rain) A rain B rains C rained D raining | 学生:B | 答案:C | 看到It就用三单

# 4. 打标签
/eng-grammar-tag-extract-yl6 【粘贴 error-code-parser 的输出】

# 5. 聚类出讲评材料
/error-cluster-yl6 【粘贴一批解析完成的错题 JSON】

# 6. 导出统计表与讲义
/error-report-export 【粘贴 cluster JSON】
```

> 没有斜杠命令的版本也没问题——直接说「用 yl6-question-generator，六上 Unit 2，一般过去时 10 题，只要 JSON」，效果一样。

### 3.3 各技能的关键约束（为什么可信）

| 技能 | 写死的规矩 |
|---|---|
| 出题 | 一题一考点；干扰项必须是真实错因；`didn't` 后只能原形；`be born` 只用 was/were；题里不出现未确认的单元标题和词汇 |
| 变式 | 答案必须跟着改；三道题答案不能同一字母；只换主语/时间/场景，不换考点（do 和 doing 不许互换） |
| 编码解析 | **学生不能直接提交错题**，必须老师编码；不猜答案、不补全、不改写学生错拼 |
| 标签 | 20 个固定标签，不许自造；`regular`（常规用法）与 `exceptions`（特例）必填 |
| 聚类 | 讲评只取前 3 个高频簇；学生姓名不进讲评材料，只用编号 |
| 报表 | 所有数字现算，不许手填；少于 5 条错题不出百分比 |

---

## 四、目录结构

```
yl6-english-platform/
├── index.html                       # 单文件网页，双击即用
├── README.md
├── LICENSE
├── data/
│   └── bank.json                    # 内置 40 题题库（6 考点）
├── skills/                          # 6 个 Hermes 技能（复制到 ~/.hermes/skills/yl6/）
│   ├── yl6-question-generator/SKILL.md
│   ├── yl6-variant-generator/SKILL.md
│   ├── error-code-parser/SKILL.md
│   ├── eng-grammar-tag-extract-yl6/SKILL.md
│   ├── error-cluster-yl6/SKILL.md
│   └── error-report-export/SKILL.md
├── examples/                        # 每一步的真实产物，照着粘就能复现
│   ├── bank-10.json                 # 出题结果（10 题）
│   ├── error-coded-sample.txt       # 老师编码的 9 条错题
│   ├── error-parsed-sample.json     # 解析后的结构化错题
│   ├── error-cluster-sample.json    # 聚类后的讲评材料
│   └── variant-sample.json          # 一道错题的 3 道变式
└── docs/
    ├── screen-desktop.png
    └── screen-mobile.png
```

---

## 五、快速开始

### 方式 A：只想给学生用（不装 Hermes）

1. 下载/克隆本仓库
2. **双击 `index.html`**
3. 填昵称，选班级，访问码输入 `LY6A`
4. 选一个考点开始做

### 方式 B：要出题、要讲评（装 Hermes）

```bash
# 1. 克隆仓库
git clone https://github.com/<你的账号>/yl6-english-platform.git
cd yl6-english-platform

# 2. 装技能（Windows 便携版把路径换成 data/hermes/state/skills/）
mkdir -p ~/.hermes/skills/yl6
cp -r skills/* ~/.hermes/skills/yl6/

# 3. 打开网页
# 双击 index.html
```

技能装好后，在 Hermes 里输入 `/yl6-question-generator unit=2 grammar_topic=一般过去时 count=10` 就能出题。

> **发布成网页（可选）**：仓库是纯静态的，推送到 GitHub 后在 Settings → Pages 选 `main` / root，即可得到 `https://<账号>.github.io/yl6-english-platform/`。注意 `github.com` 与 `*.github.io` 是两个域名，国内有时一个通一个不通，请分别测试。

---

## 六、演示操作步骤

下面是一条可以完整走通的演示线，每一步的产物都在 `examples/` 里，可以直接对照。

### 步骤 1：出题（10 题一般过去时）

```bash
/yl6-question-generator unit=2 grammar_topic=一般过去时 count=10 level=中档
```

返回严格 JSON，第一个字符必须是 `{`：

```json
{
  "schema": "yl6.item/1",
  "meta": { "grade": "六上", "unit": "Unit 2", "grammar_topic": "一般过去时", "count": 10, "level": "中档" },
  "items": [
    {
      "id": "6A-U2-C-01",
      "tag": "past_form",
      "type": "choice",
      "stem": "It ____ hard yesterday, so we stayed at home.",
      "options": { "A": "rain", "B": "rains", "C": "rained", "D": "raining" },
      "answer": "C",
      "answer_text": "rained",
      "explain": "yesterday 说明事情发生在过去，rain 是规则动词，过去式加 -ed。",
      "point": "一般过去时：主语 + 动词过去式；规则动词直接加 -ed。",
      "trap": "看到 it 就选 rains（三单），是本单元最高频错因。"
    }
  ]
}
```

**怎么用**：整段复制 → 网页「专题刷题 → 导入题库 JSON」→ 粘贴 → 点「导入并开始」，直接开练。
完整示例见 [`examples/bank-10.json`](examples/bank-10.json)。

### 步骤 2：老师把错题编码成文本

学生**不能**直接提交错题（说不清题干、说不清自己写了什么）。老师按固定格式一行一题：

```text
编号 | 年级 | 单元 | 题型 | 原题 | 学生答案 | 正确答案 | 备注
E01 | 六上 | Unit 2 | 单选 | It ___ hard yesterday, so we stayed at home. (rain) A rain B rains C rained D raining | 学生:B | 答案:C | 看到 It 就用三单
E02 | 六上 | Unit 2 | 单选 | He didn't ___ to school yesterday. A go B went C goes D going | 学生:B | 答案:A | 认为「过去」就要 went
```

示例文件：[`examples/error-coded-sample.txt`](examples/error-coded-sample.txt)（9 条真实错题）

### 步骤 3：解析 + 归因

```bash
/error-code-parser 【粘贴上面的编码文本】
```

返回 `yl6.error/1`，每条错题都带上错误归因（`概念不清` / `时态混用` / `固定搭配混淆`）和一句「错在哪一步」：

```json
{
  "eid": "E02",
  "stem": "He didn't ___ to school yesterday.",
  "student_answer": "B",
  "correct_answer": "A",
  "attribution": "时态混用",
  "attribution_alt": "固定搭配混淆",
  "attribution_detail": "知道句子在过去，但让 did 和 went 同时表示过去",
  "tag": "past_neg"
}
```

完整输出：[`examples/error-parsed-sample.json`](examples/error-parsed-sample.json)

### 步骤 4：打语法标签

```bash
/eng-grammar-tag-extract-yl6 【粘贴步骤 3 的输出】
```

每条都拿回「常规用法 + 特殊易错特例」，这正是讲评时要说的话：

```json
{
  "eid": "E04",
  "tag": "verb_pattern",
  "tag_cn": "动词搭配 to do / doing",
  "regular": "want/hope/plan + to do；enjoy/finish/practice + doing",
  "exceptions": ["look forward to doing（to 是介词）", "stop to do 与 stop doing 意思不同"],
  "contrast": [{ "right": "finished doing", "wrong": "finished to do", "why": "finish 后面接 doing" }]
}
```

### 步骤 5：一道错题出三道变式

```bash
/yl6-variant-generator 【粘贴那道错题的完整 JSON】
```

**怎么用**：复制返回的 JSON → 网页「举一反三」→ 粘贴 → 「导入并开始训练」。
示例：[`examples/variant-sample.json`](examples/variant-sample.json)（换主语 / 换时间状语 + 场景，考点仍是过去式）

### 步骤 6：聚类出讲评材料

```bash
/error-cluster-yl6 【粘贴一批解析完成的错题 JSON 数组】
```

返回每个考点的「错误形态、同类表达、易混辨析、板书三步」，并自动排好优先级：

```json
{
  "tag": "past_neg",
  "tag_cn": "过去时否定",
  "count": 3,
  "hot": true,
  "weight": 9,
  "patterns": [{ "pattern": "didn't + 过去式", "n": 1, "sample": "didn't went" }],
  "same_expressions": ["didn't go", "didn't have", "didn't watch"],
  "core_point": "didn't 后面永远是动词原形：过去时由 did 一个人承担。",
  "board_plan": { "steps": ["对比 did you go / didn't go / went", "口诀：有 did 就还原", "当堂 6 句速写"] }
}
```

完整输出：[`examples/error-cluster-sample.json`](examples/error-cluster-sample.json)

### 步骤 7：出报表与讲义

```bash
/error-report-export 【粘贴 cluster JSON】
```

得到两部分：给老师看的**统计表**（错人次、错率、优先级 P1/P2/P3）和给学生做的**讲义**（一页两题、留答题空行、附错因、不写学生姓名）。

---

## 七、数据格式说明

### 7.1 `yl6.item/1` 题库（网页「导入题库」认这个）

| 字段 | 必填 | 说明 |
|---|---|---|
| `type` | ✅ | `choice` 四选一 / `fill` 适当形式填空 |
| `stem` | ✅ | 题干；填空用 `___` 占位，括号给提示词 `(rain)` |
| `options` | 选择题必填 | `{"A":"…","B":"…","C":"…","D":"…"}`，也接受长度 4 的数组 |
| `answer` | ✅ | 选择题填字母；填空题填词形 |
| `accept` | 填空建议填 | 容错数组，如 `["didn't go","did not go"]` |
| `point` / `why` / `know` | ✅ | 考点 / 为什么 / 知识点（解析四层里的三层，第四层是答案） |
| `tag` | 建议 | 20 个固定标签之一，用于错题分组与统计 |

### 7.2 `yl6.error/1` 结构化错题

`records[]` 中每条至少包含：`eid`、`stem`、`student_answer`、`correct_answer`、`attribution`、`attribution_detail`、`tag`。
`attribution` 只允许三个值 + 一个兜底：`概念不清` / `时态混用` / `固定搭配混淆` / `待定`。

### 7.3 `yl6.variant/1` 变式

`variants[]` 里是完整的题目对象（同 7.1），额外带 `derived_from` 指回原题 id。

### 7.4 `yl6.cluster/1` 讲评材料

`clusters[]`：`tag`、`count`、`weight`（人次 × 题数）、`hot`、`patterns`（错误形态）、`confusions`（易混辨析 + 证据）、`board_plan`（板书三步）。

### 7.5 老师编码格式

```
编号 | 年级 | 单元 | 题型 | 原题 | 学生答案 | 正确答案 | 备注
```

`|` 分隔、字段顺序固定；备注可省（写 `-`）；Excel/CSV 导出的制表符分隔也能直接粘。

---

## 八、使用方法

### 8.1 学生（3 分钟上手）

1. 双击 `index.html` → 昵称 + 班级 + 访问码 `LY6A` → 开始
2. 「专题刷题」选一个考点 → 点选项或填词 → 「提交答案」
3. 答错自动展开解析；答对想细看就点「查看解析与知识点」
4. 错题自动进「错题整理」，答对一次自动移出
5. 电脑上可用 `A/B/C/D` 选答案、`回车` 提交 / 下一题

### 8.2 老师（一条完整教学闭环）

```
课前：/yl6-question-generator 出题 → 网页导入题库 → 打印或直接课堂练
课中：学生做题，数据留在各自设备
课后：收错题 → 老师按模板编码 → /error-code-parser 解析
      → /eng-grammar-tag-extract-yl6 打标签
      → /error-cluster-yl6 聚类
      → /error-report-export 出统计表 + 讲义
下节：讲评只讲 P1 的三个考点，其余个别面批
```

### 8.3 数据备份

- 学生：「个人中心 → 导出全部数据 JSON」
- 老师：「错题整理 → 导出错题文本」（可直接打印）
- **localStorage 是按浏览器 + 按文件存的**，换电脑、清缓存、换浏览器都会丢，重要成绩务必先导出

---

## 九、技术实现

| 方面 | 做法 |
|---|---|
| 技术栈 | 原生 HTML + CSS + JavaScript（ES5 语法，兼容老浏览器），无框架、无构建、无 CDN |
| 体积 | 单文件 88 KB，含 40 题题库内嵌 |
| 存储 | `localStorage`，键名 `yl6_platform_v1`，结构 `{users:{}, current}`；所有写入包 `try/catch`，失败弹提示而不是静默丢数据 |
| 响应式 | ≥ 900 px 左侧栏 + 内容双栏；< 900 px 底部 6 项 Tab；实测 390 px 宽度无横向滚动 |
| 计时 | 每题 40 秒（`Q_SECONDS`），PK 模式总 60 秒（`PK_SECONDS`）；已作答停表 |
| 虚拟键盘 | 26 字母 + 空格 + 退格 + 清空；触屏设备（`pointer:coarse`）自动弹出 |
| 可访问性 | 全部按钮 ≥ 44 px；`aria-checked` 标记选项状态；`:focus-visible` 焦点环；`prefers-reduced-motion` 关动画 |
| 可维护 | CSS 变量集中在 `:root`；关键常量写在文件顶部；JS 全程中文注释 |
| 安全/合规 | 无任何外部请求、无 `alert/prompt/confirm`、无第三方统计；页面不采集真实姓名以外信息 |

**踩过并修掉的坑**（写在这里，方便接手的人避开）：

1. `[hidden]` 被 `.btn{display:inline-flex}` 覆盖 → 「下一题」「英文键盘」在未作答时就露出来。全局加 `[hidden]{display:none !important}`。
2. 手机顶栏昵称被长说明文字挤成两行 → 520 px 以下隐藏副标题、昵称加省略号。
3. PK 模式 900 ms 后自动跳题，若此时换局会跳到新会话 → 用闭包记住当局引用再判断。
4. 无头浏览器窗口最小宽度约 496 px，直接用 390 px 截图会「看起来溢出」——用 iframe 取景才是真的移动端布局。

---

## 十、验证记录

本项目不是「看着能用」，是跑过自动化的：

| 检查 | 结果 |
|---|---|
| 题库自检（选项唯一、答案在选项内、填空 answer 落在 accept 里、考点齐全） | 40 题 / 6 考点，**0 错误** |
| 无头浏览器全流程断言 | **32 项全过**：登录 → 选题 → 选项渲染 → 倒计时 → 判错/判对 → 解析展开 → 结算 → 记录写入 → 错题收纳 → 手册 10 条 → 变式 → 导入题库 → PK → 排行榜 → localStorage |
| `file://` 直接打开 | `login=true localStorage=ok items=40`（双击即可用，不需要起服务器） |
| 桌面 1440 px / 手机 390 px 截图 | 无横向溢出、无裁切、底部 Tab 完整 |

---

## 十一、常见问题

**Q1：微信里发给学生，打开是代码？**
微信把 `.html` 当纯文本预览。要么让学生用浏览器打开文件，要么把页面发布到 GitHub Pages / Netlify，发链接。文件后缀不能丢。

**Q2：换台电脑就没记录了？**
localStorage 是本地存储，不跟人走。用「导出全部数据 JSON」备份，换设备后可以留着做档案（当前版本没有回导功能，需要的话可以加）。

**Q3：排行榜为什么只有一个名字？**
它统计的是**这台设备上登录过的账号**，不是班级真实排名。要真排名必须加后端，本项目刻意不依赖服务器。

**Q4：题库里的单元标题对不对？**
项目刻意**不写单元标题和词汇表**——译林新版不同印次的单元编排可能有差异，写错会被学生背下去。单元/词汇以老师手上的教材和单元卷为准，出题时把材料一起给 AI。

**Q5：访问码是密码吗？**
不是。它只拦住「不登记就做题」，并把记录归属到人；清浏览器数据就没了，老师也看不到学生成绩。

**Q6：能不能在平板上用？**
可以，页面适配触屏；填空题在平板上会自动弹出内置英文键盘，避免系统键盘挡住题目。

---

## 十二、课程作业说明

### 12.1 选题与设计依据

本项目对应「AI 辅助教学工具设计」类作业，选择小学六年级英语作为场景，理由是：

- **考点清晰**：一般过去时、动词搭配、词族变形、序数词、疑问句、主谓一致，都是可枚举、可判定对错的硬语法；
- **可评测**：每道题都有唯一答案，AI 输出可以被程序校验（答案必在选项内、填空题 answer 必在 accept 内）；
- **真实可用**：六年级复习阶段确实存在「题不够、错题留不住、讲评没抓手」三个问题。

### 12.2 方法

1. **把教师经验写成技能**：6 个 Skill 各自带输入约束、输出 schema、硬规则和踩坑记录；
2. **把界面做成单文件**：数据与界面分离，题库是 JSON，界面只负责渲染与判分，方便老师自己换题；
3. **把错题当作数据**：老师编码 → 结构化 → 打标签 → 聚类 → 报表，每一步都能被人检查，而不是让 AI 直接下结论。

### 12.3 局限与改进方向

| 局限 | 改进方向 |
|---|---|
| 题库只有 40 题，且未覆盖六下全部单元 | 按单元卷扩充到 100+ 题；接 OCR 把纸质卷转成编码表 |
| 数据只在本机，班级无法汇总 | 加一个最小的后端（或 GitHub Issue 收集导出文件） |
| 排行榜是模拟的 | 接真实班级数据后才能叫排行榜 |
| 单元标题未内置 | 由老师提供教材页后生成，避免版本错配 |

### 12.4 复现方式

1. `examples/` 里有每一步的真实产物，可以逐条对照；
2. 网页可离线运行，不依赖任何账号；
3. 技能文件是纯 Markdown，可直接阅读与修改。

---

## 声明

- 代码部分采用 MIT License。
- 题库中的句子为教学用通用句（教材常用人物 + 日常场景），**未复制教材课文原文**；单元标题与词汇表以教师提供的教材页为准。
- 本项目不采集学生真实身份信息，全部数据留在使用者的浏览器中。
