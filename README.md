# CMS Roles Permissions


# Guest Routes


### Register

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/register`


## Overview

This endpoint allows you to register a new administrator user in the system. The registration process creates a new admin account with the provided credentials and user information.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/register`
    

## Request Body

The request body should contain the user details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The user name |
| `username` | string | Yes | The user unique username |
| `email` | string | Yes | The user email address ( most be unique ) |
| `password` | string | Yes | The user account password |
| `password_confirmation` | string | Yes | The confirmation of the password ( must match the password field ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires no authentication (unless specified by your API configuration).

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── username: string
    ├── email: string
    └── role: string

 ```

## Response Status

### Success Response (201 Created)

When the user is successfully created, the API returns a `201 Created` status with the newly created user details.

**Example :**

``` json
{
    "success": true,
    "message": "User registered successfully.",
    "data": {
        "name": "Rayan",
        "username": "rayan",
        "email": "rayan@mail.to",
        "role": "author"
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate user email), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 2 more errors)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "username": [
            "The username field is required."
        ],
        "email": [
            "The email field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }
}

 ```

### Authorization Error Response (403 Forbidden)

It is returned when the user has already logged in. the API returns a `403 Forbidden` status.

**Example:**

``` json
{
    "success": false,
    "message": "You are already logged-in."
}

 ```

## Notes

- Consider implementing rate limiting for this endpoint to prevent abuse.


### Login

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/login`


## Overview

This endpoint authenticates an administrator user and returns an access token upon successful login. The token is automatically saved to the collection variables for use in subsequent authenticated requests.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/login`
    

## Request Body

The request body should contain the user details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `email` | string | Yes | The user email address ( most be unique ) |
| `password` | string | Yes | The user account password |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint does not require authentication as it is used to obtain the initial access token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── token: string
    └── user: object
        ├── name: string
        ├── username: string
        ├── email: string
        └── role: string

 ```

## Response Status

### Success Response (200 OK)

When the user logged in successfully, the API returns a `200 OK` status with the user details and token access.

**Example:**

``` json
{
    "success": true,
    "message": "Login successfully.",
    "data": {
        "token": "1|ncBa8ai5mRuUKL4ZHkt1sV1DeIwV0GCDwyK1wqZuc3f54e4f",
        "user": {
            "name": "Admin",
            "username": "admin",
            "email": "admin@mail.to",
            "role": "admin"
        }
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The email field is required. (and 1 more error)",
    "errors": {
        "email": [
            "The email field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }
}

 ```

### Authentication Error Response (401 Unauthorized)

It is returned when the user enters invalid credentials. the API returns a `401 Unauthorized` status.

**Example:**

``` json
{
    "success": false,
    "errorMessage": "Invalid credentials."
}

 ```

### Authorization Error Response (403 Forbidden)

It is returned when the user has already logged in. the API returns a `403 Forbidden` status.

**Example:**

``` json
{
    "success": false,
    "message": "You are already logged-in."
}

 ```

## Notes

- The access token is automatically saved to the collection variable `{{token}}` when login is successful (HTTP 200 response)


# Auth Routes


## Dashboard


#### Index

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/dashboard`


## Overview

This endpoint retrieves comprehensive dashboard data for administrators. It provides key metrics, statistics, and insights necessary for administrative oversight and decision-making within the CMS system.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/dashboard`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: object
    │
    ├── latestComment: Comment
    │   │
    │   ├── id: integer
    │   ├── name: string
    │   ├── email: string
    │   ├── body: string
    │   ├── status: string
    │   ├── createdAt: datetime
    │   ├── updatedAt: datetime
    │   │
    │   └── post: object
    │       ├── id: integer
    │       └── title: string
    │
    └── latestPost: Post
        │
        ├── id: integer
        ├── title: string
        ├── intro: string
        ├── content: string
        ├── status: string
        ├── image: string
        ├── createdAt: datetime
        ├── updatedAt: datetime
        │
        ├── author: object
        │   ├── id: integer
        │   ├── name: string
        │   ├── username: string
        │   ├── email: string
        │   ├── role: string
        │   ├── status: string
        │   ├── createdAt: datetime
        │   └── updatedAt: datetime
        │
        ├── category: object
        │   ├── id: integer
        │   ├── name: string
        │   └── showInHome: string
        │
        └── tags: Tag[]
            ├── id: integer
            ├── name: string
            └── showInHome: string

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the data.

**Example:**

``` json
{
    "success": true,
    "data": {
        "latestComment": {
            "id": 3,
            "name": "Suariz",
            "email": "suariz@mail.to",
            "body": "This is just comment for testing.",
            "status": "approved",
            "post": {
                "id": 3,
                "title": "Sample blog post 3"
            },
            "createdAt": "2024-02-06T17:21:24.000000Z",
            "updatedAt": "2024-02-06T17:21:36.000000Z"
        },
        "latestPost": {
            "id": 3,
            "title": "Sample Blog Post 3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "status": "published",
            "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHDzSa.png",
            "author": {
                "id": 1,
                "name": "Admin",
                "username": "admin",
                "email": "admin@mail.to",
                "role": "admin",
                "status": "active",
                "createdAt": "2024-02-06T13:50:03.000000Z",
                "updatedAt": "2024-02-06T13:50:03.000000Z"
            },
            "category": {
                "id": 3,
                "name": "IT Support",
                "showInHome": "no"
            },
            "tags": [
                {
                    "id": 3,
                    "name": "Linux",
                    "showInHome": "no"
                }
            ],
            "createdAt": "2024-02-06T17:21:24.000000Z",
            "updatedAt": "2024-02-06T17:21:36.000000Z"
        }
    }
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Logout

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/logout`


## Overview

This endpoint logs out an authenticated admin user from the system by invalidating their current session.

## Request Format

- **Method:** POST This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/logout`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When logged out successful, this endpoint returns a `200 OK` status.

**Example:**

``` json
{
    "success": true,
    "message": "Logout successfully."
}

 ```

## Post-Response Script

This request includes an automated script that runs after receiving the response:

- **Condition:** If response code is 200 AND `success` field is `true`
    
- **Action:** Automatically clears the `{{token}}` collection variable
    
- **Purpose:** Ensures the token is removed from the collection after successful logout, preventing accidental reuse of invalidated credentials
    

## Notes

- After a successful logout, you will need to authenticate again using the login endpoint to obtain a new token
    
- The token is automatically cleared by the post-response script, so manual cleanup is not required


## Categories


#### Fetch all categories

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/categories`


## Overview

This endpoint retrieves a complete list of all categories available in the system. It's an administrative endpoint that provides access to the full category catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/categories`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Category[]
│   ├── id: integer
│   ├── name: string
│   └── showInHome: string
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of categories.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "IT Support",
            "showInHome": "no"
        },
        {
            "id": 2,
            "name": "Software Testing",
            "showInHome": "no"
        },
        {
            "id": 1,
            "name": "Network Engineering",
            "showInHome": "no"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of categories, even if there are no categories available, in which case the array will be empty.


#### Fetch single category

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/categories/:category`


## Overview

This endpoint retrieves detailed information about a single category by its unique identifier. It's part of the admin category management system and requires authentication.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/categories/:category`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `category` | integer | Yes | The unique identifier of the category to fetch |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: object
    ├── name: string
    ├── slug: string
    └── showInHome: string

 ```

## Response Status

### Success Response (200 OK)

When the category is found successfully, the API returns a `200 OK` status with the category details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "name": "IT Support",
        "slug": "it-support",
        "showInHome": "no"
    }
}

 ```

### Error Response (404 Not Found)

When the specified category ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Store a new category

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/categories`


## Overview

This endpoint creates a new category in the system. It's an administrative endpoint that allows authorized users to add categories to organize posts within the CMS.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/categories`
    

## Request Body

The request body should contain the category details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The category name |
| `slug` | string | Yes | URL-friendly identifier |
| `show_in_home` | string | No | Indicates whether the category is displayed on the home ( accepting "no" and "yes" values ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── slug: string
    └── showInHome: string

 ```

## Response Status

### Success Response (201 Created)

When the category is successfully created, the API returns a `201 Created` status with the newly created category details including its assigned ID.

**Example :**

``` json
{
    "success": true,
    "message": "Category created successfully.",
    "data": {
        "name": "IT Support",
        "slug": "it-support",
        "showInHome": "no"
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate category name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 1 more error)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "slug": [
            "The slug field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Update a category

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/categories/:category`


## Overview

This endpoint allows you to update an existing category in the system. It modifies category details such as name, slug, or other attributes associated with a specific category identified by its ID.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/categories/:category`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `category` | integer | Yes | The unique identifier of the category to be updated |

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the category details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The category name |
| `slug` | string | Yes | URL-friendly identifier |
| `show_in_home` | string | No | Indicates whether the category is displayed on the home ( accepting "no" and "yes" values ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── slug: string
    └── showInHome: string

 ```

## Response Status

### Success Response (200 OK)

When the category is successfully updated, the API returns a `200 OK` status with updating the existing category.

**Example :**

``` json
{
    "success": true,
    "message": "Category updated successfully.",
    "data": {
        "name": "IT Support",
        "slug": "it-support",
        "showInHome": "yes"
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate category name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 1 more error)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "slug": [
            "The slug field is required."
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the specified category ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a category

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/categories/:category`


## Overview

This endpoint permanently removes a category from the system. Use this operation when you need to delete an existing category that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/categories/:category`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `category` | integer | Yes | The unique identifier of the category to be deleted. This should be the category ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the category is successfully deleted, the API returns a `200 OK` status with deleting the existing category.

**Example :**

``` json
{
    "success": true,
    "message": "Category deleted successfully."
}

 ```

### Error Response (404 Not Found)

When the specified category ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Prerequisites

Before deleting a category, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete categories.
    
2. **Dependencies:** Check if the category is associated with any posts, subcategories, or other entities that might be affected by the deletion.
    
3. **Backup:** Consider backing up category data before deletion, as this operation is typically irreversible.
    
4. **Cascade Effects:** Understand how deleting this category will impact related data in your system.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a category is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a category in production, test the operation in a development or staging environment to understand the full impact.


## Tags


#### Fetch all tags

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/tags`


## Overview

This endpoint retrieves a complete list of all tags available in the system. It's an administrative endpoint that provides access to the full tag catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/tags`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Tag[]
│   ├── id: integer
│   ├── name: string
│   └── showInHome: string
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of tags.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "Linux",
            "showInHome": "no"
        },
        {
            "id": 2,
            "name": "Blockchain",
            "showInHome": "no"
        },
        {
            "id": 1,
            "name": "DevOps",
            "showInHome": "no"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of tags, even if there are no tags available, in which case the array will be empty.


#### Fetch single tag

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/tags/:tag`


## Overview

This endpoint retrieves detailed information about a single tag by its unique identifier. It's part of the admin tag management system and requires authentication.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/tags/:tag`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `tag` | integer | Yes | The unique identifier of the tag to fetch |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: object
    ├── name: string
    ├── slug: string
    └── showInHome: string

 ```

## Response Status

### Success Response (200 OK)

When the tag is found successfully, the API returns a `200 OK` status with the tag details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "name": "Linux",
        "slug": "linux",
        "showInHome": "no"
    }
}

 ```

### Error Response (404 Not Found)

When the specified tag ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Store a new tag

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/tags`


## Overview

This endpoint creates a new tag in the system. It's an administrative endpoint that allows authorized users to add tags to organize posts within the CMS.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/tags`
    

## Request Body

The request body should contain the tag details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The tag name |
| `slug` | string | Yes | URL-friendly identifier |
| `show_in_home` | boolean | No | Indicates whether the tag is displayed on the home |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── slug: string
    └── showInHome: integer | string

 ```

## Response Status

### Success Response (201 Created)

When the tag is successfully created, the API returns a `201 Created` status with the newly created tag details including its assigned ID.

**Example :**

``` json
{
    "success": true,
    "message": "Tag created successfully.",
    "data": {
        "name": "Linux",
        "slug": "linux",
        "showInHome": 0
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate tag name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 1 more error)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "slug": [
            "The slug field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Update a tag

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/tags/:tag`


## Overview

This endpoint allows you to update an existing tag in the system. It modifies tag details such as name, slug, or other attributes associated with a specific tag identified by its ID.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/tags/:tag`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `tag` | integer | Yes | The unique identifier of the tag to be updated |

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the tag details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The tag name |
| `slug` | string | Yes | URL-friendly identifier |
| `show_in_home` | boolean | No | Indicates whether the tag is displayed on the home |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── slug: string
    └── showInHome: integer | string

 ```

## Response Status

### Success Response (200 OK)

When the tag is successfully updated, the API returns a `200 OK` status with updating the existing tag.

**Example :**

``` json
{
    "success": true,
    "message": "Tag updated successfully.",
    "data": {
        "name": "Linux",
        "slug": "linux",
        "showInHome": 1
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate tag name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 1 more error)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "slug": [
            "The slug field is required."
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the specified tag ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a tag

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/tags/:tag`


## Overview

This endpoint permanently removes a tag from the system. Use this operation when you need to delete an existing tag that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/tags/:tag`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `tag` | integer | Yes | The unique identifier of the tag to be deleted. This should be the tag ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the tag is successfully deleted, the API returns a `200 OK` status with deleting the existing tag.

**Example :**

``` json
{
    "success": true,
    "message": "Tag deleted successfully."
}

 ```

### Error Response (404 Not Found)

When the specified tag ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Prerequisites

Before deleting a tag, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete tags.
    
2. **Dependencies:** Check if the tag is associated with any posts or other entities that might be affected by the deletion.
    
3. **Backup:** Consider backing up tag data before deletion, as this operation is typically irreversible.
    
4. **Cascade Effects:** Understand how deleting this tag will impact related data in your system.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a tag is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a tag in production, test the operation in a development or staging environment to understand the full impact.


## Posts


#### Fetch all posts

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/posts`


## Overview

This endpoint retrieves a complete list of all posts available in the system. It's an administrative endpoint that provides access to the full post catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/posts`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Post[]
│   │
│   ├── id: integer
│   ├── title: string
│   ├── intro: string
│   ├── content: string
│   ├── image: string
│   ├── status: string
│   │
│   ├── category: object
│   │    ├── id: integer
│   │    └── name: string
│   │
│   ├── tags: Tag[]
│   │    ├── id: integer
│   │    └── name: string
│   │
│   ├── author: object
│   │    ├── id: integer
│   │    ├── name: string
│   │    └── email: string
│   │
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of posts.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "title": "Sample Blog Post 3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHDzSa.png",
            "featured": "no",
            "status": "published",
            "category": {
                "id": 3,
                "name": "IT Support"
            },
            "tags": [
                {
                    "id": 3,
                    "name": "Linux"
                }
            ],
            "author": {
                "id": 1,
                "name": "Admin",
                "email": "admin@mail.to"
            },
            "createdAt": "2024-02-06T17:21:24.000000Z",
            "updatedAt": "2024-02-06T17:21:36.000000Z"
        },
        {
            "id": 2,
            "title": "Sample Blog Post 2",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHDzAq.png",
            "featured": "no",
            "status": "published",
            "category": {
                "id": 2,
                "name": "Software Testing"
            },
            "tags": [
                {
                    "id": 3,
                    "name": "Linux"
                }
            ],
            "author": {
                "id": 1,
                "name": "Admin",
                "email": "admin@mail.to"
            },
            "createdAt": "2024-02-06T17:31:24.000000Z",
            "updatedAt": "2024-02-06T17:31:36.000000Z"
        },
        {
            "id": 1,
            "title": "Sample Blog Post 1",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHGzAq.png",
            "featured": "no",
            "status": "published",
            "category": {
                "id": 1,
                "name": "Network Engineering"
            },
            "tags": [
                {
                    "id": 3,
                    "name": "Linux"
                }
            ],
            "author": {
                "id": 1,
                "name": "Admin",
                "email": "admin@mail.to"
            },
            "createdAt": "2024-02-06T17:41:24.000000Z",
            "updatedAt": "2024-02-06T17:41:36.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of posts, even if there are no posts available, in which case the array will be empty.


#### Fetch single post

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/posts/:post`


## Overview

This endpoint retrieves detailed information about a single post by its unique identifier. It's part of the admin post management system and requires authentication.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/posts/:post`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `post` | integer | Yes | The unique identifier of the post to fetch |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: Post
    │
    ├── title: string
    ├── slug: string
    ├── intro: string
    ├── content: string
    ├── image: string
    ├── featured: string
    ├── status: string
    │
    ├── category: object
    │   ├── id: integer
    │   └── name: string
    │
    ├── tags: Tag[]
    │   ├── id: integer
    │   └── name: string
    │
    ├── author: object
    │   ├── id: integer
    │   ├── name: string
    │   └── email: string
    │
    ├── createdAt: datetime
    └── updatedAt: datetime

 ```

## Response Status

### Success Response (200 OK)

When the post is found successfully, the API returns a `200 OK` status with the post details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "title": "Sample Blog Post 1",
        "slug": "sample-blog-post-1",
        "intro": "sample blog post intro.",
        "content": "sample blog post content.",
        "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHU4uN.png",
        "featured": "no",
        "status": "published",
        "category": {
            "id": 1,
            "name": "IT Support"
        },
        "tags": [
            {
                "id": 1,
                "name": "Linux"
            }
        ],
        "author": {
            "id": 1,
            "name": "Admin",
            "email": "admin@mail.to"
        },
        "createdAt": "2024-02-06T17:21:24.000000Z",
        "updatedAt": "2024-02-06T17:21:36.000000Z"
    }
}

 ```

### Error Response (404 Not Found)

When the specified post ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Store a new post

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/posts`


## Overview

This endpoint creates a new post in the system. It's an administrative endpoint that allows authorized users to add posts in the CMS.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/posts`
    

## Request Body

The request body should contain the post details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | string | Yes | The post title |
| `slug` | string | Yes | URL-friendly identifier for the post |
| `intro` | string | Yes | Short description of the post |
| `content` | string | Yes | Full content of the post |
| `image` | file | Yes | Post featured image file |
| `category_id` | integer | Yes | ID of the category assigned to the post |
| `tags` | array\[integer\] | Yes | List of tag ID's assigned to the post |
| `is_featured` | string | No | Indicates whether the post is featured ( accepting "yes" and "no" values ) |
| `status` | string | No | Post publication status ( accepting "draft" and "published" values ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: Post
    │
    ├── id: integer
    ├── title: string
    ├── intro: string
    ├── content: string
    ├── image: string
    ├── featured: string
    ├── status: string
    │
    ├── category: object
    │   ├── id: integer
    │   └── name: string
    │
    ├── tags: Tag[]
    │    ├── id: integer
    │    └── name: string
    │
    ├── author: object
    │   ├── id: integer
    │   ├── name: string
    │   └── email: string
    │
    ├── createdAt: datetime
    └── updatedAt: datetime

 ```

## Response Status

### Success Response (201 Created)

When the post is successfully created, the API returns a `201 Created` status with the newly created post details including its assigned ID.

**Example :**

``` json
{
    "success": true,
    "message": "Post created successfully.",
    "data": {
        "id": 1,
        "title": "Sample blog post 1",
        "intro": "sample blog post intro.",
        "content": "sample blog post content.",
        "image": "http://127.0.0.1:8000/storage/posts/bxHyF8HwTYpuG5pis0p7i0NJqoRuxlbDqTCGWINh.png",
        "featured": "no",
        "status": "published",
        "category": {
            "id": 1,
            "name": "IT Support"
        },
        "tags": [
            {
                "id": 1,
                "name": "Linux"
            }
        ],
        "author": {
            "id": 1,
            "name": "Admin",
            "email": "admin@mail.to"
        },
        "createdAt": "2024-02-06T17:31:41.000000Z",
        "updatedAt": "2024-02-06T17:31:41.000000Z"
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate post title), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The title field is required. (and 6 more errors)",
    "errors": {
        "title": [
            "The title field is required."
        ],
        "intro": [
            "The intro field is required."
        ],
        "content": [
            "The content field is required."
        ],
        "tags": [
            "You must select at least one tag."
        ],
        "category_id": [
            "The category id field is required."
        ],
        "slug": [
            "The slug field is required."
        ],
        "image": [
            "The image field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Update a post

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/posts/:post`


## Overview

This endpoint allows you to update an existing post in the system. It modifies post details such as name, slug, or other attributes associated with a specific post identified by its ID.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/posts/:post`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `post` | integer | Yes | The unique identifier of the post to be updated |

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the post details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | string | Yes | The post title |
| `slug` | string | Yes | URL-friendly identifier for the post |
| `intro` | string | Yes | Short description of the post |
| `content` | string | Yes | Full content of the post |
| `image` | file / string | Yes | Post featured image file |
| `category_id` | integer | Yes | ID of the category assigned to the post |
| `tags` | array\[integer\] | Yes | List of tag ID's assigned to the post |
| `status` | string | No | Post publication status ( accepting "draft" and "published" values ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: Post
    │
    ├── id: integer
    ├── title: string
    ├── intro: string
    ├── content: string
    ├── image: string
    ├── featured: string
    ├── status: string
    │
    ├── category: object
    │   ├── id: integer
    │   └── name: string
    │
    ├── tags: Tag[]
    │    ├── id: integer
    │    └── name: string
    │
    ├── author: object
    │   ├── id: integer
    │   ├── name: string
    │   └── email: string
    │
    ├── createdAt: datetime
    └── updatedAt: datetime

 ```

## Response Status

### Success Response (200 OK)

When the post is successfully updated, the API returns a `200 OK` status with updating the existing post.

**Example :**

``` json
{
    "success": true,
    "message": "Post updated successfully.",
    "data": {
        "id": 1,
        "title": "Sample Blog Post 1 Up",
        "intro": "sample blog post intro.",
        "content": "sample blog post content.",
        "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHU4uN.png",
        "featured": "no",
        "status": "published",
        "category": {
            "id": 1,
            "name": "IT Support"
        },
        "tags": [
            {
                "id": 1,
                "name": "Linux"
            }
        ],
        "author": {
            "id": 1,
            "name": "Admin",
            "email": "admin@mail.to"
        },
        "createdAt": "2024-02-06T17:21:24.000000Z",
        "updatedAt": "2024-02-06T17:35:36.000000Z"
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate post title), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The title field is required. (and 5 more errors)",
    "errors": {
        "title": [
            "The title field is required."
        ],
        "intro": [
            "The intro field is required."
        ],
        "content": [
            "The content field is required."
        ],
        "tags": [
            "You must select at least one tag."
        ],
        "category_id": [
            "The category id field is required."
        ],
        "slug": [
            "The slug field is required."
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the specified post ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a post

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/posts/:post`


## Overview

This endpoint permanently removes a post from the system. Use this operation when you need to delete an existing post that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/posts/:post`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `post` | integer | Yes | The unique identifier of the post to be deleted. This should be the post ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the post is successfully deleted, the API returns a `200 OK` status with deleting the existing post.

**Example :**

``` json
{
    "success": true,
    "message": "Post deleted successfully."
}

 ```

### Error Response (404 Not Found)

When the specified post ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Prerequisites

Before deleting a post, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete posts.
    
2. **Backup:** Consider backing up post data before deletion, as this operation is typically irreversible.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a post is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a post in production, test the operation in a development or staging environment to understand the full impact.


## Roles


#### Fetch all roles

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/roles`


## Overview

This endpoint retrieves a complete list of all roles available in the system. It's an administrative endpoint that provides access to the full role catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/roles`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Role[]
│   │
│   ├── id: integer
│   ├── name: string
│   └── guardName: string
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of roles.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "author",
            "guardName": "sanctum"
        },
        {
            "id": 2,
            "name": "editor",
            "guardName": "sanctum"
        },
        {
            "id": 1,
            "name": "admin",
            "guardName": "sanctum"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of roles, even if there are no roles available, in which case the array will be empty.


#### Fetch single role

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/roles/:role`


## Overview

This endpoint retrieves detailed information about a single role by its unique identifier. It's part of the admin role management system and requires authentication.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/roles/:role`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `role` | integer | Yes | The unique identifier of the role to fetch |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: object
    ├── name: string
    ├── guardName: string
    └── permissions: string[]

 ```

## Response Status

### Success Response (200 OK)

When the role is found successfully, the API returns a `200 OK` status with the role details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "name": "admin",
        "guardName": "sanctum",
        "permissions": [
            "create_category",
            "update_category",
            "delete_category",
            "create_tag",
            "update_tag",
            "delete_tag",
            "create_post",
            "update_post",
            "delete_post",
            "create_comment",
            "update_comment",
            "delete_comment",
            "view_roles",
            "create_role",
            "update_role",
            "delete_role",
            "view_users",
            "create_user",
            "update_user",
            "delete_user"
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the specified role ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Store a new role

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/roles`


## Overview

This endpoint creates a new role in the system. It's an administrative endpoint that allows authorized users to add roles in the CMS.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/roles`
    

## Request Body

The request body should contain the role details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The role name (e.g admin, editor, author) |
| `permissions` | array\[string\] | Yes | List of permission names assigned to the role |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── guardName: string
    └── permissions: string[]

 ```

## Response Status

### Success Response (201 Created)

When the role is successfully created, the API returns a `201 Created` status with the newly created role details including its assigned ID.

**Example :**

``` json
{
    "success": true,
    "message": "Role created successfully.",
    "data": {
        "name": "Random Role",
        "guardName": "sanctum",
        "permissions": [
            "create_post",
            "update_post",
            "delete_post"
        ]
    }
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate role name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The permissions field is required. (and 1 more error)",
    "errors": {
        "permissions": [
            "The permissions field is required."
        ],
        "name": [
            "The name field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Update a role

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/roles/:role`


## Overview

This endpoint allows you to update an existing role in the system. It modifies role details such as name, slug, or other attributes associated with a specific role identified by its ID.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/roles/:role`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `role` | integer | Yes | The unique identifier of the role to be updated |

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the role details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The role name (e.g admin, editor, author) |
| `permissions` | array\[string\] | Yes | List of permission names assigned to the role |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── guardName: string
    └── permissions: string[]

 ```

## Response Status

### Success Response (200 OK)

When the role is successfully updated, the API returns a `200 OK` status with updating the existing role.

**Example :**

``` json
{
    "success": true,
    "message": "Role updated successfully.",
    "data": {
        "name": "Random Role",
        "guardName": "sanctum",
        "permissions": [
            "create_category",
            "update_category",
            "delete_category"
        ]
    }
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate role name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The permissions field is required. (and 1 more error)",
    "errors": {
        "permissions": [
            "The permissions field is required."
        ],
        "name": [
            "The name field is required."
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the specified role ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a role

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/roles/:role`


## Overview

This endpoint permanently removes a role from the system. Use this operation when you need to delete an existing role that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/roles/:role`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `role` | integer | Yes | The unique identifier of the role to be deleted. This should be the role ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the role is successfully deleted, the API returns a `200 OK` status with deleting the existing role.

**Example :**

``` json
{
    "success": true,
    "message": "Role deleted successfully."
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Error Response (404 Not Found)

When the specified role ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Prerequisites

Before deleting a role, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete roles.
    
2. **Dependencies:** Check if the role is associated with any users, or other entities that might be affected by the deletion.
    
3. **Backup:** Consider backing up role data before deletion, as this operation is typically irreversible.
    
4. **Cascade Effects:** Understand how deleting this role will impact related data in your system.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a role is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a role in production, test the operation in a development or staging environment to understand the full impact.


## Users


#### Fetch all users

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/users`


## Overview

This endpoint retrieves a complete list of all users available in the system. It's an administrative endpoint that provides access to the full user catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/users`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

StartFragment

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: User[]
│   │
│   ├── id: integer
│   ├── name: string
│   ├── username: string
│   ├── email: string
│   ├── role: string
│   ├── status: string
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of users.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "Rayan",
            "username": "rayan",
            "email": "rayan@mail.to",
            "role": "author",
            "status": "active",
            "createdAt": "2024-02-06T13:30:55.000000Z",
            "updatedAt": "2024-02-06T13:30:55.000000Z"
        },
        {
            "id": 2,
            "name": "Adam",
            "username": "adam",
            "email": "adam@mail.to",
            "role": "author",
            "status": "active",
            "createdAt": "2024-02-06T13:40:55.000000Z",
            "updatedAt": "2024-02-06T13:40:55.000000Z"
        },
        {
            "id": 1,
            "name": "David",
            "username": "david",
            "email": "david@mail.to",
            "role": "author",
            "status": "active",
            "createdAt": "2024-02-06T13:50:55.000000Z",
            "updatedAt": "2024-02-06T13:50:55.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of users, even if there are no users available, in which case the array will be empty.


#### Fetch single user

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/users/:user`


## Overview

This endpoint retrieves detailed information about a single user by its unique identifier. It's part of the admin user management system and requires authentication.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/users/:user`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `user` | integer | Yes | The unique identifier of the user to fetch |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: User
    │
    ├── name: string
    ├── username: string
    ├── email: string
    ├── role: string
    └── status: string

 ```

## Response Status

### Success Response (200 OK)

When the user is found successfully, the API returns a `200 OK` status with the user details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "name": "Admin",
        "username": "admin",
        "email": "admin@mail.to",
        "role": "admin",
        "status": "active"
    }
}

 ```

### Error Response (404 Not Found)

When the specified user ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Store a new user

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/users`


## Overview

This endpoint creates a new user in the system. It's an administrative endpoint that allows authorized users to add users in the CMS.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/users`
    

## Request Body

The request body should contain the user details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The user name |
| `username` | string | Yes | The user username |
| `email` | string | Yes | The user email address ( most be unique ) |
| `password` | string | Yes | The user account password |
| `role` | string | Yes | Role assigned to the user |
| `status` | string | No | User account status ( accepting "inactive" and "active" values ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: User
    │
    ├── name: string
    ├── username: string
    ├── email: string
    ├── role: string
    └── status: string

 ```

## Response Status

### Success Response (201 Created)

When the user is successfully created, the API returns a `201 Created` status with the newly created user details including its assigned ID.

**Example :**

``` json
{
    "success": true,
    "message": "User created successfully.",
    "data": {
        "name": "Lucas",
        "username": "lucas",
        "email": "lucas@mail.to",
        "role": "author",
        "status": "inactive"
    }
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate user name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 4 more errors)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "username": [
            "The username field is required."
        ],
        "email": [
            "The email field is required."
        ],
        "role": [
            "The role field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Update a user

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/users/:user`


## Overview

This endpoint allows you to update an existing user in the system. It modifies user details such as name, slug, or other attributes associated with a specific user identified by its ID.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/users/:user`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `user` | integer | Yes | The unique identifier of the user to be updated |

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the user details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The user name |
| `email` | string | Yes | The user email address ( most be unique ) |
| `password` | string | Yes | The user account password |
| `role` | string | Yes | Role assigned to the user |
| `status` | string | No | User account status ( accepting "inactive" and "active" values ) |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: User
    │
    ├── name: string
    ├── username: string
    ├── email: string
    ├── role: string
    └── status: string

 ```

## Response Status

### Success Response (200 OK)

When the user is successfully updated, the API returns a `200 OK` status with updating the existing user.

**Example :**

``` json
{
    "success": true,
    "message": "User updated successfully.",
    "data": {
        "name": "Lucas",
        "username": "lucas",
        "email": "lucas@mail.to",
        "role": "editor",
        "status": "active"
    }
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate user name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 4 more errors)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "username": [
            "The username field is required."
        ],
        "email": [
            "The email field is required."
        ],
        "role": [
            "The role field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the specified user ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a user

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/users/:user`


## Overview

This endpoint permanently removes a user from the system. Use this operation when you need to delete an existing user that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/users/:user`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `user` | integer | Yes | The unique identifier of the user to be deleted. This should be the user ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the user is successfully deleted, the API returns a `200 OK` status with deleting the existing user.

**Example :**

``` json
{
    "success": true,
    "message": "User deleted successfully."
}

 ```

### Error Response (404 Not Found)

When the specified user ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

### Error Response (403 Forbidden)

This response is returned when the authenticated user does not have sufficient permissions to access this endpoint.

**Example:**

``` json
{
    "success": false,
    "message": "You do not have permission to perform this action."
}

 ```

## Prerequisites

Before deleting a user, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete users.
    
2. **Backup:** Consider backing up category data before deletion, as this operation is typically irreversible.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a user is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a user in production, test the operation in a development or staging environment to understand the full impact.


## Comments


#### Fetch all comments

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/comments/all`


## Overview

This endpoint retrieves a complete list of all comments available in the system. It's an administrative endpoint that provides access to the full comment catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/comments`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Comment[]
│   │
│   ├── id: integer
│   ├── name: string
│   ├── email: string
│   ├── body: string
│   ├── status: string
│   │
│   ├── post: object
│   │   ├── id: integer
│   │   └── title: string
│   │
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of comments.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "Suariz",
            "email": "suariz@mail.to",
            "body": "This is just comment for testing.",
            "status": "approved",
            "post": {
                "id": 3,
                "title": "Sample blog post 3"
            },
            "createdAt": "2024-02-06T18:11:55.000000Z",
            "updatedAt": "2024-02-06T18:11:55.000000Z"
        },
        {
            "id": 2,
            "name": "Nikola",
            "email": "nikola@mail.to",
            "body": "This is just comment for testing.",
            "status": "approved",
            "post": {
                "id": 2,
                "title": "Sample blog post 2"
            },
            "createdAt": "2024-02-06T18:22:43.000000Z",
            "updatedAt": "2024-02-06T18:22:43.000000Z"
        },
        {
            "id": 1,
            "name": "Youcef",
            "email": "youcef@mail.to",
            "body": "This is just comment for testing.",
            "status": "approved",
            "post": {
                "id": 1,
                "title": "Sample blog post 1"
            },
            "createdAt": "2024-02-06T18:33:43.000000Z",
            "updatedAt": "2024-02-06T18:33:43.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of comments, even if there are no comments available, in which case the array will be empty.


#### Change comment status

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/comments/change-status/:comment`


## Overview

This endpoint allows you to update an existing comment in the system. It modifies comment status with a specific category identified by its ID.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/comments/:comment`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `comment` | integer | Yes | The unique identifier of the comment to be updated |

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── name: string
    ├── email: string
    ├── body: string
    ├── status: integer | string
    │
    └── post: object
        ├── id: integer
        └── title: string

 ```

## Response Status

### Success Response (200 OK)

When the comment is successfully updated, the API returns a `200 OK` status with updating the existing comment.

**Example :**

``` json
{
    "success": true,
    "message": "Comment status changed successfully.",
    "data": {
        "name": "David",
        "email": "david@mail.to",
        "body": "This is just comment for testing.",
        "status": false,
        "post": {
            "id": 1,
            "title": "Sample blog post 1"
        }
    }
}

 ```

### Error Response (404 Not Found)

When the specified comment ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a comment

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/comments/delete/:comment`


## Overview

This endpoint permanently removes a comment from the system. Use this operation when you need to delete an existing comment that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/comments/:comment`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `comment` | integer | Yes | The unique identifier of the comment to be deleted. This should be the comment ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the comment is successfully deleted, the API returns a `200 OK` status with deleting the existing comment.

**Example :**

``` json
{
    "success": true,
    "message": "Comment deleted successfully."
}

 ```

### Error Response (404 Not Found)

When the specified comment ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Prerequisites

Before deleting a comment, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete comments.
    
2. **Backup:** Consider backing up comment data before deletion, as this operation is typically irreversible.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a comment is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a comment in production, test the operation in a development or staging environment to understand the full impact.


## Notes


#### Fetch all notes

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/notes/all`


## Overview

This endpoint retrieves a complete list of all notes available in the system. It's an administrative endpoint that provides access to the full note catalog.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/notes`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication. Ensure that the `token` variable is set in your active environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Note[]
│   │
│   ├── id: integer
│   ├── title: string
│   └── content: string
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of notes.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3, 
            "title": "Untitled Note 3",
            "content": "This is just untitled note content."
        },
        {
            "id": 2,
            "title": "Untitled Note 2",
            "content": "This is just untitled note content."
        },
        {
            "id": 1,
            "title": "Untitled Note 1",
            "content": "This is just untitled note content."
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The response will be an array of notes, even if there are no notes available, in which case the array will be empty.


#### Store a new note

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/notes/save`


## Overview

This endpoint creates a new note in the system. It's an administrative endpoint that allows authorized users to add notes in the CMS.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/notes`
    

## Request Body

The request body should contain the note details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `title` | string | Yes | The note title |
| `slug` | string | Yes | URL-friendly identifier |
| `content` | string | Yes | Full note content text |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data: object
    ├── id: integer
    ├── title: string
    └── content: string

 ```

## Response Status

### Success Response (201 Created)

When the note is successfully created, the API returns a `201 Created` status with the newly created note details including its assigned ID.

**Example :**

``` json
{
    "success": true,
    "message": "Note created successfully.",
    "data": {
        "id": 1,
        "title": "Untitled Note 1",
        "content": "This is just untitled note content."
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate note name), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The title field is required. (and 2 more errors)",
    "errors": {
        "title": [
            "The title field is required."
        ],
        "slug": [
            "The slug field is required."
        ],
        "content": [
            "The content field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the nesscessary permissions to access this endpoint, as it is intended for admin use.


#### Delete a note

**Method:** `DELETE`

**Endpoint:** `{{url}}/{{version}}/admin/notes/delete/:note`


## Overview

This endpoint permanently removes a note from the system. Use this operation when you need to delete an existing note that is no longer needed.

## Request Format

- **Method:** `DELETE` GET This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/notes/:note`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `note` | integer | Yes | The unique identifier of the note to be deleted. This should be the note ID. |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the note is successfully deleted, the API returns a `200 OK` status with deleting the existing note.

**Example :**

``` json
{
    "success": true,
    "message": "Note deleted successfully."
}

 ```

### Error Response (404 Not Found)

When the specified note ID does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Prerequisites

Before deleting a note, consider the following:

1. **Admin Permissions:** Ensure your authentication token has administrative privileges to delete notes.
    
2. **Backup:** Consider backing up note data before deletion, as this operation is typically irreversible.
    

## Important Notes

⚠️ **Warning:** This is a destructive operation. Once a note is deleted, it cannot be recovered unless you have a backup.

💡 **Tip:** Before deleting a note in production, test the operation in a development or staging environment to understand the full impact.


## Profile


#### Profile details

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/admin/profile/details`


## Overview

This endpoint retrieves the detailed profile information for the currently authenticated admin user.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/profile/details`
    

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `token` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data:
    │
    ├── name: string
    ├── username: string
    ├── email: string
    └── role: string

 ```

## Response Status

### Success Response (200 OK)

The API returns a `200 OK` status with the current authenticated user profile details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "name": "Admin",
        "username": "admin",
        "email": "admin@mail.to",
        "role": "admin"
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Update profile

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/profile/update`


## Overview

This endpoint allows administrators to update their profile information. The request uses multipart/form-data to support file uploads and text data for profile updates.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/profile/update`
    

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the tag details. Common fields typically include :

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `name` | string | Yes | The name of current authenticated user |
| `username` | string | Yes | The username of current authenticated user |
| `email` | string | Yes | The email address of current authenticated user |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
├── message: string
│
└── data:
    ├── name: string
    ├── username: string
    ├── email: string
    └── role: string

 ```

## Response Status

### Success Response (200 OK)

When the current authenticated user profile details is successfully updated, the API returns a `200 OK` status with updated profile data.

**Example :**

``` json
{
    "success": true,
    "message": "Profile updated successfully.",
    "data": {
        "name": "Adam",
        "username": "adam",
        "email": "adam@mail.to",
        "role": "admin"
    }
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate username), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 1 more error)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "username": [
            "The username field is required."
        ],
        "email": [
            "The email field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


#### Change password

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/admin/profile/change-password`


## Overview

This endpoint allows an authenticated admin user to change their password by providing their current password and a new password.

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/admin/profile/change-password`
    

## HTTP Method

- **Method**: `POST` with `_method: PUT` override
    
- This endpoint uses a POST request with a form field `_method: PUT` to simulate a PUT request, which is a common pattern for frameworks that don't support PUT requests with multipart/formdata directly.
    

## Request Body

The request body should contain the user passwords. Common fields typically include :

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `old_password` | string | Yes | The current password of authenticated user for verification |
| `new_password` | string | Yes | The new password to set for the account |
| `confirm_password` | string | Yes | Password confirmation. Must match `new_password` exactly |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires authentication using a token. Ensure the `{{token}}` variable is set in your environment with a valid authentication token.

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (200 OK)

When the current authenticated user password is successfully updated, the API returns a `200 OK` status with a message confirming success.

**Example :**

``` json
{
    "success": true,
    "message": "Password changed successfully."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "success": false,
    "error": "The old password is incorrect."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The old password field is required. (and 2 more errors)",
    "errors": {
        "old_password": [
            "The old password field is required."
        ],
        "new_password": [
            "The new password field is required."
        ],
        "confirm_password": [
            "The confirm password field is required."
        ]
    }
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.


# Front Routes


## Home


#### Index

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}`


## Overview

This endpoint serves as the root/index endpoint of the API. It provides basic information about the API service, including available versions, status, and general metadata. This is typically the first endpoint to call when exploring the API.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires no authentication (unless specified by your API configuration).

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: object
    │
    ├── posts: Post[]
    │   │
    │   ├── title: string
    │   ├── slug: string
    │   ├── intro: string
    │   ├── content: string
    │   ├── image: string
    │   │
    │   ├── author: object
    │   │   ├── name: string
    │   │   └── username: string
    │   │
    │   ├── category: object
    │   │   ├── name: string
    │   │   └── slug: string
    │   │
    │   ├── tags: Tag[]
    │   │   ├── name: string
    │   │   └── slug: string
    │   │
    │   ├── createdAt: datetime
    │   └── updatedAt: datetime
    │
    ├── tags: Tag[]
    │   ├── name: string
    │   └── slug: string
    │
    └── categories: Category[]
        ├── name: string
        └── slug: string

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of latest data ( categories / posts / tags ).

**Example:**

``` json
{
    "success": true,
    "data": {
        "posts": [
            {
                "title": "Sample Blog Post 3",
                "slug": "sample-blog-post-3",
                "intro": "sample blog post intro.",
                "content": "sample blog post content.",
                "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHDzSa.png",
                "author": {
                    "name": "Admin",
                    "username": "admin"
                },
                "category": {
                    "name": "IT Support",
                    "slug": "it-support"
                },
                "tags": [
                    {
                        "name": "Linux",
                        "slug": "linux"
                    }
                ],
                "createdAt": "2024-02-06T17:21:24.000000Z",
                "updatedAt": "2024-02-06T17:21:36.000000Z"
            },
            {
                "title": "Sample Blog Post 2",
                "slug": "sample-blog-post-2",
                "intro": "sample blog post intro.",
                "content": "sample blog post content.",
                "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHDzAq.png",
                "author": {
                    "name": "Admin",
                    "username": "admin"
                },
                "category": {
                    "name": "Network Engineering",
                    "slug": "network-engineering"
                },
                "tags": [
                    {
                        "name": "Linux",
                        "slug": "linux"
                    }
                ],
                "createdAt": "2024-02-06T17:31:24.000000Z",
                "updatedAt": "2024-02-06T17:31:36.000000Z"
            },
            {
                "title": "Sample Blog Post 1",
                "slug": "sample-blog-post-1",
                "intro": "sample blog post intro.",
                "content": "sample blog post content.",
                "image": "http://127.0.0.1:8000/storage/posts/6MFCr03W5VyDGBGz86qxv0ELUrvOmFefynoHGzAq.png",
                "author": {
                    "name": "Admin",
                    "username": "admin"
                },
                "category": {
                    "name": "Software Testing",
                    "slug": "software-testing"
                },
                "tags": [
                    {
                        "name": "Linux",
                        "slug": "linux"
                    }
                ],
                "createdAt": "2024-02-06T17:41:24.000000Z",
                "updatedAt": "2024-02-06T17:41:36.000000Z"
            }
        ],
        "tags": [
            {
                "name": "Linux",
                "slug": "linux"
            },
            {
                "name": "Blockchain",
                "slug": "blockchain"
            },
            {
                "name": "DevOps",
                "slug": "devops"
            }
        ],
        "categories": [
            {
                "name": "IT Support",
                "slug": "it-support"
            },
            {
                "name": "Software Testing",
                "slug": "software-testing"
            },
            {
                "name": "Network Engineering",
                "slug": "network-engineering"
            }
        ]
    }
}

 ```

### Notes

- The response will be an list of latest data ( categories / posts / tags ), even if there are no data ( categories / posts / tags ) available, in which case the list will be empty.


## Blog


#### Index

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/blog`


## Overview

This endpoint retrieves a list of blog posts or entries from the content management system. Use this to fetch all available blog content for display or processing.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/blog`
    

## Request Headers

| Header | Value | Description |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Authentication

This endpoint requires no authentication (unless specified by your API configuration).

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Post[]
│   │
│   ├── title: string
│   ├── slug: string
│   ├── intro: string
│   ├── content: string
│   ├── image: string
│   │
│   ├── author: object
│   │   ├── name: string
│   │   └── username: string
│   │
│   ├── category: object
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── tags: Tag[]
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When successful, this endpoint returns a `200 OK` status with a JSON response containing the list of posts.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "title": "Sample blog post 3",
            "slug": "sample-blog-post-3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1oqeq.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:11:24.000000Z",
            "updatedAt": "2024-02-06T18:11:24.000000Z"
        },
        {
            "title": "Sample blog post 2",
            "slug": "sample-blog-post-2",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1hdAr.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:22:24.000000Z",
            "updatedAt": "2024-02-06T18:22:24.000000Z"
        },
        {
            "title": "Sample blog post 1",
            "slug": "sample-blog-post-1",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQyKdAze.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:33:24.000000Z",
            "updatedAt": "2024-02-06T18:33:24.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Notes

- The response will be an list of posts, even if there are no posts available, in which case the array will be empty.


#### Post details

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/blog/:slug`


## Overview

This endpoint retrieves detailed information about a specific blog post using its unique slug identifier.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/blog/:slug`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `slug` | string | Yes | The unique slug of the post to fetch |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: Post
    │
    ├── title: string
    ├── slug: string
    ├── intro: string
    ├── content: string
    ├── image: string
    │
    ├── author: object
    │   ├── name: string
    │   └── username: string
    │
    ├── category: object
    │   ├── name: string
    │   └── slug: string
    │
    ├── tags: Tag[]
    │   ├── name: string
    │   └── slug: string
    │
    ├── createdAt: datetime
    └── updatedAt: datetime

 ```

## Response Status

### Success Response (200 OK)

When the post is found successfully, the API returns a `200 OK` status with the post details.

**Example:**

``` json
{
    "success": true,
    "data": {
        "title": "Sample blog post 1",
        "slug": "sample-blog-post-1",
        "intro": "sample blog post intro.",
        "content": "sample blog post content.",
        "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1oqeq.png",
        "author": {
            "name": "Admin",
            "username": "admin"
        },
        "category": {
            "name": "IT Support",
            "slug": "it-support"
        },
        "tags": [
            {
                "name": "Linux",
                "slug": "linux"
            }
        ]
        "createdAt": "2024-02-06T18:22:24.000000Z",
        "updatedAt": "2024-02-06T18:22:24.000000Z"
    }
}

 ```

### Error Response (404 Not Found)

When the specified post slug does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The slug parameter is case-sensitive and should match exactly as stored in the system.
    
- Slugs typically use lowercase letters, numbers, and hyphens (no spaces or special characters).


#### Fetch posts by category

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/blog/categories/:slug`


## Overview

This endpoint retrieves all blog posts that belong to a specific category. Posts are filtered by the category unique slug identifier, allowing you to fetch content organized by topic or theme.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/blog/categories/:slug`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `slug` | string | Yes | The category slug used to filter blog posts |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Post[]
│   │
│   ├── title: string
│   ├── slug: string
│   ├── intro: string
│   ├── content: string
│   ├── image: string
│   │
│   ├── author: object
│   │   ├── name: string
│   │   └── username: string
│   │
│   ├── category: object
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── tags: Tag[]
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When the category is found successfully, the API returns a `200 OK` status with the related posts list.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "title": "Sample blog post 3",
            "slug": "sample-blog-post-3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1oqeq.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:11:24.000000Z",
            "updatedAt": "2024-02-06T18:11:24.000000Z"
        },
        {
            "title": "Sample blog post 2",
            "slug": "sample-blog-post-2",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1hdAr.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:22:24.000000Z",
            "updatedAt": "2024-02-06T18:22:24.000000Z"
        },
        {
            "title": "Sample blog post 1",
            "slug": "sample-blog-post-1",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQyKdAze.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:33:24.000000Z",
            "updatedAt": "2024-02-06T18:33:24.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Error Response (404 Not Found)

When the specified category slug does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The slug parameter is case-sensitive and should match exactly as stored in the system.
    
- Slugs typically use lowercase letters, numbers, and hyphens (no spaces or special characters).


#### Fetch posts by tag

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/blog/tags/:slug`


## Overview

This endpoint retrieves all blog posts that belong to a specific tag. Posts are filtered by the tag unique slug identifier, allowing you to fetch content organized by topic or theme.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/blog/tags/:slug`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `slug` | string | Yes | The tag slug used to filter blog posts |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Post[]
│   │
│   ├── title: string
│   ├── slug: string
│   ├── intro: string
│   ├── content: string
│   ├── image: string
│   │
│   ├── author: object
│   │   ├── name: string
│   │   └── username: string
│   │
│   ├── category: object
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── tags: Tag[]
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When the tag is found successfully, the API returns a `200 OK` status with the related posts list.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "title": "Sample blog post 3",
            "slug": "sample-blog-post-3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1oqeq.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:11:24.000000Z",
            "updatedAt": "2024-02-06T18:11:24.000000Z"
        },
        {
            "title": "Sample blog post 2",
            "slug": "sample-blog-post-2",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1hdAr.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:22:24.000000Z",
            "updatedAt": "2024-02-06T18:22:24.000000Z"
        },
        {
            "title": "Sample blog post 1",
            "slug": "sample-blog-post-1",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQyKdAze.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:33:24.000000Z",
            "updatedAt": "2024-02-06T18:33:24.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Error Response (404 Not Found)

When the specified tag slug does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The slug parameter is case-sensitive and should match exactly as stored in the system.
    
- Slugs typically use lowercase letters, numbers, and hyphens (no spaces or special characters).


#### Fetch posts by author

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/blog/authors/:username`


## Overview

This endpoint retrieves all blog posts that belong to a specific author. Posts are filtered by the user unique name identifier, allowing you to fetch content organized by topic or theme.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/blog/authors/:username`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `username` | string | Yes | The author username used to filter blog posts |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Post[]
│   │
│   ├── title: string
│   ├── slug: string
│   ├── intro: string
│   ├── content: string
│   ├── image: string
│   │
│   ├── author: object
│   │   ├── name: string
│   │   └── username: string
│   │
│   ├── category: object
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── tags: Tag[]
│   │   ├── name: string
│   │   └── slug: string
│   │
│   ├── createdAt: datetime
│   └── updatedAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links: object
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When the user is found successfully, the API returns a `200 OK` status with the related posts list.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "title": "Sample blog post 3",
            "slug": "sample-blog-post-3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1oqeq.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:11:24.000000Z",
            "updatedAt": "2024-02-06T18:11:24.000000Z"
        },
        {
            "title": "Sample blog post 2",
            "slug": "sample-blog-post-2",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1hdAr.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:22:24.000000Z",
            "updatedAt": "2024-02-06T18:22:24.000000Z"
        },
        {
            "title": "Sample blog post 1",
            "slug": "sample-blog-post-1",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQyKdAze.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:33:24.000000Z",
            "updatedAt": "2024-02-06T18:33:24.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Error Response (404 Not Found)

When the specified user username does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The username parameter is case-sensitive and should match exactly as stored in the system.
    
- Usernames typically use lowercase letters, numbers, and hyphens (no spaces or special characters).


#### Fetch related posts

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/blog/related/:slug`


## Overview

This retrieves a list of blog posts related to a specific post, identified by their abbreviated titles. This is useful for displaying "Related Posts" or "You May Also Like" sections on the blog post page, based on the same category associated with the current post.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/blog/related/:slug`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `slug` | string | Yes | The post slug used to return related posts |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
└── data: Post[]
    │
    ├── title: string
    ├── slug: string
    ├── intro: string
    ├── content: string
    ├── image: string
    │
    ├── author: object
    │   ├── name: string
    │   └── username: string
    │
    ├── category: object
    │   ├── name: string
    │   └── slug: string
    │
    ├── tags: Tag[]
    │   ├── name: string
    │   └── slug: string
    │
    ├── createdAt: datetime
    └── updatedAt: datetime


 ```

## Response Status

### Success Response (200 OK)

When the post is found successfully, the API returns a `200 OK` status with the related posts list.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "title": "Sample blog post 3",
            "slug": "sample-blog-post-3",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1oqeq.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:11:24.000000Z",
            "updatedAt": "2024-02-06T18:11:24.000000Z"
        },
        {
            "title": "Sample blog post 2",
            "slug": "sample-blog-post-2",
            "intro": "sample blog post intro.",
            "content": "sample blog post content.",
            "image": "http://127.0.0.1:8000/storage/posts/4pslKDfZYkrgZpYi4D56kayI6UiiACaTlQy1hdAr.png",
            "author": {
                "name": "Admin",
                "username": "admin"
            },
            "category": {
                "name": "IT Support",
                "slug": "it-support"
            },
            "tags": [
                {
                    "name": "Linux",
                    "slug": "linux"
                }
            ],
            "createdAt": "2024-02-06T18:22:24.000000Z",
            "updatedAt": "2024-02-06T18:22:24.000000Z"
        }
    ]
}

 ```

### Error Response (404 Not Found)

When the specified post does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```

## Notes

- Ensure that you have the necessary permissions to access this endpoint, as it is intended for admin use.
    
- The slug parameter is case-sensitive and should match exactly as stored in the system.
    
- Slugs typically use lowercase letters, numbers, and hyphens (no spaces or special characters).


## Comments


#### Fetch related comments

**Method:** `GET`

**Endpoint:** `{{url}}/{{version}}/comments/related/:slug`


## Overview

Retrieves all reviews related to a specific product. Use this endpoint to display or analyze reviews associated with a given product identifier.

## Request Format

- **Method:** `GET` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/reviews/related/:slug`
    

## Path Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `slug` | integer | Yes | The product slug used to return related product reviews |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
│
├── data: Comment[]
│   │
│   ├── id: integer
│   ├── name: string
│   ├── email: string
│   ├── body: string
│   └── createdAt: datetime
│
└── meta: object
    ├── currentPage: integer
    ├── lastPage: integer
    ├── perPage: integer
    ├── total: integer
    └── links
        ├── nextPageUrl: string | null
        └── previousPageUrl: string | null

 ```

## Response Status

### Success Response (200 OK)

When the product is found successfully, the API returns a `200 OK` status with the list of related reviews.

**Example:**

``` json
{
    "success": true,
    "data": [
        {
            "id": 3,
            "name": "Suariz",
            "email": "suariz@mail.to",
            "body": "This is just comment for testing.",
            "createdAt": "2024-02-06T18:11:55.000000Z"
        },
        {
            "id": 2,
            "name": "Nikola",
            "email": "nikola@mail.to",
            "body": "This is just comment for testing.",
            "createdAt": "2024-02-06T18:22:43.000000Z"
        },
        {
            "id": 1,
            "name": "Youcef",
            "email": "youcef@mail.to",
            "body": "This is just comment for testing.",
            "createdAt": "2024-02-06T18:33:43.000000Z"
        }
    ],
    "meta": {
        "currentPage": 1,
        "lastPage": 1,
        "perPage": 10,
        "total": 3,
        "links": {
            "nextPageUrl": null,
            "previousPageUrl": null
        }
    }
}

 ```

### Error Response (404 Not Found)

When the specified post does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Record not found."
}

 ```


#### Store a new comment

**Method:** `POST`

**Endpoint:** `{{url}}/{{version}}/comments/save-comment/:postID/:parentID/:replyID`


## Overview

This endpoint allows you to save a new comment to the system. Comments can be standalone posts, replies to parent comments, or nested replies within a comment thread.

## Comment Hierarchy

1. **Top-level comment :** Provide `postID` only, leave `parentID` and `replyID` empty
    
2. **Reply to comment :** Provide `postID` and `parentID`, leave `replyID` empty
    
3. **Nested reply :** Provide all three parameters
    

## Request Format

- **Method:** `POST` This method is used to request data from the specified resource.
    
- **Endpoint:** `{{url}}/{{version}}/comments/save-comment/:postID/:parentID/:replyID`
    

## Path Variables

| Variable | Type | Required | Description |
| --- | --- | --- | --- |
| `postID` | string | Yes | The unique identifier of the post where the comment is being added |
| `parentID` | string | Conditional | The ID of the parent comment. Required when adding a reply to an existing comment. Use empty or null for top-level comments |
| `replyID` | string | Conditional | The ID of the specific reply being responded to. Used for nested reply threads |

## Request Headers

| Header | Value | **Description** |
| --- | --- | --- |
| Accept | application/json | Specifies that the client expects a JSON response |

## Response Structure

The response returns a JSON object containing the following fields :

```
├── success: boolean
└── message: string

 ```

## Response Status

### Success Response (201 Created)

When the comment is successfully created, the API returns a `201 Created` status with a message confirming success.

**Example :**

``` json
{
    "success": true,
    "message": "Thank you for your comments."
}

 ```

### Validation Error Response (422 Unprocessable Content)

When the request data fails validation (e.g., missing required fields, invalid format, duplicate email), the API returns a `422 Unprocessable Content` status.

**Example :**

``` json
{
    "message": "The name field is required. (and 2 more errors)",
    "errors": {
        "name": [
            "The name field is required."
        ],
        "body": [
            "The body field is required."
        ],
        "email": [
            "The email field is required."
        ]
    }
}

 ```

### Error Response (404 Not Found)

When the post you want to comment on does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "Post not found."
}

 ```

When the original comment ( `parentID` ) you want to reply to does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "The comment ( parent ) you want to reply to does not exist."
}

 ```

When the response commet ( `replyID` ) you want to reply to does not exist, the API returns a `404 Not Found` status.

**Example:**

``` json
{
    "success": false,
    "message": "The comment ( child ) you want to reply to does not exist."
}

 ```

## Notes

- Consider implementing rate limiting for this endpoint to prevent abuse.
