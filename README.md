# ValdezLibrary_4A

## Valdez Marc John
## BS Information Technology 4A

# Library API

The Library API is a comprehensive backend service designed to optimize the management of library resources. It features endpoints for managing books, authors, users, and borrowing records. To ensure security, the API utilizes JWT authentication, offering a robust system for user verification and token handling.

## Features

- **JWT Authentication**: Secure the access to the API with JSON Web Tokens.
- **Token Revocation**: The system enforces single-use validity for tokens, meaning each token can only be used once. This approach significantly enhances security by preventing token reuse, reducing the risk of unauthorized access or replay attacks.
- **CRUD Operations**: It manages users, authors, book, and book-author relationships.


## Technologies Used

- **PHP**: Core backend logic using the Slim framework.
- **Slim Framework**: Lightweight and fast framework for PHP.
- **Firebase JWT**: Library for handling JSON Web Tokens.
- **PSR-7**: HTTP message interfaces for standardized request and response handling.

---

## Authentication (JWT)

### Overview

The API employs JWT for secure authentication:  
- Clients are required to provide a token in the `Authorization` header with every request.  
- Tokens are verified and invalidated after a single use to prevent reuse.  
- Revoked tokens are managed and tracked through PHP sessions.  

### Token Management

- **Generation**: Tokens are generated after successful login.
- **Validation**: Middleware ensures the token is valid and not expired.
- **Revocation**: Tokens are invalidated after their first use.

---

#  CRUD OPERATIONS
### A1. USER AUTHENTICAte/LOGIN
  - **Endpoint:** `/user/login`  
  - **Method:** `POST`  
  - **Description:** 
    User credentials are verified by comparing the supplied username and password with the database records. Upon successful authentication, a JSON Web Token (JWT) is created and issued for secure session management.  - **Sample Request(JSON):**
      ```json
          {
            "username": "marcky123",
            "password": "asodogcat"
          }
      ```
  - **Response:**
      - **On Success**
          ```json
              {
                  "status": "success",
                  "token": "<TOKEN>",
                  "data": null
              }
          ```
      - **On Failure (Authenthication Failed):**
          ```json
              {
                  "status": "fail",
                  "data": {
                      "title": "Authentication Failed!"
                  }
              }
          ```
---
### A2. USER REGISTER
- **Endpoint:** `/user/register`  
- **Method:** `POST`  
- **Description:**  
A new user account is created by accepting a unique username and password, both requiring a minimum length of five characters. The password is securely hashed before being stored in the database.
- **Sample Request (JSON):**
    ```json
    {
        "username": "marcky123",
        "password": "asodogcat"
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "data": null
        }
        ```

    - **On Failure (Invalid Input):**
        ```json
        {
            "status": "fail",
            "data": "Username and password must be at least 5 characters long and not empty"
        }
        ```

    - **On Failure (Duplicae Username):**
        ```json
        {
            "status": "fail",
            "data": "Username already exists"
        }
        ```
---
### A3. USER VIEW
- **Endpoint:** `/user/show`  
- **Method:** `GET`  
- **Description:**  
Fetches a list of all registered users, showing their IDs and usernames. Access to this endpoint requires a valid JWT token.

- **Authentication:**  
  Access to this endpoint requires a valid JWT token, which must be included in the `Authorization` header as a Bearer token.

- **Sample Request:**
    - **No request body is required. Authentication is handled via the Authorization header.**

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": [
                {
                    "userid": 1,
                    "username": "johndoe"
                },
                {
                    "userid": 2,
                    "username": "janedoe"
                }
            ]
        }
        ```
    - **On Failure (Unauthorized):**
        ```json
        {
            "status": "fail",
            "data": "Unauthorized: Token not provided"
        }
        ```

    - **On Failure (No Users Found):**
        ```json
        {
            "status": "fail",
            "data": "No users found"
        }
        ```
---
### A4. USER UPDATE
- **Endpoint:** `/user/update`  
- **Method:** `PUT`  
- **Description:**  
  Allows an authenticated user to modify their username and password. The updated credentials must satisfy the minimum length requirements, and the user's ID is used to locate the specific record for updating.

- **Authentication:**  
  This endpoint requires a valid JWT token for access. The JWT token must be passed in the `Authorization` header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "userid": "1",
        "username": "newusername",
        "password": "newpassword123"
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "User updated successfully"
        }
        ```

    - **On Failure (User Not Found):**
        ```json
        {
            "status": "fail",
            "data": "User with ID 1 does not exist."
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "Username and password must be at least 5 characters long and not empty"
        }
        ```
---
### A5. USER DELETE
- **Endpoint:** `/user/delete`  
- **Method:** `DELETE`  
- **Description:**  
  Deletes a specific user using the ID. The endpoint requires a valid JWT token and verifies the existence of the user before performing the deletion.

- **Authentication:**  
  This endpoint requires a valid JWT token for access. The JWT token must be passed in the `Authorization` header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "userid": "2"
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "User deleted successfully"
        }
        ```

    - **On Failure (User Does Not Exist):**
        ```json
        {
            "status": "fail",
            "data": "User with ID 2 does not exist."
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "User ID must be provided."
        }
        ```
---


### B1. AUTHOR CREATE
- **Endpoint:** `/authors/add`  
- **Method:** `POST`  
- **Description:**  
  Adds a new author to the database. The author’s name must be unique, at least five characters long, and cannot be blank.

- **Authentication:**  
  This endpoint requires a valid JWT token for access. The JWT token must be passed in the `Authorization` header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "name": "Justine Kelly"
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Author created successfully"
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "Author name must be more than 4 characters and cannot be blank."
        }
        ```
---
### B2. AUTHOR UPDATE
- **Endpoint:** `/authors/update`  
- **Method:** `PUT`  
- **Description:**  
  This API endpoint enables the modification of an existing author's details. To update the information, the request payload must include the `authorid`, which identifies the author whose data is being updated, along with the new `name` of the author. The new name provided must be at least 5 characters in length to meet the validation criteria. Additionally, the operation is secured by requiring a valid JWT token for authentication. Upon successful completion of the update request, a new JWT token will be issued and returned to the client, replacing the old token. This ensures that the token is refreshed as part of the secure update process.

- **Authentication:**  
  This endpoint requires a valid JWT token for access. The JWT token must be passed in the `Authorization` header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "authorid": "3",
        "name": "Rod Alvin"
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Author updated successfully"
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "Author name must be more than 4 characters and cannot be blank."
        }
        ```

    - **On Failure (Author Not Found):**
        ```json
        {
            "status": "fail",
            "data": "Author with ID 3 does not exist."
        }
        ```
---
### B3. AUTHOR VIEW
- **Endpoint:** `/authors/show`  
- **Method:** `GET`  
- **Description:**  
  Retrieves a list of all authors in the database, displaying their unique IDs and names.

- **Authentication:**  
  This endpoint requires a valid JWT token for access. The JWT token must be passed in the `Authorization` header as a Bearer token.

- **Sample Request:**
    - **No request body is required. Authentication is handled via the Authorization header.**

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": [
                {
                    "authorid": 1,
                    "name": "Justine Kelly"
                },
                {
                    "authorid": 2,
                    "name": "Rod Alvin"
                },
                ...
            ]
        }
        ```

    - **On Failure (No Authors Found):**
        ```json
        {
            "status": "fail",
            "data": "No authors found"
        }
        ```
---
### B4. AUTHOR DELETE
- **Endpoint:** `/authors/delete`  
- **Method:** `DELETE`  
- **Description:**  
  Deletes an existing author from the database based on their unique ID.
- **Authentication:**  
  This endpoint requires a valid JWT token for access. The JWT token must be passed in the `Authorization` header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
      "authorid": 4
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Author deleted successfully"
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "Author ID must be provided."
        }
        ```

    - **On Failure (Author Not Found):**
        ```json
        {
            "status": "fail",
            "data": "Author with ID 4 does not exist."
        }
        ```
---


### C1. BOOK CREATE
- **Endpoint:** `/books/add`  
- **Method:** `POST`  
- **Description:**  
 This API endpoint allows for the creation of a new book record in the database. The request must include the `title` of the book and the `authorid`, which is the unique identifier of the author to whom the book will be attributed. The operation is secured by requiring a valid JWT token for authentication. Upon successful creation of the book record, the response will include a newly issued JWT token and a confirmation message indicating that the book has been successfully added.

- **Authentication:**  
  Requires a valid JWT token. The JWT token must be passed in the Authorization header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "title": "Itlog sa Baki",
        "authorid": 1
    }
    ```

- **Response:**
    - **On Success:**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Book created successfully"
        }
        ```

    - **On Failure (Book Title Invalid):**
        ```json
        {
            "status": "fail",
            "data": "Book title must be more than 4 characters and cannot be blank."
        }
        ```

    - **On Failure (Author Not Found):**
        ```json
        {
            "status": "fail",
            "data": "Author not found"
        }
        ```
---
### C2. BOOK VIEW
- **Endpoint:** `/books/show`  
- **Method:** `GET`  
- **Description:**  
  Retrieves a list of all books in the database, displaying their book ID, title, author ID, and author name.

- **Authentication:**  
  Requires a valid JWT token. The JWT token must be passed in the Authorization header as a Bearer token.

- **Sample Request:**
    - **No request body is required. Authentication is handled via the Authorization header.**

- **Response:**
    - **On Success (Books Retrieved):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": [
                {
                    "bookid": 1,
                    "title": "Itlog sa Baki",
                    "authorid": 1,
                    "author_name": "Justine Kelly"
                },
                {
                    "bookid": 2,
                    "title": "Percy Jackson",
                    "authorid": 2,
                    "author_name": "Rod Alvin"
                },
                ...
            ]
        }
        ```

    - **On Failure (No Books Found):**
        ```json
        {
            "status": "fail",
            "data": "No books found"
        }
        ```
---

### C3. BOOK UPDATE
- **Endpoint:** `/books/update`  
- **Method:** `PUT`  
- **Description:**  
  Updates the information of an existing book. The request must include the bookid (the ID of the book to be updated), the new title of the book, and the authorid. The name must be at least 5 characters long.

- **Authentication:**  
  Requires a valid JWT token. The JWT token must be passed in the Authorization header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "bookid": 1,
        "title": "The Egg",
        "authorid": 1
    }
    ```

- **Response:**
    - **On Success (Book Updated):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Book updated successfully"
        }
        ```

    - **On Failure (Book Not Found):**
        ```json
        {
            "status": "fail",
            "data": "Book with ID <Book_ID> does not exist."
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "Book title must be more than 4 characters and cannot be blank."
        }
        ```
---

### C4. BOOK DELETE
- **Endpoint:** `/books/delete`  
- **Method:** `DELETE`  
- **Description:**  
  Deletes an existing book from the database based on their unique ID.

- **Authentication:**  
  Requires a valid JWT token. The JWT token must be passed in the Authorization header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "bookid": 4
    }
    ```

- **Response:**
    - **On Success (Book Deleted):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Book deleted successfully"
        }
        ```

    - **On Failure (Validation Error):**
        ```json
        {
            "status": "fail",
            "data": "Book ID must be provided."
        }
        ```
        
    - **On Failure (Book Does Not Exist):**
        ```json
        {
            "status": "fail",
            "data": "Book with ID 4 does not exist."
        }
        ```
---




### D1. BOOK-AUTHOR CREATE
- **Endpoint:** `/book_authors/add`  
- **Method:** `POST`  
- **Description:**  
  This API endpoint creates a relationship between a specific book and its corresponding author by associating them using their respective `bookid` and `authorid`. The request is protected by JWT authentication to ensure that only authorized users can perform this action. After the relationship is successfully established, the system generates a new JWT token and returns it to the user, ensuring that the token is refreshed as part of the process.

- **Authentication:**  
  A valid JWT token is required for access. The token must be passed in the Authorization header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "bookid": "1",
        "authorid": "1"
    }
    ```

- **Response:**
    - **On Success (Book-Author Relationship Created):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Book-author relationship created successfully"
        }
        ```

    - **On Failure (Book Does Not Exist):**
        ```json
        {
            "status": "fail",
            "data": "Book does not exist"
        }
        ```

    - **On Failure (Author Does Not Exist):**
        ```json
        {
            "status": "fail",
            "data": "Author does not exist"
        }
        ```

    - **On Failure (Foreign Key Constraint Violation):**
        ```json
        {
            "status": "fail",
            "data": {
                "title": "Foreign key constraint violation",
                "details": "Error message from the database"
            }
        }
        ```
---
### D2. BOOK-AUTHOR VIEW
- **Endpoint:** `/book_authors/show`  
- **Method:** `GET`  
- **Description:**  
  This endpoint retrieves all book-author relationships from the database, along with the details of the books and authors involved in each relationship.

- **Authentication:**  
  A valid JWT token is required for access. The token must be passed in the Authorization header as a Bearer token.
  
- **Sample Request:**
    - **No request body is required. Authentication is handled via the Authorization header.**

- **Response:**
    - **On Success (Book-Author Relationships Found):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": [
                {
                    "collectionid": "2",
                    "bookid": "3",
                    "authorid": "3",
                    "book_title": "Book Title",
                    "author_name": "Author Name"
                },
                {
                    "collectionid": "5",
                    "bookid": "2",
                    "authorid": "2",
                    "book_title": "Another Book",
                    "author_name": "Another Author"
                }
            ]
        }
        ```

    - **On Failure (No Book-Author Relationships Found):**
        ```json
        {
            "status": "fail",
            "data": "No book-author relationships found"
        }
        ```
---
### D3. BOOK-AUTHOR UPDATE
- **Endpoint:** `/book_authors/update`  
- **Method:** `PUT`  
- **Description:**  
  This API endpoint enables the update of an existing book-author relationship by providing the `collectionid`, `bookid`, and `authorid`. The relationship associated with the specified `collectionid` will be updated to reflect the new book-author pairing. This operation ensures that the correct association between the book and author is maintained within the given collection.
- **Authentication:**  
  A valid JWT token is required for access. The token must be passed in the Authorization header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "collectionid": "1",
        "bookid": "1",
        "authorid": "1"
    }
    ```

- **Response:**
    - **On Success (Book-Author Relationship Updated):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Book-author relationship updated successfully"
        }
        ```

    - **On Failure (Book Does Not Exist):**
        ```json
        {
            "status": "fail",
            "data": "Book does not exist"
        }
        ```

    - **On Failure (Author Not Found):**
        ```json
        {
            "status": "fail",
            "data": "Author does not exist"
        }
        ```

    - **On Failure (Foreign Key Constraint Violation):**
        ```json
        {
            "status": "fail",
            "data": {
                "title": "Foreign key constraint violation",
                "details": "<Error_Message>"
            }
        }
        ```
---
### D4. BOOK-AUTHOR DELETE
- **Endpoint:** `/book_authors/delete`  
- **Method:** `DELETE`  
- **Description:**  
  This endpoint deletes an existing book-author relationship identified by the collectionid. Once deleted, the book-author relationship is removed from the database.

- **Authentication:**  
  A valid JWT token is required for access. The token must be passed in the Authorization header as a Bearer token.

- **Request Body (JSON):**
    ```json
    {
        "collectionid": "1"
    }
    ```

- **Response:**
    - **On Success (Book-Author Relationship Deleted):**
        ```json
        {
            "status": "success",
            "token": "<New_JWT_Token>",
            "data": "Book-author relationship deleted successfully"
        }
        ```

    - **On Failure (Database Error):**
        ```json
        {
            "status": "fail",
            "data": {
                "title": "<Error_Message>"
            }
        }
        ```
---
# ValdezLibrary_4A
