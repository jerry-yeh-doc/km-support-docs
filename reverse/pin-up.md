# Pin-Up Reverse 整合技術指南

本文件提供 **Pin-Up** 品牌在 Reverse 模式下的環境資訊、API 規範及營運對接流程。

---

## 1. 環境資訊 (Environment)

| 項目 (Item) | 說明 (Description) | 備註 (Note) |
| :--- | :--- | :--- |
| **Staging 測試帳號** | [點此開啟連結](https://com.p-upp.com/) | `User: kingmidasgames_preprod_usd@test.com`<br>`PW: Hfysnsbvyfns652*` |
| **Staging API 域名** | `https://pinup-staging-api.queenmakergames.co` | 測試環境對接入口 |
| **Production API 域名** | `https://pinup-api.queenmakergames.co` | 正式環境對接入口 |
| **Staging 遊戲路徑** | N/A |  |
| **Production 遊戲路徑** | N/A |  |

---

## 2. 遊戲開啟方法 (Launch Method)

客戶端需透過 **POST /gameWebFrame API** 索取遊戲連結並返回給玩家開啟。

### **開啟遊戲API範例 (Launcher API Example):**
* **請求URL** : `https://{API 域名}/gameWebFrame?`
* **請求內容** :
```json
{
  "demo": false,
  "isMobile": false,
  "gameId": "Sugar_Blast_Frenzy",
  "lang": "es",
  "token": "69cb4e62dd0a12ce326ba221",
  "home": "https://api-br.pin-up.world/api/v1/casino/games/home/82007632/1/aHR0cHM6Ly9waW4tdXAud29ybGQvZXMtbXgvY2FzaW5vL2JvbnVzLWJ1eQ",
  "playerId": "82007632",
  "currency": "MXN",
  "country": ""
}
```
* **回覆內容** :
```json
{
  "URL": "https://lobby.qmaggsea.games/gamelauncher?token=4Y1b4r7WjiiHIUbkCrizU7kApk4E6NxaAYs1KEIz...&code=S01RTTpTdWd...&homeurl=https%3a%2f%2fapi-br.pin-up.world%2fapi%2fv1...",
  "error": null
}
```

---

## 3. API 路由與規範 (SW API)

* **`/api/games`** : 取得遊戲資訊以同步至客戶端。

### 錢包交易 (Wallet API)
> **Endpoint Base:** `https://kingmidasgames.slotsintegrationapi.com/kingmidasgames`

核心路由包含：`GET session` / `POST action`

#### **核心業務邏輯 (Important Logic):**
1.  **電子桌面遊戲 (Table Games)**: 一局`roundId`內可以多筆下注與派彩，與現有一般接入客戶一樣。
2.  **老虎機 (Slots)**: 採「一扣一補」模式。當 KM 遊戲伺服器傳送 `roundclosed=true` 時，我方才會發送 `credit` 請求給客戶。
3.  **`GET session`**: 我們的 /balance API，Launcher API也會送出該請求進行驗證
4.	**`POST action`**: 我們的 /debit、/credit API，透過同一支 action 接口, 裡面的 `transactionType` 判斷是下注/派彩/取消交易
5.  *註：lose bet 不需要送 credit amt=0 給客戶。If there is no win - no payout is required.*

---

## 4. 財務與統計 (Billing)

* **結算截止時間 (Cutoff Time)**: 未知
* **報表時區 (TimeZone)**: **UTC**。

```
Regarding “which time” we use:
Bet Acceptance - the bet posting time (when our server accepted the bet)
Payout/Jackpot - the payout posting time (when our server posted the win)
```

---

## 5. QM 品牌設定 (Brand Setup)

* **品牌代碼識別 (Prefix/Brand Code)**:
	在我方建立Brand，**不使用** `Prefix`+`operatorId`
    * `Prefix`: N/A。
    * `operatorId`: 營運商 ID。

若有新營運商 (New Operators)，我方需建立品牌，並請客戶提供以下資訊：

> [!IMPORTANT]
> **新品牌必要資訊檢查清單:**
> 1.  **Reseller** : under `Pin-Up` or `Pinco`
> 2.  **enable currencies** : 需啟用的幣別
> 3.  **API_URL`** : 回調地址
> 4.  **PROVIDER_ID** : `kingmidasgames`
> 5.  **PROVIDER_TOKEN** : 每次API回調請求path後方需要帶入的token
>
> **語言代碼轉換提醒**: 需設定外部語言代碼對應。
> * 例如: `en-US` ➔ `en`

---

## 6. 備註說明

目前共有2個不同的合約對應不同的Reseller:
* **Pin-Up**
* **Pinco**

---
最後更新：2026-03-31
維護者：Jerry Yeh