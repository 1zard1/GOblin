# fizzBuzz proga

```go
package main

import (
    "fmt"
)

func main() {
    slice := []int{1, 2, 3, 4, 5}
    fmt.Println(fizzBuzz(slice))
}

func fizzBuzz(slice []int) []string {
    result := make([]string, len(slice))
    for i, num := range slice {
        if num%15 == 0 {
            result[i] = "FizzBuzz"
        } else if num%3 == 0 {
            result[i] = "Fizz"
        } else if num%5 == 0 {
            result[i] = "Buzz"
        } else {
            result[i] = fmt.Sprintf("%d", num)
        }
    }
    return result
}
```
