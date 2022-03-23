# GDB 调试 Golang

## 初始化

1. 打开gdb初始化配置文件

```
vim ~/.gdbinit

# 添加新的一行
add-auto-load-safe-path /usr/local/go/src/runtime/runtime-gdb.py
```

2. 编写调试代码 `main.go`

```go

package main

import "fmt"

func main() {
	var result [100]int
	result[0] = addRange(1, 10)
	result[1] = addRange(1, 100)
	fmt.Printf("result[0]=%d\nresult[1]=%d\n", result[0], result[1])
}

func addRange(low int, high int) int {
	var i, sum int
	for i = low; i <= high; i++ {
		sum = sum + i
	}
	return sum
}

```

3. 编译调试,要注意以下几点

* gdb >= 7.5
* 将'-w'标志传递给链接器以省略调试信息
* 传递 -gcflags "-N -l" 参数，这样可以忽略 Go 内部做的一些优化，聚合变量和函数等优化，这样对于 GDB 调试来说非常困难，所以在编译的时候加入这两个参数避免这些优化。

```
go build -gcflags=all="-N -l" -ldflags=-w  main.go
```
