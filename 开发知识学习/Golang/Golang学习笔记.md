# Golang学习笔记

> 来源：字节跳动2022后端青训营线上课程
>
> 平台：Windows10 + VS Code

[TOC]

## 基础语法

### Hello World

代码

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("hello world")
}
```

### 变量

- var name = val 形式可以直接给变量赋值，<u>一般可以自动推导变量类型</u>
- 如有需要规定变量类型，可以在name后显式地定义变量类型，如代码第10行
- name := val 是另一种变量声明赋值形式
- 常量则是将 var 改为 const 即可，Golang 中常量没有确定的类型，它会根据使用的上下文自动确定类型。

代码

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	var a = "initial"
	var b, c int = 1, 2
	var d = true
	var e float64
	f := float32(e)

	g := a + "foo"
	fmt.Println(a, b, c, d, e, f) // initial 1 2 true 0 0
	fmt.Println(g)                // initialfoo

	const s string = "constant"
	const h = 500000000
	const i = 3e20 / h
	fmt.Println(s, h, i, math.Sin(h), math.Sin(i))
}
```

运行结果

```go
initial 1 2 true 0 0
initialfoo
constant 500000000 6e+11 -0.28470407323754404 0.7591864109375384
```

### 条件判断语句 if else

- Golang 中的 if else 写法与 C 类似，不同点是 if 后的条件语句没有括号，如果写了括号在保存时编辑器会自动去掉括号
- 条件语句后必须有大括号，不能像其它语言一样将执行语句和条件语句写在同一行

代码

```go
package main

import "fmt"

func main() {
	if 7%2 == 0 {
		fmt.Println("7 is even")
	} else {
		fmt.Println("7 is odd")
	}

	if 8%4 == 0 {
		fmt.Println("8 is divisible by 4")
	}

	if num := 9; num < 0 {
		fmt.Println(num, "is negative")
	} else if num < 10 {
		fmt.Println(num, "has 1 digit")
	} else {
		fmt.Println(num, "has multiple digits")
	}
}
```

运行结果

```go
7 is odd
8 is divisible by 4
9 has 1 digit
```

### 循环

代码

```go
package main

import "fmt"

func main() {
	i := 1
	for {
		fmt.Println("loop")
		break
	}
	for j := 7; j < 9; j++ {
		fmt.Println(j)
	}

	for n := 0; n < 5; n++ {
		if n%2 == 0 {
			continue
		}
		fmt.Println(n)
	}
	for i <= 3 {
		fmt.Println(i)
		i = i + 1
	}
}
```

运行结果

```go
loop
7
8
1
3
1
2
3
```

### 分支结构switch

- Golang 的 switch 语句比 C/C++ 更加强大，可以使用任意变量类型作条件甚至可以不使用变量直接使用，取代多个 if else 嵌套来让代码更加清晰易懂，如看代码24行

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	a := 2
	switch a {
	case 1:
		fmt.Println("one")
	case 2:
		fmt.Println("two")
	case 3:
		fmt.Println("three")
	case 4, 5:
		fmt.Println("four or five")
	default:
		fmt.Println("other")
	}

	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("It's before noon")
	default:
		fmt.Println("It's after noom")
	}
}
```

运行结果

```go
two
It's after noon
```

### 数组

- 真实应用场景中一般很少使用数组，因为它是定长的，更多使用的是切片

代码

```go
package main

import (
	"fmt"
)

func main() {
	var a [5]int
	a[4] = 100
	fmt.Println(a[4], len(a))

	b := [5]int{1, 2, 3, 4, 5}
	fmt.Println(b)

	var twoD [2][3]int
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			twoD[i][j] = i + j
		}
	}
	fmt.Println("2d: ", twoD)
}
```

运行结果

```go
100 5
[1 2 3 4 5]
2d:  [[0 1 2] [1 2 3]]
```

### 切片

- 使用make创建切片，创建后可以当作数组使用
- 使用append可以实现尾添加，但是一定要赋值回原切片，因为切片存储了数据和长度，若添加元素超出原来长度则会进行扩容，添加后会生成一个新的切片，所以要将新的切片赋值给原来的切片
- make创建切片时也可以指定长度，减少扩容带来的花销
- 使用copy可以复制切片
- slice[start : end]可以取出对应区间的元素

代码

```go
package main

import (
	"fmt"
)

func main() {
	s := make([]string, 3)
	s[0] = "a"
	s[1] = "b"
	s[2] = "c"
	fmt.Println("get:", s[2])   // c
	fmt.Println("len:", len(s)) // 3

	s = append(s, "d")
	s = append(s, "e", "f")
	fmt.Println(s) // [a b c d e f]

	c := make([]string, len(s))
	copy(c, s)
	fmt.Println(c) // [a b c d e f]

	fmt.Println(s[2:5]) // [c d e]
	fmt.Println(s[:5])  // [a b c d e]
	fmt.Println(s[2:])  // [c d e f]

	good := []string{"g", "o", "o", "d"}
	fmt.Println(good) // [g o o d]
}
```

运行结果

```go
get: c
len: 3
[a b c d e f]
[a b c d e f]
[c d e]
[a b c d e]
[c d e f]
[g o o d]
```

### map（hash/字典）

- make ( map [ keyType ] valType ) 来创建map

代码

```go
package main

import (
	"fmt"
)

func main() {
	m := make(map[string]int)
	m["one"] = 1
	m["two"] = 2
	fmt.Println(m)           // map[one:1 two:2]
	fmt.Println(len(m))      // 2
	fmt.Println(m["one"])    // 1
	fmt.Println(m["unknow"]) // 0

	r, ok := m["unknow"]
	fmt.Println(r, ok) // 0 false

	delete(m, "one")

	m2 := map[string]int{"one": 1, "two": 2}
	var m3 = map[string]int{"one": 1, "two": 2}
	fmt.Println(m2, m3)
}
```

运行结果

```go
map[one:1 two:2]
2
1
0
0 false
map[one:1 two:2] map[one:1 two:2]
```

### range（迭代器）

- 对于数组，range遍历会返回两个值，一个索引一个值，如果不需要索引可以用下划线忽略
- 对于map，range遍历返回的第一个值是key，第二个值是val

代码

```go
package main

import (
	"fmt"
)

func main() {
	nums := []int{2, 3, 4}
	sum := 0
	for i, num := range nums {
		sum += num
		if num == 2 {
			fmt.Println("index:", i, "num:", num) // index:0 num:2
		}
	}
	fmt.Println(sum) // 9

	m := map[string]string{"a": "A", "b": "B"}
	for k, v := range m {
		fmt.Println(k, v) // b 8; a A
	}
	for k := range m {
		fmt.Println("key", k) // key a; key b
	}
}
```

运行结果

```go
index: 0 num: 2
9
a A
b B
key a
key b
```

### 函数

代码

```go
package main

import (
	"fmt"
)

func add(a int, b int) int {
	return a + b
}

func add2(a, b int) int {
	return a + b
}

func exsists(m map[string]string, k string) (v string, ok bool) {
	v, ok = m[k]
	return v, ok
}

func main() {
	res := add(1, 2)
	fmt.Println(res) // 3

	v, ok := exsists(map[string]string{"a": "A"}, "a")
	fmt.Println(v, ok) // A True
}
```

运行结果

```go
3
A true
```

### 指针

代码

```go
package main

import (
	"fmt"
)

func add2(n int) {
	n += 2
}

func add2ptr(n *int) {
	*n += 2
}

func main() {
	n := 5
	add2(n)
	fmt.Println(n) // 5
	add2ptr(&n)
	fmt.Println(n) // 7
}
```

运行结果

```go
5
7
```

### 结构体

代码

```go

```

