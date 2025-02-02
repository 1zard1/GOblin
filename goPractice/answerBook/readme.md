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
Output: [1 2 3 4 5]. Пояснение: Функция ap не сработала, потому что в Go аргументы передаются по значению, а не по ссылке. Когда вы передаете срез arr в функцию ap, создается копия этого среза. Изменения, сделанные в копии, не влияют на оригинальный срез.

++++++++++
 Задача 2
++++++++++
Что выведет код?

```go
var foo []int
var bar []int

foo = append(foo, 1)
foo = append(foo, 2)
foo = append(foo, 3)
bar = append(foo, 4)
foo = append(foo, 5)

fmt.Println(foo, bar)
```
Output: [1 2 3 5] [1 2 3 5]. Пояснение: В вашем коде переменная bar не содержит ожидаемых значений, потому что она ссылается на тот же базовый массив, что и foo, в момент вызова append(foo, 4). Когда вы добавляете элемент 5 в foo, это изменение также влияет на bar.

++++++++++
 Задача 3
++++++++++
Что выведется?
```go
package main
import "fmt"
func main() {
  c := []string{"A", "B", "D", "E"}
  b := c[1:2]
  b = append(b, "TT")
  fmt.Println(c)
  fmt.Println(b)
}
```
Output: [A B TT E]
	[B TT]
Пояснение: В вашем коде, когда вы выполняете append(b, "TT"), это может изменить базовый массив, на который ссылается срез c.

++++++++++
 Задача 4
++++++++++
1. Что выведет код?

```go
func main() {
  var m map[string]int
  for _, word := range []string{"hello", "world", "from", "the",
    "best", "language", "in", "the", "world"} {
    m[word]++
  }
  for k, v := range m {
    fmt.Println(k, v)
  }
}
```
Output: panic. Пояснение: код не работает, потому что не инициализировали карту m. В Go карты нужно инициализировать перед использованием, иначе попытка доступа к ним приведет к панике (runtime panic)/
Fix: инициализировать мапу через make.

++++++++++
 Задача 5
++++++++++
1. Что будет в результате выполнения?
```go
mutate := func(a []int) {
  a[0] = 0
  a = append(a, 1)
  fmt.Println(a)
}
a := []int{1, 2, 3, 4}
mutate(a)
fmt.Println(a)
```
Output:[0 2 3 4 1] 
       [0 2 3 4]
Пояснение: Изменение элемента слайса внутри функции влияет на исходный слайс, но добавление элемента с помощью append создает новый слайс, не изменяя исходный.

++++++++++
 Задача 6
++++++++++
1. Что выведется?
2. Зная обо всех таких нюансах, которые могут возникнуть, какие есть рекомендации?

------Вариант 1
-----------
```go
func mod(a []int) {
  for i := range a {
    a[i] = 5
  }
  fmt.Println(a)
}
func main() {
  sl := []int{1, 2, 3, 5}
  mod(sl)
  fmt.Println(sl)
}
```
Output: [5 5 5 5 5][5 5 5 5 5].
Если за нюанс мы принимаем тот факт, что изменяется оригинальный слайс, то стоит просто передавать в функцию его копию.

code fix:
```go
func mod(a []int) {
    b := make([]int, len(a), len(a))
    copy(b, a)
    for i := range b {
        b[i] = 5
    }
    fmt.Println(b)
}

func main() {
    sl := []int{1, 2, 3, 5}
    mod(sl)
    fmt.Println(sl) // Вывод:
}
```
------ Вариант 2
по аналогии с вар 1
------ Вариант 3
по аналогии с вар 1
------ Вариант 4
почти по аналогии с вар 1. Исключением является то, что тут не меняется базовый массив. А не меняется он потому что при вызове функции append() емкость слайса уже заполнена и он просто создает новый слайс в котором и проводит все последующие операции.

++++++++++
 Задача 7
++++++++++
1. Что будет содержать s после инициализации?
2. Что произойдет в println для слайса и для мапы?
```go
func a(s []int) {
    s = append(s, 37)
}

func b(m map[int]int) {
    m[3] = 33
}

func main() {
    s := make([]int, 3, 8)
    m := make(map[int]int, 8)

    // add to slice
    a(s)
    println(s[3]) //?

    // add to map
    b(m)
    println(m[3]) //?
}
```
1. После инициализации s будет содержать 3 нуля "пустые" значения типа int. 
2. При попытке вывода println(s[3]) будет паника, так как в слайсе всего 3 элемента, а мы пытаемся достать 4. При выводе мапы будет видно измененное значение с индексом 3, т.е 33.


++++++++++
 Задача 8
++++++++++
1. Расскажи подробно что происходит

Вариант 1
-----------
```go
package main

import "fmt"

func main() {
    a := []int{1,2}
    a = append(a, 3)
    b := append(a, 4)
    c := append(a, 5)

    fmt.Println(b)
    fmt.Println(c)
}
```
Output: [1 2 3 5] [1 2 3 5]
Пояснение: Создание и изменение слайса a:
a := []int{1, 2} создает слайс a с элементами [1, 2].
a = append(a, 3) добавляет элемент 3 к слайсу a, и теперь a содержит [1, 2, 3].
Создание слайсов b и c:
b := append(a, 4) создает новый слайс b, добавляя элемент 4 к слайсу a. Теперь b содержит [1, 2, 3, 4].
c := append(a, 5) создает новый слайс c, добавляя элемент 5 к слайсу a. Теперь c содержит [1, 2, 3, 5].
Изменение базового массива:
Когда вы вызываете append(a, 4) и append(a, 5), оба вызова могут использовать один и тот же базовый массив, если емкость позволяет. В данном случае, емкость слайса a позволяет добавить элементы без выделения новой памяти.
Поэтому, когда вы добавляете 5 к слайсу a, это изменение также влияет на слайс b, так как они используют один и тот же базовый массив.
Вывод слайсов:
fmt.Println(b) выводит [1, 2, 3, 5], так как базовый массив был изменен при добавлении 5 к слайсу a.
fmt.Println(c) также выводит [1, 2, 3, 5] по той же причине.

Вариант 2
-----------
```go
package main

import "fmt"

func main() {
    a := []int{1,2}
    a = append(a, 3)
    a = append(a, 7)
    b := append(a, 4)
    c := append(a, 5)

    fmt.Println(b)
    fmt.Println(c)
}

```
Output: [1 2 3 7 4] [1 2 3 7 5].
Пояснение:Создание и изменение слайса a:
a := []int{1, 2} создает слайс a с элементами [1, 2].
a = append(a, 3) добавляет элемент 3 к слайсу a, и теперь a содержит [1, 2, 3].
a = append(a, 7) добавляет элемент 7 к слайсу a, и теперь a содержит [1, 2, 3, 7].
Создание слайсов b и c:
b := append(a, 4) создает новый слайс b, добавляя элемент 4 к слайсу a. Теперь b содержит [1, 2, 3, 7, 4].
c := append(a, 5) создает новый слайс c, добавляя элемент 5 к слайсу a. Теперь c содержит [1, 2, 3, 7, 5].
Изменение базового массива:
Когда вы вызываете append(a, 4), емкость слайса a может быть достаточной для добавления элемента без выделения новой памяти. В этом случае b и a будут указывать на один и тот же базовый массив.
Однако, когда вы вызываете append(a, 5), если емкость слайса a недостаточна, создается новый базовый массив, и c начинает указывать на этот новый массив.
Вывод слайсов:
fmt.Println(b) выводит [1, 2, 3, 7, 4], так как b указывает на исходный базовый массив, который был изменен при добавлении 4.
fmt.Println(c) выводит [1, 2, 3, 7, 5], так как c указывает на новый базовый массив, который был создан при добавлении 5.




# Многопоточка
++++++++++
 Задача 1
++++++++++
Что выведет код? Исправить все проблемы
```go
func main() {
	ch := make(chan int)
	wg := &sync.WaitGroup{}
	wg.Add(3)
	for i := 0; i < 3; i++ {
		go func(v int) {
			defer wg.Done()
			ch <- v * v
		}(i)
	}
	wg.Wait()
	var sum int
	for v := range ch {
		sum += v
	}
	fmt.Printf("result: %d\n", sum)
}
```
Output: result: 5.
Fix code
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	ch := make(chan int)
	wg := &sync.WaitGroup{}
	wg.Add(3)
	for i := 0; i < 3; i++ {
		go func(v int) {
			defer wg.Done()
			ch <- v * v
		}(i)
	}

	// Закрываем канал после завершения всех горутин
	go func() {
		wg.Wait()
		close(ch)
	}()

	var sum int
	for v := range ch {
		sum += v
	}
	fmt.Printf("result: %d\n", sum)
}
```
Пояснение:
Закрытие канала:
Канал ch никогда не закрывается, что приводит к бесконечному ожиданию в цикле for v := range ch. Это вызывает deadlock.
Чтение из канала после завершения всех горутин:
Вы пытаетесь читать из канала после завершения всех горутин, но не указываете, когда чтение должно прекратиться.

++++++++++
 Задача 2
++++++++++
Что выведет код? Должны выводится все значения
```go
func main() {
	a := 5000
	for i := 0; i < a; i++ {
		go fmt.Println(i)
	}
}
```
Output: код запускает 5000 горутин, каждая из которых выводит число от 0 дл 4999. есть потециальная проблема: main функция может завершиться до того, как все горутины завершат выполнение, что в свою очередь ведет к неполному выводу или его отсутствию. Для предотвращения сего безобразия можно использовать WaitGroup из пакета sync. Код будет выглядеть следующим образом:
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	a := 5000

	for i := 0; i < a; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			fmt.Println(i)
		}(i)
	}

	wg.Wait()
}
```

++++++++++
 Задача 3
++++++++++
Будет ошибка что все горутины заблокированы. Какие горутины будут заблокированы? И почему?
```go
package main
import "fmt"
func main() {
  ch := make(chan int)
  ch <- 1
  go func() {
    fmt.Println(<-ch)
  }()
}
```
Главная горутина(main func) будет заблокирована, потому что мы пытаемся отправить значение в канал ch до того, как горутина начнет его считывать.
Исправленный код:
```go
package main

import "fmt"

func main() {
	ch := make(chan int)
	
	go func() {
		fmt.Println(<-ch)
	}()
	
	ch <- 1
}
```

++++++++++
 Задача 4
++++++++++
1. Как это работает, что не так, что поправить?
```go
func main() {
  ch := make(chan bool)
  ch <- true
  go func() {
    <-ch
  }()
  ch <-true
}
```
Логика проста, записываем в канал тру, горутиной возвращаем значение из канала, но при попытке новой записи возникает дедлок. Фикс происходит простой буферизацией канала, что бы в него можно было записать не только 1 значение.
Fix:
```go
func main() {
  ch := make(chan bool, 1)
  ch <- true
  go func() {
    <-ch
  }()
  ch <-true
}
```

++++++++++
 Задача 5
++++++++++
1. Как будет работать код?
2. Как сделать так, чтобы выводился только первый ch?
```go
func main() {
        ch := make(chan bool)
        ch2 := make(chan bool)
        ch3 := make(chan bool)
        go func() {
                ch <- true
        }()
        go func() {
                ch2 <- true
        }()
        go func() {
                ch3 <- true
        }()

        select {
        case <-ch:
                fmt.Printf("val from ch")
        case <-ch2:
                fmt.Printf("val from ch2")
        case <-ch3:
                fmt.Printf("val from ch3")
        }
}
```
В этом коде мы создаем 3 канала в которые посредством горутин записываются true значения. после этого конструкцией select возвращается значение горутина которай завершилась первой. Для того что бы выводить только ch мы можем использовать метод sleep, тем самым до завершения главной горутины будет успевать срабатывать только горутина ch. 
```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch := make(chan bool)
	ch2 := make(chan bool)
	ch3 := make(chan bool)

	go func() {
		ch <- true
	}()

	go func() {
		time.Sleep(10 * time.Second) // Слипаем ненужную горутину
		ch2 <- true
	}()
	go func() {
		time.Sleep(10 * time.Second) // Слипаем ненужную горутину
		ch3 <- true
	}()

	select {
	case <-ch:
		fmt.Printf("val from ch\n")
	case <-ch2:
		fmt.Printf("val from ch2\n")
	case <-ch3:
		fmt.Printf("val from ch3\n")
	}
}
```
++++++++++
 Задача 6
++++++++++
1. Что выведет код и как исправить?
```go
var globalMap = map[string][]int{"test": make([]int, 0), "test2": make([]int, 0), "test3": make([]int, 0)}
var a = 0
 
func main() {
    wg := sync.WaitGroup{}
    wg.Add(3)
    go func() {
        wg.Done()
        a=10
        globalMap["test"] = append(globalMap["test"], a)
         
    }()
    go func() {
        wg.Done()
        a=11
        globalMap["test2"] = append(globalMap["test2"], a)
    }()
    go func() {
        wg.Done()
        a=12
        globalMap["test3"] = append(globalMap["test3"], a)
    }()
    wg.Wait()
    fmt.Printf("%v", globalMap)
    fmt.Printf("%d", a)
}
```
Код выведет мапу с рандомно разбросанными 10 11 и 12, проблема возникает изза конкурентного доступа к переменной a. Что бы это исправить воспользуемся пакетом sync, а именно мьютексами.
```go
package main

import (
	"fmt"
	"sync"
)

var globalMap = map[string][]int{"test": make([]int, 0), "test2": make([]int, 0), "test3": make([]int, 0)}
var a = 0
var mu sync.Mutex

func main() {
	wg := sync.WaitGroup{}
	wg.Add(3)

	go func() {
		defer wg.Done()
		mu.Lock()
		a = 10
		globalMap["test"] = append(globalMap["test"], a)
		mu.Unlock()
	}()

	go func() {
		defer wg.Done()
		mu.Lock()
		a = 11
		globalMap["test2"] = append(globalMap["test2"], a)
		mu.Unlock()
	}()

	go func() {
		defer wg.Done()
		mu.Lock()
		a = 12
		globalMap["test3"] = append(globalMap["test3"], a)
		mu.Unlock()
	}()

	wg.Wait()
	fmt.Printf("%v\n", globalMap)
	fmt.Printf("%d\n", a)
}
```
++++++++++
 Задача 7
++++++++++
```go
type Result struct{}

type SearchFunc func(ctx context.Context, query string) (Result, error)

func MultiSearch(ctx context.Context, query string, sfs []SearchFunc) (Result, error) {
    // Нужно реализовать функцию, которая выполняет поиск query во всех переданных SearchFunc
    // Когда получаем первый успешный результат - отдаем его сразу. Если все SearchFunc отработали
    // с ошибкой - отдаем последнюю полученную ошибку
} 
```

```go
package main

import (
	"context"
	"errors"
)

type Result struct{}

type SearchFunc func(ctx context.Context, query string) (Result, error)

func MultiSearch(ctx context.Context, query string, sfs []SearchFunc) (Result, error) {
	var lastErr error
	for _, i := range sfs {
		result, err := i(ctx, query)
		if err == nil {
			return result, nil
		}
		lastErr = err
	}
	if lastErr != nil {
		return Result{}, lastErr
	}
	return Result{}, errors.New("no search functions provided")
}
```

++++++++++
 Задача 8
++++++++++
1. Что выведется и как исправить?
```go
func main() {
  var counter int
  for i := 0; i < 1000; i++ {
    go func() {
      counter++
    }()
  }
  fmt.Println(counter)
}
```
Output: любое число от 0 до 1000. Исправляется этот код с помощью WaitGroup и добавления еще одного цикла.
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	ch := make(chan int, 1000) // Объявляем канал с буфером на 1000 элементов
	var wg sync.WaitGroup      // Создаем WaitGroup для ожидания завершения всех горутин

	for i := 0; i < 1000; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			ch <- i
		}(i)
	}

	wg.Wait() // Ждем завершения всех горутин
	close(ch) // Закрываем канал, чтобы завершить цикл range

	for val := range ch {
		fmt.Println(val)
	}
}
```


++++++++++
 Задача 9
++++++++++
1. Что выведется и как исправить?
2. Что поправить, чтобы сохранить порядок?
```go
func main() {
  m := make(char string, 3)
  cnt := 5
  for i := 0; i < cnt; i++ {
    go func() {
      m <- fmt.Sprintf("Goroutine %d", i)
    }()
  }
  for i := 0; i < cnt; i++ {
    go ReceiveFromCh(m)
  }
}
func ReceiveFromCh(ch chan string) {
  fmt.Println(<-ch)
}
```
Output: ничего. Исправляется при помощи пакета sync, а именно WaitGroup.
```go
package main

import (
	"fmt"
	"sync"
)

func main() {
	var wg sync.WaitGroup
	m := make(chan string, 5) // Increase buffer size to avoid deadlock
	cnt := 5

	for i := 0; i < cnt; i++ {
		wg.Add(1)
		go func(i int) {
			defer wg.Done()
			m <- fmt.Sprintf("Goroutine %d", i)
		}(i)
	}

	for i := 0; i < cnt; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			ReceiveFromCh(m)
		}()
	}

	wg.Wait()
	close(m) // Close the channel after all sends are done
}

func ReceiveFromCh(ch chan string) {
	fmt.Println(<-ch)
}

```


