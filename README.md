# Технологии создания программного обеспечения 
# Усанов Егор Леонидович
## Практика 6 - Создание REST API на языке GO
### 1. Установка GIN
![image](https://github.com/user-attachments/assets/31b73121-2a4f-4342-acb0-3b325861eae7)

### 2. Простой сервер на GIN с добавлением CRUD-операций
```
package main

import (
    "github.com/gin-gonic/gin"
    "net/http"
)

type Book struct {
    ID     string `json:"id"`
    Title  string `json:"title"`
    Author string `json:"author"`
}

var books = []Book{
    {ID: "1", Title: "1984", Author: "George Orwell"},
    {ID: "2", Title: "Brave New World", Author: "Aldous Huxley"},
    {ID: "3", Title: "Fahrenheit 451", Author: "Ray Bradbury"},
}

func main() {
    // Создаем новый роутер Gin
    router := gin.Default()

    // Определяем маршруты
    router.GET("/ping", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{
            "message": "pong",
        })
    })

	// Получение всех книг
    router.GET("/books", getBooks)

    // Получение книги по ID
    router.GET("/books/:id", getBookByID)

    // Создание новой книги
    router.POST("/books", createBook)

    // Обновление существующей книги
    router.PUT("/books/:id", updateBook)

    // Удаление книги
    router.DELETE("/books/:id", deleteBook)

    // Запускаем сервер на порту 8080
    router.Run(":8080")
}

func getBooks(c *gin.Context) {
    c.JSON(http.StatusOK, books)
}

func getBookByID(c *gin.Context) {
    id := c.Param("id")

    for _, book := range books {
        if book.ID == id {
            c.JSON(http.StatusOK, book)
            return
        }
    }

    c.JSON(http.StatusNotFound, gin.H{"message": "book not found"})
}

func createBook(c *gin.Context) {
    var newBook Book

    if err := c.BindJSON(&newBook); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    books = append(books, newBook)
    c.JSON(http.StatusCreated, newBook)
}

func updateBook(c *gin.Context) {
    id := c.Param("id")
    var updatedBook Book

    if err := c.BindJSON(&updatedBook); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    for i, book := range books {
        if book.ID == id {
            books[i] = updatedBook
            c.JSON(http.StatusOK, updatedBook)
            return
        }
    }

    c.JSON(http.StatusNotFound, gin.H{"message": "book not found"})
}

func deleteBook(c *gin.Context) {
    id := c.Param("id")

    for i, book := range books {
        if book.ID == id {
            books = append(books[:i], books[i+1:]...)
            c.JSON(http.StatusOK, gin.H{"message": "book deleted"})
            return
        }
    }

    c.JSON(http.StatusNotFound, gin.H{"message": "book not found"})
}
```
### 3. Проверка операций и подключения к серверу через POSTMAN
#### 3.1 GET /books — получение списка всех книг.
#### 3.2 GET /books/:id — получение информации о книге по её ID.
#### 3.3 POST /books — добавление новой книги.
#### 3.4 PUT /books/:id — обновление информации о книге по её ID.
#### 3.5 DELETE /books/:id — удаление книги по её ID.
