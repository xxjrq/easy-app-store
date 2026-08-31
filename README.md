<div align="center">

<img src="assets/icon-512.png" width="112" height="112" alt="easyAppStore 图标">

# easyAppStore

## App Store Connect 自动上架 Agent Skill

让 Claude Code、Codex、OpenCode、WorkBuddy 等 AI Agent 读取 iOS 项目，完成构建、上传、商店资料、审核提交、状态检查和驳回处理。

[简体中文](README.md) | [English](README.en.md)

[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-compatible-16BDEB?style=flat-square)](https://agentskills.io/)
[![Easy WebBridge](https://img.shields.io/badge/runtime-Easy%20WebBridge-111827?style=flat-square)](https://github.com/xxjrq/easy-webbridge)
[![License](https://img.shields.io/badge/license-MIT-16A34A?style=flat-square)](LICENSE)

</div>

## 它能做什么

- 自动识别 Xcode、Flutter、React Native、Unity 和 Fastlane 项目。
- 根据项目现状生成 Release 构建命令，得到并上传 IPA。
- 上传 App 图标、iPhone/iPad 截图、预览视频和审核附件。
- 填写名称、描述、关键词、分类、隐私、年龄分级和审核信息。
- 选择构建、提交 App Review，并检查真实审核或上架状态。
- 读取驳回信息，判断应修改代码、截图、文案还是审核资料。

## 它与 Easy WebBridge 的关系

```text
Claude Code / Codex / OpenCode / WorkBuddy
                    |
             easyAppStore Skill
          项目构建 + 上架业务流程
                    |
          Easy WebBridge 127.0.0.1:17777
                    |
        浏览器扩展 + 已登录 App Store Connect
```

`easyAppStore` 是独立业务 Skill，`Easy WebBridge` 是所有浏览器业务 Skill 共用的运行时。业务 Skill 不引用你电脑上的仓库路径，而是通过本机 HTTP API 连接运行时，因此换一台电脑后仍有明确的安装和自检流程。

## 安装

### 1. 安装一次 Easy WebBridge

```bash
git clone https://github.com/xxjrq/easy-webbridge.git
cd easy-webbridge
npm install
node cli/easy-webbridge.mjs start
```

在已登录 App Store Connect 的 Chromium 浏览器中加载 Easy WebBridge 扩展。这个步骤只需为运行环境配置一次，后续其他 Easy WebBridge 业务 Skill 直接共用。

### 2. 安装 easyAppStore

跨 Agent 安装器：

```bash
npx skills add xxjrq/easy-app-store
```

通用手动安装位置：

```bash
git clone https://github.com/xxjrq/easy-app-store.git ~/.agents/skills/easy-app-store
```

仓库根目录的标准 [`SKILL.md`](SKILL.md) 是所有 Agent 的公共入口；[`agents/openai.yaml`](agents/openai.yaml) 只是 Codex 的可选展示信息，不参与核心运行。

## 运行自检

```bash
node scripts/easy-webbridge-client.mjs preflight
```

自检成功会列出在线浏览器的名称和 `browserId`；运行时、Token 或浏览器扩展未就绪时，会返回明确的下一步。

## 直接使用

```text
使用 easyAppStore，把 /项目绝对路径 的 iOS App 上架到 App Store。
复用已经登录的 App Store Connect 浏览器，完成构建、上传、资料填写、提交审核，并告诉我最终状态。
```

也可以只做一个阶段：

```text
使用 easyAppStore 检查这个 iOS 项目和 App Store Connect 当前版本，告诉我还缺哪些资料，先不要提交审核。
```

## 运行要求

- Node.js 20+
- macOS 与项目需要的 iOS 构建工具
- Easy WebBridge 0.3.2+
- 一个已经登录 App Store Connect 的 Chromium 浏览器环境

## 开源协议

[MIT](LICENSE)
