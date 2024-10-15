# Создаем переменную string, выводим ее в консоль. Создаем слайс, выводим переменную с помощью слайса, меняем текст с помощью слайса. выводим текст побуквенно с помощью рун.
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

}
```


