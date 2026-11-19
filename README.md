# 影視TV

Android TV、手機、平板都能用，支援本地檔案與網址播放。

[使用指南](https://fongmi.github.io/TV) · [討論群組](https://t.me/fongmi_official)

## 主要功能

- **瀏覽**：分類篩選、跨站搜尋、收藏、播放記錄與無痕模式。
- **播放**：倍速、字幕、彈幕、片段跳過、畫中畫與背景音訊。
- **直播**：分組選台、節目表、頻道收藏與線路切換。
- **操作**：電視遙控器、手機手勢、DLNA 投放與接收。
- **互通**：網頁遙控、檔案傳輸、記錄同步與 Android Auto。

爬蟲開發說明與 Java、Python、QuickJS 範例見 [CatVodSpider](https://github.com/FongMi/CatVodSpider)。

## Star History

<a href="https://www.star-history.com/?repos=FongMi%2FTV&amp;type=date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=FongMi/TV&amp;type=date&amp;theme=dark" />
    <img alt="FongMi/TV Star History" src="https://api.star-history.com/chart?repos=FongMi/TV&amp;type=date" />
  </picture>
</a>


---

## 关于本仓库（复刻说明）

本仓库保存的是 **FongMi/TV 官方 `fongmi` 分支的完整源码**，版本为 **5.8.9（versionCode 589）**，
对应官方 commit `5bf18d6ae8b0`。源码与官方完全一致，仅额外附带 `third_party/maven`
（来自 webhtv 的定制 Media3 依赖，见下）。

### 关于「一模一样的原版 APK」

你手上的 `mobile-arm64_v8a (1).apk.1` **就是官方原版本身**：它与
[FongMi/Release 的 5.8.9 发布](https://github.com/FongMi/Release/releases/tag/5.8.9)
中 `mobile-arm64_v8a.apk` 大小精确到字节一致（57,672,761 字节），
官方 SHA256 为 `b9f76788103c25d1f2b1488bbda3ec52ebc31fb73a4a6e3a92329492ec12b1fd`。

### 为什么无法从公开源码重新编译出完全一致的 APK

5.8.9 源码依赖 FongMi 自行 fork 并深度定制的 **Media3 1.11.1**，其中含
`MediaChapter`、`DecoderMode`、`DolbyVisionOutputPolicy`、`DiscMenuController`、
`SubtitleContent` 等大量私有类。这些类：

- 不在 Google 官方 Media3 中；
- 不在公开的 [FongMi/media](https://github.com/FongMi/media) fork（最新仅 1.11.0）中；
- 不在任何公开仓库以源码或 aar 形式提供。

因此仅凭公开源码与依赖，**无法重新编译出与官方 5.8.9 一致的可运行 APK**。
本仓库的 `build-apk.yml` 可完成环境准备与编译尝试，但会在这些缺失类处停止。

### 推荐做法：同步官方原版 APK

使用 `Sync official FongMi TV release APK` 工作流（Actions → 运行），
填入版本号（默认 `5.8.9`），即可把官方 release 的 4 个原版 APK
**字节级原样**下载并发布到本仓库的 Release，附带 SHA256 校验。
官方发布新版时，改一下版本号再运行即可完成更新。这是获得「一模一样原版 APK」
且可持续更新的可靠方式。

### 许可

源码遵循上游 FongMi/TV 的 GPL-3.0 许可，详见 `LICENSE.md`。
