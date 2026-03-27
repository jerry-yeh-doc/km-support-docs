# GoldenRace (Xpress) Reverse 整合技術指南

本文件提供 **GoldenRace** 品牌在 Reverse 模式下的環境資訊、API 規範及營運對接流程。

---

## 1. 環境資訊 (Environment)

| 項目 (Item) | 說明 (Description) | 備註 (Note) |
| :--- | :--- | :--- |
| **Staging 測試帳號** | [點此開啟系統](https://casino-client.xpressgaming.net) | `User: jerry.yeh@kingmidasgames.com`<br>`PW: 1234567890@QMisawesome` |
| **Staging API 域名** | `https://casino-reverse-api.xpressgaming.net` | 測試環境對接入口 |
| **Production API 域名** | `https://reverse-api.xpressgaming.net/` | 正式環境對接入口 |
| **Staging 遊戲路徑** | `https://goldenrace-staging-launcher.queenmakergames.co` | [查看開啟範例](#launch-example) |
| **Production 遊戲路徑** | `https://goldenrace-launcher.queenmakergames.co` | 正式環境開啟路徑 |

---

## 2. 遊戲開啟方法 (Launch Method)

客戶端需透過 **Game Launch URL** 讓玩家進入遊戲。

<a id="launch-example"></a>
### **範例連結 (Launch URL Example):**
> [!TIP]
> `https://goldenrace-staging-launcher.queenmakergames.co/?game_id=example&site_id=1&player_id=1&operator_id=1&currency=EUR&mode=real&token=23910a85-3953-4afc-8f97-07c5fcd427fc&platform=desktop&language=EN&return_url=https%3A%2F%2Fcasino-client.xpressgaming.net%2Fresources%2Fproducts%2F13313`

---

## 3. API 路由與規範 (SW API)

### 🩺 基礎連通性
* **`/api/health-check`** : 確保 SW 伺服器運作狀態。
* **`/api/games`** : 獲取完整遊戲列表以同步至客戶端。

### 💰 錢包交易 (Wallet API)
> **Endpoint Base:** `/api/wallet/`

核心路由包含：`authenticate` / `balance` / `debit` / `credit` / `rollback` / `status` / `terminate`

#### **核心業務邏輯 (Important Logic):**
1.  **電子桌面遊戲 (Table Games)**: 我們會將一局內**不同下注**拆分為多個 `debit`。每個下注的 **`roundId`** 下會獨立，因此一筆`debit` 有對應的`credit` 或 `rollback`。
2.  **老虎機 (Slots)**: 採「一扣一補」模式。當 KM 遊戲伺服器傳送 `roundclosed=true` 時，我方才會發送 `credit` 請求給客戶。

---

## 4. 財務與統計 (Billing)

* **結算截止時間 (Cutoff Time)**: 以 **Transaction Start Time** (交易發起時間) 為準。
* **報表時區 (TimeZone)**: **UTC**。

---

## 5. QM 品牌設定 (Brand Setup)

* **品牌代碼識別 (Prefix/Brand Code)**:
	在我方建立Brand會使用 `Prefix`+`operator_id`
    * `Prefix`: `GoldenRace-`。
    * `operator_id`: 營運商 ID。
    * `site_id`: 我方建立玩家userid的前綴代碼 (Player's Prefix)。

若有新營運商 (New Operators)，我方需建立品牌，並請客戶提供以下資訊：

> [!IMPORTANT]
> **新品牌必要資訊檢查清單:**
> 1.  **operator_id** : 營運商 ID
> 2.  **site_id** : 站點 ID
> 3.  **enable currencies** : 需啟用的幣別
> 
> **語言代碼轉換提醒**: 需設定外部語言代碼對應。
> * 例如: `en-US` ➔ `en`
> 
> *註：正式環境之 API Endpoint、ProviderId、Access Key、Private Key 皆保持一致。*

---

## 📝 6. 合約說明 (Resellers)

目前共有 3 個不同的合約對應不同的Reseller：
* **GoldenRace**
* **GoldenRaceReg**
* **GoldenRaceAsia**

---
最後更新：2026-03-27