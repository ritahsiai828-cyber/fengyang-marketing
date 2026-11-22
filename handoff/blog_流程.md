# blog_流程｜過去怎麼寫、怎麼上架 Wix（移交給小編）

整理日期：2026-11-22。來源：排程提示（寫稿 A／B、每日發文）、專案文件 seo-rulebook.md §8–10、blog-format-spec.md。

## 1. 用到的連接器與 ID
| 項目 | 值 |
|---|---|
| Wix 連接器 | ExecuteWixAPI（一次呼叫裡寫完讀、寫、讀回）；API 路徑先用 SearchWixRESTDocumentation 確認，不憑記憶 |
| Wix 網站 | 鳳洋行天下，siteId `4ccd4731-069e-4c0a-85c1-2d756552ada3`，https://www.feng-yang.com |
| 作者 memberId | `65dd987b-5c61-4387-8140-ad2604f34551` |
| 分類 ID | 景點美食伴手禮攻略 `fcb69ce3-98c4-4a01-82c0-847c85f1c8f4`｜員工旅遊規劃 `f0681626-29f4-4723-bcdd-9aa53dbbb55c`｜旅遊注意事項與行前準備 `adb7fa37-120f-4224-a841-e0c89900441a` |
| CMS「旅遊」（行程） | `9cs80cl5t9z6b`，行程頁網址 https://www.feng-yang.com/lu-you-1/{行程名稱編碼} |
| CMS「詳細行程」 | `9nzv0h82jlvd`（每日行程；欄位「編號」同一行程共用，Sort Num 排天數） |
| 關鍵字 | Ubersuggest keyword_overview／keyword_suggestions／google_suggestions，locId 2158、zh；每日額度 300 |
| Search Console | 經 Wix：GET https://www.wixapis.com/gsc/v1/search-analytics?startDate=&endDate=&dimensions=page&rowLimit=500（資料晚約 2 天） |
| Wix 分析（網站流量） | POST https://www.wixapis.com/analytics/semantic-model/v3/semantic-models/query-data；流量 semanticModelId `cad7fd34-2c8b-4dda-8296-3f9d47fb484d`；Google 搜尋總覽 `6bb09282-feef-4135-ba0e-9039bb5dbfe7`、搜尋字 `5ea1f7d0-9919-40da-af77-be2d1877a808`、文章 `17c57703-6d96-43b1-bfe7-87d2f60cef53` |
| 專案文件（claude.ai 專案「歐巴馬」） | 規範與狀態檔放這裡，所有排程共用；本次全文已複製到 handoff/專案文件/ |

## 2. 過去的分工（2026-11-22 前）
| 排程 | 時間（台北） | 做什麼 | 現況 |
|---|---|---|---|
| 部落格每日寫稿 A | 每天 02:07 | 寫 3 篇未發佈草稿（plan-NNN） | **仍啟用**：我試著停用，被系統權限擋下，請使用者在排程設定裡手動暫停 |
| 部落格每日寫稿 B | 每天 13:41 | 寫 3 篇未發佈草稿 | **仍啟用**（同上，請手動暫停） |
| 每日發文（一次 5 篇） | 每天 08:53 | 發佈最小編號的 plan- 草稿 5 篇 | **已停用**（2026-11-22 22:50），避免兩邊同時發 |
| 鳳洋 LINE 點擊紀錄週分析 | 每週一 10:43 | 原本會依 LINE 點擊寫 1 篇文章並發佈 | **已改成只寫報告＋給小編的選題建議**，不再寫文章 |
| 每三天排程健康與草稿存量檢核 | 每 3 天 09:13 | 只讀：檢查草稿存量、抽查圖片 | 仍啟用（只讀，可留可關） |
寫作順序（第三輪）：plan-161～230，企業／福委 × 14 個非大陸目的地 × 5 受眾（welfare、visit、union、agency、gala）。目前寫到 plan-179，**下一篇是 plan-180-pattaya-gala**（關鍵字未查，建議先查「泰國尾牙」「芭達雅尾牙」）。目的地順序見 `專案文件/blog-writer-state.md` 與 README 的排程提示摘要。

## 3. 寫一篇的步驟
1. 讀 seo-rulebook、blog-format-spec、blog-writer-state（主關鍵字清單＋目的地事實）、blog-image-pool。
2. 目的地事實：狀態檔有就用；沒有才查 CMS「旅遊」與「詳細行程」。行程事實只用 CMS。
3. 主關鍵字：比較 2–3 種寫法，Ubersuggest 查量，選量最大且站上沒有的；沒數據用 google_suggestions 佐證並註明「量未知」。
4. 寫內容（規範見 blog_規範.md），每個節點帶樣式。
5. 圖片：從圖片池拿該目的地「未用」的 images.unsplash.com 網址，加 `?w=1920&h=1080&fit=crop&q=80&fm=jpg`（內文圖 w=1280&h=600）用 `POST https://www.wixapis.com/site-media/v1/bulk/files/import-v2` 匯入，displayName `pNNNc／pNNNi-photo-<ID>.jpg`；用 `GET https://www.wixapis.com/site-media/v1/files?mediaTypes=IMAGE&paging.limit=100` 確認 READY。
6. 建草稿：`POST https://www.wixapis.com/blog/v3/draft-posts`（title、memberId、categoryIds、excerpt、media、richContent、seoData、seoSlug、hashtags）。
7. 讀回檢查：標題、slug、字數、圖片 READY、表格數、空行、主關鍵字、hashtag、樣式。缺樣式就 PATCH richContent；**草稿 PATCH 會清掉 hashtags，補完要單獨再 PATCH 一次 hashtags（不 publish）**。
8. 更新狀態檔（N、主關鍵字）與圖片池（標已用）。

## 4. 發佈步驟（已實測）
1. 發布前檢查：`GET /blog/v3/draft-posts/{id}?fieldsets=RICH_CONTENT`；hasUnpublishedChanges 為 true 且不是自己建的 → 跳過（不要蓋掉別人的修改）。必填欄位、主關鍵字不重複、所有圖片 `GET /site-media/v1/files/{id}` operationStatus=READY、樣式檢查。
2. 今天全站已發篇數 T，可發 min(5, 10−T)。
3. `POST /blog/v3/draft-posts/{id}/publish`，body `{}`。
4. 補 hashtag（一定最後做）：`PATCH /blog/v3/draft-posts/{id}`，body 只帶 `{"draftPost":{"id","memberId","hashtags"}}`、**不帶 action**；再 publish 一次。
5. 讀回：`GET /blog/v3/posts/{id}?fieldsets=CONTENT_TEXT&fieldsets=SEO`；hashtag 空就重做第 4 步。
6. 送收錄：Wix 收錄 API 只收首頁，文章頁回 400 → 把新網址列給使用者到 Search Console「網址檢查 → 要求建立索引」手動送。

改已發佈文章：先確認 hasUnpublishedChanges=false、記下 hashtags → `PATCH /blog/v3/draft-posts/{id}`（richContent 送完整 nodes）＋`action:"UPDATE_PUBLISH"` → 補 hashtag → publish → 讀回。大改要先問使用者。

## 5. 踩過的坑
- hashtag 每次更新都會被清空 → 最後單獨補並讀回。
- borderColors 寫 `{}` 表格線會消失 → 四邊明確寫色碼。
- 先發布後補封面，Wix 會自動抓內文圖當封面 → 發布前一定設封面。
- 用量上限時排程整天失敗、不會重試。
- Canva AI 生圖額度已用完（11/21），改用 Unsplash 圖片池。
- 商品頁驗證用 Stores Catalog V3（v1 回 501）。
- 同一網站同時只能開一個編輯器分頁。

## 6. 待確認
- 排程提示裡的 SEO 標題後綴還寫「｜歐巴馬旅行社」、署名「歐巴馬」，換品牌後未改（若小編接手新排程，請直接用鳳洋行天下）。
- 寫稿 A／B 兩支排程需使用者手動暫停（見第 2 節）。
