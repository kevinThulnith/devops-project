![Django](https://img.shields.io/badge/Django-%23092E20.svg?logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/Django_REST-FF1709?logo=django&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?logo=JSON%20web%20tokens)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
[![Vite](https://img.shields.io/badge/Vite-646CFF?logo=vite&logoColor=fff)](#)
[![React](https://img.shields.io/badge/React-%2320232a.svg?logo=react&logoColor=%2361DAFB)](#)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwind-css&logoColor=white)
![Axios](https://img.shields.io/badge/Axios-5A29E4?logo=axios&logoColor=white)
[![NodeJS](https://img.shields.io/badge/Node.js-6DA55F?logo=node.js&logoColor=white)](#)
![npm](https://img.shields.io/badge/npm-CB3837?logo=npm&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)

# DevOps Project - Live Reloading Inventory Management System

The project includes a complete Docker Compose setup for easy deployment and development with version 3.0 configuration | Websocker | Live reloading.

## 🚀 Quick Docker Setup

1. **📋 Prerequisites**:

   - Docker Engine 20.0+
   - Docker Compose 3.0+

2. **⚙️ Environment Configuration**:

   Create a `.env.prod` file in the project root:

   ```env
   # Database Configuration
   POSTGRES_DB=devops_inventory
   POSTGRES_USER=devops_user
   POSTGRES_PASSWORD=your_secure_password
   DATABASE_URL=postgresql://devops_user:your_secure_password@lr-database:5432/devops_inventory

   # Django Configuration
   DEBUG=False
   DJANGO_SECRET_KEY=your-super-secret-key-here
   ALLOWED_HOSTS=localhost,127.0.0.1,lr-backend

   # Redis Configuration
   REDIS_HOST=lr-redis
   REDIS_PORT=6379
   ```

3. **🏃‍♂️ Start All Services**:

   ```powershell
   docker-compose up -d
   ```

4. **📊 Check Service Status**:

   ```powershell
   docker-compose ps
   ```

5. **📝 Run Database Migrations**:

   ```powershell
   docker-compose exec lr-backend python manage.py migrate
   ```

6. **👤 Create Superuser**:

   ```powershell
   docker-compose exec lr-backend python manage.py createsuperuser
   ```

7. **📦 Populate Sample Data**:

   ```powershell
   docker-compose exec lr-backend python manage.py populate_products --count 40
   ```

### 🌐 Service Access Points

- **Frontend Application**: <http://localhost> (via Nginx proxy)
- **Backend API**: <http://localhost/api/> (via Nginx proxy)
- **Django Admin**: <http://localhost/admin/> (via Nginx proxy)
- **PostgreSQL Database**: localhost:5432
- **Redis Cache**: localhost:6379

### 🔧 Docker Services Overview

| Service         | Description              | Image/Build              | Ports     | Dependencies      |
| --------------- | ------------------------ | ------------------------ | --------- | ----------------- |
| **lr-proxy**    | 🌐 Nginx reverse proxy   | `nginx:1.25-alpine-slim` | 80:80     | frontend, backend |
| **lr-frontend** | ⚛️ React application     | Custom build             | 5173:5173 | -                 |
| **lr-backend**  | 🐍 Django API server     | Custom build             | 8000:8000 | database, redis   |
| **lr-database** | 🐘 PostgreSQL database   | `postgres:14-alpine`     | 5432:5432 | -                 |
| **lr-redis**    | 🧠 Redis cache & pub/sub | `redis:7-alpine`         | 6379:6379 | -                 |

### 📁 Required Docker Files

Make sure you have these Dockerfiles in their respective directories:

**Backend Dockerfile** (`backend/Dockerfile`):

```dockerfile
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["daphne", "-b", "0.0.0.0", "-p", "8000", "backend.asgi:application"]
```

**Frontend Dockerfile** (`frontend/Dockerfile`):

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
```

### 🛠️ Docker Management Commands

```powershell
   # Start all services
   docker-compose up -d

   # Stop all services
   docker-compose down

   # View logs
   docker-compose logs -f [service-name]

   # Rebuild and restart a service
   docker-compose up -d --build [service-name]

   # Execute commands in containers
   docker-compose exec lr-backend python manage.py [command]
   docker-compose exec lr-frontend npm [command]

   # Clean up unused Docker resources
   docker system prune -a
```

A full-stack, real-time inventory management system with live product updates via WebSockets, modern authentication, and complete Docker containerization for DevOps deployment.

## 🏗️ System Architecture

```mermaid
graph TB
   subgraph "🐳 Docker Environment"
      Proxy[🛡️ Nginx Proxy<br/>Port 80]
      Frontend[⚛️ React App<br/>Port 5173]
      Backend[🐍 Django Backend<br/>Port 8000]
      Database[(🐘 PostgreSQL Database<br/>Port 5432)]
      Redis[(🧠 Redis Pub/Sub<br/>Port 6379)]
   end

   User[👤 User] --> Proxy
   Proxy --> Frontend
   Proxy --> Backend
   Frontend --> Backend
   Frontend -.-> WS[📡 WebSocket]
   Backend --> Database
   WS --> Redis
   Backend -.-> Redis

   style Frontend fill:#00008B,color:#fff
   style Backend fill:#092E20,color:#fff
   style Database fill:#336791,color:#fff
   style Redis fill:#DC382D,color:#fff
   style Proxy fill:#009639,color:#fff
```

## 📊 Data Flow

```mermaid
sequenceDiagram
    participant U as 👤 User
    participant F as ⚛️ Frontend
    participant A as 🔧 API
    participant D as 🗄️ DB

    U->>F: Login
    F->>A: POST /token/
    A->>D: Validate
    D-->>A: User OK
    A-->>F: JWT Token

    U->>F: Create Product
    F->>A: POST /products/
    A->>D: Save Product
    A-->>F: Product Created
    F-->>U: Live Update ⚡
```

## Simple Component Flow

```mermaid
flowchart TD
    A[🔑 Login] --> B[🏠 Dashboard]
    B --> C[🛍️ Products]
    C --> D[➕ Add Product]
    C --> E[✏️ Edit Product]
    C --> F[🗑️ Delete Product]

    D --> G[📡 Real-time Update]
    E --> G
    F --> G

    G --> C

    style A fill:#00796B,color:#fff
    style B fill:#0288D1,color:#fff
    style C fill:#C62828,color:#fff
    style D fill:#2E7D32,color:#fff
    style E fill:#EF6C00,color:#fff
    style F fill:#6D4C41,color:#fff
    style G fill:#4527A0,color:#fff
```

## ✨ Features

### 🎯 Core Functionality

- **📦 Product Management**: CRUD operations for inventory items with image uploads
- **⚡ Real-time Updates**: Live WebSocket connections for instant product changes
- **🔐 User Authentication**: JWT-based auth with automatic token refresh
- **📱 Responsive UI**: Modern React interface with Tailwind CSS and Framer Motion animations
- **🔍 Advanced Filtering**: Search, category filtering, and sorting capabilities

### ⚙️ Technical Features

- **🛡️ Custom JWT WebSocket Middleware**: Secure WebSocket authentication using JWT tokens
- **📡 Django Signals Integration**: Automatic real-time notifications on model changes
- **🚧 Protected Routes**: Client-side route protection with automatic redirects
- **🖼️ Image Handling**: Product image uploads with media file management
- **🌐 CORS Support**: Configured for cross-origin frontend-backend communication

### 🐳 DevOps Features

- **🐳 Full Docker Containerization**: Complete multi-container setup with Docker Compose 3.0
- **🌐 Nginx Reverse Proxy**: Load balancing and SSL termination ready
- **📦 Multi-stage Builds**: Optimized Docker images for production deployment
- **🔧 Environment Configuration**: Separate configurations for development and production
- **📊 Service Orchestration**: Automated service dependency management
- **💾 Persistent Data Volumes**: Configured volumes for database and Redis data persistence
- **🔄 Health Checks**: Container health monitoring and automatic restarts
- **🌍 Network Isolation**: Secure inter-service communication via Docker networks

## 🛠️ Tech Stack

### 🐍 Backend

- **Django 5.2+** - 🌐 Web framework
- **Django REST Framework** - 🔧 API development
- **Django Channels** - 📡 WebSocket support
- **Daphne** - 🚀 ASGI server for WebSockets
- **Redis** - 💾 Channel layer backend
- **Simple JWT** - 🔐 Authentication
- **Pillow** - 🖼️ Image processing

### ⚛️ Frontend

- **React 18+** - 🎨 UI library
- **Vite 7.0+** - ⚡ Build tool and dev server
- **Tailwind CSS** - 💅 Styling framework
- **Framer Motion** - 🎬 Animations
- **Axios** - 📬 HTTP client
- **React Router** - 🗺️ Navigation
- **JWT Decode** - 🔓 Token handling

### 🗄️ Database & Infrastructure

- **PostgreSQL** (production) or **SQLite** (development) - 📊 Database
- **Redis** - 📡 WebSocket channel layer & caching
- **WhiteNoise** - 📁 Static file serving

### 🐳 DevOps & Deployment

- **Docker** - 🐳 Containerization platform
- **Docker Compose 3.0** - 🔧 Multi-container orchestration
- **Nginx** - 🌐 Reverse proxy and load balancer
- **Alpine Linux** - 🏔️ Lightweight container base images

## 📋 Prerequisites

### For Development (Local Setup)

- **🐍 Python 3.13+**
- **🟢 Node.js 16+** and npm
- **🔴 Redis server** (for WebSocket channel layer)
- **📝 Git**

### For Docker Deployment

- **🐳 Docker Engine 20.0+**
- **🔧 Docker Compose 3.0+**
- **📝 Git**

## � Docker Deployment

For a complete containerized deployment with all services, check out the **`docker-compose`** branch which includes:

- **🐍 Backend Container**: Django application with PostgreSQL support
- **⚛️ Frontend Container**: React application served with Nginx
- **🐘 PostgreSQL Database**: Fully configured database container
- **🌐 Nginx Proxy**: Load balancer and reverse proxy
- **🔴 Redis Server**: Cache and WebSocket channel layer

To use the Docker setup:

```powershell
git checkout docker-compose
docker-compose up -d
```

This will spin up all containers and services automatically with proper networking and volume mounting.

## �🚀 Quick Start

### 1️⃣ Clone the Repository

```powershell
git clone https://github.com/kevinThulnith/devops-project.git
cd live-reloading-inventory-management-system
```

### 2️⃣ Backend Setup

1. **Install uv python package in pc via pip:**

   ```powershell
   pip install uv
   ```

2. **Install dependencies via uv:** _This installs the project and dependencies from `pyproject.toml`_

   ```powershell
   uv sync
   ```

3. **Activate auto generated vertual env:**

   ```powershell
   .\.venv\Scripts\Activate.ps1 # On windows
   ```

4. **Go to backend dir:**

   ```powershell
   cd backend
   ```

5. **⚙️ Configure environment variables:**

   Create a `.env` file in the project root:

   ```powershell
   DEBUG=True # Important when importing images to frontend
   DJANGO_SECRET_KEY=your-secret-key-here
   DATABASE_URL=sqlite:///db.sqlite3
   REDIS_HOST=127.0.0.1
   REDIS_PORT=6379
   ```

6. **🗄️ Run database migrations:**

   ```powershell
   py manage.py migrate
   ```

7. **👤 Create a superuser (optional):**

   ```powershell
   py manage.py createsuperuser
   ```

8. **📦 Populate sample products (optional):** create at leaat 4 users

   ```powershell
   py manage.py populate_products --count 40
   ```

9. **🚀 Run the Django server with daphniw:**

   ```powershell
   daphne -b 0.0.0.0 -p 8000 backend.asgi:application

   # If vertual environment not acticated
   uv run daphne -b 0.0.0.0 -p 8000 backend.asgi:application
   ```

### 3️⃣ Frontend Setup

1. **📁 Navigate to frontend directory:**

   ```powershell
   cd frontend
   ```

2. **📦 Install npm dependencies:**

   ```powershell
   npm i
   ```

3. **⚙️ Configure environment variables:**

   Create a `.env` file in the `frontend` directory:

   ```env
   VITE_API_URL=http://localhost:8000
   VITE_WS_URL=ws://localhost:8000
   ```

4. **🚀 Start the development server:**

   ```powershell
   npm run dev
   ```

   The frontend will be available at `http://localhost:5173`

## 🎯 API Endpoints

### 🔐 Authentication

- `POST /api/token/` - 🎫 Obtain JWT tokens
- `POST /api/token/refresh/` - 🔄 Refresh access token
- `POST /api/token/blacklist/` - 🚪 Logout (blacklist refresh token)
- `POST /api/user/register/` - 📝 User registration
- `GET /api/user/` - 👤 Get current user info

### 📦 Products

- `GET /api/products/` - 📋 List all products (with filtering, search, ordering)
- `POST /api/products/` - ➕ Create a new product
- `GET /api/products/{id}/` - 👁️ Retrieve a specific product
- `PUT/PATCH /api/products/{id}/` - ✏️ Update a product (owner only)
- `DELETE /api/products/{id}/` - 🗑️ Delete a product (owner only)
- `GET /api/products/my_products/` - 📄 List current user's products

### 📡 WebSocket

- `ws://localhost:8000/ws/products/?token={jwt_token}` - ⚡ Real-time product updates

## 🏗️ Project Architecture

### Backend Structure

```text
backend/
├── api/                    # Main application
│   ├── consumers.py       # WebSocket consumers
│   ├── middleware.py      # JWT WebSocket authentication
│   ├── models.py          # Product model
│   ├── permissions.py     # Custom permissions
│   ├── routing.py         # WebSocket URL routing
│   ├── serializers.py     # API serializers
│   ├── signals.py         # Real-time update signals
│   ├── views.py           # API viewsets
│   └── management/
│       └── commands/
│           └── populate_products.py  # Sample data command
└── backend/               # Django project settings
    ├── asgi.py           # ASGI configuration for WebSockets
    ├── settings.py       # Django settings
    └── urls.py           # URL configuration
```

### Frontend Structure

```text
frontend/
├── src/
│   ├── components/        # Reusable React components
│   │   ├── Navbar.jsx
│   │   ├── ProtectedRoute.jsx
│   │   └── LoadingIndicator.jsx
│   ├── pages/            # Page components
│   │   ├── Home.jsx      # Product listing with real-time updates
│   │   ├── Login.jsx     # Authentication
│   │   ├── Register.jsx  # User registration
│   │   ├── AddProduct.jsx
│   │   ├── UpdateProduct.jsx
│   │   └── MyProducts.jsx
│   ├── api.js            # Axios configuration
│   ├── constants.js      # JWT token constants
│   └── App.jsx           # Main app component
└── package.json
```

## 🔑 Key Features Explained

### ⚡ Real-time WebSocket Updates

The system uses Django Channels with custom JWT authentication middleware to provide secure, real-time updates:

- **🔐 Authentication**: WebSocket connections authenticate using JWT tokens passed as query parameters
- **📡 Signal Integration**: Django model signals automatically broadcast changes to connected clients
- **👥 Group Communication**: All connected users receive live updates when products are created, updated, or deleted

### 🎫 JWT Authentication Flow

1. 🔑 User logs in and receives access/refresh token pair
2. 💾 Frontend stores tokens in localStorage
3. 📬 API requests include Bearer token in Authorization header
4. 🔗 WebSocket connections authenticate via query parameter
5. 🔄 Tokens automatically refresh 5 minutes before expiration

### 📦 Product Management

- **🔧 CRUD Operations**: Full create, read, update, delete functionality
- **🖼️ Image Uploads**: Product images stored in media directory
- **👤 Owner Permissions**: Users can only modify their own products
- **🔍 Advanced Filtering**: Search by name/description, filter by category, sort by various fields

## 🧪 Testing WebSocket Functionality

A test HTML file (`ws_test.html`) is included for WebSocket testing:

1. 🚀 Start the Django server
2. 🌐 Open `ws_test.html` in a browser
3. ➕ Create/update/delete products in Django Admin
4. 👀 See real-time updates in the test page

## 🛠️ Development

### 🔧 Running in Development Mode

**🐍 Backend (Django with auto-reload):**

```powershell
cd backend
python manage.py runserver
```

**⚛️ Frontend (Vite with HMR):**

```powershell
cd frontend
npm run dev
```

### 🏗️ Building for Production

**⚛️ Frontend:**

```powershell
cd frontend
npm run build
```

The built files will be in `frontend/dist/`

## 🔧 Configuration

### ⚙️ Environment Variables

**🐍 Backend (.env):**

```env
DEBUG=True
DJANGO_SECRET_KEY=your-secret-key
DATABASE_URL=sqlite:///db.sqlite3
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
```

**⚛️ Frontend (.env):**

```env
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000
```

## 📝 License

This project is licensed under the MIT License. Feel free to fork and modify as needed. 🎉
