# 域名封鎖處理流程 (Domain Blocked SOP)

當客戶反映遊戲無法開啟或域名被封鎖時，請依照以下步驟進行排查。

## 🔍 第一步：確認受影響的域名
請向客戶索取完整的錯誤截圖，並確認 URL 屬於哪一種類型：

| 域名類型 | 範例 | 處理對象 |
| :--- | :--- | :--- |
| **舊版遊戲域名** | `lobby.queenmakergames.co` | 客戶端更換 |
| **新版啟動遊戲API域名** | `lobby.aggqmsea.net` | 我方後台更換 |
| **遊戲商端域名** | 透過上述遊戲域名跳轉後的遊戲商 URL | 我方後台更換 |

---

## 🛠️ 第二步：執行對應處理方案

### 方案 A：舊版開啟方法域名封鎖
* **動作**：如確認域名於該國家IP無法開啟，請客戶端工程師將程式碼中的域名更換為最新的 **備用 Domain**。

### 方案 B：新版啟動遊戲API返回域名封鎖
* **操作路徑**：`BO` > `System` > `Endpoint`
* **設定對象**：選擇對應 Reseller (例如: **LuckyStreak**) > `KingMidas`
* **修改位置**：`KingMidas World` & `Kaiun World` -> **`Aggregator Endpoint`**

### 方案 C：遊戲商 (Game Provider) 域名封鎖 (高頻)
> 💡 **註**：土耳其地區 (如客戶 LuckyStreak) 最常發生此類封鎖。
* **操作路徑**：`BO` > `System` > `Endpoint`
* **設定對象**：選擇對應 Reseller (例如: **LuckyStreak**) > `KingMidas`
* **修改位置**：`KingMidas World` & `Kaiun World` -> **`Endpoint`**

### 方案 D：單一 Brand 客戶封鎖
* **操作路徑**：`BO` > `Licensee` > `Brand` > `搜尋特定 Brand` > `Edit Brand` > `Providers` > `KingMidas`
* **修改位置**：`KingMidas World` -> **`Game Provider Endpoint`**
* **重要**：Desktop 與 Mobile 兩個欄位皆須同步更改。

---
*最後更新日期：2026-03-26*