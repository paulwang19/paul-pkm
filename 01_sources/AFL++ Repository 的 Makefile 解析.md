---
tags:
  - 📥
---
# 來源資訊

- **Repository** : [AFLplusplus](https://github.com/AFLplusplus/AFLplusplus)
- **File** : [./Makefile](https://github.com/AFLplusplus/AFLplusplus/blob/stable/Makefile)

# 原始碼片段

```makefile
all:
	@echo trying to use GNU make...
	@gmake all || echo please install GNUmake

source-only:
	@gmake source-only

binary-only:
	@gmake binary-only

distrib:
	@gmake distrib
```

# 詢問 Copilot

Copilot 告訴我這份檔案是將所有指令轉發給 `gmake` (GNU make) 執行

# 我的疑問

1. 除了有 `make`，還有 `gmake`，是什麼東西? 兩個差在哪裡?
2. 語法中的 `@` 是甚麼意思?

# 待提煉

- [ ] make (裡面提到 gmake)
- [ ] Makefile - @
