<p align="center">
  <img src="./.assets/logo.png" alt="公司 Logo" width="200"/>
</p>

<h1 align="center">專案協作與開發規範指南</h1>

<p align="center">
本文件旨在提供與本組織進行技術專案協作時應遵循的標準流程、權限管理及安全規範。
</p>

---

### 目錄
- [1. 協作角色與權限](#1-協作角色與權限-collaboration-roles--permissions)
    - [1.1 工程師角色 (Engineer Role)](#11-工程師角色-engineer-role---程式碼貢獻者)
    - [1.2 專案經理角色 (PM Role)](#12-專案經理角色-pm-role---議題管理者)
- [2. GitHub 平台額度限制](#2-github-平台額度限制-github-plan-quotas--limits)
- [3. 資通安全規範](#3-資通安全規範)
- [4. 通用溝通禮儀](#4-通用溝通禮儀)
- [5. 版權宣告](#5-版權宣告)

---

## 1. 協作角色與權限 (Collaboration Roles & Permissions)

本專案協作主要分為兩種角色：**工程師 (Engineer)** 與 **專案經理 (Project Manager)**。

### 1.1 工程師角色 (Engineer Role)

#### **1.1.1 存取方式：Personal Access Token (PAT)**
- **存取媒介**：工程師將透過 GitHub **Fine-Grained Personal Access Token (PAT)** 進行程式碼的讀取與推送 (Push)。
- **Token 效期與更新**：Token 將於**隔年 3 月 1 日**統一到期失效。新的 Token 會在**每年 1 月**寄出，以確保有充足的交接與更換時間。
- **Token 權限設定**：Token 包含以下設定 Repository Permissions，有額外需求再請聯繫資訊承辦人。

| 權限 (Permission) | 存取等級 (Access Level) | 用途說明 |
| :--- | :--- |:--- |
| **Contents** | `Read and write` | 允許 push/pull 程式碼與 CI/CD 檔案 |
| **Workflows** | `Read and write` | 建立、修改、刪除 CI/CD 流程檔案 |
| **Issues** | `Read and write` | 允許回報與追蹤議題 (Issue) |
| **Pull requests** | `Read and write` | 允許提交程式碼進行審核 (PR) |
| **Actions** | `Read and write` | 管理 CI/CD 工作流程 (重跑、查看紀錄) |
| **Commit statuses** | `Read and write` | 讓 CI/CD 可以回報 Commit 的成功/失敗狀態 |
| **Deployments** | `Read and write` | 讓 CI/CD 可以記錄部署歷史 |
| **Code scanning alerts**| `Read-only` | 允許開發者查看安全性掃描報告 |


#### **1.1.2 開發流程與版本控制**
- **`.gitignore` 檔案控管**：請務必在專案根目錄建立並維護 `.gitignore` 檔案，以確保倉儲的乾淨。**嚴禁**提交 IDE 設定檔、本機環境變數檔 (`.env`)、專案依賴套件 (`node_modules/`)等檔案。
  > **提示**：您可以使用 [gitignore.io](https://www.toptal.com/developers/gitignore) 等線上工具快速產生推薦的設定檔。

- **檔案大小限制**：超過 100MB 的大型檔案請使用 **Git Large File Storage (Git LFS)** 管理。

- **分支與環境對應**：
    - `main` -> **正式環境 (Production)**
    - `develop` -> **測試環境 (Testing)**

- **開發工作流程 (Development Workflow)**：
    1. **接收 Issue**：從 PM 指派的 Issue 開始工作。
    2. **建立分支**：從 `develop` 分支出來，並遵循命名規範：`[type] / [issue#id] - [description]`
        - **範例**：`feature/issue#42-user-login-page`
        - **分支類型 `[type]` 說明**：
          <details>
            <summary><strong>點此展開查看所有分支類型的詳細說明</strong></summary>
            
          ---

          #### `feature/` (功能分支)
            - **目的**: 用於開發一個**全新的功能**。
            - **使用情境**: 開發使用者登入頁面、實作購物車功能等需要較長開發週期的任務。

          #### `bugfix/` (錯誤修復分支)
            - **目的**: 用於修復在**開發過程**中發現的、**非緊急**的錯誤。
            - **使用情境**: 修復測試環境中按鈕顏色錯誤、表單驗證邏輯錯誤等問題。

          #### `hotfix/` (緊急修復分支)
            - **目的**: 緊急修復已上線的**正式環境 (Production)** 中發現的**嚴重錯誤**。
            - **使用情境**: 修復線上支付功能失效、使用者資料外洩等需要立即處理的嚴重問題。完成後需同時合併回 `main` 和 `develop`。

          #### `release/` (發布分支)
            - **目的**: 用於準備**發布新版本**。此分支用於處理版本號更新、產出發布說明文件 (Changelog) 等最後階段的工作。
            - **使用情境**: 當 `develop` 分支已集成了所有預計要發布的功能後，建立此分支進行最後的準備。在此分支上不再增加新功能。

          #### `refactor/` (程式碼重構分支)
            - **目的**: 在**不改變既有功能**的前提下，對現有程式碼進行結構性調整、優化或清理。
            - **使用情境**: 優化資料庫查詢效能、簡化複雜的演算法、提升程式碼可讀性等。

          #### `docs/` (文件分支)
            - **目的**: 用於新增、修改或刪除**專案文件**。
            - **使用情境**: 撰寫 API 使用說明、更新 `README.md`、補充系統架構圖等。

          #### `chore/` (雜務分支)
            - **目的**: 用於處理不影響原始碼或測試的**雜項事務**。
            - **使用情境**: 更新建置腳本、調整 CI/CD 設定檔、升級開發工具等與專案維護相關的任務。

            ---
          </details>

- **提交訊息 (Commit Message)**：提交訊息應清晰、有意義，並連結對應的 Issue。
    - **格式**：`[type]: [issue#id] - [description]`
    - **範例**：
        - `feat: issue#42 - implement user login page`
        - `fix: issue#123 - correct button color on profile`
    - > **提示**：使用 `issue#` 加上編號，會自動將您的 Commit 連結到 GitHub 上的 Issue，方便追蹤。

#### **1.1.3 CI/CD 與部署說明**
- **職責劃分**：我方負責準備 AWS 環境與權限串接；廠商僅需在 CI/CD 腳本中填入我方提供的部署目標位置（如 S3 Bucket 名稱）。
- **部署流程**：`main` 分支的變更部署至正式環境；`develop` 分支的變更部署至測試環境。
- **安全須知**：廠商**無需**處理任何 AWS 金鑰 (Access Keys)，所有權限將透過 GitHub 與 AWS 的信任關係自動授予。

### 1.2 專案經理角色 (PM Role) - 議題管理者

#### **1.2.1 存取方式：GitHub 帳號**
- **權限等級**：PM 將被授予 **Triage** 權限，專注於議題管理，**無法**推送或修改程式碼。

#### **1.2.2 主要工作流程**
1.  **建立議題 (Issue Creation)**：所有工作任務都應從建立一個包含清晰標題、詳細描述的 Issue 開始。
2.  **指派任務 (Task Assignment)**：在 Issue 中透過 `Assignees` 功能指派給對應的開發者。
3.  **進度追蹤與管理**：透過 Issue 留言區溝通需求，並管理 Pull Request 的審核流程。

---

## 2. GitHub 平台額度限制 (GitHub Plan Quotas & Limits)

本組織的儲存庫根據用途設置可見性，並適用不同的免費資源額度：
- **前端儲存庫 (Front-end Repository):** 設定為 **公開 (Public)**
- **後端儲存庫 (Back-end Repository):** 設定為 **私有 (Private)**

| 功能 (Feature) | 後端 (Private Repo) | 前端 (Public Repo) |
| :--- | :--- | :--- |
| **GitHub Actions** | **2,000** 分鐘 / 月 | **3,000** 分鐘 / 月 |
| **GitHub Packages** | **500 MB** 儲存<br>**1 GB** 傳輸 / 月 | **1 GB** 儲存<br>**無限** 傳輸 / 月 |
| **Git LFS**¹ | **1 GB** 儲存<br>**1 GB** 傳輸 / 月 | **1 GB** 儲存<br>**1 GB** 傳輸 / 月 |
| **儲存庫容量** | 建議 < 5GB (硬上限 100GB) | 建議 < 5GB (硬上限 100GB) |

> **¹共用額度說明 (Shared Quota Notice)**：
> 請特別注意，**GitHub Actions, GitHub Packages, 和 Git LFS** 這三項服務的免費額度，都是以**整個 GitHub 組織 (Organization) 為單位進行計算的**，而非為單一儲存庫提供獨立額度。任一專案的用量增加，都會消耗所有專案可用的共同額度。

> **費用與用量評估請求**：
> 若貴公司評估有任何可能會超過上述免費額度的需求，**請務必提前告知我方專案負責人**，以便我們能及時評估升級方案，避免因額度耗盡而中斷開發流程。

---

## 3. 資通安全規範

**此為最高優先級規範。** 所有在本儲存庫的協作行為，皆須嚴格遵守**國家表演藝術中心國家兩廳院資通安全管理制度**。

- **制度文件網址**：[https://isms.npac-ntch.org/](https://isms.npac-ntch.org/)

請確保所有交付項目與操作流程均符合該制度要求。

---

## 4. 通用溝通禮儀
- **保持專業**：在所有溝通中保持專業、尊重的態度。
- **具體明確**：無論是描述問題還是提出建議，請盡可能具體明確。
- **善用 `@`**：若需要特定人員注意，請使用 `@username` 來提及該位協作者。

感謝您的合作！

---
## 5. 版權宣告
© 2025 國家表演藝術中心國家兩廳院。版權所有，保留一切權利。
Copyright © 2025 National Performing Arts Center - National Theater and Concert Hall. All Rights Reserved.