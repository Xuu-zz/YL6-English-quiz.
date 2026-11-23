---
name: eng-grammar-tag-extract-yl6
description: 给译林六年级错题打语法标签，附常规用法与易错特例。
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [education, yilin, grade6, tagging, grammar, json]
    related_skills: [error-code-parser, yl6-question-generator, error-cluster-yl6, grammar-variant-factory]
---

# 六年级语法标签抽取（含常规用法 + 特例）

打标签的目的不是「这是哪一课」，而是「学生卡在哪一步」。同一条标签必须配两句话：**常规用法**（会了就该这么做）+ **特殊易错特例**（会了也常在这里翻车）。

## When to Use / 什么时候用

- 错题入库后要归类
- 出题前要确认考点是否说得清
- 老师问「这个点到底有哪些坑」

## 标签总表（20 个，只认这些，不许自造）

| tag | 中文名 | 判定特征（看到什么就打它） |
|---|---|---|
| `past_form` | 一般过去时·动词过去式 | 句中过去时间状语，空在动词位，要填过去式 |
| `past_neg` | 过去时否定 | `didn't / did not` 后留空，考原形 |
| `past_yesno` | Did 引导一般疑问句 | `Did ...` 起句，考原形或 Yes/No 简答 |
| `be_past` | was / were | 空在 be 位，主语决定单复数 |
| `be_born` | be born | 出生表达，只考 was / were |
| `wh_word` | 特殊疑问词 | 空在句首疑问词位（What/Where/When/Who/How many/Why） |
| `transform_ask` | 划线部分提问 | 「对划线部分提问」，要写整句 |
| `word_family` | 词族变形 | 括号给词根，要改名/形/副/动 |
| `verb_pattern` | 动词搭配 to do / doing | 动词后接不定式还是 -ing |
| `plural` | 名词复数 | 数量词后、名词位 |
| `ordinal` | 序数词 | 日期、楼层、第几 |
| `prep_time` | 时间介词 in/on/at | 时间名词前的介词 |
| `sva` | 主谓一致 | 主语与谓语单复数匹配 |
| `indef_pron` | 不定代词 | everyone/something/anyone/nothing 作主语或宾语 |
| `continuous` | 现在进行时 | be + doing，`now / Look! / Listen!` |
| `tense_signal` | 时态标志词 | 靠 `yesterday / now / every day` 定时态 |
| `whose` | 物主代词 | my/mine/your/yours/his/her/hers |
| `transform_neg` | 改否定句 | 「改为否定句」 |
| `phrase_fill` | 固定搭配/短语 | 动词+介词、be+形容词+介词 |
| `turn_into` | 旧物改造介词 | make ... into / turn ... into（旧库兼容保留） |

## 常规用法 + 特殊易错特例（核心 10 个先背）

| tag | 常规用法 | 特殊易错特例 |
|---|---|---|
| `past_form` | 过去发生的动作用过去式；规则加 -ed，不规则单独记 | `stop → stopped`（双写）、`study → studied`（y 变 i）；`read` 拼写不变、读音变 |
| `past_neg` | `didn't + 动词原形` | **`didn't` 后一定是原形**，不能写过去式；`didn't went` 是最高频错 |
| `past_yesno` | `Did + 主语 + 原形 ...?` 答 `Yes, I did. / No, I didn't.` | 答句用 `did`，不能用 `was/does`；`Did you went` 错 |
| `be_past` | I/he/she/it 用 `was`，you/we/they 用 `were` | `There was / There were` 看后面名词；`you` 单数也是 `were` |
| `be_born` | `was/were born`，时间用 `in + 年份 / on + 日期` | **只用 was/were，不用 did**；`I born` 缺 be，`did you born` 全错 |
| `wh_word` | 问什么选什么：`What` 事物、`Where` 地点、`When` 时间、`Who` 人、`How many` 可数数量 | `How many + 复数名词`；`What` 后不加 `do` 又问时间；`When` 与 `What time` 混 |
| `word_family` | 看空的位置定词性：名词位、形容词位（名前/be 后）、副词位（动后）、动词位 | `care → careful / carefully / care`；`happy → happily`（y 变 i）；`good → well` |
| `verb_pattern` | `want/hope/plan/learn/decide/need + to do`；`like/enjoy/finish/mind/practice + doing` | `like` 两可但教材统一 `like doing`；`look forward to doing`（to 是介词）；`stop to do` 与 `stop doing` 意思不同 |
| `sva` | 主语单复数决定谓语形式 | 主语后有 `with / together with / and` 时看**第一个主语**；`The number of ... is` |
| `indef_pron` | `everyone/everybody/someone/something/nobody/nothing` 作主语，谓语用**单数** | `Everyone like` 错，应 `likes`；`something` 放肯定句，`anything` 放否定/疑问；形容词后置 `something new` |

## 其余 10 个的要点

| tag | 常规用法 | 特例 |
|---|---|---|
| `plural` | 一般加 -s | `box → boxes`、`child → children`、`knife → knives`、`sheep` 单复同形 |
| `ordinal` | 第几用 the + 序数词 | `first/second/third` 不规则；`twentieth`（y 变 ie）；日期用 `on the 5th of May` |
| `prep_time` | `in` 年/月/季节，`on` 具体某天，`at` 时刻 | `at night / at noon`；`on Monday morning` 有修饰就用 on |
| `continuous` | `be + doing`，看 `now / Look / Listen` | 静态动词 `like/know` 一般不用进行时；`be` 别漏 |
| `tense_signal` | `every day` 一般现在、`now` 进行、`yesterday` 过去 | 同一句里两个时间状语冲突时，以紧邻动词的为准 |
| `whose` | `whose + 名词`；`mine/yours/hers` 独立使用 | `its` 不是 `it's`；`whose` 与 `who's` 读音同、写法不同 |
| `transform_neg` | 有 be/情态动词直接加 not；实义动词借 `don't/doesn't/didn't` | 改否定后 `some → any`、`too → either`、`already → yet` |
| `transform_ask` | 划线主语：`Who + 谓语`；划线其他：疑问词 + 助动词 + 主语 | 划线主语时不再借 `do`（`Who likes ...?`） |
| `phrase_fill` | 记整条：`be good at`、`be interested in`、`take part in`、`look after` | 介词后接 doing；`be good at` 后不加 to |
| `turn_into` | `make/turn A into B` | 别写成 `make A to B`；被动少见，六年级只考主动 |

## 输出

```json
{
  "schema": "yl6.tag/1",
  "items": [
    {
      "eid": "E01",
      "tag": "past_neg",
      "tag_cn": "过去时否定",
      "confidence": 0.92,
      "evidence": "didn't ___ (go) to school",
      "regular": "didn't + 动词原形",
      "exceptions": ["didn't 后不能写过去式", "didn't 后不能写 -ing"],
      "contrast": [{"right": "didn't go", "wrong": "didn't went", "why": "助动词已经承担了过去时，实义动词还原形"}],
      "also": ["past_form"],
      "need_review": false
    }
  ]
}
```

## 打标签的规矩

1. **一个主标签**，最多 2 个次要标签（`also`）。
2. 打「学生错在哪一步」的标签：题干是 `didn't ___` 但学生写 `went`，主标签是 `past_neg`，不是 `past_form`。
3. 标签只能从表里选；想加新标签，先改这张表，再改 `yl6-question-generator` 的 TAGS 集合。
4. `regular` 与 `exceptions` 必填。`exceptions` 为空说明这个点没坑，检查是不是选错了标签。
5. `evidence` 抄题干里触发判断的那个片段，不整句复制。
6. 批量处理：输出条目数、`eid` 顺序与输入完全对齐；不确定的给 `confidence < 0.6` + `need_review: true`，不要硬判。
7. 单选题选项差异在时态上 → `past_form/past_neg`；差异在搭配上 → `verb_pattern`；差异在词性上 → `word_family`。

## 踩过的坑

1. **`be_born` 常被误打成 `past_form`**：它只考 was/were，单独成 tag 才能聚出「学生老写 did you born」这类错。
2. **`indef_pron` 与 `sva` 会打架**：主语是不定代词就打 `indef_pron`，`sva` 放 `also`；主语是人/物名词才主打 `sva`。
3. **`tense_signal` 是辅助标签**，不要当主标签，否则聚类会把所有时态题挤成一堆。
4. `turn_into` 只为兼容旧题库，新出的题不用它。
