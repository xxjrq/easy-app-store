<div align="center">

<img src="assets/icon-512.png" width="112" height="112" alt="easyAppStore icon">

# easyAppStore

## App Store Connect release automation for AI agents

Let Claude Code, Codex, OpenCode, WorkBuddy, and other Agent Skills-compatible agents inspect an iOS project, build and upload it, complete App Store Connect metadata, submit it for review, verify status, and handle rejections.

[简体中文](README.md) | [English](README.en.md)

</div>

## What it does

- Detects Xcode, Flutter, React Native, Unity, and Fastlane projects.
- Chooses project-appropriate Release commands and prepares an IPA.
- Uploads builds, icons, screenshots, preview videos, and review attachments.
- Completes listing metadata, privacy, age rating, compliance, and review information.
- Selects the correct build, submits App Review, and verifies the actual status.
- Reads rejection details and routes the fix to code, assets, metadata, or review notes.

## How the repositories work together

```text
Claude Code / Codex / OpenCode / WorkBuddy
                    |
             easyAppStore Skill
          build decisions + release workflow
                    |
          Easy WebBridge 127.0.0.1:17777
                    |
       browser extension + logged-in App Store Connect
```

`easyAppStore` is an independent business Skill. [Easy WebBridge](https://github.com/xxjrq/easy-webbridge) is the shared browser runtime installed once for every browser-based business Skill. The Skill talks to the local runtime over HTTP and never depends on a repository path from the author's computer.

## Install

Install and configure Easy WebBridge once:

```bash
git clone https://github.com/xxjrq/easy-webbridge.git
cd easy-webbridge
npm install
node cli/easy-webbridge.mjs start
```

Load its extension in the Chromium profile that is already logged in to App Store Connect. Then install this Skill:

```bash
npx skills add xxjrq/easy-app-store
```

Universal manual location:

```bash
git clone https://github.com/xxjrq/easy-app-store.git ~/.agents/skills/easy-app-store
```

The root [`SKILL.md`](SKILL.md) is the cross-agent entry point. [`agents/openai.yaml`](agents/openai.yaml) is optional Codex UI metadata and is not required at runtime.

## Preflight

```bash
node scripts/easy-webbridge-client.mjs preflight
```

The command verifies Node.js, the Easy WebBridge runtime, its local token, and connected browsers without exposing credentials.

## Prompt

```text
Use easyAppStore to release the iOS app at /absolute/project/path to the App Store. Reuse my logged-in App Store Connect browser, build and upload the app, complete the listing and review information, submit it, and report the final status.
```

## Requirements

- Node.js 20+
- macOS and the build tools required by the iOS project
- Easy WebBridge 0.3.2+
- A Chromium browser profile already logged in to App Store Connect

## License

[MIT](LICENSE)
