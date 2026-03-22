# 🎓 台灣學生免費資源追蹤器

[English](README.en.md)

一套 Claude Code skill + 跨平台提醒系統，幫助台灣大專院校學生（`.edu.tw`）發現、追蹤並領取 **37+ 項免費訂閱服務**，總價值超過 **$4,400 美元/年**。

## 包含哪些福利？

| 分類 | 範例 | 數量 |
|------|------|------|
| GitHub Student Pack | Copilot、DigitalOcean $200、Frontend Masters | 12 |
| 開發工具 & IDE | JetBrains 全家桶、Cursor Pro、Postman | 3 |
| 雲端服務 | Azure $100、AWS、Google Cloud $300、Oracle | 5 |
| 資料庫 | Supabase、Neon | 2 |
| 設計工具 | Figma Professional、Autodesk、Miro | 3 |
| 生產力工具 | Notion Plus、Microsoft 365、Obsidian | 3 |
| 學習平台 & 認證 | Coursera、Kaggle、NVIDIA DLI、IBM | 5+ |
| 影音串流 | Spotify、Apple Music、YouTube Premium | 3 |
| AI 工具 | Perplexity Pro | 1 |
| **合計** | | **37+ 項** |

## 快速開始

### 方式一：搭配 Claude Code 使用

1. Clone 這個 repo 並連結到 Claude Code 的 skills 目錄：

   ```bash
   git clone https://github.com/bingo-taiwan/taiwan-student-benefits.git
   ```

   **Windows（以系統管理員開啟 PowerShell）：**
   ```powershell
   New-Item -ItemType SymbolicLink `
     -Path "$env:USERPROFILE\.claude\skills\taiwan-student-benefits" `
     -Target "你的路徑\taiwan-student-benefits"
   ```

   **macOS / Linux：**
   ```bash
   ln -s /你的路徑/taiwan-student-benefits ~/.claude/skills/taiwan-student-benefits
   ```

2. 在 Claude Code 中直接說：
   - 「幫我設定學生福利追蹤」
   - 「我有 .edu.tw 信箱，有哪些免費訂閱可以申請？」
   - 「設定學生福利提醒」

### 方式二：獨立使用（不需要 Claude Code）

1. 複製追蹤模板：
   ```bash
   cp tracker_template.json student_benefits_tracker.json
   ```

2. 編輯 `student_benefits_tracker.json`：填入你的 `.edu.tw` 信箱，移除不需要的項目

3. 執行提醒腳本：

   **Windows：**
   ```powershell
   powershell -ExecutionPolicy Bypass -File scripts/check_benefits.ps1
   ```

   **macOS / Linux：**
   ```bash
   bash scripts/check_benefits.sh
   ```

## 設定排程提醒

### Windows（工作排程器）

```powershell
powershell -ExecutionPolicy Bypass -File scripts/setup_schedule.ps1
```

會建立：每天早上 9:00 提醒 + 每週一更新追蹤狀態。

### macOS（launchd）

```bash
bash scripts/setup_schedule_macos.sh
```

會建立：每天早上 9:00 的 launchd 排程。

### Linux（cron）

```bash
chmod +x scripts/check_benefits.sh
(crontab -l 2>/dev/null; echo "0 9 * * * $(pwd)/scripts/check_benefits.sh") | crontab -
```

## 運作方式

```
tracker_template.json          ← 37 項福利的完整模板
        │
        ▼
student_benefits_tracker.json  ← 你的個人追蹤檔（記錄進度）
        │
        ▼
check_benefits.ps1 / .sh      ← 讀取追蹤檔，顯示待辦項目
        │
        ▼
reminder.log                   ← 提醒紀錄（持續累加）
        +
桌面通知                        ← Windows 氣泡 / macOS / Linux
```

### 狀態值說明

| 狀態 | 說明 |
|------|------|
| `pending` | 尚未申請 |
| `done` | 已成功領取（請填入 `done_date`） |
| `skipped` | 刻意跳過（不需要） |
| `expired` | 已過期，需要續約 |

## 建議申請順序

### 第 1 週：基礎建設（不需要 Pack）

| 優先順序 | 服務 | 價值 | 說明 |
|---------|------|------|------|
| 1 | GitHub Student Pack | 門票 | 申請後可解鎖 12+ 項服務 |
| 2 | JetBrains 全家桶 | ~$250/年 | `.edu.tw` 信箱直接申請，秒過 |
| 3 | Microsoft Azure | $100 | 不需信用卡 |
| 4 | Figma Education | ~$144/年 x 2年 | 設計相關必備 |
| 5 | Notion Plus | ~$96/年 | 生產力即刻提升 |

### 第 2 週：Pack 審核通過後

| 優先順序 | 服務 | 價值 |
|---------|------|------|
| 6 | GitHub Copilot | ~$120/年（自動啟用） |
| 7 | DigitalOcean | $200 額度 |
| 8 | Frontend Masters | 6 個月（~$234） |
| 9 | 1Password | 1 年（~$36） |
| 10 | MongoDB Atlas | $50 額度 |

### 第 3 週之後：其餘項目

- 雲端額度：AWS Educate、Google Cloud、Oracle Free Tier
- 學習平台：Coursera 財務補助、Kaggle、NVIDIA DLI
- 影音折扣：Spotify、Apple Music、YouTube Premium 學生方案

## 帳號類型速查表

> 類型定義：
> - **A 類**：可用個人信箱，另外驗證學生身份（SheerID 等）
> - **B 類**：必須用 `.edu.tw` 信箱
> - **C 類**：透過 GitHub Pack 兌換（可用個人信箱）
> - **D 類**：透過學校入口

| 服務名稱 | 帳號類型 | 需要 .edu.tw 信箱？ | .edu.tw 失效後影響 | 備註 |
|----------|---------|-------------------|-------------------|------|
| GitHub Copilot | C | 否（加入 GitHub 帳號即可） | 可保留（綁 GitHub 帳號） | Pack 核准後自動啟用 |
| GitHub Pro | C | 否 | 可保留 | Pack 核准後自動啟用 |
| GitKraken Pro | C | 否 | 可保留 | 透過 GitHub Education 兌換 |
| Educative | C | 否 | 可保留 | 透過 GitHub Education 兌換 |
| Frontend Masters | C | 否 | 可保留 | 6 個月效期，透過 GitHub Education 兌換 |
| 1Password | C | 否 | 可保留 | 1 年效期，連結 GitHub 帳號 |
| DigitalOcean | C | 否 | 可保留 | $200 額度，需信用卡 |
| MongoDB Atlas | C | 否 | 可保留 | $50 額度，Pack 核准後 90 天內兌換 |
| Namecheap (.me) | C | 否 | 可保留 | `.edu.tw` 可能不支援，建議改用 Name.com |
| Name.com | C | 否 | 可保留 | 透過 GitHub Education 兌換 |
| Heroku | C | 否 | 可保留 | 每月 $13 額度 x 24 個月 |
| Stripe | C | 否 | 可保留 | $1,000 交易手續費免除 |
| JetBrains 全家桶 | A | 否（最佳路徑：連結 GitHub Pack） | 可保留（綁個人帳號） | 2024/7 起移除文件上傳選項 |
| Cursor Pro | B | 是（帳號信箱需一致） | 需遷移 | 台灣需人工審核 |
| Postman Student Expert | A | 否 | 可保留 | 完成訓練模組即可，永久有效 |
| Microsoft Azure | B | 是 | 失去存取 | 不需信用卡；可嘗試透過 GitHub Pack 繞過 |
| AWS Educate | A | 偏好（個人信箱需補件） | 可保留 | 用個人信箱審核較慢 |
| Google Cloud $300 | D | 否（coupon 套用到任何 Google 帳號） | 可保留 | 需教授申請後發放 coupon |
| Netlify | A | 否 | 可保留 | 免費 Starter 方案，不需學生驗證 |
| Oracle Cloud Free Tier | A | 否 | 可保留 | 永久免費層，需信用卡（不扣款） |
| Supabase | A | 否 | 可保留 | 免費層不需學生驗證 |
| Neon | A | 否 | 可保留 | 免費層不需學生驗證 |
| Figma Education | B | 是（強烈偏好） | 需遷移 | 個人信箱常被拒，可寄信申訴 |
| Autodesk Education | A | 否（透過 SheerID 驗證） | 可保留（綁個人帳號） | 需學生證或 edu 信箱驗證 |
| Miro Education | A | 否（可用學生證替代） | 可保留 | edu 信箱未辨識可聯繫客服 |
| Blender | A | 否 | 可保留 | 開源軟體，任何人可用 |
| Canva Education | D | 否（透過學校訂閱） | 失去存取 | 需學校訂閱 Campus 方案 |
| Coursera 助學金 | A | 否 | 可保留 | 台灣學生核准率高 |
| edX | A | 否 | 可保留 | 免費旁聽不需驗證 |
| Kaggle Learn | A | 否 | 可保留 | 完全免費，不需學生驗證 |
| Notion Plus | B | 是 | 需遷移（匯出資料） | 學校需在 WHED 資料庫中 |
| Microsoft 365 | B | 是 | 失去存取 | 授權綁定學校信箱域名 |
| Obsidian | A | 否 | 可保留 | 開源軟體，任何人可用 |
| Spotify 學生方案 | A | 否（SheerID 驗證） | 可保留（恢復原價） | 每年需重新驗證學生身份 |
| Apple Music 學生方案 | B | 是（UNiDAYS 驗證） | 恢復原價 | UNiDAYS 支援台灣 |
| YouTube Premium 學生方案 | A | 否（SheerID 驗證） | 恢復原價 | 每年需重新驗證 |
| Perplexity Pro | A | 否（SheerID 驗證） | 恢復原價 | 帳號信箱和驗證信箱可不同 |

## 福利用途與應用場景

### GitHub Pack 相關

- **GitHub Copilot**：AI 程式碼補完工具 → 寫程式時自動建議下一行程式碼，大幅加速開發效率，特別適合寫作業和做專題
- **GitHub Pro**：GitHub 進階帳號 → 無限私有 repo 放作業程式碼、3000 分鐘 Actions 跑自動測試、180 小時 Codespaces 雲端開發
- **GitKraken Pro**：Git 圖形化介面 → 用視覺化方式管理版本控制，適合 Git 指令還不熟的同學
- **Educative**：互動式程式課程 → 70+ 門課直接在瀏覽器寫程式練習，適合準備面試刷題
- **Frontend Masters**：前端進階課程 → 6 個月免費學 React、TypeScript、Node.js 等，業界講師授課
- **1Password**：密碼管理工具 → 管理所有帳號密碼，支援 SSH 金鑰管理，開發和日常生活都實用
- **DigitalOcean $200**：雲端虛擬主機額度 → 可架設個人網站、API 伺服器、或練習 Linux 伺服器管理
- **MongoDB Atlas $50**：雲端 NoSQL 資料庫 → 搭配 Node.js 做全端專案，免費認證還能放履歷
- **Namecheap / Name.com**：免費網域 → 註冊個人網域架設作品集網站，面試加分
- **Heroku**：雲端應用平台 → 快速部署 Web 應用，適合展示專題成果
- **Stripe**：線上金流服務 → 學習串接付款功能，做電商專題時不用自掏腰包付手續費

### 開發工具

- **JetBrains 全家桶**：專業 IDE 套件 → IntelliJ 寫 Java、PyCharm 寫 Python、WebStorm 寫前端，比免費編輯器多了強大的重構和除錯功能
- **Cursor Pro**：AI 驅動程式編輯器 → 類似 VS Code 但內建 AI 對話，可以直接用中文問它怎麼寫程式
- **Postman Student Expert**：API 測試工具認證 → 學會 API 測試並取得官方徽章，放在 LinkedIn 上增加專業度

### 雲端服務

- **Microsoft Azure $100**：微軟雲端平台 → 不需信用卡就能開虛擬機、架網站、玩 AI 服務，適合初學雲端
- **AWS Educate**：亞馬遜雲端學習平台 → 免費實驗室練習雲端架構，業界市佔最高的雲端平台
- **Google Cloud $300**：Google 雲端平台 → 用 BigQuery 做資料分析、用 Vertex AI 訓練模型，適合資料科學相關系所
- **Netlify**：靜態網站託管 → 把 React/Vue 專案一鍵部署上線，適合前端作品集
- **Oracle Cloud Free Tier**：Oracle 雲端永久免費層 → 2 台 ARM 虛擬機 + 24GB RAM 永久免費，架設長期運行的專案最划算

### 資料庫

- **Supabase**：開源 Firebase 替代方案 → 500MB PostgreSQL 資料庫 + 即時 API，快速開發全端應用
- **Neon**：Serverless PostgreSQL → 用多少算多少的雲端資料庫，適合小型專案和原型開發

### 設計工具

- **Figma Professional**：專業設計工具 → UI/UX 設計、做簡報、團隊協作設計稿，設計系和資工系都實用
- **Autodesk Education**：工程設計套件 → AutoCAD 畫工程圖、Maya 做 3D 動畫、Fusion 360 做產品設計
- **Miro Education**：線上白板工具 → 團隊腦力激盪、專案流程圖、使用者旅程地圖，分組報告必備
- **Blender**：3D 建模與動畫 → 開源免費的專業 3D 套件，做遊戲素材、3D 列印模型、動畫短片
- **Canva Education**：圖形設計平台 → 快速做出好看的簡報、海報、社群貼文（需學校有訂閱）

### 學習平台

- **Coursera 助學金**：線上課程平台 → 免費修 Stanford、Google 等名校/企業課程並取得證書，台灣學生核准率很高
- **edX**：開放式課程平台 → 免費旁聽 MIT、Harvard 課程，想深入特定領域的好資源
- **Kaggle Learn**：資料科學入門 → 免費學 Python、SQL、機器學習並取得證書，適合想入門 AI 的同學
- **IBM SkillsBuild**：IBM 技能培訓 → AI、雲端、資安課程，取得 IBM 數位徽章放履歷
- **HackerRank**：程式技能驗證 → 考取 Python、Java、SQL 等認證，求職時展示程式能力

### 生產力工具

- **Notion Plus**：全能筆記與專案管理 → 做課堂筆記、管理專題進度、建立個人知識庫，Plus 方案有無限上傳
- **Microsoft 365**：Office 全家桶 → Word 寫報告、Excel 做資料分析、PowerPoint 做簡報、Teams 線上開會
- **Obsidian**：本地知識管理工具 → 用 Markdown 做筆記，雙向連結建立知識網路，完全離線也能用

### 網域與 SSL

- **Let's Encrypt**：免費 SSL 憑證 → 讓你的網站有 HTTPS 加密，架站必備且自動續約

### 影音串流

- **Spotify 學生方案**：音樂串流服務 → 台灣學生價 NT$88/月（約半價），聽歌不受廣告干擾
- **Apple Music 學生方案**：Apple 音樂串流 → 約 5 折學生優惠，Apple 生態系用戶首選
- **YouTube Premium 學生方案**：YouTube 無廣告 → 學生折扣看影片無廣告，還能背景播放當 Podcast 聽

### AI 工具

- **Perplexity Pro**：AI 搜尋引擎 → 5 折學生價，寫報告時快速查找並整理資料，附帶引用來源

### DevOps 與進階

- **NVIDIA DLI**：深度學習課程 → 免費學 GPU 運算和深度學習，取得 NVIDIA 官方證照
- **Red Hat Developer**：企業級 Linux → 免費取得 RHEL 和 OpenShift，學習企業環境的 Linux 系統管理

## .edu.tw 信箱有效期限提醒

### 信箱失效時程

大多數台灣大學的 `.edu.tw` 信箱會在畢業後 **1 至 6 個月**內停用，實際時間依各校政策而定。部分學校（如台大）提供較長的保留期，但多數學校會在學期結束後不久關閉信箱。**請務必向學校資訊處確認你的信箱保留期限。**

### B 類服務的應對策略

B 類服務（帳號綁定 `.edu.tw` 信箱的服務）是畢業後受影響最大的。以下是建議的應對方式：

- **Notion Plus**：畢業前將所有重要頁面匯出為 Markdown 或 HTML，或轉移到用個人信箱註冊的新帳號
- **Microsoft 365**：將 OneDrive 檔案下載到本地或轉存到個人 Google Drive / iCloud
- **Figma Education**：將設計稿轉移到用個人信箱註冊的帳號，或匯出為 .fig 檔案備份
- **Cursor Pro**：效期內善用，畢業後改用免費版或自行訂閱
- **Azure for Students**：將重要資源遷移到個人帳號的付費方案，或匯出資料

### 需要定期重新驗證的服務

以下服務即使用個人信箱註冊（A 類），仍需**每年重新驗證**學生身份，畢業後將無法通過驗證而恢復原價：

- **Spotify 學生方案**：每 12 個月透過 SheerID 重新驗證
- **YouTube Premium 學生方案**：每年重新驗證
- **Apple Music 學生方案**：透過 UNiDAYS 每年驗證
- **Perplexity Pro**：依 SheerID 驗證週期

### 畢業前必做清單

1. **設定信箱轉寄**：將 `.edu.tw` 信箱的信件轉寄到個人信箱（Gmail 等），確保不漏接任何續約或安全通知
2. **匯出 B 類服務資料**：Notion 筆記、OneDrive 檔案、Figma 設計稿等，全部備份或遷移
3. **更新帳號聯絡信箱**：能改成個人信箱的服務，盡早更改（A 類和 C 類通常可以）
4. **截圖保存證照與認證**：Coursera 證書、HackerRank 認證、Postman 徽章等，下載 PDF 備份
5. **用完雲端額度**：Azure $100、DigitalOcean $200、Google Cloud $300 等，把握在學期間用完
6. **確認 GitHub Pack 狀態**：Pack 效期通常到畢業，C 類服務在效期內兌換的福利不受影響
7. **記錄所有已申請的服務**：用 `student_benefits_tracker.json` 記錄清楚，避免遺忘

## 台灣學生申請小提醒

- **`.edu.tw` 信箱**是申請大部分服務的關鍵，請確認信箱仍然有效
- **GitHub Student Pack** 用 `.edu.tw` 通常很快通過，如果被拒絕，試試上傳更清晰的學生證照片
- **SheerID 驗證**（Spotify、Apple Music 等）支援大部分台灣大學
- **Coursera 財務補助**台灣學生申請通過率很高，值得嘗試
- **Microsoft 365** 幾乎所有台灣大學都有提供，可向學校資訊處詢問
- **Cursor Pro** 台灣不在官方支援名單中，但可嘗試手動驗證

## 系統需求

- 有效的 `.edu.tw` 電子信箱（在學中）
- **Windows 腳本**：PowerShell 5.1+（Windows 10/11 內建）
- **macOS/Linux 腳本**：bash + [jq](https://jqlang.github.io/jq/)
  - macOS：`brew install jq`
  - Ubuntu/Debian：`sudo apt install jq`

## 檔案結構

```
taiwan-student-benefits/
├── SKILL.md                          # Claude Code skill 定義
├── README.md                         # 正體中文說明（本檔案）
├── README.en.md                      # English README
├── tracker_template.json             # 37 項福利追蹤模板
├── references/
│   ├── benefits-catalog.md           # 完整福利目錄（英文版，含網址與注意事項）
│   └── benefits-catalog.zh-TW.md     # 完整福利目錄（正體中文版）
└── scripts/
    ├── check_benefits.ps1            # Windows 提醒腳本
    ├── check_benefits.sh             # macOS/Linux 提醒腳本
    ├── setup_schedule.ps1            # Windows 排程設定
    └── setup_schedule_macos.sh       # macOS 排程設定
```

## 貢獻

發現新的學生福利？網址有變更？歡迎發 PR！

新增福利的步驟：
1. 在 `tracker_template.json` 加入新項目
2. 在 `references/benefits-catalog.md`（英文）和 `references/benefits-catalog.zh-TW.md`（中文）加入說明
3. 提交 Pull Request

## 授權

MIT

## 參考資料

- 福利清單參考自 [2025-2026 台灣學生免費訂閱指南](https://claude-world.com/zh-tw/articles/taiwan-student-free-subscriptions-guide-2025-2026/)
- 使用 [Claude Code](https://claude.ai/claude-code) 建立
