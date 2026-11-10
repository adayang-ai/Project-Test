---
name: ig-audit
description: >-
  @pmcareerlife（百萬PM養成所）的每週 IG／Threads 貼文成效檢討：哪些貼文真的有效、為什麼、
  哪些該停。觸發情境：「分析這週貼文」、「IG 週報」、「哪篇有效」、「為什麼這篇沒流量」、
  「audit my content」、「what's working」，或每週排程的流量受眾分析。
---

# ig-audit（百萬PM養成所版）

帳號自己的貼文才是唯一可信的證據。任何經營指南（包含這份）都只是預設值，
有數據時以數據為準。

## 資料來源

用 Windsor.ai 拉資料，不要請使用者貼截圖，除非 Windsor 拉不到。

- 先呼叫 `get_connectors`。Instagram 帳號 id 是 `17841476264449345`（pmcareerlife）。
- **Threads：** 如果 `get_connectors` 回傳裡有 `threads`，一併分析；沒有的話，
  報告開頭寫明「Threads 未連接，本次只分析 IG」，並提醒使用者到 Windsor 連接。

### 每篇貼文（connector `instagram`，日期範圍抓近 21 天，用來算中位數）

```
media_id, timestamp, media_type, media_caption, media_permalink,
media_views, media_reach, media_like_count, media_comments_count,
media_saved, media_shares, media_follows, media_profile_visits
```

Reel 另外試抓 `media_reel_avg_watch_time`、`media_reel_skip_rate`。如果報錯就拿掉，不要卡住。

已知限制：
- `media_reposts` 不支援（帳號用 Instagram Login 連接），不要放進欄位。
- Reel 的 `media_follows`、`media_profile_visits` 一律是 null，不要當成 0。

### 帳號每日（近 14 天，拆成本週與前週）

```
date, views, reach_1d, reach_followers, reach_non_followers,
views_followers, views_non_followers, total_interactions,
follower_count_1d, followers_count
```

`followers_count` 只有今天有值。

### 受眾（每次只能一種 breakdown，分開呼叫）

- `audience_gender_age_name, audience_gender_age_size`
- `audience_country_name, audience_country_size`
- `city, audience_city_size`

受眾和上週差不多時，一句帶過就好，不要每週重講。

## 要算的指標

原始觀看數最沒參考價值，它主要反映追蹤者多寡。改算以下指標，並列出算式：

| 指標 | 算法 | 代表什麼 |
| --- | --- | --- |
| **爆款倍數** | 觀看 ÷ 近 21 天同形式貼文的觀看中位數 | 真的爆了，還是普通的一天。Reel、輪播、單圖分開算中位數 |
| **分享率** | 分享 ÷ 觸及 | 最強的單一訊號。分享代表有人願意把你的內容轉給別人 |
| **收藏率** | 收藏 ÷ 觸及 | 這個帳號的教學內容主要靠收藏，跟分享率並列 |
| **非追蹤者占比** | 當日 `reach_non_followers` ÷ `reach_1d` | 有沒有推出圈。只有帳號層級數據，以發文當天做近似值，要註明 |
| **略過率** | `media_reel_skip_rate`（3 秒內滑走的比例） | 開頭有沒有效。拉得到才用 |
| **平均觀看秒數** | `media_reel_avg_watch_time` ÷ 1000 | 中段有沒有留住人。拉得到才用 |
| **追蹤轉換** | 本週新增追蹤 ÷ 本週觸及（帳號層級） | 注意力有沒有變成追蹤 |

排名用爆款倍數與分享率加收藏率，不用觀看數。

**發文不滿 48 小時的貼文不納入排名**，另外列出「數據未穩定」。

## 找出規律

把前 5 名和後 5 名並排，找出差異，有證據時也要願意說出使用者不想聽的結論。依序檢查：

1. **開頭類型。** 用下列分類標記每篇的第一句：
   - 結果先行（「改了這一行，就有公司主動來問」）
   - 具體數字（「4 個 offer」、「漲幅 60%」）
   - 反直覺或打破迷思（「別再找資料了」）
   - 直接提問（「35 歲以上找工作，機會真的比較少嗎？」）
   - 引述金句（「你不要停下來，但是你可以走慢一點」）
   - 情境鋪陳（「想換工作的念頭，放了快一年」）
2. **內容主軸。** 履歷與求職文件／面試技巧／AI PM／學員故事（Amy、Ruta 等）／
   職場溝通與心理學研究／硬體 PM／Podcast 或直播導流。
3. **形式。** Reel、輪播、單圖。
4. **是不是「看完可以直接照做」。** 有沒有具體改法或範例句。
5. **CTA 類型。** 私訊「陪跑」／留言關鍵字領資源／留言分享經驗。
6. **有沒有在第一小時回留言。**
7. **發文時段**（台灣時間）。**最後才看**，前面都看不出差異時才拿來解釋，
   它幾乎從來不是主因。

結論要寫成「主張加證據」，並說明信心程度。週報通常只有 10–20 篇，樣本小，
n 少於 5 時要明講「訊號弱，再觀察一週」，不要硬湊規律。

## 區分兩種問題

**有觸及但沒有追蹤，不是內容失敗，是個人檔案或 CTA 問題。**
**沒有觸及，是開頭問題。**
先分清楚再給建議。非追蹤者占比高但追蹤轉換低時，停止改開頭，改去檢查個人檔案、
置頂貼文和簡介連結。

## 與前幾週比較

讀 `.claude/instagram/log.md`（在 repo 裡，會跟著 commit 保存）。它記錄每週的核心數字和結論。
- 比較本週與前一週：觀看、觸及、非追蹤者占比、新增追蹤、追蹤者總數。
- 檢查上週的「下週規劃」有沒有執行，以及執行的貼文表現如何。

分析完把本週一列加進 `log.md`，格式照檔案裡的範例。

## 輸出（繁體中文）

```
IG 週報  ·  11/3–11/9  ·  9 篇  ·  Reel 中位數 231 觀看（近 21 天）
（Threads 未連接）

帳號
  觀看 5,830（前週 2,706，+115%）  新增追蹤 17（前週 7）  追蹤者 307
  非追蹤者占觸及 約 80%

前 5 名（依爆款倍數）
  13.5x  Reel  履歷總年資   3,124 觀看  分享率 0.48%  收藏率 1.47%  結果先行
  ...

後 5 名
  ...

數據未穩定（不滿 48 小時）
  ...

數據說了什麼
1. ……（主張，證據，信心：高／中／低）
2. ……

停止：……
多做：……
```

最後把結論交給 `/ig-plan`，讓下週規劃根據這個帳號自己的證據，而不是預設值。
