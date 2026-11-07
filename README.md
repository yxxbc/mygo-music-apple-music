# 蹲一个 Apple Music 插件 · MyGo Music

> **状态：需求收集中，还没有可安装的插件。**
> 这个仓库不是插件：没有 `plugin.json`，没有 Release，也没有安装链接。安装器不会把它当成插件。

## 为什么单独开这个仓库

MyGo Music 的插件是“音源”：宿主向插件要曲目、歌词和播放 URL，然后由宿主自己的播放器解码、输出。Apple Music 的音频是 **FairPlay DRM**，必须通过 MusicKit / AVPlayer 和 Apple 的授权播放；插件 API 拿不到可播放的音频流，宿主也解不了。

所以这里先把“能做什么、不能做什么”讲清楚，再收集需求，避免做一个装了不能用的假插件。

## 能做到的范围

### A. macOS Music.app 遥控（可做，但有边界）

用一个 `playback: "remote"` 的 MyGo Music 插件，通过 `host.spawn("/usr/bin/osascript")` 控制官方 Music.app：

- 浏览 Music.app 里已有的资料库、播放列表；
- 搜索**用户资料库**，不是 Apple Music 全曲库；
- 播放 / 暂停 / 上一首 / 下一首 / 进度 / 音量；
- 读取当前曲目和播放状态；
- 音频由 Music.app 输出，Apple Music 的无损/高解析由 Music.app 自己决定。

限制：

- 仅 macOS；
- 需要 `process` 权限和系统“自动化”授权；
- 依赖用户已登录 Music.app，且资料库里有歌；
- **不能搜 Apple Music 全曲库**，不能在 MyGo 的队列/EQ/缓存里处理音频；
- 不是 Apple Music 音源，只是遥控器。

### B. 在 MyGo Music 内直接播放 Apple Music 全曲/无损

**不可行。** FairPlay DRM、MusicKit 授权和音频解密不在插件 API 的能力范围内。任何声称做到的方案，要么是 30 秒试听，要么是遥控 Music.app，要么绕过 DRM——最后一种不做。

## 如果你想要

- 用 [Issue 模板](.github/ISSUE_TEMPLATE/apple-music-request.yml) 开一个需求，写清楚使用场景；
- 在已有需求 issue 上点 👍，不要重复开空 issue；
- 说明你的 macOS 版本、Music.app 资料库规模、是否接受“只能遥控、不能搜全曲库”。

## 立项条件

- 有足够多的真实需求（同一个 issue 的 👍 + 具体使用场景）；
- 接受“macOS 遥控”而不是“全曲库音源”；
- 能接受 `process` 权限和自动化授权。

达到条件后再新建插件仓库、写代码和 Release；在那之前这个仓库只做需求与范围说明。

## 替代方案

- **本地文件 + 自动刮削**：不依赖任何商业音源，能补标签、封面、歌词；适合音质优先。
- 其他 MyGo Music 音源插件：Spotify、YouTube Music 等，各自也有 API/DRM 边界。
- Apple Music 官方 App：目前唯一能完整播放 Apple Music 的地方。

## 不做什么

- 不绕过 DRM，不提取 Apple Music 音频；
- 不发布没有实际功能的“占位插件”；
- 不把 Apple Music 的商标/服务说成官方合作。

MyGo Music: https://github.com/egoist/mygo-music

Apple Music 名称与服务归 Apple 所有；本项目不是 Apple 或 MyGo Music 的官方项目。
