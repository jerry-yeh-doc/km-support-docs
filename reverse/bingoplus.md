# BingoPlus Reverse 整合技術指南

本文件提供 **BingoPlus** 品牌在 Reverse 模式下的環境資訊、API 規範及營運對接流程。

---

## 1. 環境資訊 (Environment)

| 項目 (Item) | 說明 (Description) | 備註 (Note) |
| :--- | :--- | :--- |
| **Staging 測試帳號** | [點此開啟連結](https://m-l.uatextfront66.com/rewards/) | `User: 09400390867`<br>`PW: 1234qwer` |
| **Staging API 域名** | `https://bingoplus-staging-api.queenmakergames.co` | 測試環境對接入口 |
| **Production API 域名** | `https://bingoplus-api.queenmakergames.co` | 正式環境對接入口 |
| **Staging 遊戲路徑** | N/A |  |
| **Production 遊戲路徑** | N/A |  |

---

## 2. 遊戲開啟方法 (Launch Method)

客戶端需透過 **POST /user/login API** 索取遊戲連結並返回給玩家開啟。

### **開啟遊戲API範例 (Launcher API Example):**
* **請求URL** : `https://{API 域名}/user/login`
* **請求內容** :
```json
{
  "ts": "2026-03-31 14:04:24",
  "traceId": "8fd8adbb-22d7-45ba-8541-fa0a704376ff",
  "userName": "perya9jc5bsu",
  "gameId": "Coin_Pusher",
  "vendorCode": "km",
  "language": "EN",
  "currency": "PHP",
  "homeUrl": "https://bingoplus.com",
  "ipAddress": "103.186.138.163",
  "parent": "C66",
  "terminalType": "H5",
  "extField": {
    "nick": "perya9jc5bsu",
    "icon": "https://publicavatar.s3.ap-east-1.amazonaws.com/avatardefault.png",
    "registerSiteId": "4",
    "betSiteId": "1"
  },
  "payload": "S/J07rAU3lrH1eXeDXfGJXixSEzVAO8yGRmLqITrN6BxkxX+lFBcGZd...",
  "brandCode": "BingoPlus"
}
```
* **回覆內容** :
```json
{
  "data": {
    "gameUrl": "https://lobby.qmaggsea.games/gamelauncher?token=13iJE0oNW0F6crrP6UcGKkd33zmYS1I5K...&code=S01RTTpDb2l...&homeurl=https%3a%2f%2fbingoplus.com",
    "token": "8560a8ff-9aab-4114-93cd-f3a356e3861d"
  },
  "traceId": "8fd8adbb-22d7-45ba-8541-fa0a704376ff",
  "status": "0"
}
```

---

## 3. API 路由與規範 (SW API)

* **`/transaction/list`** : 送出請求給我們，取得注單資料。類似我們的 history/rounds API
* **`/transaction/detail`** : 送出請求給我們，取得注單詳情。類似我們的 round detail history API
* **`/transaction/summary`** : 送出請求給我們，取得指定時間區間內的總數據。

### 錢包交易 (Wallet API)
> **Endpoint Base:** `https://prossgamessip.ewallet66.com/km/wallet`

核心路由包含：`balance` / `bet` / `settle` / `rollback` / `reward`

#### **核心業務邏輯 (Important Logic):**
1.  **電子桌面遊戲 (Table Games)**: 無論一局 `billId` 內有多少注單，當 KM 遊戲伺服器傳送 `roundclosed=true` 時，統一最後計算總派彩金額才送 `settle`給客戶。
2.  **老虎機 (Slots)**: 採「一扣一補」模式。當 KM 遊戲伺服器傳送 `roundclosed=true` 時，我方才會發送 `settle` 請求給客戶。
3.  **`balance`**: 我們的 /balance API
4.	**`bet`**: 我們的 /debit API。*註：客製化參數 `gmcode` 是我們的 sharedroundid，客戶自行驗證是否同局的狀況*
5.  **`settle`**: 我們的 /credit API。*註：客製化參數 `gmcode` 是我們的 sharedroundid，客戶自行驗證是否同局的狀況*
6.	**`rollback`**: 我們的 /credit txtype=560
7.  **`reward`**: 我們的 /wallet/reward API, 用於活動加錢給玩家

---

## 4. 財務與統計 (Billing)

* **結算截止時間 (Cutoff Time)**: 以 **round settled time** (交易發起時間) 為準。
* **報表時區 (TimeZone)**: **UTC+8**。

---

## 5. QM 品牌設定 (Brand Setup)

* **品牌代碼識別 (Prefix/Brand Code)**:
	在我方建立Brand，**不使用** `Prefix`+`operatorId`
    * `Prefix`: N/A。
    * `operatorId`: 營運商 ID。

若有新營運商 (New Operators)，我方需建立品牌，並請客戶提供以下資訊：

> [!IMPORTANT]
> **新品牌必要資訊檢查清單:**
> 1.  **enable currencies** : 需啟用的幣別
> 2.  **API_URL`** : 回調地址
> 3.  **api_key** : 設置在我們的 `Brand Client Id`，會放在回調請求的 header `X-API-KEY`
> 4.  **api_secret** : 設置在我們的 `Brand Client Secret`，透過HMAC-SHA256將請求中的"payload"字段對應的值，用 `api_secret` 進行生成。然後將加密後的值放在回調請求的 header `X-Signature`
> 5.  **payload_secret** : 用來加密請求內容(payload)的金鑰
>
> **語言代碼轉換提醒**: 需設定外部語言代碼對應。
> * 例如: `en-US` ➔ `en`

---

## 6. 備註說明

N/A


---
最後更新：2026-03-31
維護者：Jerry Yeh