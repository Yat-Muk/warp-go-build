-----

# warp-go-build: Automated Binary Distribution

[![Mirror Binaries (Sync v1.0.8)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/release.yml/badge.svg)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/release.yml)
[![Mirror WGCF (Auto-Latest)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/mirror_wgcf.yml/badge.svg)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/mirror_wgcf.yml)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/Yat-Muk/warp-go-build)](https://github.com/Yat-Muk/warp-go-build/releases)

**Enterprise-grade distribution mirror for Cloudflare WARP clients.**
**企業級 Cloudflare WARP 客戶端自動分發鏡像。**

本倉庫是一個全自動化的二進制文件供應鏈節點。它利用 GitHub Actions 自動追蹤上游官方/穩定源，下載、校驗、標準化並重新發布 `warp-go` 和 `wgcf` 執行檔。

---

## 📦 組件列表 (Components)

本倉庫同時維護兩個核心組件，所有文件均包含 **SHA256 校驗**。

| 組件名稱 | 核心描述 | 上游來源 (Upstream) | 更新策略 |
| :--- | :--- | :--- | :--- |
| **warp-go** | 輕量級 WARP 客戶端 (Go語言) | [Fangliding/warp-go](https://github.com/Fangliding/warp-go) | **鎖定穩定版 (v1.0.8)** |
| **wgcf** | WireGuard 帳戶註冊工具 | [ViRb3/wgcf](https://github.com/ViRb3/wgcf) | **每週自動同步最新版** |

---

## 📥 下載地址 (Downloads)

您可以直接從 [Releases 頁面](https://github.com/Yat-Muk/warp-go-build/releases) 獲取文件，或使用以下永久鏈接：

### 1. WARP-GO (v1.0.8 Stable)

| 架構 (Arch) | 下載鏈接 (Direct Link) |
| :--- | :--- |
| **Linux AMD64** | [`https://github.com/Yat-Muk/warp-go-build/releases/download/v1.0.8/warp-go_linux_amd64`](https://github.com/Yat-Muk/warp-go-build/releases/download/v1.0.8/warp-go_linux_amd64) |
| **Linux ARM64** | [`https://github.com/Yat-Muk/warp-go-build/releases/download/v1.0.8/warp-go_linux_arm64`](https://github.com/Yat-Muk/warp-go-build/releases/download/v1.0.8/warp-go_linux_arm64) |

### 2. WGCF (Latest Auto-Sync)

*注意：WGCF 文件名已標準化，不帶版本號，方便腳本調用。*

| 架構 (Arch) | 下載鏈接 (Direct Link) |
| :--- | :--- |
| **Linux AMD64** | [`https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64`](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64) |
| **Linux ARM64** | [`https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_arm64`](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_arm64) |

---

## 🛡️ 安全驗證 (Security & Integrity)

為了防止供應鏈攻擊或傳輸損壞，**強烈建議**在運行前校驗文件哈希值。

**校驗示例 (以 wgcf 為例)：**

# 1. 下載二進制文件

```bash
wget -O wgcf [https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64)
```
# 2. 下載對應的校驗文件

```bash
wget -O wgcf.sha256 [https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64.sha256](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64.sha256)
```
# 3. 進行比對 (SHA256)
# 手動比對：

```bash
cat wgcf.sha256
sha256sum wgcf
```
# 或自動比對：

```bash
echo "$(cat wgcf.sha256)  wgcf" | sha256sum -c -
```

-----

## 🤖 自動化機制 (Automation Workflows)

本倉庫的維護完全無人值守，確保了構建過程的透明性與可追溯性。

1.  **Mirror Binaries (Sync v1.0.8)**:
      * 手動觸發或標籤觸發。
      * 從 GitHub Mirror 拉取 `warp-go` 已編譯文件，確保版本與主流腳本兼容。
2.  **Mirror WGCF (Auto-Latest)**:
      * **每週一凌晨 02:00 (UTC) 自動運行**。
      * 調用 GitHub API 查詢 `ViRb3/wgcf` 的最新 Release。
      * 自動下載、重命名為通用格式 (`wgcf_linux_amd64`) 並發布到 Latest Release。

-----

## 👨‍💻 開發者集成 (For Developers)

如果您正在維護 `CFwarp` 類腳本，可以使用以下代碼片段自動獲取本倉庫的資源：

# 定義倉庫基礎 URL

```bash
REPO="[https://github.com/Yat-Muk/warp-go-build/releases](https://github.com/Yat-Muk/warp-go-build/releases)"
```

# 下載 WGCF (始終最新)

```bash
wget -O wgcf "$REPO/latest/download/wgcf_linux_amd64"
```

# 下載 WARP-GO (鎖定版本)

```bash
wget -O warp-go "$REPO/download/v1.0.8/warp-go_linux_amd64"
```

-----

## ⚠️ 免責聲明 (Disclaimer)

  * 本項目僅為 **二進制文件分發鏡像 (Mirror)**，不涉及源代碼修改。
  * 所有程序版權歸原作者所有。
  * 使用本倉庫文件所產生的任何後果由使用者自行承擔。

**Upstream Credits:**

  * [ProjectWARP/warp-go](https://gitlab.com/ProjectWARP/warp-go)
  * [ViRb3/wgcf](https://github.com/ViRb3/wgcf)

-----

**License**
MIT License. See [LICENSE](https://www.google.com/search?q=LICENSE) file for details.
