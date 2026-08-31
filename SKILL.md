---
name: easy-app-store
description: 自动完成 iOS 应用的 App Store 上架工作。Use when an agent needs to inspect an iOS project, build or locate an IPA, upload a build and store assets, fill App Store Connect metadata and compliance information, submit a version for review, check review or publication status, or read a rejection and fix the app or listing. Requires Easy WebBridge and an already logged-in App Store Connect browser.
license: MIT
metadata:
  author: xxjrq
  version: "0.2.0"
  runtime: "easy-webbridge >=0.3.2"
  runtime-repository: "https://github.com/xxjrq/easy-webbridge"
---

# easyAppStore

把一个 iOS 项目从本地工程带到 App Store。Agent 根据项目实际情况自动决定构建命令，并通过 Easy WebBridge 操作已经登录的 App Store Connect。本 Skill 遵循 Agent Skills 标准，可由 Claude Code、Codex、OpenCode、WorkBuddy 及其他兼容 Agent 使用。

## 先确认可以运行

在本 Skill 根目录执行：

```bash
node scripts/easy-webbridge-client.mjs preflight
```

自检会确认 Node.js、Easy WebBridge、访问令牌和在线浏览器，并返回可选浏览器的名称与 `browserId`。若运行时尚未安装，按自检结果先完成一次 [Easy WebBridge](https://github.com/xxjrq/easy-webbridge) 安装；后续所有 Easy WebBridge 业务 Skill 共用这一套运行时。

## 能做什么

- 识别 Xcode、Flutter、React Native、Unity、Fastlane 等项目，找到正确的应用、Bundle ID、版本号和构建号。
- 检查签名与发布配置，按项目现有工具构建或找到可上传的 IPA。
- 上传构建包、App 图标、各尺寸截图、预览视频和审核附件。
- 填写应用名称、副标题、描述、关键词、分类、支持网址、隐私政策和版本说明。
- 填写年龄分级、隐私信息、出口合规、内容版权和审核联系信息。
- 按需要填写审核账号、演示步骤、二维码或测试附件。
- 选择正确构建版本，提交 App Review，并继续检查真实审核状态。
- 读取审核意见，判断应修改代码、截图、商店文案还是审核资料，然后完成修正和重新提交。

## 开始

1. 读取应用项目和现有发布配置，沿用项目当前构建体系。
2. 确认目标 App、版本、市场、发布范围和用户期望的终点。
3. 根据自检结果选择已经登录 App Store Connect 的浏览器。
4. 为本次上架创建一个 `easy-app-store-<run-id>` 标签组；后续操作复用组内当前页面。
5. 汇总本次需要的文字、图片、视频、账号和审核附件；能从项目可靠读取的内容直接读取。

## 执行

1. 使用项目已有的 Xcode、构建脚本或 Fastlane 完成 Release 构建和上传。
2. 在 App Store Connect 选择正确应用和版本，关联刚上传的构建。
3. 填完版本页面、商店资料、隐私与合规页面、审核信息。
4. 上传所需截图和附件，并等待页面显示处理完成。
5. 用户要求“上架”或“提交审核”时，继续完成提交；用户只要求准备或检查时，停在对应结果页。
6. 提交后重新读取页面，记录实际显示的版本、构建号和状态。
7. 若被退回，读取完整审核信息，定位问题，修改对应内容并重新走本次流程。

## Easy WebBridge 使用方式

- Easy WebBridge 地址固定为 `127.0.0.1:17777`。
- 一次任务固定控制一个 `browserId`。
- 顺序步骤复用同一标签页；确实需要对照时才在同一任务组增加标签页。
- 使用本 Skill 的 `scripts/easy-webbridge-client.mjs` 调用 `list`、`navigate`、`snapshot`、`click`、`fill`、`upload` 和 `command`；它直接连接运行中的 Easy WebBridge，不依赖另一个 Skill 的本地路径。
- 使用 `upload` 上传绝对路径文件；页面变化后重新读取页面再继续。
- 任务结束时只关闭本 Skill 创建的标签组，保留用户原来的页面。

## 完成标准

向用户清楚给出：

- App 名称、Bundle ID、版本号和构建号。
- IPA 的生成或上传结果。
- 商店资料、截图、隐私合规和审核资料的完成情况。
- App Store Connect 当前真实状态，例如“准备提交”“正在等待审核”“审核中”“被拒绝”“可供销售”。
- 若有审核问题，给出具体原因、已修改内容和下一步。

分别记录“上传成功”“提交成功”“审核通过”和“商店可见”，以 App Store Connect 最终页面为准。
