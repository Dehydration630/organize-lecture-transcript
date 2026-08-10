# 课堂转写知识笔记 Skill

将带有时间戳、发言人标签、口语重复和识别错误的中文课堂转写，整理成忠于原文、便于复习的 Markdown 知识笔记。

这是一个遵循 Agent Skills 结构的公开 Skill 源码仓库，不是 Custom GPT。它支持两种运行方式：

- 在 ChatGPT 或 Codex 中安装后，使用当前平台提供的模型整理用户粘贴或上传的转写稿。
- 在 macOS 本地运行监听脚本，自动读取新复制的课堂转写并调用用户自己的 DeepSeek API。

## 在 ChatGPT 中使用

### 推荐：上传 Skill 压缩包

1. 下载 [`dist/organize-lecture-transcript.zip`](dist/organize-lecture-transcript.zip)。
2. 在 ChatGPT 中打开“插件 → 技能”。
3. 选择“创建 → 从电脑上传”，上传压缩包并按提示安装。
4. 新建对话，粘贴或上传课堂转写稿。

ChatGPT 会使用当前账号可用的 GPT 模型，不会调用 DeepSeek，也不需要用户提供 DeepSeek API Key。ChatGPT Skills 的可用性取决于账号方案和工作区权限，具体以 [OpenAI 官方说明](https://help.openai.com/en/articles/20001066) 为准。

### 便捷方式：通过公开仓库地址创建

如果当前 ChatGPT 会话能够读取公开 GitHub 仓库并具有创建 Skill 的权限，可以尝试发送：

```text
请读取下面公开 GitHub 仓库中的 organize-lecture-transcript Agent Skill，
保持原有目录结构和规则，为我创建并提示安装这个 Skill：
https://github.com/Dehydration630/organize-lecture-transcript
```

这不是官方保证的一键安装入口。如果 ChatGPT 无法读取仓库或没有出现安装提示，请使用上面的压缩包上传方式。

## 在 macOS 本地运行

### 1. 获取项目

从 GitHub 下载或克隆本仓库，然后进入仓库目录。

### 2. 在当前终端会话安全输入 DeepSeek Key

以下方式不会把完整 Key 写入仓库或命令历史：

```zsh
read -s "DEEPSEEK_API_KEY?DeepSeek API Key: "
export DEEPSEEK_API_KEY
```

默认配置：

- 模型：`deepseek-v4-flash`
- API 地址：`https://api.deepseek.com`
- 输出目录：`~/Documents/LectureNotes/output_notes`

### 3. 选择运行模式

监听剪贴板：

```zsh
python3 organize-lecture-transcript/scripts/run.py watch
```

处理单个文件：

```zsh
python3 organize-lecture-transcript/scripts/run.py file /path/to/transcript.txt
```

批量处理目录中的 `.txt`：

```zsh
python3 organize-lecture-transcript/scripts/run.py batch /path/to/transcripts
```

如需保存后自动打开笔记或把成品写回剪贴板，可在监听命令末尾添加 `--open-result` 或 `--copy-result`。

## 工作方式对照

| 环境 | 模型 | 输入 | 本地剪贴板监听 |
|---|---|---|---|
| ChatGPT 网页 Skill | ChatGPT 当前可用模型 | 粘贴或上传 | 不支持 |
| Codex 对话 | Codex 当前模型 | 粘贴、上传或本地文件 | 按权限使用 |
| macOS 本地脚本 | 用户配置的 DeepSeek | 剪贴板、文件或目录 | 支持 |

## 项目结构

```text
organize-lecture-transcript/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   └── example.md
└── scripts/
    └── run.py
```

`SKILL.md` 是所有环境共用的唯一笔记规则来源。本地脚本会读取同一文件构造 DeepSeek 请求，因此不需要分别维护两套提示词。

## 隐私与安全

- 不要把任何 API Key 提交到 GitHub、写进 Skill 或发送给其他用户。
- ChatGPT 网页模式只处理用户主动粘贴或上传的内容，不访问本机剪贴板。
- 本地模式会把转写内容发送到用户配置的模型 API，请在处理敏感课堂内容前确认合规要求。
- 错误日志不记录完整课堂转写或 API Key。

## 发布提醒

仓库地址确定后，将本文中的 `https://github.com/Dehydration630/organize-lecture-transcript` 替换为真实地址。公开发布前还应由仓库所有者选择并添加合适的开源许可证；在未添加许可证时，默认保留全部权利。
