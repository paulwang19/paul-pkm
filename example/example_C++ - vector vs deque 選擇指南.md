```markdown
---
tags:
  - 🔗
  - C++
---
# 連結的概念

- [[C++ - vector]]
- [[C++ - deque]]

## 核心洞察

**預設用 vector，除非需要頻繁在頭部插入/刪除。**

原因：
- vector 記憶體完全連續 → cache 效能更好
- vector 記憶體開銷更小
- deque 唯一優勢是頭部操作 O(1)

## 快速比較

| 操作 | vector | deque |
|------|--------|-------|
| 隨機存取 `[]` | O(1) ⚡ | O(1) |
| 尾端插入 `push_back` | 平均 O(1) | O(1) |
| **頭端插入** `push_front` | **O(n)** | **O(1)** ⭐ |
| **頭端刪除** `pop_front` | **O(n)** | **O(1)** ⭐ |
| 記憶體 | 完全連續 | 分段連續 |

**關鍵差異**：只有頭端操作！

## 決策流程

	需要頻繁在頭部插入/刪除？
		│
		├─ 是 → deque
		└─ 否 → vector（預設選擇）

## 實際場景

### ✅ 用 vector

	// 一般動態陣列
	vector<int> data;
	data.push_back(1);
	
	// 堆疊（只在尾端操作）
	vector<int> stack;
	stack.push_back(x);
	stack.pop_back();

### ✅ 用 deque

	// 佇列（頭部刪除，尾部插入）
	deque<int> queue;
	queue.push_back(x);    // 入隊
	queue.pop_front();     // 出隊
	
	// 滑動視窗
	deque<int> window;
	window.push_back(x);   // 加入尾端
	window.pop_front();    // 移除頭端

### ✅✅ 用標準適配器

	queue<int> q;        // 佇列（底層預設 deque）
	stack<int> s;        // 堆疊（底層預設 deque）

## 效能測試（1,000,000 元素）

| 測試 | vector | deque | 結論 |
|------|--------|-------|------|
| 尾端插入 | 5ms | 8ms | vector 較快 |
| 頭端插入（10K） | 1500ms | 2ms | deque 壓倒性勝利 |
| 隨機存取 | 10ms | 15ms | vector 較快 |

## 總結

- **預設**：vector
- **佇列**：deque 或 `queue`
- **堆疊**：vector 或 `stack`
- **滑動視窗**：deque
```
