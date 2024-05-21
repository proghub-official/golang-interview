
# Вопросы для собеседования по Go

:information_source: В этом репозитории содержаться вопросы и ответы с помощью которых вы можете подготовиться к собеседованию по Golang

:iphone: Telegram-канал - [@golangquiz](https://t.me/golangquiz)

:bar_chart: Вопросов - 14.

:pencil: Вы можете добавить свой вопрос или обьяснение, исправить/дополнить существующий с помощью пул реквеста :)

# Todo:
- разделить вопросы по категориям
- разделить вопросы по сложности

# Список вопросов и ответов

<details>
<summary><b>1. Какие основные фичи GO?</b></summary><br>

* Сильная статическая типизация - тип переменных не может быть изменен с течением времени, и они должны быть определены во время компиляции
* Быстрое время компиляции
* Встроенная конкурентность
* Встроеный сборщик мусора
* Компилируется в один бинарник - все, что вам нужно для запуска приложения. Очень полезно для управления версиями во время выполнения.

</details>

<details>
<summary><b>2. Что выведет код?</b></summary><br>

```golang
package main
 
import (
	"fmt"
	"sync"
	"time"
)
 
func main() {
	var wg sync.WaitGroup
	
	wg.Add(1)
	go func() {
		time.Sleep(time.Second * 2)
		fmt.Println("1")
		wg.Done()
	}()

	go func() {
		fmt.Println("2")
	}()

	wg.Wait()
	fmt.Println("3")
}
````

<details>
<summary><b>Ответ</b></summary><br>
всегда 2 1 3
</details>

</details>

<details>
<summary><b>3. Что выведет код?</b></summary><br>

```golang
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	timeout := 3 * time.Second
	ctx, cancel := context.WithTimeout(context.Background(), timeout)
	defer cancel()

	select {
	case <-time.After(1 * time.Second):
		fmt.Println("waited for 1 sec")
	case <-time.After(2 * time.Second):
		fmt.Println("waited for 2 sec")
	case <-time.After(3 * time.Second):
		fmt.Println("waited for 3 sec")
	case <-ctx.Done():
		fmt.Println(ctx.Err())
	}
}
```

<details>
<summary><b>Ответ</b></summary><br>
waited for 1 sec
</details>

</details>

<details>
<summary><b>4. Что выведет код?</b></summary><br>

```golang
package main

import (
	"container/heap"
	"fmt"
)

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
	*h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func main() {
	h := &IntHeap{2, 1, 5}
	heap.Init(h)
	fmt.Printf("first: %d\n", (*h)[0])
}
```

<details>
<summary><b>Ответ</b></summary><br>
first: 1
</details>

</details>

<details>
<summary><b>5. Что выведет код?</b></summary><br>

```golang
package main

import (
	"fmt"
)

func mod(a []int) {
	// Как и почему изменится вывод если раскомментировать строку ниже?
	// a = append(a, 125)
	
	for i := range a {
		a[i] = 5
	}
	
	fmt.Println(a)
}

func main() {
	sl := []int{1, 2, 3, 4}
	mod(sl)
	fmt.Println(sl)
}
```

<details>
<summary><b>Ответ</b></summary><br>
[5 5 5 5]<br>
[5 5 5 5]<br>
если раскомментировать `a = append(a, 125)`<br>
[5 5 5 5 5]<br>
[1 2 3 4]
</details>

</details>

<details>
<summary><b>6. Что выведет код? (тема - defer)</b></summary><br>

```golang
package main

import (
	"fmt"
)

func main() {
	i := 0
	defer fmt.Println(i)
	i++
	return
}
```

<details>
<summary><b>Ответ</b></summary><br>
0
</details>

</details>

<details>
<summary><b>7. Что выведет код? (тема - defer)</b></summary><br>

```golang
package main

import (
	"fmt"
)

func main() {
	for i := 0; i < 5; i++ {
		defer func(i *int) {
			fmt.Printf("%v ", *i)
		}(&i)
	}

}
```

<details>
<summary><b>Ответ</b></summary><br>
Зависит от версии компилятора.

До версии 1.22 - 5 5 5 5 5

Начиная с 1.22 - 4 3 2 1 0
</details>

</details>

<details>
<summary><b>8. Предположим, что <code>x</code> объявлен, а <code>y</code> не объявлен, какие пункты ниже верны?</b></summary><br>

```golang
x, _ := f()
x, _ = f()
x, y := f()
x, y = f()
```

<details>
<summary><b>Ответ</b></summary><br>
x, _ = f()<br>
x, y := f()
</details>

</details>

<details>
<summary><b>9. Что делает <code>runtime.newobject()</code>?</b></summary><br>

Выделяет память в куче.
https://golang.org/pkg/runtime/?m=all#newobject

</details>


<details>
<summary><b>10. Что такое <code>$GOROOT</code> и <code>$GOPATH</code>?</b></summary><br>

<code>$GOROOT</code> каталог для стандартной библиотеки, включая исполняемые файлы и исходный код.<br>
<code>$GOPATH</code> каталго для внешних пакетов.

</details>


<details>
<summary><b>11. Что выведет код? (тема - slice)</b></summary><br>

```golang
package main

import (
	"fmt"
)

func main() {
	test1 := []int{1, 2, 3, 4, 5}
	test1 = test1[:3]
	test2 := test1[3:]
	fmt.Println(test2[:2])
}
```

<details>
<summary><b>Ответ</b></summary><br>
[4 5]
</details>

</details>


<details>
<summary><b>12. Перечислите функции которые могу остановить выполнение текущей горутины</b></summary><br>

runtime.Gosched<br>
runtime.gopark<br>
runtime.notesleep<br>
runtime.Goexit

</details>

<details>
<summary><b>13. Что выведет код?</b></summary><br>

```golang
x := []int{1, 2}
y := []int{3, 4}
ref := x
x = y
fmt.Println(x, y, ref)
```

<details>
<summary><b>Ответ</b></summary><br>
<code>[3 4] [3 4] [1 2]</code>
</details>

</details>

<details>
<summary><b>14. Как скопировать slice?</b></summary><br>

Следует воспользоваться встроенной функцией <code>copy</code>:

```golang
x := []int{1, 2}
y := []int{3, 4}
ref := x
copy(x, y)
fmt.Println(x, y, ref)
// Output: [3 4] [3 4] [3 4]
```

</details>

