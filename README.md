```mermaid
classDiagram
    %% Enumerations
    class Role <<enumeration>> {
        <<enum>> ADMIN
        <<enum>> USER
        <<enum>> GUEST
    }
    class Activation <<enumeration>>
    class Region <<enumeration>>
    class OrderStatus <<enumeration>>
    class RequirementType <<enumeration>>

    %% User
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

    %% Product
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

    %% GameKey
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

    %% Order, OrderItem, Payment
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

    %% CartItem
    class CartItem {
      +int id
      +int userId
      +int productId
      +int quantity
      +datetime addedAt
    }
    CartItem --> User
    CartItem --> Product

    %% Review
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

    %% Favorite
    class Favorite {
      +int userId
      +int productId
    }
    Favorite --> User
    Favorite --> Product

    %% SystemRequirement
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

    %% Screenshot
    class Screenshot {
      +int id
      +varchar url
      +int productId
      +datetime createdAt
    }
    Screenshot --> Product

    %% Publisher & Developer
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

    %% Genre & Tag
    class Genre {
      +int id
      +varchar name
    }
    class Tag {
      +int id
      +varchar name
    }

    %% Junction tables
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

\```
