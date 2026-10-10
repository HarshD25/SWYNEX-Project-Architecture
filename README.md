# SWYNEX-Project-Architecture: DevSync

Architecture documentation and technical planning for Task 1 of the SWYNEX internship. This project outlines the system design for **DevSync**, a collaborative code snippet and developer workspace platform.

## 1. Project Overview
DevSync is designed to help small development teams organize code snippets, manage shared workspaces, and collaborate through inline comments and code reviews. 

### Core Features
- JWT-based user authentication and access control.
- Workspace creation and organization by tech stack or project.
- Code snippet editor with language tagging and instant updates.
- Comment threads for code reviews and team feedback.
  
## 2. Database Schema
Relational database design planned using PostgreSQL and SQLAlchemy:

### Users Table
- `id` (INTEGER, Primary Key)
- `username` (VARCHAR, Unique, Not Null)
- `email` (VARCHAR, Unique, Not Null)
- `password_hash` (VARCHAR, Not Null)
- `created_at` (TIMESTAMP)

### Workspaces Table
- `id` (INTEGER, Primary Key)
- `title` (VARCHAR, Not Null)
- `description` (TEXT)
- `owner_id` (INTEGER, Foreign Key referencing `users.id`)
- `created_at` (TIMESTAMP)

### Snippets Table
- `id` (INTEGER, Primary Key)
- `workspace_id` (INTEGER, Foreign Key referencing `workspaces.id`)
- `title` (VARCHAR, Not Null)
- `code_content` (TEXT, Not Null)
- `language` (VARCHAR, Default: 'python')
- `created_by` (INTEGER, Foreign Key referencing `users.id`)

### Comments Table
- `id` (INTEGER, Primary Key)
- `snippet_id` (INTEGER, Foreign Key -> `snippets.id`)
- `user_id` (INTEGER, Foreign Key -> `users.id`)
- `comment_text` (TEXT, Not Null)
- `created_at` (TIMESTAMP)

## 3. REST API Endpoints

| Method | Endpoint | Description |

| **POST** | `/api/auth/register` | Register a new user |
| **POST** | `/api/auth/login` | Authenticate user & issue token |
| **GET** | `/api/workspaces` | Retrieve user's workspaces |
| **POST** | `/api/workspaces` | Create a new workspace |
| **GET** | `/api/workspaces/:id/snippets` | Get all snippets in a workspace |
| **POST** | `/api/workspaces/:id/snippets` | Add a code snippet to a workspace |
| **PATCH** | `/api/snippets/:id` | Update snippet content or language |
| **DELETE** | `/api/snippets/:id` | Delete a specific snippet |
| **POST** | `/api/snippets/:id/comments` | Post a comment on a snippet |

## 4. Frontend Component Structure
- **Authentication View (`/login`, `/register`):** Forms for login and account creation with client-side error handling.
- **Workspaces Dashboard (`/dashboard`):** Main landing view displaying a grid of user workspaces with quick access links.
- **Workspace Editor View (`/workspace/:id`):** Split layout featuring a file/snippet navigation sidebar on the left and a code editor view on the right.
