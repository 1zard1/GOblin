# ДЗ1
```go
package main

import "fmt"

func main() {
	var helloVladislave string = "Hello Vladislave!" // Создали переменную стринг

	fmt.Println(helloVladislave) // Вывели переменную в консоль

	slisik := []string{helloVladislave} // Создали слайс содержащий переменную

	fmt.Println(slisik) // Вывели слайс содержащий переменную в консоль

	slisik[0] = "helloVladislav" // Меняем текст с помощью слайса

	fmt.Println(slisik) // Вывели слайс содержащий переменную в консоль

	for _, runeValue := range slisik[0] { // Перебираем строку внутри слайса
		fmt.Printf("%c ", runeValue)
	}
}
```


