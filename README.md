graph TD
    %% Client Layer
    subgraph Client ["Client Layer"]
        WebBrowser["Browser / Client Application\n(HTML, CSS, JS)"]
    end

    %% Security & Network Filter Layer
    subgraph SecurityLayer ["Spring Security / Infrastructure"]
        JwtFilter["JwtFilter\n(Token Validation & Authentication)"]
        SecurityConfig["SecurityConfig\n(Stateless, CORS & Permit Rules)"]
    end

    %% Presentation / Controller Layer
    subgraph ControllerLayer ["Presentation Layer (Controllers)"]
        AuthController["AuthController\n(/users/login, /logout, /refresh)"]
        UserController["UserController\n(/users/signup)"]
        TodoListController["TodoListController\n(/api/todo-lists)"]
        TodoController["TodoController\n(/api/todos)"]
        CategoryController["CategoryController\n(/api/categories)"]
        CalendarController["CalendarController\n(/calendars)"]
    end

    %% Service / Business Logic Layer
    subgraph ServiceLayer ["Business Logic Layer (Services)"]
        UserService["UserServiceImpl\n(Auth & Token Management)"]
        TodoService["Todo / TodoList Service"]
        CalendarService["Calendar Service"]
        JwtUtil["JwtUtil\n(Create / Parse JWT)"]
    end

    %% Data Access Layer
    subgraph PersistenceLayer ["Persistence Layer (Spring Data JPA)"]
        UserRepo["UserRepository"]
        TodoRepo["Todo / TodoList Repository"]
        CalendarRepo["Calendar / Schedule Repository"]
    end

    %% Database Layer
    subgraph DBLayer ["Database Layer"]
        MySQL[("MySQL Database\n(todo_db)")]
    end

    %% Flow Connections
    WebBrowser -->|HTTP Request / JWT Bearer| SecurityConfig
    SecurityConfig --> JwtFilter
    JwtFilter -->|Extract & Validate JWT| JwtUtil
    JwtFilter -->|Authenticated Request| ControllerLayer

    AuthController --> UserServiceImpl
    UserController --> UserServiceImpl
    TodoListController --> TodoService
    TodoController --> TodoService
    CategoryController --> TodoService
    CalendarController --> CalendarService

    UserServiceImpl --> JwtUtil
    UserServiceImpl --> UserRepo
    TodoService --> TodoRepo
    CalendarService --> CalendarRepo

    UserRepo -->|JPA / JDBC| MySQL
    TodoRepo -->|JPA / JDBC| MySQL
    CalendarRepo -->|JPA / JDBC| MySQL
