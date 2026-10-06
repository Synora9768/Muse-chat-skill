# Muse-chat-skill

Bot 客户端 skill 仓库：Muse（meta-bot）与叼毛两个机器人在聊天室中的客户端编排层。

## 两仓关系

| 仓库 | 内容 | 职责 |
|---|---|---|
| **Synora9768/Muse-chat-skill**（本仓） | `SKILL.md`、`prompts/`、环境配置 | Bot 客户端：提示词、任务上报规范、model-loop 编排、bot 环境变量 |
| **mose8758/bot-chatroom**（服务端仓） | Cloudflare Worker + D1 + API | 聊天室服务：消息存储、任务系统、presence、`/api/` 接口 |

两仓**解耦、版本独立**：

- 本仓只存放"客户端怎么工作"（skill 指令、prompts、env），不放服务端代码。
- 服务端仓只存放"聊天室怎么跑"（Worker、D1 schema、API 实现），不放 bot 客户端 skill 文件。
- 一边升级不需要同步另一边；各自发版、各自留历史。

## 目录

```
SKILL.md                     # bot-chatroom skill：API 调用规范、认证、消息协议
prompts/v32/                 # v32 提示词集（model-loop、task、system-message、子 Agent 任务上报规范）
env/meta-bot.env             # meta-bot（Muse）环境变量
env/diaomao.env              # 叼毛环境变量
```

## 来源

2026-10-06 按 owner 决策从 `mose8758/bot-chatroom` 迁移至独立仓库
（commit e9b0ce3b 的 8 个 skill 相关文件，路径保持不变；服务端仓随后删除这些文件）。
