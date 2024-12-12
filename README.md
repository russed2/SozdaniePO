# Технологии создания программного обеспечения 
# Усанов Егор Леонидович
## Практика 7 - Аутентификация и авторизация в REST API
### 1. Установим библиотеку для работы с JWT
![image](https://github.com/user-attachments/assets/98501ee0-e234-404e-82c0-1aa32a9b5c78)
### 2. Создание простого механизма регистрации и аутентификации
#### 2.1 Создадим структуру пользователя с добавлением ролей, токена и его рефреш:
```
var jwtKey = []byte("my_secret_key")

type Credentials struct {
    Username string `json:"username"`
    Password string `json:"password"`
    Role string `json:"role"`
}

type Claims struct {
    Username string `json:"username"`
    Role string `json:"role"`
    jwt.StandardClaims
}

type RefreshClaims struct {
    Username string `json:"username"`
    jwt.StandardClaims
}
```
#### 2.2 Сами функции для генерации токена и его рефреша:
```
func generateToken(username string, role string) (string, error) {
            expirationTime := time.Now().Add(5 * time.Minute)
            claims := &Claims{
                Username: username,
                Role: role,
                StandardClaims: jwt.StandardClaims{
                    ExpiresAt: expirationTime.Unix(),
                },
            }
            token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
            return token.SignedString(jwtKey)
        }

func refresh(c *gin.Context) {
	tokenString := c.GetHeader("Authorization")
	claims := &Claims{}

	token, err := jwt.ParseWithClaims(tokenString, claims, func(token *jwt.Token) (interface{}, error) {
		return jwtKey, nil
	})

	if err != nil || !token.Valid {
		c.JSON(http.StatusUnauthorized, gin.H{"message": "unauthorized"})
		return
	}

	if time.Unix(claims.ExpiresAt, 0).Sub(time.Now()) > 30*time.Second {
		c.JSON(http.StatusBadRequest, gin.H{"message": "token not expired enough"})
		return
	}

	newToken, err := generateToken(claims.Username, claims.Role)
	if err != nil {
		c.JSON(http.StatusInternalServerError, gin.H{"message": "could not create token"})
		return
	}

	c.JSON(http.StatusOK, gin.H{"token": newToken})
}
```
#### 2.3 Функция логинирования и middleware для проверки токена:
```
func login(c *gin.Context) {
    var creds Credentials
    if err := c.BindJSON(&creds); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    storedPassword, exists := users[creds.Username]
    if !exists || storedPassword != creds.Password {
        c.JSON(http.StatusUnauthorized, gin.H{"message": "unauthorized"})
        return
    }


    role, roleExists := roles[creds.Username]
	if !roleExists {
		c.JSON(http.StatusUnauthorized, gin.H{"message": "role not assigned"})
		return
	}

    token, err := generateToken(creds.Username, role)
    if err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"message": "could not create token"})
        return
    }

    c.JSON(http.StatusOK, gin.H{"token": token})
}

func authMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        tokenString := c.GetHeader("Authorization")

        claims := &Claims{}
        token, err := jwt.ParseWithClaims(tokenString, claims, func(token *jwt.Token) (interface{}, error) {
            return jwtKey, nil
        })

        if err != nil || !token.Valid {
            c.JSON(http.StatusUnauthorized, gin.H{"message": "unauthorized"})
            c.Abort()
            return
        }

        c.Next()
    }
}
```
#### 2.4 Функция регистрации пользователей:
```
func register(c *gin.Context) {
    var creds Credentials
    if err := c.BindJSON(&creds); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    if _, exists := users[creds.Username]; exists {
        c.JSON(http.StatusConflict, gin.H{"message": "user already exists"})
        return
    }

    users[creds.Username] = creds.Password
    roles[creds.Username] = creds.Role

    c.JSON(http.StatusCreated, gin.H{"message": "user registered successfully"})
}
```
#### 2.5 Добавим проверку ролей:
```
func roleMiddleware(requiredRole string) gin.HandlerFunc {
    return func(c *gin.Context) {

        tokenString := c.GetHeader("Authorization")
        
        claims := &Claims{}
        token, err := jwt.ParseWithClaims(tokenString, claims, func(token *jwt.Token) (interface{}, error) {
            return jwtKey, nil
        })

        if err != nil || !token.Valid {
            c.JSON(http.StatusUnauthorized, gin.H{"message": "unauthorized"})
            c.Abort()
            return
        }

        if claims.Role != requiredRole {
            c.JSON(http.StatusForbidden, gin.H{"message": "forbidden"})
            c.Abort()
            return
        }

        c.Next()
    }
}
```
#### 2.6 Добавим записи пользователей и ролей:
```
var users = map[string]string{
	"admin": "admin",
	"user1": "123",
}

var roles = map[string]string{
	"admin": "admin",
	"user":  "user",
}
```
### 3 Добавим в main middleware и операции с аутентификацией:
```
func main() {
    // Создаем новый роутер Gin
    router := gin.Default()

    router.POST("/login", login)
	router.POST("/register", register)
	router.POST("/refresh", refresh)

    protected := router.Group("/")
	protected.Use(authMiddleware())
	{
        router.GET("/films", getFilms)
        router.GET("/films/:id", getFilmByID)
        router.POST("/films", roleMiddleware("admin"), createFilm)
        router.PUT("/films/:id", roleMiddleware("admin"), updateFilm)
        router.DELETE("/films/:id", roleMiddleware("admin"), deleteFilm)
    }

    // Запускаем сервер на порту 8080
    router.Run(":8080")
}
```
### 4 Проверим работоспособность кода:
#### 4.1 Регистрация обычного пользователя:
![image](https://github.com/user-attachments/assets/39bc26c5-4b72-4d37-9046-ca1c8bd3d421)
#### 4.2 Вход под новым пользователем:
![image](https://github.com/user-attachments/assets/73d94aba-72ea-49c2-ab8f-78d1202cf483)
#### 4.3 Вход под новым пользователем c неправильным паролем:
![image](https://github.com/user-attachments/assets/71a88c06-e207-405e-bbf9-5bc574a36e7b)
#### 4.4 Получения списка фильмов:
![image](https://github.com/user-attachments/assets/57580f0e-c7af-430b-af09-16ecd341a310)
#### 4.5 Попытка удаления фильма под обычным пользователем:
![image](https://github.com/user-attachments/assets/6c941a15-0ce4-4a79-8c98-0a22350706a0)
#### 4.6 Вход под админом:
![image](https://github.com/user-attachments/assets/5da28948-78ca-4013-aec8-95c4d9c61817)
#### 4.7 Попытка удаления фильма под админом:
![image](https://github.com/user-attachments/assets/31ad18e4-6350-49c4-8203-57a7f053ccab)
#### 4.8 Проверка удаления:
![image](https://github.com/user-attachments/assets/1663fd43-dc87-4198-a5c8-e7fafae10ed7)
