# Supabase Free Tier Keepalive

> 🗂️ **一次性專案**：Supabase 保活排程腳本，非對外應用，內容不再變動，因此不做版本管理。

**避免 Supabase 免費版專案 7 天沒活動自動暫停。**

GitHub Actions cron 每 5 天打一次 Supabase REST API，讓專案保持 ACTIVE 狀態。完全免費、雲端跑、不需開機。

## 目前在 keepalive 名單上

| 專案 | Project Ref | 用途 |
| :--- | :--- | :--- |
| speech-training | `ybxfsmrawdcisylwjger` | [國語演說訓練平台](https://github.com/cagoooo/speech) |
| smes-inventory | `xcnmmaayrtiklntvhdhc` | smes 庫存系統 |

## 排程

每 5 天 UTC 03:00（台灣時間 11:00）執行一次 — `cron: '0 3 */5 * *'`

> 5 天間隔說明：Supabase 免費版 7 天沒活動暫停，5 天 + 2 天安全餘量，cron 偶爾失敗下次也來得及。

## 新增專案

1. 從 Supabase MCP 或 Dashboard 拿該專案的 publishable key（`sb_publishable_...`）
2. 用 stdin pipe 設成 GitHub Secret，**不要直接複製貼上**：
   ```bash
   printf '%s' 'sb_publishable_xxxxx' | gh secret set SUPABASE_KEY_<NICKNAME> --repo cagoooo/supabase-keepalive
   ```
3. 編輯 `.github/workflows/keepalive.yml` 的 `matrix.project` 加一條：
   ```yaml
   - name: '<project-name>'
     ref: '<project-ref>'
     secret_name: 'SUPABASE_KEY_<NICKNAME>'
   ```
4. push、用 `gh workflow run keepalive.yml` 手動觸發測試

## 手動觸發

```bash
gh workflow run keepalive.yml --repo cagoooo/supabase-keepalive
gh run watch --repo cagoooo/supabase-keepalive
```

## 為什麼選這個方案

- ❌ **Supabase pg_cron 自我 ping**：專案被暫停時 pg_cron 也停了，無法自我喚醒
- ❌ **個人電腦排程**：要開機、容易忘記
- ❌ **每個 App repo 各放一個 workflow**：分散難管理
- ✅ **集中 GitHub Actions cron**：免費、雲端、新增專案改一個檔

完整原理與踩雷見 `~/.claude/skills/supabase-free-tier-keepalive/SKILL.md`。

---

Made with ❤️ by [阿凱老師](https://www.smes.tyc.edu.tw/)

---

<!-- BEGIN:PROJECT_GUIDE -->
## 專案導覽

Cron-ping Supabase free-tier projects to prevent 7-day auto-pause

- 專案定位：實用工具／自動化原型
- Repository：`cagoooo/supabase-keepalive`
- 可見性：公開
- 主要技術：請依根目錄檔案與部署設定判斷
- 線上入口：未在 GitHub repository metadata 設定

### 可以怎麼應用

- 解決特定工作流程中的重複操作或資訊整理需求
- 作為相近工具的功能原型與程式碼參考
- 串接新的資料來源、服務或介面後延伸到其他情境

這些是依目前專案定位整理的延伸方向，不代表所有情境都已內建完成；實作前請先確認現有功能與資料格式。

### 技術與專案結構

- `README.md`

檔案結構會隨版本演進；若本節與程式碼不一致，以目前預設分支的原始碼為準。

### 本機執行

請先閱讀根目錄設定檔與原始碼入口，再依專案所使用的語言／平台建立環境。此 repo 未提供可安全推定的通用啟動指令。

### 給 AI Agent 的接手指南

1. 先閱讀本 README、`AGENTS.md`（若有）、套件腳本與部署設定。
2. 先從入口檔、設定檔與資料流確認真實行為，不要只依 repo 名稱推測。
3. 修改前檢查環境變數、外部服務、檔案格式與失敗處理。
4. 完成後執行既有檢查，並以最小可重現案例驗證主要流程。
5. 不要捏造尚未存在的功能；README 與實作有落差時，應同時更新文件。
6. 提交前只納入本次任務檔案，並記錄實際執行過的驗證。

### 安全與資料注意事項

- 不要提交 `.env`、服務帳號、API 金鑰、token、學生個資或正式環境匯出資料。
- 使用 Firebase、Supabase、Google API 或其他雲端服務時，請建立自己的測試專案並套用最小權限。
- 若要公開衍生作品，請先確認程式碼、圖片、音訊、字型與教材內容的授權。

### 貢獻與客製化

歡迎依教學現場、活動或工作流程需求進行 fork／客製化。建議在變更說明中交代使用情境、主要修改、測試方式，以及是否影響資料格式或部署設定。
<!-- END:PROJECT_GUIDE -->
