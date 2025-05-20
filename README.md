```mermaid
classDiagram
    class Role {
        ADMIN
        USER
        GUEST
    }
    class Activation
    class Region
    class OrderStatus
    class RequirementType

    class User {
      +int id
      +varchar email
      +text passwordHash
      +Role role
      +int bonusPoints
      +datetime createdAt
      +datetime updatedAt
    }
    User "1" --> "*" Order
    User "1" --> "*" CartItem
    User "1" --> "*" Review
    User "1" --> "*" Favorite

    class Product {
      +int id
      +varchar title
      +text description
      +decimal price
      +decimal discount
      +datetime releaseDate
      +Activation activation
      +Region region
      +int publisherId
      +int developerId
      +int stock
      +boolean isActive
      +datetime createdAt
      +datetime updatedAt
    }
    Product "1" --> "*" GameKey
    Product "1" --> "*" OrderItem
    Product "1" --> "*" CartItem
    Product "1" --> "*" Review
    Product "1" --> "*" Screenshot
    Product "1" --> "*" SystemRequirement
    Product "1" <-- "*" ProductGenre : genres
    Product "1" <-- "*" ProductTag : tags

    class GameKey {
      +int id
      +int productId
      +varchar key
      +boolean isSold
      +int orderItemId
      +datetime assignedAt
      +datetime createdAt
    }
    GameKey --> Product
    GameKey --> OrderItem

    class Order {
      +int id
      +int userId
      +decimal total
      +OrderStatus status
      +datetime createdAt
      +datetime updatedAt
    }
    Order "1" --> "*" OrderItem
    Order "1" --> "*" Payment

    class OrderItem {
      +int id
      +int orderId
      +int productId
      +int quantity
      +decimal unitPrice
    }
    OrderItem --> Order
    OrderItem --> Product

    class Payment {
      +int id
      +int orderId
      +varchar method
      +varchar transactionId
      +decimal amount
      +varchar status
      +datetime createdAt
    }
    Payment --> Order

    class CartItem {
      +int id
      +int userId
      +int productId
      +int quantity
      +datetime addedAt
    }
    CartItem --> User
    CartItem --> Product

    class Review {
      +int id
      +int rating
      +text text
      +int userId
      +int productId
      +datetime createdAt
    }
    Review --> User
    Review --> Product

    class Favorite {
      +int userId
      +int productId
    }
    Favorite --> User
    Favorite --> Product

    class SystemRequirement {
      +int id
      +RequirementType type
      +varchar os
      +varchar cpu
      +varchar gpu
      +varchar ram
      +varchar storage
      +int productId
    }
    SystemRequirement --> Product

    class Screenshot {
      +int id
      +varchar url
      +int productId
      +datetime createdAt
    }
    Screenshot --> Product

    class Publisher {
      +int id
      +varchar name
    }
    class Developer {
      +int id
      +varchar name
    }
    Product --> Publisher
    Product --> Developer

    class Genre {
      +int id
      +varchar name
    }
    class Tag {
      +int id
      +varchar name
    }

    class ProductGenre {
      +int productId
      +int genreId
    }
    class ProductTag {
      +int productId
      +int tagId
    }
    ProductGenre --> Product
    ProductGenre --> Genre
    ProductTag --> Product
    ProductTag --> Tag
