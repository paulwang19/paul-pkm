---
tags:
  - 🔗
---
# 連結的概念

- [[CSS 長度單位]]
- [[CSS - display 屬性]]

# 瀏覽器如何決定元素寬度與高度？

## 寬度 - 由外而內，看我是誰

對於 `width: auto` 元素的寬度行為，取決於該元素[[CSS - display 屬性|不同 display 屬性]]

### 區塊級元素 (`display: block`)

- 例如 : `<div>`, `<p>`, `<h1>`, `<section>`, `<article>` 等
- 行為 : 盡可能佔滿父層可用寬度

對於區塊級元素，瀏覽器計算出父元素寬度後，即對此 `width: auto` 的元素寬度設為父元素寬度，這也是為什麼 `<div>` 預設會占滿一整行

### 行內級元素 (`display: inline`)

- 例如： `<span>`, `<a>`, `<strong>`, `<em>` 等
- 行為：由內容決定寬度

行內級元素會依據其子元素內容來計算寬度，例如 `<span>` 的寬度會剛好包住它裡面的文字，不會試圖占滿父元素寬度

## 高度 - 由內而外，內容撐開我

計算 `height: auto` 元素時，其高度取決於子內容高度，會等於其內部所有子內容加總起來的高度，例如 : 內部有一個 200 px 高的圖片，則此高度至少就是 200 px

也因為如此，當元素設定 `height: 100%` 時，由於其父元素依賴子元素的實際高度，子元素又需要父元素的高度才能計算 100% 的高度，造成「循環依賴」，因此此時 `height: 100%` 會失效

# 瀏覽器渲染順序

通常是先計算寬度，再計算高度
1. 寬度 : 從最外層 `<html>` 開始，由外而內計算，父層寬度決定了子層 `width: auto` 寬度
2. 高度 : 確定寬度後，代表得知了內容文字在哪裡換行，進而能計算元素高度。此時瀏覽器由內到外，逐一計算所有父層 `height: auto` 的高度，最終撐開所有父元素

# `position: absolute` 計算規則

有時彈出的對話框、或是警告窗，會用到 `position: absolute` 來固定其出現位置，不隨著正常排版流，此時同樣會改變瀏覽器最此元素的長度計算方式

## 對其父元素的改變

- 無視 : 父元素在計算自己的 `height: auto` 時，會當這個子元素不存在。
	- 如果一個父元素 `<div>` 內部_只有_一個 `position: absolute` 的子元素，那麼這個父元素的高度將會塌陷 (collapse) 為 0

## 元素本身的改變

- 脫離正常排版流 : 其下面的元素會往上移動，好像這個 `position: absolute` 不存在一樣
- 寬度 : 對於 `width: auto` 的元素，寬度會採用收縮到內容大小 (shrink-to-fit) 的計算方式（就像是 `display: inline-block`）
- 定位 : 將定位於相對其「定位祖先 (Positioned Ancestor)」的位置

> [!tip] 定位祖先 (Positioned Ancestor)
> 也稱為包含塊 (Containing Block)，這是元素用來計算位置和百分比寬度的基準。定位祖先指的是離 `absolute` 元素最近的、`position` 值不是 `static`（即 `relative`, `absolute`, `fixed` 或 `sticky`）的祖先元素。如果一個都沒有，基準就是 `<html>` 元素。
> 
> 註記 : HTML 元素 `position` 預設值即為 `static`

### 動態獲取元素定位祖先方法

1. 打開開發者工具 (F12)，切換到「主控台」(Console) 頁籤
2. 先選中要查詢的元素：
    - 方法 A：在「元素」(Elements) 頁籤中點選該元素，然後在 Console 中輸入 `$0`。 (`$0` 代表在「元素」頁籤中最後點選的項目)
    - 方法 B：如果您的元素有 ID (例如 `menu`)，可以直接輸入 `document.getElementById('menu')`
3. 接著，在該元素後面加上 `.offsetParent` 並按下 Enter
        
**範例** :
```
$0.offsetParent
```