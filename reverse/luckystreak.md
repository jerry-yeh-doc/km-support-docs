# LuckyStreak Reverse 整合技術指南

本文件提供 **LuckyStreak** 品牌在 Reverse 模式下的環境資訊、API 規範及營運對接流程。

---

## 1. 環境資訊 (Environment)

| 項目 (Item) | 說明 (Description) | 備註 (Note) |
| :--- | :--- | :--- |
| **Staging 測試帳號** | [點此開啟系統](https://dev.luckystreaklive.com) | `User: KingMidas1`<br>`PW: KingMidas1` |
| **Staging API 域名** | `https://luckystreak-staging-api.queenmakergames.co` | 測試環境對接入口 |
| **Production API 域名** | `https://luckystreak-api.queenmakergames.co` | 正式環境對接入口 |
| **Staging 遊戲路徑** | N/A |  |
| **Production 遊戲路徑** | N/A |  |

---

## 2. 遊戲開啟方法 (Launch Method)

客戶端需透過 **POST /launcher API** 索取遊戲連結並返回給玩家開啟。

### **開啟遊戲API範例 (Launcher API Example):**
* **請求URL** : `https://{API 域名}/launcher`
* **請求內容** :
```json
{
  "gameId": "Sugar_Blast_Frenzy",
  "language": "EN",
  "lobbyUrl": "https://959betvole.com/tr/game/casino/play/47123",
  "operator": "PRONET",
  "token": "69c620d3fb6aac4848b1fb74"
}
```
* **回覆內容** :
```json
{
  "gameUrl": "https://lobby.aggqmsea.net/gamelauncher?token=2tXSBPFLXW0CwRubk3Y6SOEuGiL5qGRn...&code=S01RTTpTdWdhc...&homeurl=https%3a%2f%2f959betvole.com%2ftr%2fgame%2fcasino%2fplay%2f47123",
  "error": null,
  "description": null
}
```

---

## 3. API 路由與規範 (SW API)

### 🩺 基礎連通性
* **`/api/games`** : 客戶會送出此API獲取完整遊戲列表。

### 💰 錢包交易 (Wallet API)
> **Endpoint Base:** `/api/wallet/`

核心路由包含：`validate` / `balance` / `bet` / `result` / `cancelbet` / `endround`

#### **核心業務邏輯 (Important Logic):**
1.  **電子桌面遊戲 (Table Games)**: 一局`roundId`內可以多筆下注與派彩，與現有一般接入客戶一樣。
2.  **老虎機 (Slots)**: 一個 `bet` 可能會有多筆 `result`，與現有一般接入客戶一樣。
3.  **`validate`**: 玩家要開啟遊戲時，我們會送出此API給客戶。
4.	**`balance`**: 我們的 /balance API
5.  **`bet`**、**`result`**: 我們的 /debit、/credit API
6.  **`cancelbet`**: 我們的 /credit txtype=560
7.  **`endround`**: 我們的 /closeround API

---

## 4. 財務與統計 (Billing)

* **結算截止時間 (Cutoff Time)**: 以收到請求的時間點為準。
* **報表時區 (TimeZone)**: **UTC**。

```
We generate the reports based on the time the request was placed.
For the example shared bet will be in 2nd result in 3rd,
 
/bet at 23:59:59 2nd Apr 
/result at 00:00:01 3d Apr
```

---

## 5. QM 品牌設定 (Brand Setup)

* **品牌代碼識別 (Prefix/Brand Code)**:
	在我方建立Brand會使用 `Prefix`+`operatorId`
    * `Prefix`: `LS-`。
    * `operatorId`: 營運商 ID。

若有新營運商 (New Operators)，我方需建立品牌，並請客戶提供以下資訊：

> [!IMPORTANT]
> **新品牌必要資訊檢查清單:**
> 1.  **operator_id** : 營運商 ID
> 2.  **enable currencies** : 需啟用的幣別
> 
> **語言代碼轉換提醒**: 需設定外部語言代碼對應。
> * 例如: `en-US` ➔ `EN`
> 
> *註：正式環境之 API Endpoint、ProviderId、API Key 皆保持一致。*

---

## 6. 備註說明

目前該客戶主要是土耳其市場，須留意域名被擋的問題。

---
最後更新：2026-03-27