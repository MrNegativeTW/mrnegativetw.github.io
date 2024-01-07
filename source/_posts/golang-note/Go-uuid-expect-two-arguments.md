---
title: 使用 satori/go.uuid 卻收到 expects two arguments
date: 2024-01-05 10:00:00
tags:
categories:
- GoLang 筆記
---

前言：你接手了一個舊專案，你嘗試將這個專案 build and run，然後發生了 expects two arguments，WTF?

<!--more-->

專案中有用到 [satori/go.uuid](https://github.com/satori/go.uuid)，但只有你 build failed，警告訊息是 `expects two arguments`

```go
func dosomething(url string) (output string, err error) {
  // ...
  random_name := uuid.Must(uuid.NewV4())
  // ...
}
```

沒事，換一下 branch 就好

```
go mod edit -replace=github.com/satori/go.uuid@v1.2.0=github.com/satori/go.uuid@master
go mod tidy
go build
```

最後你的 go.mod 應該會長這樣

```
module yourmomsofat

go 1.14
require (
  ...
  github.com/satori/go.uuid v1.2.0
 )

replace github.com/satori/go.uuid v1.2.0 => github.com/satori/go.uuid v1.2.1-0.20181028125025-b2ce2384e17b
```

---

**Refer:**

- [not enough arguments in call to uuid.Must #70](https://github.com/satori/go.uuid/issues/70)