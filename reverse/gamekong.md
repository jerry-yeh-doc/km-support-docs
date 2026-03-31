# GameKong Reverse 整合技術指南

本文件提供 **GameKong** 品牌在 Reverse 模式下的環境資訊、API 規範及營運對接流程。

---

## 1. 環境資訊 (Environment)

| 項目 (Item) | 說明 (Description) | 備註 (Note) |
| :--- | :--- | :--- |
| **Staging 測試帳號** | [點此開啟Chicken Crossy遊戲](https://dev.casinia.com/en/game/chicken-crossy-v2) | `User: kingmidas@casinia.com`<br>`PW: Qwert123*` |
| **Staging 測試帳號** | [點此開啟Sicbo遊戲](https://dev.casinia.com/en/game/sicbo-v6) | `User: kingmidas@casinia.com`<br>`PW: Qwert123*` |
| **Staging 測試帳號** | [點此開啟Cleopatra's Treasure遊戲](https://dev.casinia.com/en/game/cleopatras-treasure-v1) | `User: kingmidas@casinia.com`<br>`PW: Qwert123*` |
| **Staging API 域名** | `https://gamekong-staging-api.queenmakergames.co` | 測試環境對接入口 |
| **Production API 域名** | `https://gamekong-api.queenmakergames.co` | 正式環境對接入口 |
| **Staging 遊戲路徑** | N/A |  |
| **Production 遊戲路徑** | N/A |  |

---

## 2. 遊戲開啟方法 (Launch Method)

客戶端需透過 **POST /session API** 索取遊戲連結並返回給玩家開啟。

### **開啟遊戲API範例 (Launcher API Example):**
* **請求URL** : `https://{API 域名}/session`
* **請求內容** :
```json
{
  "clientType": "mobile",
  "currency": "EUR",
  "gameId": "Flame_of_Fortune",
  "gameMode": "real",
  "locale": "FI",
  "operatorDepositUrl": "https://kingmaker.com/#deposit",
  "operatorHomeUrl": "https://kingmaker.com",
  "operatorId": "gamekong",
  "playerIPAddress": "83.245.234.202",
  "playerId": "UP_26r6j8m2vd5h097o1",
  "sessionId": "UP_b0364c22-b04b-4c86-9b86-d682c5637306",
  "signatureSalt": "DiyOGGpeSb4UaN50",
  "registrationCountry": "FI",
  "playerName": "R*********"
}
```
* **回覆內容** :
```json
{
  "data": {
    "gameUrl": "https://lobby.qmaggsea.games/gamelauncher?token=PXHlFppgVEt05d4sntJLXBJuhvEntJMNC1BPLPFikaaRztdloPKV3lEzpnYJwBtrOJxzSud7k92nkRQQIhuIDq1USbRbyQR6weEEVEVCwJweEqOMCAEbev5QnPj4c2dy67DsIE0ymVkWfmJmT6lftD32&code=S01RTTpGbGFtZV9vZl9Gb3J0dW5l&homeurl=https%3a%2f%2fkingmaker.com",
    "providerSessionId": "21d3741a-dd2d-41e3-9e9b-b0a2ac217ba7"
  }
}
```

---

## 3. API 路由與規範 (SW API)

### 錢包交易 (Wallet API)
> **Endpoint Base:** `https://api.gamekong.com/providers/games/v1/portal/callback`

核心路由包含：`GET balance` / `POST bet` / `POST win` / `DELETE bet`

#### **核心業務邏輯 (Important Logic):**
1.  **電子桌面遊戲 (Table Games)**: 一局`roundId`內可以多筆下注與派彩，與現有一般接入客戶一樣。
2.  **老虎機 (Slots)**: 採「一扣一補」模式。當 KM 遊戲伺服器傳送 `roundclosed=true` 時，我方才會發送 `credit` 請求給客戶。
3.  **`GET balance`**: 我們的 /balance API，Launcher API也會送出該請求進行驗證
4.	**`POST bet`**: 我們的 /debit API
5.  **`POST win`**: 我們的 /credit API
6.  **`DELETE bet`**: 我們的 /credit txtype=560

---

## 4. 財務與統計 (Billing)

* **結算截止時間 (Cutoff Time)**: round close 時間。
* **報表時區 (TimeZone)**: **UTC**。

---

## 5. QM 品牌設定 (Brand Setup)

* **品牌代碼識別 (Prefix/Brand Code)**:
	在我方建立Brand會使用 `Prefix`+`operatorId`
    * `Prefix`: `GK-`。
    * `operatorId`: 營運商 ID。

若有新營運商 (New Operators)，我方需建立品牌，並請客戶提供以下資訊：

> [!IMPORTANT]
> **新品牌必要資訊檢查清單:**
> 1.  **operatorId** : 營運商 ID
> 2.  **enable currencies** : 需啟用的幣別
> 3.  **ApiKey`** : 於放在請求的header: `X-Api-Key`
> 4.  **apiSecret** : 用來做HMAC-SHA256加密的Secret，header:`X-Request-Signature`
> 
> **語言代碼轉換提醒**: 需設定外部語言代碼對應。
> * 例如: `en-US` ➔ `EN`
> 
> *註：正式環境之 API Endpoint皆保持一致。*

---

## 6. 備註說明

N/A

---
最後更新：2026-03-30  
維護者：Jerry Yeh