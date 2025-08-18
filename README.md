![Django](https://img.shields.io/badge/Django-%23092E20.svg?logo=django&logoColor=white)
![DRF](https://img.shields.io/badge/Django_REST-FF1709?logo=django&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?logo=JSON%20web%20tokens)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?logo=vite&logoColor=fff)
![React](https://img.shields.io/badge/React-%2320232a.svg?logo=react&logoColor=%2361DAFB)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-38B2AC?logo=tailwind-css&logoColor=white)
![Axios](https://img.shields.io/badge/Axios-5A29E4?logo=axios&logoColor=white)
![NodeJS](https://img.shields.io/badge/Node.js-6DA55F?logo=node.js&logoColor=white)
![npm](https://img.shields.io/badge/npm-CB3837?logo=npm&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)

# DevOps Project - Live Reloading Inventory Management System

The project includes a complete Docker Compose setup for easy deployment and development with version 3.0 configuration | **WebSocket** | **Live reloading**. With Django backend and React frontend styled with Tailwind CSS.

<img src="diagram.png" />

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

## 🛠️ Development Environment Setup

### 📁 Project Structure

```
📂 LR 3.0/
├── 🔧 pyproject.toml          # Python dependencies & project config
├── 🔒 uv.lock                 # Dependency lock file
├── 📖 README.md               # Project documentation
├── 📂 backend/                # Django application
│   ├── 🗄️ db.sqlite3          # Development database
│   ├── ⚙️ manage.py           # Django CLI
│   └── 📂 api/                # Main API app
└── 📂 frontend/               # React application
    ├── 📦 package.json        # Node.js dependencies
    ├── ⚡ vite.config.js       # Vite configuration
    └── 🎨 tailwind.config.js  # Tailwind setup
```

### 🚀 Quick Start Commands

#### Clone Git Repository

```sh
git clone https://github.com/kevinThulnith/devops-project.git

# Go to project directory
cd devops-project

# Go to main branch
git checkout main
```

#### Backend Setup 🔙

- Create `.env` file in `backend/` directory and add this configuration:

```env
DEBUG=True
REDIS_PORT=6379
REDIS_HOST=127.0.0.1
DATABASE_URL=sqlite:///db.sqlite3
SECRET_KEY="your_secret_key_here_generate_a_secure_one"
```

- **Redis Installation Required**: Install Redis on your local device for WebSocket functionality

```powershell
# If uv is not installed locally
pip install uv

# Install Python dependencies
uv sync

# Activate virtual environment (Windows PowerShell)
.\.venv\Scripts\Activate.ps1

# Navigate to backend directory
cd backend

# Run database migrations
py .\manage.py migrate

# Start backend server with Daphne (ASGI server)
daphne -b 0.0.0.0 -p 8000 backend.asgi:application

# Alternative: If virtual environment not activated
uv run daphne -b 0.0.0.0 -p 8000 backend.asgi:application
```

#### Frontend Setup 🎨

```powershell
# Install Node.js dependencies
cd frontend
npm i

# Start development server with hot reload
npm run dev

# Create production build files
npm run build

# Start with network access (accessible from other devices)
npm run host
```

## 🧪 Testing WebSocket Functionality

A test HTML file (`ws_test.html`) is included for WebSocket testing:

1. 🚀 Start the Django server
2. 🌐 Open `ws_test.html` in a browser
3. ➕ Create/update/delete products in Django Admin
4. 👀 See real-time updates in the test page

## Production Setup - 🐳 Docker Deployment

For a complete containerized deployment with all services, check out the **`docker-compose`** branch which includes:

- **🐍 Backend Container**: Django application with PostgreSQL support
- **⚛️ Frontend Container**: React application served with Nginx
- **🐘 PostgreSQL Database**: Fully configured database container
- **🌐 Nginx Proxy**: Load balancer and reverse proxy
- **🔴 Redis Server**: Cache and WebSocket channel layer

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
   # Change branch
   git checkout docker-compose

   # In root dir
   docker-compose up -d
   ```

4. **📊 Check Service Status**:

   ```powershell
   docker-compose ps
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

## 📚 API Documentation

### 🔗 Main Endpoints

| Method   | Endpoint                     | Description                 | Authentication           |
| -------- | ---------------------------- | --------------------------- | ------------------------ |
| `POST`   | `/api/user/register/`        | User registration           | ❌ Public                |
| `POST`   | `/api/token/`                | User login (get JWT token)  | ❌ Public                |
| `POST`   | `/api/token/refresh/`        | Refresh JWT token           | 🔐 Required              |
| `POST`   | `/api/token/blacklist/`      | Logout (blacklist token)    | 🔐 Required              |
| `GET`    | `/api/user/`                 | Get current user info       | 🔐 Required              |
| `GET`    | `/api/products/`             | List all products           | 🔐 Required              |
| `POST`   | `/api/products/`             | Create new product          | 🔐 Required              |
| `GET`    | `/api/products/{id}/`        | Get product details         | 🔐 Required              |
| `PUT`    | `/api/products/{id}/`        | Update product (full)       | 🔐 Required (Owner only) |
| `PATCH`  | `/api/products/{id}/`        | Update product (partial)    | 🔐 Required (Owner only) |
| `DELETE` | `/api/products/{id}/`        | Delete product              | 🔐 Required (Owner only) |
| `GET`    | `/api/products/my_products/` | Get current user's products | 🔐 Required              |

### 🔍 Product Filtering & Search

The `/api/products/` endpoint supports advanced filtering:

| Parameter   | Description                    | Example                 |
| ----------- | ------------------------------ | ----------------------- |
| `search`    | Search in name and description | `?search=laptop`        |
| `category`  | Filter by category             | `?category=electronics` |
| `is_active` | Filter by active status        | `?is_active=true`       |
| `ordering`  | Sort by fields                 | `?ordering=-created_at` |

**Available Categories**: `books`, `other`, `sports`, `clothing`, `home`, `electronics`

**Sortable Fields**: `name`, `price`, `quantity`, `created_at` (use `-` prefix for descending)

### 🔌 WebSocket Endpoints

| Endpoint        | Description               | Authentication  |
| --------------- | ------------------------- | --------------- |
| `/ws/products/` | Real-time product updates | 🔐 JWT Required |

### 📋 Example API Usage

```javascript
// User Registration
const registerResponse = await fetch("/api/user/register/", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    username: "newuser",
    password: "securepassword",
    email: "user@example.com",
  }),
});

// User Login (Get JWT Token)
const loginResponse = await fetch("/api/token/", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ username: "user", password: "pass" }),
});

const tokens = await loginResponse.json();
// Response: { "access": "...", "refresh": "..." }

// Get Current User Info
const userInfo = await fetch("/api/user/", {
  headers: { Authorization: `Bearer ${tokens.access}` },
});

// Create Product with Image
const productData = {
  name: "Gaming Laptop",
  description: "High-performance gaming laptop with RGB keyboard",
  price: 1299.99,
  quantity: 5,
  category: "electronics",
  is_active: true,
};

const formData = new FormData();
Object.keys(productData).forEach((key) => {
  formData.append(key, productData[key]);
});
// Add image file
formData.append("image", imageFile);

const createResponse = await fetch("/api/products/", {
  method: "POST",
  headers: { Authorization: `Bearer ${tokens.access}` },
  body: formData,
});

// Search Products
const searchResponse = await fetch(
  "/api/products/?search=laptop&category=electronics&ordering=-created_at",
  {
    headers: { Authorization: `Bearer ${tokens.access}` },
  }
);

// Get My Products Only
const myProductsResponse = await fetch("/api/products/my_products/", {
  headers: { Authorization: `Bearer ${tokens.access}` },
});

// Refresh Token
const refreshResponse = await fetch("/api/token/refresh/", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ refresh: tokens.refresh }),
});

// Logout (Blacklist Token)
const logoutResponse = await fetch("/api/token/blacklist/", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${tokens.access}`,
  },
  body: JSON.stringify({ refresh: tokens.refresh }),
});
```

## 🐛 Troubleshooting

### 🔴 Common Issues & Solutions

#### **Backend Issues**

| Problem                      | Solution                                    |
| ---------------------------- | ------------------------------------------- |
| 🚫 Redis connection failed   | Install and start Redis server locally      |
| 🗄️ Database migration errors | Run `python manage.py migrate --run-syncdb` |
| 🔐 JWT authentication fails  | Check SECRET_KEY in .env file               |
| 📁 Static files not loading  | Run `python manage.py collectstatic`        |
| 🌐 CORS errors               | Verify CORS_ALLOWED_ORIGINS in settings     |

#### **Frontend Issues**

| Problem                         | Solution                                                  |
| ------------------------------- | --------------------------------------------------------- |
| 📦 npm install fails            | Delete `node_modules` and `package-lock.json`, then retry |
| ⚡ Vite dev server won't start  | Check if port 5173 is available                           |
| 🔗 API connection refused       | Ensure backend is running on port 8000                    |
| 🎨 Tailwind styles not applying | Run `npm run build` to regenerate CSS                     |

#### **Docker Issues**

| Problem                       | Solution                                             |
| ----------------------------- | ---------------------------------------------------- |
| 🐳 Container build fails      | Check Docker is running and has sufficient resources |
| 🌐 Services can't communicate | Verify Docker network configuration                  |
| 📊 Database connection fails  | Check environment variables in `.env.prod`           |
| 💾 Volume mount errors        | Ensure proper file permissions                       |

### 🔧 Debug Commands

```powershell
# Check backend logs
docker-compose logs lr-backend

# Check database connection
python manage.py dbshell

# Test WebSocket connection
# Open browser console and run:
# const ws = new WebSocket('ws://localhost:8000/ws/products/');

# Check Redis connection
redis-cli ping

# Verify environment variables
docker-compose config
```

## 🚀 Performance Optimization

### ⚡ Backend Optimizations

- **Database Indexing**: Add indexes to frequently queried fields
- **Query Optimization**: Use `select_related()` and `prefetch_related()`
- **Caching**: Implement Redis caching for frequently accessed data
- **Image Optimization**: Compress uploaded images automatically

### 🎨 Frontend Optimizations

- **Code Splitting**: Implement React lazy loading for routes
- **Image Optimization**: Use WebP format and responsive images
- **Bundle Analysis**: Use `npm run build -- --analyze` to check bundle size
- **Service Worker**: Implement PWA features for offline support

## 🔒 Security Best Practices

### 🛡️ Implemented Security Features

- **JWT Authentication** with automatic token refresh
- **CORS Configuration** for cross-origin requests
- **Input Validation** on all API endpoints
- **File Upload Security** with type and size restrictions
- **Environment Variables** for sensitive configuration

### 🔐 Additional Security Recommendations

```powershell
# Generate secure SECRET_KEY
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"

# Set strong database passwords
openssl rand -base64 32

# Enable HTTPS in production
# Update ALLOWED_HOSTS and SECURE_* settings
```

## 📊 Monitoring & Logging

### 📈 Available Endpoints for Monitoring

- **Django Admin**: `/admin/` - Administrative interface
- **API Root**: `/api/` - API endpoint listing
- **DRF Browsable API**: `/api-auth/` - Interactive API browser

### 🔧 Manual Health Checks

```powershell
# Test database connection
cd backend
python manage.py dbshell

# Test Redis connection (if Redis is running)
redis-cli ping

# Check Django application
curl http://localhost:8000/api/

# Test WebSocket connection
# Open browser console and run:
# const ws = new WebSocket('ws://localhost:8000/ws/products/');
```

### 📝 Log Locations

```
📂 Logs/
├── 🐍 Django: /app/logs/django.log
├── 🌐 Nginx: /var/log/nginx/
├── 🐳 Docker: docker-compose logs [service_name]
└── 🔴 Redis: /var/log/redis/redis-server.log
```

## 🤝 Contributing

We welcome contributions! Please follow these steps:

### 🔄 Development Workflow

1. **🍴 Fork the repository**
2. **🌿 Create a feature branch**
   ```powershell
   git checkout -b feature/amazing-feature
   ```
3. **💻 Make your changes**
4. **✅ Run tests**

   ```powershell
   # Backend tests
   cd backend
   python manage.py test

   # Frontend tests (if available)
   cd frontend
   npm test
   ```

5. **📝 Commit your changes**
   ```powershell
   git commit -m "Add: Amazing new feature"
   ```
6. **🚀 Push to your branch**
   ```powershell
   git push origin feature/amazing-feature
   ```
7. **📋 Open a Pull Request**

### 📋 Code Style Guidelines

- **Python**: Follow PEP 8 standards
- **JavaScript**: Use ESLint configuration provided
- **Commit Messages**: Use conventional commit format
  - `feat:` for new features
  - `fix:` for bug fixes
  - `docs:` for documentation changes
  - `style:` for formatting changes
  - `refactor:` for code refactoring

### 🧪 Testing Requirements

- Write unit tests for new features
- Ensure all existing tests pass
- Test WebSocket functionality manually
- Verify Docker deployment works

## 📞 Support & Contact

### 💬 Getting Help

- **📧 Email**: [your-email@domain.com]
- **🐛 Issues**: [GitHub Issues](https://github.com/kevinThulnith/lr-final/issues)
- **💡 Discussions**: [GitHub Discussions](https://github.com/kevinThulnith/lr-final/discussions)

### 📚 Additional Resources

- **Django Documentation**: https://docs.djangoproject.com/
- **React Documentation**: https://react.dev/
- **Docker Documentation**: https://docs.docker.com/
- **Tailwind CSS**: https://tailwindcss.com/docs

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

### 📜 MIT License Summary

- ✅ Commercial use allowed
- ✅ Modification allowed
- ✅ Distribution allowed
- ✅ Private use allowed
- ❌ No warranty provided
- ❌ No liability assumed

## 🙏 Acknowledgments

- **Django Team** for the amazing web framework
- **React Team** for the powerful UI library
- **Tailwind CSS** for the utility-first CSS framework
- **Docker** for containerization technology
- **Redis** for real-time functionality
- **All Contributors** who helped improve this project

## 📈 Project Statistics

![GitHub stars](https://img.shields.io/github/stars/kevinThulnith/lr-final?style=social)
![GitHub forks](https://img.shields.io/github/forks/kevinThulnith/lr-final?style=social)
![GitHub issues](https://img.shields.io/github/issues/kevinThulnith/lr-final)
![GitHub license](https://img.shields.io/github/license/kevinThulnith/lr-final)

---

**🎉 Happy Coding!** If you found this project helpful, please give it a ⭐ star on GitHub!
