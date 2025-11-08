```markdown
---
tags:
  - 📖
  - C++
---
# 概念

vector 是動態陣列，支援隨機存取。記憶體連續儲存，可自動擴展。

**預設首選**：不確定用什麼容器時，先用 vector。

# 基礎操作

## 建立

	#include <vector>
	using namespace std;
	
	vector<int> v1;                    // 空
	vector<int> v2(10);                // 10 個 0
	vector<int> v3(10, 5);             // 10 個 5
	vector<int> v4 = {1, 2, 3, 4, 5};  // 初始化列表
	vector<int> v5(v4);                // 複製

## 存取

	v[i];        // 不檢查邊界
	v.at(i);     // 檢查邊界，越界拋例外
	v.front();   // 第一個元素
	v.back();    // 最後一個元素
	v.data();    // 指向底層陣列的指標

## 大小與容量

	v.size();         // 元素數量
	v.empty();        // 是否為空
	v.capacity();     // 容量
	v.reserve(100);   // 預留空間
	v.resize(50);     // 改變大小
	v.shrink_to_fit(); // 釋放多餘空間

## 修改

	v.push_back(x);    // 尾端插入
	v.pop_back();      // 尾端刪除
	v.insert(pos, x);  // 指定位置插入
	v.erase(pos);      // 刪除指定位置
	v.clear();         // 清空
	v.emplace_back(args...); // 原地構造（更高效）

## 走訪

	// 索引
	for (int i = 0; i < v.size(); i++) {
	    cout << v[i];
	}
	
	// 範圍 for（推薦）
	for (int x : v) {
	    cout << x;
	}
	
	// 範圍 for + 參考（修改元素）
	for (int& x : v) {
	    x *= 2;
	}
	
	// 迭代器
	for (auto it = v.begin(); it != v.end(); ++it) {
	    cout << *it;
	}

## 常用演算法

	#include <algorithm>
	
	sort(v.begin(), v.end());              // 排序
	reverse(v.begin(), v.end());           // 反轉
	auto it = find(v.begin(), v.end(), 5); // 查找
	int cnt = count(v.begin(), v.end(), 5); // 計數
	
	// 去重（需先排序）
	sort(v.begin(), v.end());
	v.erase(unique(v.begin(), v.end()), v.end());

# 時間複雜度

| 操作 | 複雜度 |
|------|--------|
| 隨機存取 `v[i]` | O(1) |
| 尾端插入 `push_back` | 平均 O(1) |
| 尾端刪除 `pop_back` | O(1) |
| 頭部插入/刪除 | O(n) |
| 中間插入/刪除 | O(n) |
| 查找 `find` | O(n) |

# 關鍵注意事項

1. **`size` ≠ `capacity`**
   - `size`：實際元素數量
   - `capacity`：已分配空間
2. **迭代器失效**
	v.push_back(x);  // 可能觸發擴展，所有迭代器失效
	v.erase(it);     // it 失效，需重新獲取
3. **`reserve` vs `resize`**
   - `reserve(n)`：預留空間，`size` 不變
   - `resize(n)`：改變大小，可立即存取
4. **避免頻繁擴展**
   v.reserve(1000000);  // 事先預留，避免多次擴展
5. **不要用 `vector<bool>`**
   - 特殊化版本，用 bit 存儲
   - 改用 `vector<char>` 或 `deque<bool>`

## 參考

- [cppreference - vector](https://en.cppreference.com/w/cpp/container/vector)
```