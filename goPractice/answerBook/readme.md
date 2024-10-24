# Слайсы, Мапы
++++++++++
 Задача 1
++++++++++
Что выведет код?
```go
func main() {
	v := []int{3, 4, 1, 2, 5}
	ap(v)
	sr(v)
	fmt.Println(v)
}

func ap(arr []int) {
	arr = append(arr, 10)
}

func sr(arr []int) {
	sort.Ints(arr)
}
```
Output: [1,2,3,4,5]. Пояснение: Функция ap не сработала, потому что в Go аргументы передаются по значению, а не по ссылке. Когда вы передаете срез arr в функцию ap, создается копия этого среза. Изменения, сделанные в копии, не влияют на оригинальный срез.

