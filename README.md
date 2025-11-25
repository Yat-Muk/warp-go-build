-----


# warp-go-build: Automated Binary Mirror & Supply Chain

[![Build warp-go (v1.0.8)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/release.yml/badge.svg)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/release.yml)
[![Mirror WGCF (Auto-Latest)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/mirror_wgcf.yml/badge.svg)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/mirror_wgcf.yml)
[![Mirror Tools (Weekly)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/mirror_tools.yml/badge.svg)](https://github.com/Yat-Muk/warp-go-build/actions/workflows/mirror_tools.yml)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/Yat-Muk/warp-go-build)](https://github.com/Yat-Muk/warp-go-build/releases)

**Enterprise-grade, self-hosted supply chain for Cloudflare WARP tools.**
**企業級 Cloudflare WARP 工具鏈自動化分發鏡像。**

本倉庫是一個全自動化的二進制文件供應鏈節點。它利用 GitHub Actions 定時從各個上游（Official/Stable Sources）自動同步、校驗、重命名並重新發布核心組件，旨在為自動化腳本提供一個**穩定**、**高速**、**統一**且**可驗證**的下載源。

---

## 📦 核心組件 (Core Components)

所有文件均託管於 [GitHub Releases](https://github.com/Yat-Muk/warp-go-build/releases)，支持全球 CDN 加速。

### 1. WARP 內核 (Kernels)

| 組件 | 描述 | 版本策略 | 架構 | 下載鏈接 |
| :--- | :--- | :--- | :--- | :--- |
| **warp-go** | 輕量級 WARP 客戶端 (Go語言) | **鎖定 v1.0.8** (穩定版) | AMD64 | [下載 warp-go (amd64)](https://github.com/Yat-Muk/warp-go-build/releases/download/v1.0.8/warp-go_linux_amd64) |
| | | | ARM64 | [下載 warp-go (arm64)](https://github.com/Yat-Muk/warp-go-build/releases/download/v1.0.8/warp-go_linux_arm64) |
| **wgcf** | WireGuard 帳戶註冊工具 (官方版) | **自動追蹤最新** (Latest) | AMD64 | [下載 wgcf (amd64)](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64) |
| | | | ARM64 | [下載 wgcf (arm64)](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_arm64) |

### 2. 輔助工具 (Utilities)

這些工具統一發布在 `tools-latest` 標籤下，每週自動同步。

| 工具名稱 | 描述 | 架構 | 下載鏈接 |
| :--- | :--- | :--- | :--- |
| **warp-plus** | WARP+ 流量刷取/生成工具 | AMD64 | [下載 warp-plus (amd64)](https://github.com/Yat-Muk/warp-go-build/releases/download/tools-latest/warp_plus_linux_amd64) |
| | | ARM64 | [下載 warp-plus (arm64)](https://github.com/Yat-Muk/warp-go-build/releases/download/tools-latest/warp_plus_linux_arm64) |
| **warp_endpoint** | Cloudflare Endpoint 優選腳本 | 通用 | [下載 warp_endpoint](https://github.com/Yat-Muk/warp-go-build/releases/download/tools-latest/warp_endpoint) |

---

## 🛡️ 安全與驗證 (Security)

為了防止供應鏈攻擊或文件傳輸損壞，本倉庫為所有發布的文件提供 **SHA256 校驗和**。

**如何驗證下載的文件：**

```bash
# 1. 下載文件與校驗單 (以 wgcf 為例)
wget -O wgcf [https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64)
wget -O wgcf.sha256 [https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64.sha256](https://github.com/Yat-Muk/warp-go-build/releases/latest/download/wgcf_linux_amd64.sha256)
```
# 2. 進行比對
# 方法 A: 自動比對 (如果格式支持)
sha256sum -c wgcf.sha256

# 方法 B: 手動比對
echo "預期 Hash: $(cat wgcf.sha256)"
echo "實際 Hash: $(sha256sum wgcf | awk '{print $1}')"


-----

## 👨‍💻 給腳本開發者 (Integration Guide)

如果您正在維護類似 `CFwarp.sh` 的自動化腳本，建議使用本倉庫作為依賴源，以獲得標準化的文件名和穩定的版本控制。

**一鍵下載示例 (Auto Detect Arch):**

```bash
# 定義基礎 URL
REPO="[https://github.com/Yat-Muk/warp-go-build/releases](https://github.com/Yat-Muk/warp-go-build/releases)"
ARCH=$(uname -m)

# 判斷架構
if [[ "$ARCH" == "x86_64" ]]; then
    SUFFIX="amd64"
elif [[ "$ARCH" == "aarch64" ]]; then
    SUFFIX="arm64"
else
    echo "不支持的架構"; exit 1
fi

# 下載 WARP-GO (穩定版)
wget -O warp-go "$REPO/download/v1.0.8/warp-go_linux_${SUFFIX}"

# 下載 WGCF (最新版)
wget -O wgcf "$REPO/latest/download/wgcf_linux_${SUFFIX}"

# 下載 WARP+ 工具
wget -O warp-plus "$REPO/download/tools-latest/warp_plus_linux_${SUFFIX}"

chmod +x warp-go wgcf warp-plus
```

-----

## 🤖 自動化機制 (Automation Logic)

本倉庫通過 GitHub Actions 實現全無人值守維護：

1.  **Mirror Binaries (warp-go)**:
      * 手動或 Tag 觸發。從 `Fangliding` 鏡像拉取 `v1.0.8` 穩定版，計算 Hash 並發布。
2.  **Mirror WGCF**:
      * **每週一 02:00 UTC 自動運行**。調用 GitHub API 查詢 `ViRb3/wgcf` 最新版，自動下載、重命名為通用格式 (`wgcf_linux_amd64` 等) 並發布到 Latest。
3.  **Mirror Tools**:
      * **每週一 03:00 UTC 自動運行**。自動從上游腳本倉庫拉取 `cf.sh` (優選 IP) 和 `warp-plus` 二進制文件，統一重命名並發布到 `tools-latest` 標籤。

-----

## 🔗 致謝與聲明 (Credits & Disclaimer)

本項目僅作為 **二進制文件分發鏡像 (Binary Mirror)**，不擁有核心代碼的版權，也不修改原程序的邏輯。

**上游項目致謝：**

  * **Warp-Go Core**: [ProjectWARP/warp-go](https://gitlab.com/ProjectWARP/warp-go) & [Fangliding](https://github.com/Fangliding/warp-go)
  * **WGCF Core**: [ViRb3/wgcf](https://github.com/ViRb3/wgcf)
  * **IP Scanner**: [badafans/better-cloudflare-ip](https://github.com/badafans/better-cloudflare-ip)
  * **WARP+ Gen**: [rwkgyg/CFwarp](https://gitlab.com/rwkgyg/CFwarp)

**免責聲明：**
使用者應自行承擔使用這些工具的風險。本倉庫與 Cloudflare Inc. 無關。

-----

[MIT License](https://www.google.com/search?q=LICENSE) | Copyright (c) 2025 Yat-Muk

