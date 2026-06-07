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

## Project Structure

The application follows a layered architecture:

- Controllers handle incoming requests.
- Services contain business logic.
- Policies manage authorization.
- Form Requests handle validation.
- Resources transform API responses.
- Models interact with the database.

## Database Schema

![Database Schema](docs/images/database-schema.png)

## Authorization Flow

The application uses a two stages authorization system based on:

* Spatie Laravel Permission for Role-Based Access Control.
* Laravel Policies for resource ownership authorization.

---

### Roles

The system defines three default roles:

| Role   | Description                                     |
| ------ | ----------------------------------------------- |
| Admin  | Full system access                              |
| Editor | Content management access                       |
| Author | Content creation and ownership-based management |

---

### Permissions

#### Category Permissions

| Permission      |
| --------------- |
| create_category |
| update_category |
| delete_category |

#### Tag Permissions

| Permission |
| ---------- |
| create_tag |
| update_tag |
| delete_tag |

#### Post Permissions

| Permission  |
| ----------- |
| create_post |
| update_post |
| delete_post |

#### Comment Permissions

| Permission     |
| -------------- |
| create_comment |
| update_comment |
| delete_comment |

#### Role Permissions

| Permission  |
| ----------- |
| view_roles  |
| create_role |
| update_role |
| delete_role |

#### User Permissions

             | Category        | Tag        | Post        | Comment        | Role        | User        |
-------------|-----------------|------------|-------------|----------------|-------------|-------------|
             | create_category | create_tag | create_post | create_comment | view_users  | view_users  |
 Permissions | update_category | update_tag | update_post | update_comment | create_user | create_user |
             | delete_category | delete_tag | delete_post | delete_comment | update_user | update_user |
             |                 |            |             |                | delete_user | delete_user |

---

### Role Permission Matrix

#### Admin

Administrators receive every permission available in the application.

| Module     | Access |
| ---------- | ------ |
| Posts      | Full   |
| Comments   | Full   |
| Categories | Full   |
| Tags       | Full   |
| Roles      | Full   |
| Users      | Full   |

#### Editor

Editors receive permissions related to content management only.

| Module     | Access    |
| ---------- | --------- |
| Posts      | Full      |
| Comments   | Full      |
| Categories | Full      |
| Tags       | Full      |
| Roles      | No Access |
| Users      | No Access |

#### Author

Authors receive permissions related to posts and comments only.

| Module     | Access    |
| ---------- | --------- |
| Posts      | Limited   |
| Comments   | Limited   |
| Categories | No Access |
| Tags       | No Access |
| Roles      | No Access |
| Users      | No Access |

---

### Resource Ownership Policies

Permissions determine what actions a user may perform.
Policies determine on which resources those actions may be performed.

#### Post Authorization

Authors may only modify posts they created.

Example policy rule:

```text
Author
            ↓
Has update_post permission ?
            ↓
Yes
            ↓
Owns the post ?
            ↓
Yes → Access Granted
No  → Access Denied (403)
```

#### Comment Authorization

Authors may only modify comments they created.

Example policy rule:

```text
Author
            ↓
Has update_comment permission ?
            ↓
Yes
            ↓
Owns the comment ?
            ↓
Yes → Access Granted
No  → Access Denied (403)
```

---

### Authorization Matrix

| Action             | Admin | Editor | Author |
| ------------------ | ----- | ------ | ------ |
| Create Category    | ✅     | ✅      | ❌      |
| Update Category    | ✅     | ✅      | ❌      |
| Delete Category    | ✅     | ✅      | ❌      |
| Create Tag         | ✅     | ✅      | ❌      |
| Update Tag         | ✅     | ✅      | ❌      |
| Delete Tag         | ✅     | ✅      | ❌      |
| Create Post        | ✅     | ✅      | ✅      |
| Update Own Post    | ✅     | ✅      | ✅      |
| Update Any Post    | ✅     | ✅      | ❌      |
| Delete Own Post    | ✅     | ✅      | ✅      |
| Delete Any Post    | ✅     | ✅      | ❌      |
| Create Comment     | ✅     | ✅      | ✅      |
| Update Own Comment | ✅     | ✅      | ✅      |
| Update Any Comment | ✅     | ✅      | ❌      |
| Delete Own Comment | ✅     | ✅      | ✅      |
| Delete Any Comment | ✅     | ✅      | ❌      |
| View Roles         | ✅     | ❌      | ❌      |
| Create Role        | ✅     | ❌      | ❌      |
| Update Role        | ✅     | ❌      | ❌      |
| Delete Role        | ✅     | ❌      | ❌      |
| View Users         | ✅     | ❌      | ❌      |
| Create User        | ✅     | ❌      | ❌      |
| Update User        | ✅     | ❌      | ❌      |
| Delete User        | ✅     | ❌      | ❌      |

## Requirements

Before installing, ensure your environment meets these requirements:

- PHP ( version 8.x )
- Laravel ( version 10.x )
- MySQL database
- Composer
- Web server ( Xampp )

## Installation

Follow these steps to install and set up `cms-roles-permissions` project :

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

## Usage

Follow these steps to run `cms-roles-permissions` project:


1. **Run the application:**
   ```bash
   php artisan serve
   ```

2. **Access the API:**
   - API base URL: [http://localhost:8000/v1/api](http://localhost:8000/v1/api)


## API testing with Postman

You can test all available API endpoints using the provided Postman collection.

### 1. Download the Collection

[CMS Roles Permissions Postman Collection](collection.json)

### 2. How to Use

1. Open [Postman](https://www.postman.com/downloads/)
2. Click `Import` and Choose the file you downloaded above.
3. Set the `url` environment variable to your API base URL (e.g `http://127.0.0.1:8000/api/v1`)
4. Use the available requests grouped under:  
   📁 Guest Routes  
       │  
   📁 Auth Routes  
       ├── 📁 Dashboard  
       ├── 📁 Categories  
       ├── 📁 Tags  
       ├── 📁 Roles  
       ├── 📁 Users  
       ├── 📁 Comments  
       ├── 📁 Notes  
   📁 Front Routes  
       ├── 📁 Home  
       ├── 📁 Blog  
       └── 📁 Comments  

5. Check the full documentation via the following link: https://documenter.getpostman.com/view/YOUR_WORKSPACE_ID/YOUR_DOCUMENT_ID

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
