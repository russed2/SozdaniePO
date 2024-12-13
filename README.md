# Технологии создания программного обеспечения 
# Усанов Егор Леонидович
## Практика 8_2 - Подключение бд
### 1. Подключим драйвер для PostgreSQL и библиотеку GORM:
![image](https://github.com/user-attachments/assets/154db1fc-c442-4607-9718-8e6657a301de)
### 2. Пропишем структуру продукта:
```
type Item struct {  
    IdItem INT 
    CostItem FLOAT64 
    InfoItem string
    SupplierItem INT 
    Manufacturer INT 
    ReviewsItem INT 
    CategoryItem VARCHAR(100)
    AvailItem BOOLEAN 
}
```
### 3. Выполним подключение к бд:
```
var db *gorm.DB

func initBD() {
	dsn := "host=localhost user=postgres password=admin dbname=PO port=5432 sslmode=disable"
	var err error
	db, err = gorm.Open(postgres.Open(dsn), &gorm.Config{})
	if err != nil {
		log.Fatal("Failed to connect to database:", err)
	}

	db.AutoMigrate(&Item{})
}
```
### 4. Функция main изменится так:
```
func main() {
    initBD()

    // Создаем новый роутер Gin
    router := gin.Default()

    router.POST("/login", login)
	router.POST("/register", register)
	router.POST("/refresh", refresh)

    protected := router.Group("/")
	protected.Use(authMiddleware())
	{
        router.GET("/item", getItem)
        router.GET("/items/:id", getItemByID)
        router.POST("/items", roleMiddleware("admin"), createItem)
        router.PUT("/items/:id", roleMiddleware("admin"), updateItem)
        router.DELETE("/items/:id", roleMiddleware("admin"), deleteItem)
    }

    // Запускаем сервер на порту 8080
    router.Run(":8080")
}
```
### 5. Изменяся и функции запрсов CRUD:
```
func getItems(c *gin.Context) {
    var items []Items
	db.Find(&items)
    c.JSON(http.StatusOK, items)
}

func getItemByID(c *gin.Context) {
    id := c.Param("id")
    var items Items

    if err := db.First(&item, id).Error; err != nil {
		c.JSON(http.StatusNotFound, gin.H{"message": "item not found"})
	}

    c.JSON(http.StatusOK, item)
}

func createItem(c *gin.Context) {
    var newItem Item

    if err := c.BindJSON(&newitem); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    db.Create(&newItem)
    c.JSON(http.StatusCreated, Item)
}

func updateItem(c *gin.Context) {
    id := c.Param("id")
    var updatedItem Item

    if err := c.BindJSON(&updatedItem); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"message": "invalid request"})
        return
    }

    if err := db.Model(&Item{}).Where("id = ?", id).Updates(updatedItem).Error; err != nil {
		c.JSON(http.StatusNotFound, gin.H{"message": "item not found"})
	}

    c.JSON(http.StatusNotFound, gin.H{"message": "item not found"})
}

func deleteItem(c *gin.Context) {
    id := c.Param("id")

    if err := db.Delete(&Item{}, id).Error; err != nil {
		c.JSON(http.StatusNotFound, gin.H{"message": "item not found"})
		return
	}

    c.JSON(http.StatusNotFound, gin.H{"message": "item not found"})
}
```
### 6. Вывод всех продуктов:
![image](https://github.com/user-attachments/assets/839191fa-d5ab-415f-ae9a-32af1774efea)
### 3. Выполним подключение к бд:
```

```
