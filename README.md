# warp-go-build: Automated Binary Mirror

[![Build and Mirror](https://github.com/Yat-Muk/warp-go-build/actions/workflows/release.yml/badge.svg)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/release.yml)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/Yat-Muk/warp-go-build)](https://github.com/Yat-Muk/warp-go-build/releases)

這是一個全自動化的 `warp-go` 二進制文件分發倉庫。
本倉庫利用 GitHub Actions 自動從上游穩定源同步、校驗並重新打包發布 `warp-go` 執行檔，旨在為腳本開發者和終端用戶提供一個**穩定**、**高速**且**可驗證**的下載源。

This is an automated mirror repository for `warp-go` binaries. It utilizes GitHub Actions to sync, verify, repackage, and release binaries from upstream stable sources, providing a secure and fast download endpoint.

---

## 📥 下載與安裝 (Downloads)

所有二進制文件均託管於 [GitHub Releases](https://github.com/Yat-Muk/warp-go-build/releases) 頁面，支持全球 CDN 加速。

| 架構 (Arch) | 說明 (Description) | 下載鏈接 (Link) |
| :--- | :--- | :--- |
| **Linux AMD64** | 常見的 Intel/AMD VPS | [最新版本下載](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/warp-go_linux_amd64) |
| **Linux ARM64** | ARM 架構 (如 Oracle Cloud ARM) | [最新版本下載](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/warp-go_linux_arm64) |

---

## 🛡️ 安全校驗 (Verification)

為了確保供應鏈安全，每次發布均包含 SHA256 校驗文件。建議在下載後進行比對。

**驗證步驟：**

```bash
# 1. 下載執行檔與校驗文件
wget [https://github.com/Yat-Muk/warp-go-build/releases/latest/download/warp-go_linux_amd64](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/warp-go_linux_amd64)
wget [https://github.com/Yat-Muk/warp-go-build/releases/latest/download/warp-go_linux_amd64.sha256](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/warp-go_linux_amd64.sha256)

# 2. 進行比對 (輸出 OK 即為安全)
sha256sum -c warp-go_linux_amd64.sha256
```
## 🔗 上游致謝 (Credits)
本項目僅作為分發鏡像，核心代碼歸原作者所有。

Core Logic: [ProjectWARP/warp-go](https://gitlab.com/ProjectWARP/warp-go)

Binary Source: [Fangliding/warp-go](https://github.com/Fangliding/warp-go)

## ⚠️ 免責聲明 (Disclaimer)
本倉庫提供的二進制文件來源於開源社區。使用者應自行承擔使用風險。本倉庫與 Cloudflare Inc. 無關。

