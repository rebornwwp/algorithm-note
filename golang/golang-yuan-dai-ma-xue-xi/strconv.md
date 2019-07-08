# strings

## 字母转换

ASCII编码中，**'a'**的编码是97，**'A'**的编码是65，并且**'a'-'A'**的结果是32，32的二进制编码是'0b100000

```go
// 将字符串中所有的大写字母变成小写字母
func lower(c byte) byte {
	return c | uint8('x' - 'X')
}

// 将字符串中所有的大写字母变成小写字母
func upper(c byte) byte {
	return c & ^uint8('x' - 'X')
}
```

## go语言返回方式

```go
// 自动就能返回相应的值，默认是类型的'零'值
func f() (i int, ok bool) {
    return
}
```

## 计算int类型的二进制的位数

```go
// ^unint(0) >> 63 结果0或者1
const intSize = 32 << (^uint(0) >> 63)
```

