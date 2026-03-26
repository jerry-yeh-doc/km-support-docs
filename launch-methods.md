## 1. 遊戲開啟方法 (Game Launch Methods)

目前系統支援兩種開啟方式，兩者的主要差異在於「域名（Domain）」的由來。

### 1.1 [舊版] 客戶端自行組裝連結
* **邏輯**：客戶端程式碼寫死域名，自行拼接參數。
* **網址格式**：
  `https://{game_domain}/gamelauncher?gpcode=KMQM&gcode={game_code}&token={player_token}`
* **預設域名**：
  * **Staging**: `https://staging-lobby.queenmakergames.co`
  * **Production**: `https://lobby.queenmakergames.co`

### 1.2 [新版] 透過 API 獲取連結 (v1 / v2)
* **邏輯**：客戶呼叫 API，由我方系統回傳一組完整的 URL。
* **API Endpoints**：
  * `POST /api/v2/launcher/real`
  * `POST /api/launcher/real`
* **範例回傳**：
  `https://lobby.aggqmsea.net/gamelauncher?token=...&code=...&homeurl=...`
* **優點**：域名由 **QM BO 後台** 動態配置，被封鎖時不需客戶改 code。

---
*最後更新日期：2026-03-26*