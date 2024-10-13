# Технологии создания программного обеспечения 
# Усанов Егор Леонидович
## Практика 4 - Задачи для практической работы на языке Go
### 1. Задачи на линейное программирование (без условных операторов и циклов)
1. Напишите программу, которая принимает целое число и вычисляет сумму его цифр.
Листинг:
```
package main

import "fmt"

func main() {
	var num, result int
	result = 0
	fmt.Println("Введите число: ")
	fmt.Scan(&num)

	result += num % 10
	num /= 10

	result += num % 10
	num /= 10

	result += num % 10
	num /= 10
	
	result += num % 10
	num /= 10

	fmt.Println("Сумма цифр равна: ", result)

}
```
2. Напишите программу, которая преобразует температуру из градусов Цельсия в Фаренгейты и обратно.
   Листинг:
```
package main

import "fmt"

func toFahrenheit(C float32) float32{
	return 1.8 * C + 32
}

func toCelsius(F float32) float32{
	return 5 * (F - 32)/9
}

func main() {
	var degrees float32
	var scale string

	fmt.Println("Введите температуру (c - для цельсия, f - для фаренгейтов): ")
	fmt.Scan(&degrees, &scale)	

	//map - это таблица которая позволяет связывать ключи со значениями. В данном случае используется для 
	//вызыва методов в зависимости от введенных данных
	converters := map[string]func(float32) float32{
		"c": toFahrenheit,
		"f": toCelsius,
	}

	resultScales := map[string]string{
		"c": "Фаренгейту",
		"f": "Цельсию",
	}

	converter := converters[scale]
	result := converter(degrees)
	scaleResult := resultScales[scale]

	fmt.Printf("Результат: %.2f градусов по %s\n", result, scaleResult)
}
```
3. Напишите программу, которая принимает массив чисел и возвращает новый массив, где каждое число удвоено.
   Листинг:
```
package main

import "fmt"

func main() {
	array := [4]int{1, 2, 3, 4}

	array[0] *= 2
	array[1] *= 2
	array[2] *= 2
	array[3] *= 2

	fmt.Print(array)
}
```
4. Напишите программу, которая принимает несколько строк и объединяет их в одну строку через пробел.
   Листинг:
```
package main

import "fmt"
import "bufio"
import "os"
import "strings"

func main() {
	var sentence1, sentence2, result string

	//Позволяет считать целую строку
	reader := bufio.NewReader(os.Stdin)

	fmt.Println("Введите два предложения: ")

	//_ — символ подчеркивания, который позволяет игнорировать возвращенное значение ошибки (error). 
	sentence1, _ = reader.ReadString('\n')
	//Удаление последнего символа \n
	sentence1 = strings.TrimSpace(sentence1)

	sentence2, _ = reader.ReadString('\n')

	result = sentence1 + " " + sentence2

	fmt.Println(result)
}
```
5. Напишите программу, которая вычисляет расстояние между двумя точками в 2D пространстве.
   Листинг:
```
package main

import "fmt"
import "math"

func main() {
    var x1, x2, y1, y2, result float64

    fmt.Println("Введите координаты первой точки: ")
    fmt.Scan(&x1, &y1)

    fmt.Println("Введите координаты второй точки: ")
    fmt.Scan(&x2, &y2)

    result = math.Sqrt(math.Abs(math.Pow((x2-x1), 2) + math.Pow((y2-y1), 2)))

    fmt.Printf("Расстояние равно: %.4f\n", result)
}
```
### 2. Задачи с условным оператором
1. Напишите программу, которая проверяет, является ли введенное число четным или нечетным.
   Листинг:
```
package main

import "fmt"

func main() {
	var num int

	fmt.Println("Введите число:")
	fmt.Scan(&num)

	if num % 2 == 0 {
		fmt.Println("Четное")
	}else {
		fmt.Println("Не четное")
	}
}
```
2. Напишите программу, которая проверяет, является ли введенный год високосным.
   Листинг:
```
package main

import "fmt"

func main() {
	var year int

	fmt.Println("Введите год:")
	fmt.Scan(&year)

	if (year % 4 == 0 && year % 100 != 0) || year % 400 == 0 {
		fmt.Println("Високосный")
	}else {
		fmt.Println("Не високосный")
	}
}
```
3. Напишите программу, которая принимает три числа и выводит наибольшее из них.
   Листинг:
```
package main

import "fmt"

func main() {
	var x1, x2, x3 int

	fmt.Println("Введите 3 числа:")
	fmt.Scan(&x1, &x2, &x3)

	if x1 > x2 {
		if x1 > x3 {
			fmt.Println(x1)
		}else {
			fmt.Println(x3)
		}
	}else {
		if x2 > x3 {
			fmt.Println(x2)
		}else {
			fmt.Println(x3)
		}
	}
}
```
4. Напишите программу, которая принимает возраст человека и выводит, к какой возрастной группе он относится (ребенок, подросток, взрослый, пожилой. В комментариях указать возрастные рамки).
   Листинг:
```
//ребенок 0 - 12, подросток 13 - 21, взрослый 22 - 59, пожилой >60
package main

import "fmt"

func main() {
	var year int

	fmt.Println("Введите возраст:")
	fmt.Scan(&year)

	switch {
	case year <= 12:
		fmt.Println("Ребенок")

	case year > 12 && year < 22:
		fmt.Println("Подросток")

	case year > 21 && year < 60:
		fmt.Println("Взрослый")

	case year >= 60:
		fmt.Println("Пожилой")
	}
}
```
5. Напишите программу, которая проверяет, делится ли число одновременно на 3 и 5.
   Листинг:
```
package main

import "fmt"

func main() {
	var num int

	fmt.Println("Введите число:")
	fmt.Scan(&num)

	if num % 3 == 0 && num % 5 == 0 {
		fmt.Println("Делится")
	}else {
		fmt.Println("Не делится")
	}
}
```
### 3. Задачи на циклы
1. Напишите программу, которая вычисляет факториал числа.
   Листинг:
```
package main

import "fmt"

func main() {
	var num int
	var result = 1

	fmt.Printf("Введите число: ")
	fmt.Scan(&num)

	for ; num > 0; num-- {
		result *= num
	}

	fmt.Println("Факториал равен:", result)
}
```
2. Напишите программу, которая выводит первые `n` чисел Фибоначчи.
   Листинг:
```
package main

import "fmt"

func main() {
	var n int

	fmt.Println("Введите количество чисел Фибоначчи:")
	fmt.Scan(&n)

	fib := make([]int, n)
		if n > 0 {
   			fib[0] = 0
		}
		if n > 1 {
   			fib[1] = 1
		}

	for i := 2; i < n; i++ {
    	fib[i] = fib[i-1] + fib[i-2]
	}

	fmt.Println("Первые", n, "чисел Фибоначчи:", fib)
}
```
3. Напишите программу, которая переворачивает массив чисел.
   Листинг:
```
package main

import "fmt"
import "math/rand"

func main() {
	var n int

	fmt.Println("Введите количество элементов в массиве: ")
	fmt.Scan(&n)

	array := make([]int, n)
	array1 := make([]int, n)

	for i := 0; i < n; i++ {
		array[i] = rand.Intn(20)
	}

	fmt.Println("Начальный массив: ", array)

	n1 := n - 1

	for i := 0; i < n; i++ {
		array1[i] = array[n1]
		n1--
	}

	fmt.Println("Конечный массив: ", array1)
}
```
4. Напишите программу, которая выводит все простые числа до заданного числа.
   Листинг:
```
package main

import "fmt"

func getPrimes(limit int) []int {
    primes := []int{}
    for i := 2; i < limit; i++ {
        if isPrime(i) {
            primes = append(primes, i)
        }
    }
    return primes
}

func isPrime(num int) bool {
    for i := 2; i*i <= num; i++ {
        if num % i == 0 {
            return false
        }
    }
    return true
}

func main() {
    var n int
	
    fmt.Println("Введите число для поиска всех простых чисел до него:")
    fmt.Scan(&n)

    primes := getPrimes(n)
    fmt.Println("Простые числа", primes)
}
```
5. Напишите программу, которая вычисляет сумму всех чисел в массиве.
   Листинг:
```
package main

import "fmt"
import "math/rand"

func main() {
	var n, sum int

	fmt.Println("Введите количество элементов в массиве: ")
	fmt.Scan(&n)

	array := make([]int, n)

	for i := 0; i < n; i++ {
		array[i] = rand.Intn(10)
		sum += array[i]
	}

	fmt.Println("Начальный массив: ", array)
	fmt.Println("Сумма его чисел:", sum)
}
```
