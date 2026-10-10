# OKR 起草与检查插件

一个用中文起草、修改和检查 OKR 的 Agent Skill 插件。它区分 Objective（目标）、Key Result（关键结果）和 Initiative（行动），帮助发现指标不可衡量、缺少基线、把任务写成结果、与上级目标不对齐等问题。

这是纯 Skill 插件：不连接外部 OKR 系统，也不自动读取实时业务数据。缺少基线、目标值、负责人或数据来源时，Skill 会标记“待确认”，不会编造数字。

## 仓库结构

~~~text
plugin.json
skills/
└── okr-draft-review/
    └── SKILL.md
~~~

plugin.json 是插件清单；SKILL.md 定义触发场景、起草与检查流程及输出要求。

## 使用示例

> 帮我为客户成功团队起草下季度 OKR。上级目标是降低新客户流失；目前 90 天留存率为 72%，数据来自 CRM。先列出需要我补充的关键信息。

> 请检查下面这组 OKR。逐条指出必须修正的问题，保留我的原意给出改写；未知的基线不要猜测。

> 这些 KR 是否真正支撑 O？请检查重复计数、负责人影响力和数据口径。

## 安装

### 仅使用 Skill

把仓库中的 skills/okr-draft-review/ 复制到目标项目的 .agents/skills/okr-draft-review/。Codex 会从项目的 .agents/skills 发现本地 Skill。

### 作为本地插件使用

将整个仓库放到目标项目的 plugins/okr-plugin/，然后在目标项目的 .agents/plugins/marketplace.json 中登记：

~~~json
{
  "name": "local-okr",
  "plugins": [
    {
      "name": "okr-plugin",
      "source": {
        "source": "local",
        "path": "./plugins/okr-plugin"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
~~~

重启桌面应用，在插件目录中找到 local-okr 并安装 okr-plugin。若目标项目已有 marketplace 文件，把插件条目加入现有 plugins 数组，保留其他条目。上传到 GitHub 只会托管源码，使用者仍需安装 Skill 或插件。

安装方法见 [OpenAI 插件文档](https://developers.openai.com/plugins/build/plugins)和 [Codex Skill 文档](https://learn.chatgpt.com/docs/build-skills)。

## 设计参考

本插件的起草与逐条审查方式参考了 [create-okrs](https://github.com/yehonathanlitman/create-okrs)，中文 OKR 工作流程参考了 [agenmod/okr-skill](https://github.com/agenmod/okr-skill)。SKILL.md 为本项目重新编写的简化版本。
