# Задача Дано целое число num. Повторно складывайте все его цифры, пока результат не станет однозначным числом, и верните его. Дополнительное задание: сделать это без использования циклов/рекурсии за O(1) время выполнения.

```go
package main

import (
    "fmt"
)

func addDigits(num int) int {
    if num == 0 {
        return 0
    }
    return 1 + (num-1)%9
}

func main() {
    fmt.Println(addDigits(38)) // Output: 2
    fmt.Println(addDigits(0))  // Output: 0
}
```
