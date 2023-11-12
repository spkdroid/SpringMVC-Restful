# SpringMVC-Restful

We'll go through the steps to create a basic project that includes a controller, a Thymeleaf template, and Maven dependencies.

### Step 1: Open Spring Initializer

1. Open your web browser and go to [https://start.spring.io/](https://start.spring.io/).

### Step 2: Configure Project

2. Fill in the project metadata:
   - **Project:** Maven Project
   - **Language:** Java
   - **Spring Boot:** Latest stable version
   - **Project Metadata:** Enter `SpringMvcDemo` for the `Artifact`, and set the `Group` as per your organization or domain.

3. Choose the following dependencies:
   - **Spring Web**
   - **Thymeleaf** (as a template engine for simplicity in this example)

4. Click on the "Generate" button to download the project ZIP file.

### Step 3: Extract and Import into IDE

5. Extract the downloaded ZIP file to your preferred location.

6. Import the project into your preferred Integrated Development Environment (IDE). For example:
   - In IntelliJ IDEA: File -> Open -> Select the project folder.
   - In Eclipse: File -> Import -> Existing Maven Projects -> Select the project folder.

### Step 4: Create a Controller

7. Inside the `src/main/java/com/example/SpringMvcDemo` package, create a new Java class named `HomeController`:

   ```java
   package com.example.SpringMvcDemo;

   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.GetMapping;

   @Controller
   public class HomeController {

       @GetMapping("/")
       public String home(Model model) {
           model.addAttribute("message", "Hello, Spring MVC!");
           return "home";
       }
   }
   ```

### Step 5: Create a Thymeleaf Template

8. Inside the `src/main/resources/templates` directory, create a new HTML file named `home.html`:

   ```html
   <!DOCTYPE html>
   <html xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Spring MVC Demo</title>
   </head>
   <body>
       <h1 th:text="${message}"></h1>
   </body>
   </html>
   ```

### Step 6: Run the Application

9. Run the application by executing the `SpringMvcDemoApplication` class.

10. Open your web browser and go to [http://localhost:8080/](http://localhost:8080/). You should see a page displaying the message "Hello, Spring MVC!"

Congratulations! You've successfully created a simple Spring MVC application using Spring Initializer. Feel free to explore further by adding more controllers, templates, and functionalities to enhance your Spring MVC project.

# RESTful service using Spring MVC

### Step 1: Create a Project

Use Spring Initializer to create a new Spring Boot project with the following configurations:

- Project: Maven Project
- Language: Java
- Spring Boot: Latest stable version
- Group: com.example
- Artifact: BookService
- Dependencies: Spring Web

Generate the project, download it, and open it in your preferred IDE.

### Step 2: Define the Model

Create a `Book` class to represent the data model. Place it in the `src/main/java/com/example/BookService` package:

```java
package com.example.BookService;

public class Book {
    private Long id;
    private String title;
    private String author;

    // getters and setters
}
```

### Step 3: Create the Controller

Create a `BookController` to handle RESTful requests. Place it in the `src/main/java/com/example/BookService` package:

```java
package com.example.BookService;

import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;

@RestController
@RequestMapping("/api/books")
public class BookController {

    private final List<Book> books = new ArrayList<>();

    @GetMapping
    public List<Book> getAllBooks() {
        return books;
    }

    @GetMapping("/{id}")
    public Book getBookById(@PathVariable Long id) {
        return books.stream()
                .filter(book -> book.getId().equals(id))
                .findFirst()
                .orElse(null);
    }

    @PostMapping
    public Book addBook(@RequestBody Book book) {
        book.setId((long) (books.size() + 1));
        books.add(book);
        return book;
    }

    @PutMapping("/{id}")
    public Book updateBook(@PathVariable Long id, @RequestBody Book updatedBook) {
        Book existingBook = getBookById(id);
        if (existingBook != null) {
            existingBook.setTitle(updatedBook.getTitle());
            existingBook.setAuthor(updatedBook.getAuthor());
        }
        return existingBook;
    }

    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable Long id) {
        books.removeIf(book -> book.getId().equals(id));
    }
}
```

### Step 4: Run the Application

Run the `BookServiceApplication` class to start the Spring Boot application.

### Step 5: Test the RESTful API

Use a tool like Postman or cURL to test the RESTful API:

1. **Get all books:**
   - Endpoint: `GET http://localhost:8080/api/books`

2. **Get a specific book by ID:**
   - Endpoint: `GET http://localhost:8080/api/books/{id}`

3. **Add a new book:**
   - Endpoint: `POST http://localhost:8080/api/books`
   - Body (JSON):
     ```json
     {
       "title": "The Great Gatsby",
       "author": "F. Scott Fitzgerald"
     }
     ```

4. **Update an existing book:**
   - Endpoint: `PUT http://localhost:8080/api/books/{id}`
   - Body (JSON):
     ```json
     {
       "title": "Updated Title",
       "author": "Updated Author"
     }
     ```

5. **Delete a book by ID:**
   - Endpoint: `DELETE http://localhost:8080/api/books/{id}`

These are basic examples, and you can expand the functionality based on your application requirements. This example illustrates the fundamental concepts of building a RESTful service with Spring MVC.
