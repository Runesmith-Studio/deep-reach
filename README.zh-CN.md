<div align="center">

# 🛰️ deep-reach

### 问一句,拿回一份带引用、敢标盲区的调研简报。

**AI 舆情监测 · 市场调研 · 竞品情报 —— 合成一个 Claude Code 技能。**

[![License: MIT](https://img.shields.io/badge/License-MIT-black.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-skill-da7756.svg)](https://docs.claude.com/en/docs/claude-code)

[English →](README.md)

</div>

---

> **不用再养一套每月几千块、却很少打开的舆情监测后台。**
> **不用再左边开个趋势工具、右边开个爬虫,来回切。**
> **不用再手动把 Reddit 帖、视频字幕、论坛回复一条条复制进文档。**
>
> 问一句就行。**deep-reach** 扫近 30 天大家在说什么,把 **13+ 平台的原文**读全,
> 再交给你一份**会告诉你"这结论能信几分"的带引用简报** —— 一个技能搞定,就在 Claude Code 里。

---

## 你会得到什么

用大白话问一句 —— *"大家到底怎么看 X?"*、*"竞品 Y 的真实用户吐槽,带出处"*、
*"这个方向值不值得做?"* —— deep-reach 替你跑完整条调研链:

```
①  扫温度   →   ②  挖原文   →   ③  出简报
```

1. **扫温度** —— 找出你的话题现在在哪儿热、风向往哪走。
2. **挖原文** —— 把**完整原文**(帖子、评论区、视频字幕、文章)从 **Reddit、X、
   YouTube、GitHub、B站、小红书、V2EX 和全网**读出来。
3. **出简报** —— 写成一份**带引用的证据简报**:每条带出处原话、明确列出没覆盖的盲区、
   再给一个**可信度评级(厚 / 中 / 薄)**,让你绝不会把一个薄猜测当成硬结论。

→ 看一份[示例简报](examples/example-brief.md)。

---

## 为什么用 deep-reach

|  | 企业级监测套件 | 自己拼工具 | **deep-reach** |
|---|---|---|---|
| 价格 | 每月几千上万 | "免费"(烧你的时间) | **免费开源** |
| 上手 | 销售对接 + 培训 | 自己粘 5 个工具 | **一个技能,即插即用** |
| 读全原文 | 部分 | 手动 | **✅ 自动** |
| 中文平台(B站/小红书/雪球) | 基本没有 | 很难 | **✅ 内置** |
| 告诉你能信几分 | ❌ | ❌ | **✅ 可信度评级 + 盲区** |
| 真实用户原话 + 出处 | 只给看板 | 手动复制 | **✅ 原话 + 链接** |
| 在你干活的地方 | 另开 App | 一堆 App | **✅ 就在 Claude Code 里** |

**一句话卖点**:企业级情报套件的覆盖广度 + 实时趋势扫描的新鲜度,但去掉了高价、
去掉了来回切标签页、也去掉了"这答案到底靠不靠谱"的猜测。

---

## 能力一览

- 🔭 **舆情监测 / 热度追踪** —— 任意话题、品牌、事件的近 30 天温度。
- 🧭 **市场 & 竞品调研** —— 用客户自己的话挖出真实吐槽、好评和需求信号。
- ✅ **选品 / 方向验证** —— 动手前先把真实需求证据攒够。
- 🗣️ **用户原声挖掘** —— 用户描述痛点的原话,直接反推文案或关键词。
- 🌏 **13+ 平台,中英通吃** —— Reddit、X、YouTube、GitHub、B站、小红书、V2EX、全网。
- 🧪 **内置证据纪律** —— 每份简报都标清覆盖范围、盲区和可信度,绝不拿包装过的猜测糊弄。
- 🔒 **隐私优先、默认免费** —— 默认零账号;绝不碰你的主账号或日常浏览器 Cookie。

---

## 安装

```bash
# 1. 把技能放进 Claude Code
git clone https://github.com/Runesmith-Studio/deep-reach.git
cp -r deep-reach/skills/deep-reach ~/.claude/skills/
```

这样就能用了。想要**最深**的多平台覆盖,deep-reach 还能驱动两个可选的调研引擎
(它会帮你配好):

```bash
# 可选增强(满血推荐)
pipx install https://github.com/Panniantong/agent-reach/archive/main.zip
agent-reach install --env=auto      # 免费多平台渠道
pipx install bilibili-cli           # 免登录 B站 搜索
#  + last30days 技能:https://github.com/mvanhorn/last30days-skill
```

没装它们也能用——只要你的 Claude 宿主有联网搜索/抓取能力,就照常出简报,只是覆盖浅一些。
而且它永远会**老实告诉你哪些没覆盖**,不会假装查过。

---

## 怎么用

直接对 Claude 说:

- *"调研一下大家到底怎么看 \<话题\>,挖进原文"*
- *"竞品 \<产品\> 的真实用户吐槽,带出处"*
- *"\<方向\> 值不值得做?给我看真实需求信号"*

deep-reach 会跑 扫温度 → 挖原文 → 出简报,结果写进 `./research/`。

---

## 许可

MIT © [Runesmith Studio](https://github.com/Runesmith-Studio) —— 一家独立 AI 应用工作室。

由开源工具 [last30days](https://github.com/mvanhorn/last30days-skill)、
[agent-reach](https://github.com/Panniantong/Agent-Reach) 提供底层能力支持。
