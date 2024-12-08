# Технологии создания программного обеспечения 
# Усанов Егор Леонидович
## Практика 5. Задачи по языку программирования Go 

1. **Проверка на простоту**  
Напишите функцию, которая проверяет, является ли переданное число простым. Ваша программа должна использовать циклы для проверки делителей, и если число не является простым, выводить первый найденный делитель. Листинг:
```
package main

import "fmt"

func checkPrime(num int) (bool, int) {
    if num <= 1 {
        return false, 1
    }
    for i := 2; i*i <= num; i++ {
        if num%i == 0 {
            return false, i
        }
    }
    return true, 1
}

func main() {
    var num int
    fmt.Println("Введите число для проверки, является ли оно простым:")
    fmt.Scan(&num)

    isPrime, divisor := checkPrime(num)
    if isPrime {
        fmt.Println("Число", num, "является простым")
    } else {
        fmt.Println("Число", num, "не является простым. Первый делитель:", divisor)
    }
}
```
2. **Наибольший общий делитель (НОД)**  
   Напишите программу для нахождения наибольшего общего делителя (НОД) двух чисел с использованием алгоритма Евклида. Используйте цикл `for` для вычислений. Листинг:
```
package main

import "fmt"

func findNOD(a, b int) int {
    for b != 0 {
        a, b = b, a%b
    }
    return a
}

func main() {
    var a, b int

    fmt.Println("Введите два числа для нахождения НОД:")
    fmt.Scan(&a, &b)

    NOD := findNOD(a, b)
    fmt.Println("Наибольший общий делитель:", NOD)
}
```
3. **Сортировка пузырьком**  
   Реализуйте сортировку пузырьком для списка целых чисел. Программа должна выполнять сортировку на месте и выводить каждый шаг изменения массива. Листинг:
```
package main

import "fmt"

func bubbleSort(arr []int) {
    n := len(arr)
    for i := 0; i < n-1; i++ {
        for j := 0; j < n-i-1; j++ {
            if arr[j] > arr[j+1] {
                arr[j], arr[j+1] = arr[j+1], arr[j] 
            }
            fmt.Println("Шаг сортировки:", arr)
        }
    }
}

func main() {
    var n int

    fmt.Println("Введите количество элементов в массиве:")
    fmt.Scan(&n)

    array := make([]int, n)

    fmt.Println("Введите элементы массива:")
    for i := 0; i < n; i++ {
        fmt.Scan(&array[i])
    }

    bubbleSort(array)
}
```
4. **Таблица умножения в формате матрицы**  
   Напишите программу, которая выводит таблицу умножения в формате матрицы 10x10. Используйте циклы для генерации строк и столбцов. Листинг:
```
package main

import "fmt"

func main() {
    for i := 1; i <= 10; i++ {
        for j := 1; j <= 10; j++ {
            fmt.Printf("%4d", i*j)
        }
        fmt.Println()
    }
}
```
5. **Фибоначчи с мемоизацией**  
   Напишите функцию для вычисления числа Фибоначчи с использованием мемоизации (сохранение ранее вычисленных результатов). Программа должна использовать рекурсию и условные операторы. Листинг:
```
package main

import "fmt"

var memo = map[int]int{}

func fibonacci(n int) int {
    if n <= 1 {
        return n
    }

    if value, exists := memo[n]; exists {
        return value
    }

    memo[n] = fibonacci(n-1) + fibonacci(n-2)

    return memo[n]
}

func main() {
    var n int
    fmt.Println("Введите число для вычисления его числа Фибоначчи:")
    fmt.Scan(&n)

    result := fibonacci(n)
    fmt.Printf("Число Фибоначчи для %d равно %d\n", n, result)
}
```
6. **Обратные числа**  
   Напишите программу, которая принимает целое число и выводит его в обратном порядке. Например, для числа 12345 программа должна вывести 54321. Используйте цикл для обработки цифр числа. Листинг:
```
package main

import "fmt"

func main() {
    var num int
    fmt.Println("Введите целое число:")
    fmt.Scan(&num)

    reversed := 0

    for num != 0 {
        remainder := num % 10           
        reversed = reversed*10 + remainder 
        num /= 10                        
    }

    fmt.Println("Число в обратном порядке:", reversed)
}
```
7. **Треугольник Паскаля**  
   Напишите программу, которая выводит треугольник Паскаля до заданного уровня. Для этого используйте цикл и массивы для хранения предыдущих значений строки треугольника. Листинг:
```
package main

import "fmt"

func main() {
    var levels int
    fmt.Println("Введите количество уровней треугольника Паскаля:")
    fmt.Scan(&levels)

    triangle := make([][]int, levels)

    for i := 0; i < levels; i++ {
        triangle[i] = make([]int, i+1)
        triangle[i][0], triangle[i][i] = 1, 1

        for j := 1; j < i; j++ {
            triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j]
        }

        fmt.Println(triangle[i])
    }
}
```
8. **Число палиндром**  
   Напишите программу, которая проверяет, является ли число палиндромом (одинаково читается слева направо и справа налево). Не используйте строки для решения этой задачи — работайте только с числами. Листинг:
```
package main

import "fmt"

func main() {
    var num int
    fmt.Println("Введите число для проверки:")
    fmt.Scan(&num)

    if isPalindrome(num) {
        fmt.Println("Число", num, "является палиндромом.")
    } else {
        fmt.Println("Число", num, "не является палиндромом.")
    }
}

func isPalindrome(num int) bool {
    original := num
    reversed := 0

    for num > 0 {
        remainder := num % 10
        reversed = reversed*10 + remainder
        num /= 10
    }

    return original == reversed
}
```
9. **Нахождение максимума и минимума в массиве**  
   Напишите функцию, которая принимает массив целых чисел и возвращает одновременно максимальный и минимальный элемент с использованием одного прохода по массиву. Листинг:
```
package main

import "fmt"

func findMinMax(arr []int) (int, int) {
    min, max := arr[0], arr[0]

    for _, value := range arr[1:] {
        if value < min {
            min = value
        }
        if value > max {
            max = value
        }
    }

    return min, max
}

func main() {
    var n int
    fmt.Println("Введите количество элементов в массиве:")
    fmt.Scan(&n)

    array := make([]int, n)
    fmt.Println("Введите элементы массива:")
    for i := 0; i < n; i++ {
        fmt.Scan(&array[i])
    }

    min, max := findMinMax(array)
    fmt.Printf("Минимальный элемент: %d, Максимальный элемент: %d\n", min, max)
}
```
10. **Игра "Угадай число"**  
   Напишите программу, которая загадывает случайное число от 1 до 100, а пользователь пытается его угадать. Программа должна давать подсказки "больше" или "меньше" после каждой попытки. Реализуйте ограничение на количество попыток. Листинг:
```
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    secretNumber := rand.Intn(100) + 1
    maxAttempts := 7
    var guess int

    fmt.Println("Я загадал число от 1 до 100. Попробуйте угадать его")

    for attempts := 1; attempts <= maxAttempts; attempts++ {
        fmt.Printf("Попытка %d: Введите ваше предположение: ", attempts)
        fmt.Scan(&guess)

        if guess == secretNumber {
            fmt.Println("Поздравляю! Вы угадали число.")
            return
        } else if guess < secretNumber {
            fmt.Println("Загаданное число больше.")
        } else {
            fmt.Println("Загаданное число меньше.")
        }
    }

    fmt.Printf("Вы не угадали число. Было загадано: %d\n", secretNumber)
}
```
11. **Числа Армстронга**  
   Напишите программу, которая проверяет, является ли число числом Армстронга (число равно сумме своих цифр, возведённых в степень, равную количеству цифр числа). Например, 153 = 1³ + 5³ + 3³. Листинг:
```
package main

import (
    "fmt"
    "math"
)

func isArmstrong(num int) bool {
    original := num
    sum := 0
    digits := countDigits(num)

    for num > 0 {
        digit := num % 10
        sum += int(math.Pow(float64(digit), float64(digits)))
        num /= 10
    }

    return sum == original
}

func countDigits(num int) int {
    count := 0
    for num != 0 {
        num /= 10
        count++
    }
    return count
}

func main() {
    var num int
    fmt.Println("Введите число:")
    fmt.Scan(&num)

    if isArmstrong(num) {
        fmt.Printf("Число %d является числом Армстронга.\n", num)
    } else {
        fmt.Printf("Число %d не является числом Армстронга.\n", num)
    }
}
```
12. **Подсчет слов в строке**  
   Напишите программу, которая принимает строку и выводит количество уникальных слов в ней. Используйте `map` для хранения слов и их количества. Листинг:
```
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
)

func main() {
    fmt.Println("Введите строку:")
    reader := bufio.NewReader(os.Stdin)
    input, _ := reader.ReadString('\n')
    input = strings.TrimSpace(input)

    words := strings.Fields(input)
    wordCount := make(map[string]bool)

    for _, word := range words {
        wordCount[word] = true
    }

    uniqueCount := len(wordCount)

    fmt.Printf("Количество уникальных слов: %d\n", uniqueCount)
}
```
13. **Игра "Жизнь" (Conway's Game of Life)**  
   Реализуйте клеточный автомат "Жизнь" Конвея для двухмерного массива. Каждая клетка может быть либо живой, либо мертвой. На каждом шаге состояния клеток изменяются по следующим правилам:
   - Живая клетка с двумя или тремя живыми соседями остаётся живой, иначе умирает.
   - Мёртвая клетка с тремя живыми соседями оживает.
   Используйте циклы для обработки клеток. Листинг:
```
package main

import (
	"bufio"
	"fmt"
	"os"
)

func createMatrix(matrixSize int) [][]int {

	matrix := make([][]int, matrixSize)

	for i := range matrix {
		matrix[i] = make([]int, matrixSize)
	}

	return matrix
}

func getAliveNeighbours(x int, y int, matrix [][]int) int {
	result := 0
	for neighbourX := x - 1; neighbourX <= x+1; neighbourX++ {
		for neighbourY := y - 1; neighbourY <= y+1; neighbourY++ {
			if neighbourX < 0 || neighbourY < 0 || neighbourX >= len(matrix) ||
				neighbourY >= len(matrix[0]) || (neighbourX == x && neighbourY == y) {
				continue
			}
			if matrix[neighbourX][neighbourY] == 1 {
				result++
			}
		}
	}

	return result
}

func showMatrix(matrix [][]int) {
	for _, line := range matrix {
		fmt.Println(line)
	}
}

func makeStep(matrixSize int, matrix [][]int) [][]int {
	result := createMatrix(matrixSize)
	for x, line := range matrix {
		for y, cell := range line {
			result[x][y] = matrix[x][y]
			aliveNeighbours := getAliveNeighbours(x, y, matrix)
			if cell == 1 && !(aliveNeighbours == 3 || aliveNeighbours == 2) {
				result[x][y] = 0
				continue
			}

			if cell == 0 && aliveNeighbours == 3 {
				result[x][y] = 1
				continue
			}
		}
	}

	return result
}
func main() {

	const fieldSize = 10

	matrix := createMatrix(fieldSize)

	matrix[1][4] = 1
	matrix[2][2] = 1
	matrix[2][4] = 1
	matrix[3][3] = 1
	matrix[3][4] = 1

	for true {

		showMatrix(matrix)

		fmt.Println("Нажмите любую кнопку для продолжения")
		bufio.NewScanner(os.Stdin).Scan()

		matrix = makeStep(fieldSize, matrix)
	}
}
```
14. **Цифровой корень числа**  
   Напишите программу, которая вычисляет цифровой корень числа. Цифровой корень — это рекурсивная сумма цифр числа, пока не останется только одна цифра. Например, цифровой корень числа 9875 равен 2, потому что 9+8+7+5=29 → 2+9=11 → 1+1=2. Листинг:
```
package main

import (
	"fmt"
)

func digitsSqrt(number int) int {

	currentNum := 0
	for ok := true; ok; ok = (number/10 != 0) {
		for ; number > 0; number /= 10 {
			currentNum += number % 10
		}

		number = currentNum
		currentNum = 0
	}

	return number
}

func main() {

	var num int
	fmt.Println("Введите число:")
	fmt.Scan(&num)

	fmt.Print(digitsSqrt(num))
}
```
15. **Римские цифры**  
   Напишите функцию, которая преобразует арабское число (например, 1994) в римское (например, "MCMXCIV"). Программа должна использовать циклы и условные операторы для создания римской записи. Листинг:
```
package main

import (
	"fmt"
	"sort"
)

func createRomanMapAndKeyList() (map[int]string, []int) {
	romanMap := map[int]string{
		1:    "I",
		4:    "IV",
		5:    "V",
		9:    "IX",
		10:   "X",
		40:   "XL",
		50:   "L",
		90:   "XC",
		100:  "C",
		400:  "CD",
		500:  "D",
		900:  "CM",
		1000: "M",
	}

	keys := make([]int, 0, len(romanMap))
	for k := range romanMap {
		keys = append(keys, k)
	}
	sort.Ints(keys)

	for i := 0; i < len(keys)/2; i++ {
		temp := keys[i]
		keys[i] = keys[len(keys)-1-i]
		keys[len(keys)-1-i] = temp
	}

	return romanMap, keys
}

func toRomanNumbers(number int) string {

	romanMap, keys := createRomanMapAndKeyList()

	result := ""

	for number > 0 {
		for _, key := range keys {
			for number/key != 0 {
				result += romanMap[key]
				number -= key
			}
		}
	}

	return result
}

func main() {

	var num int
	fmt.Println("Введите число:")
	fmt.Scan(&num)

	fmt.Print(toRomanNumbers(num))
}
```
