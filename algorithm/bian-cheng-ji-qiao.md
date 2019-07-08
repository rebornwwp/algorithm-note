# 编程技巧

## 窗函数

当需要窗函数去框选小于等于X长度的时候

```go
l := []int{1,2,3,4,5,6,7,8,9]
windowSum := 0
X = 3 // 窗大小
for i := range l {
    windowSum += l[i]
    if i >= X {
        windowSum -= l[i-X]
    }
    fmt.Println(windowSum)
}


// 当需要对左边界做限制
l := []int{1,2,3,4,5,6,7,8,9]
windowSum := 0
X = 3 // 窗大小
for i := range l {
    windowSum += l[i]
    if i >= X && (i-X对应值的限制条件) {
        windowSum -= l[i-X]
    }
    fmt.Println(windowSum)
}
```



