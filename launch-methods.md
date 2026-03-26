# 🎮 遊戲開啟方法 (Game Launch Methods)

本文件說明 KingMidas 系統目前支援的兩種啟動機制。兩者最大的差異在於 **「域名 (Domain) 的取得方式」** 與 **「維運靈活性」**。

---

## 1. [舊版] 客戶端自行組裝連結 (Legacy)

* **運作邏輯**：客戶端系統直接將參數拼接到預設域名後方。
* **網址結構**：
    `https://{game_domain}/gamelauncher?gpcode=KMQM&gcode={game_code}&token={player_token}`

### 🌐 預設域名 (Game Domain)
| 環境 | 域名路徑 |
| :--- | :--- |
| **Staging** | `https://staging-lobby.queenmakergames.co` |
| **Production** | `https://lobby.queenmakergames.co` |

### 📋 參數說明
* `gpcode`：固定值 `KMQM`。
* `gcode`：欲開啟的遊戲代碼 (Game Code)。
* `token`：玩家驗證令牌 (Player Token)。

---

## 2. [新版] 透過 API 獲取連結 (v1 / v2)

* **運作邏輯**：客戶呼叫 API，由我方系統動態分派可用域名並回傳完整加密 URL。
* **優點**：若域名遭 ISP 封鎖，我方可在 **QM BO 後台** 直接切換，客戶端 **無須修改程式碼**。

### 📡 API 請求路徑 (Endpoints)
* **v2 版本**：`POST https://{api_domain}/api/v2/launcher/real`
* **v1 版本**：`POST https://{api_domain}/api/launcher/real`

### 📘 API 技術文件 (Documentation)
為了確保您取得最新版本的參數定義與錯誤代碼，請參閱以下文檔：
> [!TIP]
> **[點此查看完整 API 文檔 (Google Drive)](https://drive.google.com/drive/folders/1QMBO3uLUH9tuOjl2Mj1LoPYYMrnnxPQO)**

### 📥 範例回傳 (JSON Response)
```json
{
  "url": "[https://lobby.aggqmsea.net/gamelauncher?token=TiFkFy...&code=S01RTT...&homeurl=https%3a%2f%2f](https://lobby.aggqmsea.net/gamelauncher?token=TiFkFy...&code=S01RTT...&homeurl=https%3a%2f%2f)..."
}
```
---

*最後更新日期：2026-03-26*