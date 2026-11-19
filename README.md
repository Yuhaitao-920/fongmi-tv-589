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

### 从源码编译 5.8.9（已成功）

`build-from-source.yml` 已端到端跑通（run `44587755785`，全部 16 步 success），
真正从公开源码编译出 5.8.9：

1. **Media3 依赖**：官方 `FongMi/media` 未公开与 5.8.9 匹配的定制 Media3；
   使用公开 fork `Diaojiangjun/media@release-1.11.0-fongmi`（2026-11-19 的
   `Add FongMi TV playback APIs and unified FFmpeg natives` 提交补齐了全部私有类
   `DecoderMode`/`DiscMenu*`/`Subtitle*`/`MpvPlayer.VideoOutputOption`/
   `FfmpegMotionInterpolationController` 等，并自带统一 FFmpeg/mpv native）。
   另打一个小补丁给 `PlayerView` 补 `getVideoBounds()`（取自 `dlgt7/media` fork）。
2. **AAR 装配**：编译产物严格按官方 `move.txt` 只取 20 个 `lib-*-release.aar`
   放进 `app/libs`（不引入 GMS 依赖的 IMA 模块）。
3. **编译**：`FongMi/TV@fongmi`（versionCode 589）+ 上述 AAR →
   `./gradlew assembleRelease`，LEANBACK 与 MOBILE、arm64-v8a 与 armeabi-v7a
   四个变体全部产出。

产物发布在 Release `from-source-5.8.9-8`（4 个 APK + SHA256SUMS.txt）。
编译产物为**自签名**，功能与官方 5.8.9 一致，但**不是字节级相同**——
若要字节级一致的官方原版，请用 `official-5.8.9` Release（由
`sync-official-apk.yml` 从 `FongMi/Release` 同步）。

手动重编译：Actions → `Build FongMi TV 5.8.9 fully from source` → Run workflow。
