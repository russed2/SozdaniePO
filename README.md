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

type Film struct {
    ID     string `json:"id"`
    Title  string `json:"title"`
    Writer string `json:"writer"`
	Director string `json:"director"`
	Year string `json:"year"`
}

var films = []Film{
    {ID: "1", Title: "The Boondock Saints", Writer: "Troy Duffy", Director: "Troy Duffy", Year: "1999"},
    {ID: "2", Title: "Blade", Writer: "David S. Goyer", Director: "Stephen Norrington", Year: "1998"},
    {ID: "3", Title: "Gladiator", Writer: "John Logan", Director: "Ridley Scott", Year: "2000"},
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

	// Получение всех фильмов
    router.GET("/films", getFilms)

    // Получение фильма по ID
    router.GET("/films/:id", getFilmByID)

    // Создание нового фильма
    router.POST("/films", createFilm)

    // Обновление существующего фильма
    router.PUT("/films/:id", updateFilm)

    // Удаление фильма
    router.DELETE("/films/:id", deleteFilm)

    // Запускаем сервер на порту 8080
    router.Run(":8080")
}

func getFilms(c *gin.Context) {
    c.JSON(http.StatusOK, films)
}

func getFilmByID(c *gin.Context) {
    id := c.Param("id")

    for _, film := range films {
        if film.ID == id {
            c.JSON(http.StatusOK, film)
            return
        }
    }

    c.JSON(http.StatusNotFound, gin.H{"message": "film not found"})
}

func createFilm(c *gin.Context) {
    var newFilm Film

    if err := c.BindJSON(&newFilm); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    films = append(films, newFilm)
    c.JSON(http.StatusCreated, newFilm)
}

func updateFilm(c *gin.Context) {
    id := c.Param("id")
    var updatedFilm Film

    if err := c.BindJSON(&updatedFilm); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    for i, film := range films {
        if film.ID == id {
            films[i] = updatedFilm
            c.JSON(http.StatusOK, updatedFilm)
            return
        }
    }

    c.JSON(http.StatusNotFound, gin.H{"message": "film not found"})
}

func deleteFilm(c *gin.Context) {
    id := c.Param("id")

    for i, film := range films {
        if film.ID == id {
            films = append(films[:i], films[i+1:]...)
            c.JSON(http.StatusOK, gin.H{"message": "film deleted"})
            return
        }
    }

    c.JSON(http.StatusNotFound, gin.H{"message": "film not found"})
}
```
### 3. Проверка операций и подключения к серверу через POSTMAN:
#### 3.1 GET /films — получение списка всех фильмов.
![image](https://github.com/user-attachments/assets/702a5e20-a042-4c51-b19e-58ad1c81b00e)
#### 3.2 GET /films/:id — получение информации о фильме по его ID.
![image](https://github.com/user-attachments/assets/9bba3a1b-6e8c-4acf-9f2e-37ed8af417a3)
#### 3.3 POST /films — добавление нового фильма.
![image](https://github.com/user-attachments/assets/09ead160-6f8a-43f3-a3bf-2367b85ead12)
#### 3.4 PUT /films/:id — обновление информации о фильме по его ID.
![image](https://github.com/user-attachments/assets/932aed2f-59fb-4016-b77e-692f486785f1)
![image](https://github.com/user-attachments/assets/42e6693a-fe01-4b21-a80d-b66327b6b371)
#### 3.5 DELETE /films/:id — удаление фильма по его ID.
![image](https://github.com/user-attachments/assets/f7777451-a6a1-4ae4-be6d-a4d1604f77b3)
