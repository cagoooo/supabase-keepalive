# Supabase Free Tier Keepalive

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
