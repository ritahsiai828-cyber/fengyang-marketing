# 部落格文字格式規範（Wix 富文本節點，2026-11-22 修訂）

所有排程與對話建立或修改文章時一律照此規格；與 seo-rulebook.md 衝突時以本文件為準。
問題背景：2026-11-17 郵輪文章起，新文章標題、內文、表格沒有帶樣式；2026-11-21 已批次修正，全站 269 篇共 511 個表格都補上線條顏色（plan-014-jeju-family 因有未發布變更而跳過，待補）。

## 標題與內文

| 元素 | 字級 | 行距 | 顏色 | 其他 |
|---|---|---|---|---|
| H2（HEADING level 2） | 28px | 2 | #93616A | 粗體 |
| H3（level 3，常見問題題目） | 預設 | 2 | #93616A | 粗體 |
| 內文（PARAGRAPH、清單項目內段落） | 16px | 2 | #2D2D2D | 句首重點句粗體 |
| 內文連結文字 | 16px | 2 | #93616A | LINK decoration |
| 按鈕（BUTTON） | — | — | 字 #FFFFFF | styles = {borderWidth:0, borderRadius:6, textColor, backgroundColor}；LINE 按鈕底 #1F3A5F、YouTube 按鈕底 #93616A；不要用 styles.colors 寫法 |

節點寫法：
- H2：TEXT decorations = [BOLD(700), COLOR #93616A, FONT_SIZE PX 28]；headingData = {level:2, textStyle:{textAlignment:"AUTO", lineHeight:"2"}}
- H3：decorations = [BOLD, COLOR #93616A]；headingData.textStyle 同上
- PARAGRAPH：每個 TEXT 加 COLOR #2D2D2D、FONT_SIZE 16；paragraphData = {textStyle:{textAlignment:"AUTO", lineHeight:"2"}}；清單項目內的 PARAGRAPH 同樣處理；空白段落不需樣式

## 表格（TABLE）

| 項目 | 規格 |
|---|---|
| 首行（表頭）儲存格 | 背景 #A1AEC0；文字 #2D2D2D、16px、粗體 |
| 其他列文字 | #2D2D2D、16px、不粗體 |
| 內文行距 | 1.5（儲存格內 PARAGRAPH 的 paragraphData.textStyle = {textAlignment:"AUTO", lineHeight:"1.5"}） |
| 線條 | 顏色 #86C6E5；**每個 TABLE_CELL 都必須明確寫 borderColors = {left:"#86C6E5", right:"#86C6E5", top:"#86C6E5", bottom:"#86C6E5"}** |

**重要（2026-11-22 更正）**：borderColors 寫成 `{}` 或省略，前台表格線條會消失，這是之前「線條遺失」的原因。一定要四邊都寫明確色碼。Wix API 沒有邊框粗細欄位，粗細沿用網站預設。

節點寫法：TABLE_CELL.tableCellData = {cellStyle:{verticalAlignment:"MIDDLE", backgroundColor:"#A1AEC0"（僅第一列）}, borderColors:{left,right,top,bottom 皆 "#86C6E5"}}；tableData 只放 dimensions 與 cellPadding，不要設 rowHeader、header；每篇最多 1 個表格、最多 4 欄（seo-rulebook 規定）。

## 發文後檢查（加進初檢，每次發文後必做）

讀回 RICH_CONTENT：
- H2 都有 FONT_SIZE 28 與 COLOR #93616A；
- 有字的 PARAGRAPH 都有 FONT_SIZE 16、COLOR #2D2D2D、lineHeight "2"；
- 表格每格有四邊 #86C6E5 的 borderColors、第一列背景 #A1AEC0、沒有 rowHeader；
- BUTTON 用 textColor／backgroundColor 寫法。
缺項就 UPDATE_PUBLISH 補樣式，再補回 hashtag。

## 修改既有文章的安全做法

先 GET draft 確認 hasUnpublishedChanges 為 false（否則跳過回報）；記下 hashtags；PATCH richContent + action UPDATE_PUBLISH；再 PATCH hashtags（不帶 action）；再 publish；讀回確認。未發佈草稿：PATCH richContent 會清空 hashtags，最後要單獨再 PATCH 一次 hashtags，不要 publish。

## 待確認

- plan-014-jeju-family 表格線條尚未補（有未發布變更）。
- 表格線條粗細 1px 無法由 API 設定；若前台看起來不是 1px，請提供正確範例文章網址再對照。
