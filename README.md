flowchart TD
    %% Style
    classDef client fill:#EBF5FB,stroke:#3498DB,stroke-width:2px;
    classDef security fill:#FEF9E7,stroke:#F1C40F,stroke-width:2px;
    classDef service fill:#E8F8F5,stroke:#1ABC9C,stroke-width:2px;
    classDef db fill:#F4ECF7,stroke:#8E44AD,stroke-width:2px;

    %% Client
    Browser["🖥️ React Client<br/>Browser"]:::client

    %% Security
    subgraph Security["Spring Security"]
        JwtFilter["JwtFilter<br/>JWT 검증"]:::security
        JwtUtil["JwtUtil<br/>토큰 생성/검증"]:::security
    end

    %% Backend
    subgraph SpringBoot["Spring Boot"]
        AuthController["AuthController"]:::service
        TodoController["TodoController"]:::service
        CalendarController["CalendarController"]:::service

        UserService["UserService"]:::service
        TodoService["TodoService"]:::service
        CalendarService["CalendarService"]:::service

        UserRepository["UserRepository"]:::service
        TodoRepository["TodoRepository"]:::service
        CalendarRepository["CalendarRepository"]:::service

        AuthController --> UserService
        TodoController --> TodoService
        CalendarController --> CalendarService

        UserService --> UserRepository
        TodoService --> TodoRepository
        CalendarService --> CalendarRepository
    end

    %% Database
    MySQL[("MySQL")]:::db

    %% Flow
    Browser -->|Login / API Request| JwtFilter
    JwtFilter --> JwtUtil
    JwtFilter --> AuthController
    JwtFilter --> TodoController
    JwtFilter --> CalendarController

    UserRepository --> MySQL
    TodoRepository --> MySQL
    CalendarRepository --> MySQL

    MySQL --> UserRepository
    MySQL --> TodoRepository
    MySQL --> CalendarRepository

    AuthController -. JWT 발급 .-> Browser
    TodoController -. JSON Response .-> Browser
    CalendarController -. JSON Response .-> Browser
