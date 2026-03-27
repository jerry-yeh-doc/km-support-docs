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

### 基礎連通性
* **`/api/health-check`** : 確保 SW 伺服器運作狀態。
* **`/api/games`** : Push 遊戲資訊以同步至客戶端。
> 範例如下：
```json
{
    "games": [
        {
            "game_id": "Sugar_Blast_Frenzy",
            "name": "Sugar Blast Frenzy",
            "description": "Sugar Blast is a slot reel-style game. Eliminate candy symbols to get a winning combination.The more winning combinations achieved, the higher the winnings! You can choose to fire up to 3 cannon bombs to start the game.",
            "type": 0,
            "thumbnail": "https://cdn-static.queenmakergames.co/game/en-US/Game_KMQM_Sugar_Blast_Frenzy_343x200.jpg",
            "platform": 2,
            "version": "v.2.3.101",
            "release_date": "2024-05-28 07:00:00",
            "free_bet_support": true,
            "in_game_free_bets": true,
            "volatility": "High",
            "rtp": 96.98,
            "pay_lines": 0,
            "hit_ratio": 28.17,
            "bonus_buy": true
        },
        {
            "game_id": "Chicken_Crossy",
            "name": "Chicken Crossy",
            "description": "Get ready for high-stakes fun in Chicken Crossy! Guide your daring chicken through lanes of speeding traffic, collecting multipliers with every step. The further you go, the bigger the rewards! Cash out anytime to secure your wins or push your luck for the ultimate prize with payouts of over 3,000,000X! Cross to Win!",
            "type": 19,
            "thumbnail": "https://cdn-static.queenmakergames.co/game/en-US/Game_KMQM_Chicken_Crossy_343x200.jpg",
            "platform": 2,
            "version": "v2.3.41",
            "release_date": "2025-07-15 07:00:00",
            "free_bet_support": false,
            "in_game_free_bets": false,
            "volatility": "High",
            "rtp": 95.99,
            "pay_lines": 0,
            "hit_ratio": 0.0,
            "bonus_buy": false
        }
    ]
}
```

### 錢包交易 (Wallet API)
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

## QM 品牌設定 (Brand Setup)

* **品牌代碼識別 (Prefix/Brand Code)**:
	在我方建立Brand會使用 `Prefix`+`operator_id`
    * `Prefix`: `GoldenRace-`。
    * `operator_id`: 營運商 ID。
    * `site_id`: 我方建立玩家userid會使用 `site_id` 作為前綴代碼。

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

## 6. 合約說明 (Resellers)

目前共有 3 個不同的合約對應不同的Reseller：
* **GoldenRace**
* **GoldenRaceReg**
* **GoldenRaceAsia**

---
最後更新：2026-03-27