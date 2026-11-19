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

### 为什么无法仅凭公开源码重新编译出 5.8.9 原版

5.8.9（2026-11-18）依赖 FongMi 自行深度定制的 Media3，其中包含
`DecoderMode`、`DiscMenuController`、`DiscMenuState`、`SubtitleContent`、
`SubtitleOffsets`、`VideoTransform`、`VideoAdjustment`、`BdjRuntime`、
`FfmpegMotionInterpolationController` 等 22 个私有类。

经核查：

- 公开仓库 [FongMi/media](https://github.com/FongMi/media) 只有两个分支：
  `release`（纯上游 Media3 1.11.x）和 `release-1.11.0-fongmi`（定制分支，
  最后提交 **2026-08-12**）。后者只含 `MediaChapter`/`MediaEdition` 等**部分**
  定制类，上述 22 个类一个都没有；
- `FongMi/Release` 只发布 APK 与运行时组件，没有发布定制 AAR；
- 全 GitHub 代码搜索只能在这些类的**使用方**（FongMi/TV）找到它们，
  没有任何公开仓库提供定义源码或 aar；
- 官方 APK 中的这些类名已被 R8 混淆/内联，无法反编译还原成可编译的源码。

实测结果（`build-from-source.yml`，run `44574617664`）：

1. 用 `release-1.11.0-fongmi` 编译 20 个定制 AAR ✅ 成功；
2. 用官方运行时 APK 与官方 5.8.9 APK 中的 native 库替换旧的 split-ffmpeg 库，
   通过 `verify*MpvNative` 校验 ✅ 成功；
3. 编译 TV 源码时因上述 22 个私有类缺失失败 ❌（302 处 `cannot find symbol`）。

**结论：原版 APK 可以在本仓库 Release 中原样获取，但「用公开源码自行构建出
5.8.9 原版」客观上不可行**——作者没有公开与 5.8.9 匹配的 Media3 定制源码/产物。
手写桩类虽然能过编译，但播放器核心功能会失效，不是「一模一样」。

### 推荐做法：同步官方原版 APK

使用 `Sync official FongMi TV release APK` 工作流（Actions → 运行），
填入版本号（默认 `5.8.9`），即可把官方 release 的 4 个原版 APK
**字节级原样**下载并发布到本仓库的 Release，附带 SHA256 校验。
官方发布新版时，改一下版本号再运行即可完成更新。这是获得「一模一样原版 APK」
且可持续更新的可靠方式。

### 许可

源码遵循上游 FongMi/TV 的 GPL-3.0 许可，详见 `LICENSE.md`。
