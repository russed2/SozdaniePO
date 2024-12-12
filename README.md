# Технологии создания программного обеспечения 
# Усанов Егор Леонидович
## Практика 8 - Создание бд
### 1. Создадим базу данных разработанную в одной из прошлых практик:
#### 1.1 Запрос на создание таблиц:
```
CREATE TABLE "Role" (
    IdRole SERIAL PRIMARY KEY,
    NameRole VARCHAR(50) NOT NULL
);

CREATE TABLE Auth (
    IdAuth SERIAL PRIMARY KEY,
    LoginAuth VARCHAR(50) NOT NULL,
    PassAuth VARCHAR(50) NOT NULL
);

CREATE TABLE Address (
    IdAddress SERIAL PRIMARY KEY,
    CityAddress VARCHAR(100) NOT NULL,
    BuildAddress VARCHAR(100) NOT NULL,
    EntranceAddress VARCHAR(50),
    ApartAddress VARCHAR(50),
    MetroAddress VARCHAR(100)
);

CREATE TABLE Infouser (
    IdInfouser SERIAL PRIMARY KEY,
    FINfouser VARCHAR(100) NOT NULL,
    LNInfouser VARCHAR(100) NOT NULL,
    MNInfouser VARCHAR(100),
    BirthdayInfouser DATE NOT NULL,
    PhoneInfouser VARCHAR(15) NOT NULL,
    MailInfouser VARCHAR(100) NOT NULL
);

CREATE TABLE Favorites (
    IdFavorites SERIAL PRIMARY KEY,
    ItemFavorites INT NOT NULL
);

CREATE TABLE "User" (
    IdUser SERIAL PRIMARY KEY,
    InfouserUser INT NOT NULL REFERENCES Infouser(IdInfouser),
    FavoritesUser INT REFERENCES Favorites(IdFavorites),
    AddressUser INT NOT NULL REFERENCES Address(IdAddress),
    AuthUser INT NOT NULL REFERENCES Auth(IdAuth),
    RoleUser INT NOT NULL REFERENCES "Role"(IdRole)
);

CREATE TABLE Reviews (
    IdReviews SERIAL PRIMARY KEY,
    RatingReviews INT NOT NULL,
    PositiveReviews INT,
    NegativeReviews INT,
    TextReviews TEXT
);

CREATE TABLE Manufacturer (
    IdManufacturer SERIAL PRIMARY KEY,
    NameManufacturer VARCHAR(100) NOT NULL,
    CountryManufacturer VARCHAR(100),
    ContactManufacturer VARCHAR(100)
);

CREATE TABLE Supplier (
    IdSupp SERIAL PRIMARY KEY,
    NameSupp VARCHAR(100) NOT NULL,
    CountrySupp VARCHAR(100),
    ContactSupp VARCHAR(100)
);

CREATE TABLE Item (
    IdItem SERIAL PRIMARY KEY,
    CostItem NUMERIC(10, 2) NOT NULL,
    InfoItem TEXT NOT NULL,
    SupplierItem INT REFERENCES Supplier(IdSupp),
    Manufacturer INT REFERENCES Manufacturer(IdManufacturer),
    ReviewsItem INT REFERENCES Reviews(IdReviews),
    CategoryItem VARCHAR(100),
    AvailItem BOOLEAN NOT NULL
);

CREATE TABLE ShoppingCart (
    IdShop SERIAL PRIMARY KEY,
    ItemShop INT NOT NULL REFERENCES Item(IdItem),
    CountShop INT NOT NULL
);

CREATE TABLE Delivery (
    IdDelivery SERIAL PRIMARY KEY,
    AvailDelivery BOOLEAN NOT NULL,
    AddressDelivery INT NOT NULL REFERENCES Address(IdAddress),
    CostDelivery NUMERIC(10, 2) NOT NULL,
    DateDelivery DATE NOT NULL
);

CREATE TABLE "Order" (
    IdOrder SERIAL PRIMARY KEY,
    NeedDelOrder BOOLEAN NOT NULL,
    NumberOrder VARCHAR(50) NOT NULL,
    CostOrder NUMERIC(10, 2) NOT NULL,
    CommentsOrder TEXT,
    DateOrder DATE NOT NULL,
    ShopOrder INT NOT NULL REFERENCES ShoppingCart(IdShop),
    DeliveryOrder INT NOT NULL REFERENCES Delivery(IdDelivery),
    UserOrder INT NOT NULL REFERENCES "User"(IdUser)
);
```
#### 1.2 Запрос на заполнение таблиц:
```
INSERT INTO "Role" (NameRole) VALUES
('admin'),
('user');

INSERT INTO Auth (LoginAuth, PassAuth) VALUES
('admin', 'admin'),
('user1', '123'),
('user2', '123');

INSERT INTO Address (CityAddress, BuildAddress, EntranceAddress, ApartAddress, MetroAddress) VALUES
('Москва', 'Ул. Проспект Мира', '1', '56', 'ВДНХ'),
('Санкт-Петербург', 'Лиговский Проспект', '12', '202', 'Лиговский Проспект'),
('Москва', 'Ул. Менжинского', '23', '14', 'Бабушкинская');

INSERT INTO Infouser (FINfouser, LNInfouser, MNInfouser, BirthdayInfouser, PhoneInfouser, MailInfouser) VALUES
('Сергей', 'Петров', 'Маратович', '1990-01-01', '+1234567890', 'admin@example.com'),
('Кирилл', 'Демидов', 'Валерьевич', '1995-05-05', '+9876543210', 'user1@example.com'),
('Светлана', 'Лисина', 'Сергеевна', '2000-03-15', '+1230984567', 'user2@example.com');

INSERT INTO Favorites (ItemFavorites) VALUES
(1),
(2),
(3);

INSERT INTO "User" (InfouserUser, FavoritesUser, AddressUser, AuthUser, RoleUser) VALUES
(1, 1, 1, 1, 1),
(2, 2, 2, 2, 2),
(3, 3, 3, 3, 2);

INSERT INTO Reviews (RatingReviews, PositiveReviews, NegativeReviews, TextReviews) VALUES
(5, 100, 2, 'Отличный продукт!'),
(4, 80, 10, 'Хорошо, но есть некоторые проблемы.'),
(2, 20, 50, 'Не доволена продуктом!');

INSERT INTO Manufacturer (NameManufacturer, CountryManufacturer, ContactManufacturer) VALUES
('Sony', 'Japan', 'contact@sony.com'),
('Samsung', 'South Korea', 'contact@samsung.com'),
('Apple', 'USA', 'contact@apple.com');

INSERT INTO Supplier (NameSupp, CountrySupp, ContactSupp) VALUES
('Trustend', 'USA', 'trustend@tech.com'),
('Selecuiem', 'Germany', 'selecuiem@tech.com'),
('Dorcty', 'China', 'Dorcty@tech.com');

INSERT INTO Item (CostItem, InfoItem, SupplierItem, Manufacturer, ReviewsItem, CategoryItem, AvailItem) VALUES
(499.99, 'High-quality headphones', 1, 1, 1, 'Electronics', TRUE),
(999.99, 'Smartphone with excellent features', 2, 2, 2, 'Electronics', TRUE),
(1499.99, 'High-performance laptop', 3, 3, 3, 'Computers', TRUE);

INSERT INTO ShoppingCart (ItemShop, CountShop) VALUES
(1, 2),
(2, 1), 
(3, 1); 

INSERT INTO Delivery (AvailDelivery, AddressDelivery, CostDelivery, DateDelivery) VALUES
(TRUE, 2, 15.00, '2024-12-15'),
(TRUE, 2, 20.00, '2024-12-16'),
(FALSE, 3, 0.00, '2024-12-17');

INSERT INTO "Order" (NeedDelOrder, NumberOrder, CostOrder, CommentsOrder, DateOrder, ShopOrder, DeliveryOrder, UserOrder) VALUES
(TRUE, 'ORD001', 1029.98, 'Доставить КАК МОЖНО СКОРЕЕ', '2024-12-15', 1, 1, 2),
(TRUE, 'ORD002', 999.99, 'Пожалуйста, обращайтесь с ним осторожно', '2024-12-16', 2, 2, 2),
(FALSE, 'ORD003', 1499.99, 'Самовывоз в магазине', '2024-12-17', 3, 3, 3);
```
#### 1.3 Пример заполнения таблиц:
![image](https://github.com/user-attachments/assets/2e646598-fe95-4353-bc29-d3fb6c785c90)
