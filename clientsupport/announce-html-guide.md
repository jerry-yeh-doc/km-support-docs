# 📢 Bot 公告訊息：HTML 語法指南

在發佈 Telegram / Teams 自動化公告發送訊息時，可使用以下 HTML 標籤來強化視覺效果。

---

## 🛠️ 標籤語法對照表

| 樣式效果 | HTML 標籤 | 範例代碼 |
| :--- | :--- | :--- |
| **粗體 (Bold)** | `<b>`, `<strong>` | `<b>粗體文字</b>` |
| *斜體 (Italic)* | `<i>`, `<em>` | `<i>斜體文字</i>` |
| <u>底線 (Underline)</u> | `<u>`, `<ins>` | `<u>底線文字</u>` |
| ~~刪除線 (Strike)~~ | `<s>`, `<strike>`, `<del>` | `<s>刪除線文字</s>` |
| **隱藏內容 (Spoiler)** | `<tg-spoiler>` | `<tg-spoiler>點擊顯示內容</tg-spoiler>` |
| **內嵌連結 (URL)** | `<a href="...">` | `<a href="https://docs.j-yeh.com">點此查看文檔</a>` |
| **提及用戶 (Mention)** | `<a href="tg://user?id=...">` | `<a href="tg://user?id=123456789">聯絡工程師</a>` |
| `行內代碼 (Inline)` | `<code>` | `<code>/api/v2/launcher</code>` |
| **自定義 Emoji** | `<tg-emoji>` | `<tg-emoji emoji-id="5368324170">📌</tg-emoji>` |

---

## 📝 區塊代碼範例 (Code Block)

若需要發送多行格式化代碼（例如 API 的 JSON 回傳值），請使用 `<pre>` 標籤：

~~~html
<pre>
{
  "status": "success",
  "data": "KMQM-S01"
}
</pre>
~~~

---

## 💡 提示 (Pro Tips)

1. **嚴格閉合**：所有標籤必須完整閉合（例如有 `<b>` 就必須有 `</b>`），否則 Telegram 會報錯導致訊息發送失敗。
2. **特殊符號轉義**：若訊息內容包含以下符號，必須進行 HTML 實體轉義：
    * `<` 需改為 `&lt;`
    * `>` 需改為 `&gt;`
    * `&` 需改為 `&amp;`

---
*最後更新：2026-03-26*
