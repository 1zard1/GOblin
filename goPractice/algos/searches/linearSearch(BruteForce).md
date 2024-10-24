```go
package main

import "fmt"

func main() {
	numbers := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	needNumber := 1
	fmt.Println(linPoisk(numbers, needNumber))
}

func linPoisk(numbers []int, needNumber int) string {
	for _, i := range numbers {
		if i == needNumber {
			return "Ваше число есть в массиве/слайсе"
		}
	}
	return "Вашего числа нет в массиве/слайсе"
}
```
