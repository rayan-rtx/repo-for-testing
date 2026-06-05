# CMS Roles & Permissions

## Introduction

The `cms-roles-permissions` repository is REST API that provides authentication, authorization, and role-based access control for content management systems ( CMS ).

The project implements a complete user management system using Laravel Policies and the Spatie Permission package, allowing administrators to manage users, roles, permissions, posts, categories, tags, comments, and notes through a modular architecture, secure, and extensible API.

## Features

- Modular architecture for easy customization and scalability.
- User authentication and role-based access control.
- RESTful API for seamless integration with frontend frameworks.
- Media management for images and files.
- Dashboard with analytics widgets and content overview.
- SEO-friendly URLs and metadata management.

## Requirements

Before installing, ensure your environment meets these requirements:

- PHP ( version 8.x )
- Laravel ( version 10.x )
- MySQL database
- Composer
- Web server ( Apache )

## Project Structure

The application follows a layered architecture:

- Controllers handle incoming requests.
- Services contain business logic.
- Policies manage authorization.
- Form Requests handle validation.
- Resources transform API responses.
- Models interact with the database.

## Installation

Follow these steps to install and set up `cms-roles-permissions`:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/rayanguendouz/cms-roles-permissions.git
   cd cms-roles-permissions
   ```

2. **Install dependencies:**
   ```bash
   composer install
   ```

3. **Create the environment file:**

    ```bash
    cp .env.example .env
    ```

4. **Configure your database credentials in .env .**
    ```.env
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=cms-roles-permissions
    DB_USERNAME=root
    DB_PASSWORD=
    ```

5. **Generate the application key:**
    ```bash
    php artisan key:generate
    ```

6. **Run database migrations and seeders:**
   ```bash
   php artisan migrate --seed
   ```

7. **Create a symbolic link for storage:**
    ```bash
    php artisan storage:link
    ```

8. **Run the application:**
   ```bash
   php artisan serve
   ```

9. **Access the API:**
   - API base URL: [http://localhost:8000/v1/api](http://localhost:8000/v1/api)


## API using with Postman

You can test all available API endpoints using the provided Postman collection.

### 1. Download the Collection

[CMS Roles Permissions Postman Collection](collection.json)

### 2. How to Use

1. Open [Postman](https://www.postman.com/downloads/)
2. Click `Import` and Choose the file you downloaded above.
3. Set the `url` environment variable to your API base URL (e.g `http://127.0.0.1:8000/api/v1`)
4. Use the available requests grouped under:
   - Guest Routes  
       │  
   - Auth Routes  
       ├── Dashboard  
       ├── Categories  
       ├── Tags  
       ├── Roles  
       ├── Users  
       ├── Comments  
       ├── Notes  
   - Front Routes  
       ├── Home  
       ├── Blog  
       └── Comments  


## Contributing

We welcome contributions! Please follow these guidelines:

- Fork the repository and create a new branch for your feature or fix.
- Write clear commit messages and document your code.
- Ensure all tests pass before submitting a pull request.
- Follow the established code style and project structure.
- Open an issue for discussion before major changes.

## License

This project is open-sourced under the [MIT License](LICENSE).

---

Thank you for using `cms-roles-permissions`! For questions or support, please open an issue on GitHub.
