# mdisubd

## Евдоковец Андрей, 253505
ФКСиС ИиТП, 2024г.

# News Website Database Design

This project outlines the structure and relationships of a news website, where users can write articles, leave comments, like content, and more.

## Сущности и описание их полей

### 1. **User (Пользователь)**
- **Attributes**:
  - `user_id` (PK): Unique identifier.
  - `username` (VARCHAR, unique): Username.
  - `email` (VARCHAR, unique): Email address.
  - `password_hash` (VARCHAR): Password hash.
  - `is_author` (BOOLEAN): Flag indicating if the user is an author.
  - `registration_date` (DATE): Registration date.
- **Relationships**:
  - One-to-Many with `Article`: One user can write many articles.
  - One-to-Many with `Comment`: One user can post many comments.
  - One-to-Many with `Like`: One user can like many articles.
  - One-to-Many with `Subscription`: One user can subscribe to many authors.
  - One-to-Many with `Bookmark`: One user can bookmark many articles.
  - One-to-Many with `Notification`: One user can receive many notifications.
  - One-to-One with `Moderator`: One user can be assigned as a moderator.
  - One-to-Many with `Comment_Like`: One user can like many comments.
  - One-to-Many with `Category_Subscription`: One user can subscribe to many categories.

### 2. **Article (Статья)**
- **Attributes**:
  - `article_id` (PK): Unique identifier.
  - `title` (VARCHAR): Title of the article.
  - `content` (TEXT): Article content.
  - `publication_date` (DATE): Date of publication.
  - `author_id` (FK): Reference to `User` (author).
  - `category_id` (FK): Reference to `Category`.
  - `views` (INT): Number of views.
- **Relationships**:
  - Many-to-One with `User`: One article is written by one author.
  - One-to-Many with `Comment`: One article can have many comments.
  - One-to-Many with `Like`: One article can receive many likes.
  - Many-to-Many with `Tag`: One article can have many tags.
  - One-to-Many with `Bookmark`: One article can be bookmarked by many users.

### 3. **Comment (Комментарий)**
- **Attributes**:
  - `comment_id` (PK): Unique identifier.
  - `content` (TEXT): Comment content.
  - `created_at` (DATE): Date of comment creation.
  - `article_id` (FK): Reference to `Article`.
  - `user_id` (FK): Reference to `User`.
- **Relationships**:
  - Many-to-One with `Article`: One comment belongs to one article.
  - Many-to-One with `User`: One comment belongs to one user.
  - One-to-Many with `Comment_Like`: One comment can receive many likes.

### 4. **Like (Лайк)**
- **Attributes**:
  - `like_id` (PK): Unique identifier.
  - `user_id` (FK): Reference to `User`.
  - `article_id` (FK): Reference to `Article`.
- **Relationships**:
  - Many-to-One with `User`: One like is given by one user.
  - Many-to-One with `Article`: One like belongs to one article.

### 5. **Category (Категория)**
- **Attributes**:
  - `category_id` (PK): Unique identifier.
  - `name` (VARCHAR): Category name.
  - `description` (TEXT): Description of the category.
- **Relationships**:
  - One-to-Many with `Article`: One category can include many articles.
  - One-to-Many with `Category_Subscription`: One category can have many subscribers.

### 6. **Tag (Тег)**
- **Attributes**:
  - `tag_id` (PK): Unique identifier.
  - `name` (VARCHAR): Tag name.
- **Relationships**:
  - Many-to-Many with `Article`: One tag can be assigned to many articles.

### 7. **Bookmark (Закладка)**
- **Attributes**:
  - `bookmark_id` (PK): Unique identifier.
  - `user_id` (FK): Reference to `User`.
  - `article_id` (FK): Reference to `Article`.
  - `created_at` (TIMESTAMP): Timestamp when the article was bookmarked.
- **Relationships**:
  - Many-to-One with `User`: One bookmark belongs to one user.
  - Many-to-One with `Article`: One bookmark belongs to one article.

### 8. **Subscription (Подписка на автора)**
- **Attributes**:
  - `subscription_id` (PK): Unique identifier.
  - `subscriber_id` (FK): Reference to `User` (subscriber).
  - `author_id` (FK): Reference to `User` (author).
  - `subscription_date` (DATE): Date of subscription.
- **Relationships**:
  - Many-to-One with `User`: One subscription belongs to one user and one author.

### 9. **Notification (Уведомление)**
- **Attributes**:
  - `notification_id` (PK): Unique identifier.
  - `content` (TEXT): Notification content (e.g., "New article from Author X").
  - `user_id` (FK): Reference to `User`.
  - `created_at` (DATE): Date of notification creation.
  - `is_read` (BOOLEAN): Flag indicating whether the notification has been read.
- **Relationships**:
  - Many-to-One with `User`: One notification belongs to one user.

### 10. **Category_Subscription (Подписка на категорию)**
- **Attributes**:
  - `subscription_id` (PK): Unique identifier.
  - `user_id` (FK): Reference to `User`.
  - `category_id` (FK): Reference to `Category`.
  - `subscription_date` (DATE): Date of subscription.
- **Relationships**:
  - Many-to-One with `User`: One category subscription belongs to one user.
  - Many-to-One with `Category`: One category subscription belongs to one category.

### 11. **Comment_Like (Лайк Комментария)**
- **Attributes**:
  - `comment_like_id` (PK): Unique identifier.
  - `comment_id` (FK): Reference to `Comment`.
  - `user_id` (FK): Reference to `User`.
- **Relationships**:
  - Many-to-One with `Comment`: One comment like belongs to one comment.
  - Many-to-One with `User`: One comment like belongs to one user.

### 12. **Moderator (Модератор)**
- **Attributes**:
  - `moderator_id` (PK): Unique identifier.
  - `user_id` (FK): Reference to `User`.
  - `assigned_date` (DATE): Date of moderator assignment.
- **Relationships**:
  - One-to-One with `User`: One moderator is one user.

