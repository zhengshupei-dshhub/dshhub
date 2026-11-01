# dshhub · DeepSeek Harness 插件目录

本机自用的 **DSH（DeepSeek Harness）插件集合**。每个插件一个独立仓库，这里是索引：目录、功能介绍、安装方式和当前版本。

> 所有插件都是 bundle 形态（`package.json` 的 `dsh.bundle.patch` 指向 `cordis.patch.yml`），通过 `dsh plugin add` 或 profile 的 `pnpm` 依赖安装。**仓库里不放任何密钥**。

## 插件一览

| 插件 | 仓库 | 当前版本 | 一句话 |
|---|---|---|---|
| **dsh-memory** | [zhengshupei-dshhub/dsh-memory](https://github.com/zhengshupei-dshhub/dsh-memory) | `0.2.5` | 本机长期记忆 + 人设养成 + 自我迭代台账 |
| **dsh-dafeiyu** | [zhengshupei-dshhub/dsh-dafeiyu](https://github.com/zhengshupei-dshhub/dsh-dafeiyu) | `0.1.28-local.1` | 桌面大肥鱼：人设联动、桌面对话、季节皮肤 |
| **dsh-doubao** | [zhengshupei-dshhub/dsh-doubao](https://github.com/zhengshupei-dshhub/dsh-doubao) | `0.1.5` | 豆包（火山方舟）感官：看图、出图、视频 |

机器可读的清单见 [`plugins.json`](plugins.json)。

## 自动化任务

不属于插件，是「到点让 agent 做一件事」的定时任务，单独放在 **[dsh-automation](https://github.com/zhengshupei-dshhub/dsh-automation)**（每个任务一个目录 + 提示词原文）。

| 任务 | 触发 | 类型 | 一句话 |
|---|---|---|---|
| **WorkBuddy 签到 + 礼包** | 每天 09:10 | 脚本 | 领每日签到（约 100/天）与礼包/补偿 |
| **空闲自我进化** | 每天 22:00 | 提示词 | 没有任务时自己推进一项改进 |

> 自动化任务依赖 DSH 常驻运行；应用退出就不会触发。

---

## dsh-memory

**本机长期记忆 + 人设养成 + 自我迭代台账。** 压缩注定有损，记忆可以无损——它把值得记住的事实、偏好、坑存成 JSON，跨会话检索；还管着「我是谁」和「要改什么」。

| 能力 | 工具 |
|---|---|
| 长期记忆 | `memory_save` / `memory_search` / `memory_forget` |
| 历史会话全文回捞 | `memory_recall` |
| 人设养成 | `persona_read` / `persona_grow` / `persona_diary` |
| 自我迭代台账 | `evolution_note` / `evolution_list` / `evolution_log` |

- 数据：`~/.dsh/memory/`（`memories.json`、`persona.json`、`evolution/*.jsonl`）
- 依赖：零第三方依赖，只用 Node 内置模块
- 安装：

```bash
dsh plugin --profile desktop add git+https://github.com/zhengshupei-dshhub/dsh-memory.git
```

## dsh-dafeiyu

**桌面大肥鱼伴侣。** 透明置顶窗口，跟着 DSH 的任务状态变化；本地分支在原生桌宠之外加了人设联动和桌面对话。

- 人设联动：读 `persona.json`，人设成长 / 写日记 / 换天时冒气泡；空闲久了会自己说一句
- 桌面对话：右键菜单直接跟它说话，回复走 `agent.followup` 回显在气泡里
- 季节皮肤：`lookTheme` / `lookIntensity`，按月份切换柔光与飘落粒子，**不改精灵图**，可一键关闭
- 动作表现：按气泡内容触发歪头 / 蔫一下 / 弹跳 / 说话抖动
- 平台：macOS（原生 Helper）；Windows/Linux 走上游的 Python 路径
- 安装：

```bash
dsh plugin --profile desktop add git+https://github.com/zhengshupei-dshhub/dsh-dafeiyu.git
# 仓库不含预编译 Helper，装完需要构建一次（macOS）：
cd "$(dirname "$(node -p "require.resolve('dsh-dafeiyu/package.json')")")" && bash native/macos/build.sh
```

## dsh-doubao

**给 DSH 接上豆包的感官。** 火山方舟的视觉理解、Seedream 出图、Seedance 视频，零第三方依赖。

| 工具 | 作用 |
|---|---|
| `doubao_status` | 检查 Key 与模型开通情况 |
| `doubao_see` | 看图：本地路径 / URL / dataURL + 提问 |
| `doubao_draw` | Seedream 文生图 / 图生图，存到输出目录 |
| `doubao_video` | Seedance 文生 / 图生视频，异步轮询后落 mp4 |

- Key：`~/.dsh/doubao.json`（权限 600，不入库）
- 输出：默认 `~/DeepSeekHarness/BigFish/doubao-output/`，可用 `outputDir` 覆盖
- 安装：

```bash
dsh plugin --profile desktop add git+https://github.com/zhengshupei-dshhub/dsh-doubao.git
```

---

## 安装通用说明

```bash
# 国内网络拉 GitHub 需要代理
export HTTPS_PROXY=http://127.0.0.1:7897

# 装某个插件（以 profile desktop 为例）
dsh plugin --profile desktop add git+https://github.com/zhengshupei-dshhub/<仓库>.git

# 或写进 ~/.dsh/profiles/desktop/package.json 后
pnpm install
```

装完确认 `dsh.profile.bundles` 里有对应名字，重启 DSH 生效。

## 维护约定

- **新增插件**：建独立仓库 → 在本页「插件一览」加一行 → 在下面补一节功能介绍 → 更新 `plugins.json`
- **插件更新**：版本号、功能变化同步回本仓库，保证目录和实际代码一致
- **新增自动化任务**：放到 [dsh-automation](https://github.com/zhengshupei-dshhub/dsh-automation) → 在本页「自动化任务」表格加一行
- 不收录：密钥、token、运行时数据、构建产物

## 版本记录

| 日期 | 变更 |
|---|---|
| 2026-11-01 | 目录建立：收录 dsh-memory、dsh-dafeiyu、dsh-doubao |
| 2026-11-01 | 新增「自动化任务」一节，指向 dsh-automation（签到 + 自我进化） |
