---
title: "関数"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

関数について復習
Pythonについても勉強

# Pythonの関数
引数と戻り値の型は以下のようにつける。
```python
def triangleArea(width: int, height: int) -> int:
    return width * height / 2

print (triangleArea(5, 4))
```

# 文字列を返す
```python
# 姓、名をそれぞれ受け取り、イニシャルを返すgetInitialという関数を作成します
def getInitial(lastName: str, firstName: str) -> str:
    return lastName[0] + '.' + firstName[0]
```

# Go

```go
package main

import (
    "fmt"
    "math"
)

func vacationRental(people int32, day int32) int32{
    // 関数を完成させてください
    var total int32 = 0
    if day <= 3 {
        total = people * 80 * day
    }
    if day >= 4 {
        total = people * 60 * day
    }
    if day >= 10 {
        total = people * 50 * day
    }

    totalFloat := float64(total) * 1.12 * 1.08
    return int32(math.Floor(totalFloat))
}


```
GoはPHPと違って型が厳しい。
引数で受け取っているpeopleやdayがintだが、計算する際は1.12などのfloat型と掛け算をしなければならない。
intとfloatは掛け算できないため、一時的にキャストしている。

# なぜか通らない
```go
package main

import (
    "fmt"
    "math"
)

func howMuchIsYourDebt(year int32) int32{
    // 関数を完成させてください
    total := 10000 * math.Pow(1.2, float64(year))
    fmt.Println(float64(year))
    return int32(math.Floor(total))
}

```