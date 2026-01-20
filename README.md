# 🎯 Blog API - Node.js with SQLite

Complete REST API for Blog Management with Node.js, Next.js, and SQLite Database.

## 🚀 Quick Start

```bash
# 1. Install dependencies
npm install

# 2. Start server
npm run dev

# 3. Initialize database
curl -X POST http://localhost:3000/api/v1/admin/init-db

# 4. Test API
curl http://localhost:3000/api/v1/blogs
```

**Time to setup: 5 minutes ⏱️**

---

## 📚 Documentation

**👉 START HERE:** [`00_START_HERE.md`](./00_START_HERE.md)

| Document | Purpose |
|----------|---------|
| [00_START_HERE.md](./00_START_HERE.md) | **Main entry point - start here!** |
| [QUICKSTART.md](./QUICKSTART.md) | 5-minute quick start guide |
| [SETUP_CHECKLIST.md](./SETUP_CHECKLIST.md) | Step-by-step setup validation |
| [SQL_SETUP_GUIDE.md](./SQL_SETUP_GUIDE.md) | Complete setup & deployment |
| [API_DOCUMENTATION.md](./API_DOCUMENTATION.md) | Full API reference |
| [DATABASE_REFERENCE.md](./DATABASE_REFERENCE.md) | SQL queries & database info |
| [MIGRATION_SUMMARY.md](./MIGRATION_SUMMARY.md) | MongoDB → SQLite migration |
| [COMPLETION_REPORT.md](./COMPLETION_REPORT.md) | What was delivered |
| [FINAL_SUMMARY_VI.md](./FINAL_SUMMARY_VI.md) | Summary in Vietnamese |
| [PROJECT_OVERVIEW.txt](./PROJECT_OVERVIEW.txt) | Visual project overview |

---

## ✨ Features

### 🔐 Authentication
- JWT-based authentication
- Role-based authorization (admin/user)
- Secure password hashing with bcrypt
- 7-day token expiration

### 📝 Blog Management
- Create, Read, Update, Delete blogs
- Draft and published status
- Auto-generated URL slugs
- View count tracking
- Tag system for organization

### 🔍 Search & Filter
- Full-text search
- Filter by status, author, tags
- Sort by any field
- Pagination with configurable limits

### 📤 File Management
- Image upload support
- File type validation
- Size limit enforcement
- Secure file storage

### 🗄️ Database
- SQLite (file-based, no server needed)
- 3 normalized tables (users, blogs, blog_tags)
- Foreign key constraints
- Performance indexes
- Sample data included

---

## 🎯 API Endpoints

### Public Endpoints (14 total)

**Authentication**
```
POST   /api/v1/auth/login      - Login with credentials
POST   /api/v1/auth/logout     - Logout (requires token)
GET    /api/v1/auth/me         - Get current user (requires token)
```

**Blogs - Read**
```
GET    /api/v1/blogs           - List blogs (with search, filter, sort, pagination)
GET    /api/v1/blogs/{id}      - Get single blog by ID
```

**Blogs - Write**
```
POST   /api/v1/blogs           - Create new blog (requires token)
PUT    /api/v1/blogs/{id}      - Update blog (requires token)
DELETE /api/v1/blogs/{id}      - Delete blog (requires token)
```

**Media**
```
POST   /api/v1/blogs/upload    - Upload file (requires token)
```

**Admin**
```
POST   /api/v1/admin/init-db   - Initialize database
```

---

## 🔐 Default Credentials

```
Admin Account:
  Username: admin
  Password: admin123

Regular User:
  Username: user
  Password: user123
```

---

## 🗄️ Database Schema

**Users Table**
```sql
id          TEXT PRIMARY KEY
username    TEXT UNIQUE NOT NULL
email       TEXT UNIQUE NOT NULL
password    TEXT NOT NULL (bcrypt hashed)
role        TEXT ('admin' or 'user')
created_at  DATETIME
updated_at  DATETIME
```

**Blogs Table**
```sql
id          TEXT PRIMARY KEY
title       TEXT NOT NULL
slug        TEXT UNIQUE NOT NULL (auto-generated)
content     TEXT NOT NULL
excerpt     TEXT
thumbnail   TEXT (URL)
status      TEXT ('draft' or 'published')
author_id   TEXT FOREIGN KEY
view_count  INTEGER
created_at  DATETIME
updated_at  DATETIME
```

**Blog Tags Table**
```sql
id          TEXT PRIMARY KEY
blog_id     TEXT FOREIGN KEY
tag         TEXT NOT NULL
UNIQUE (blog_id, tag)
```

---

## 📁 Project Structure

```
blog-api/
├── app/api/v1/
│   ├── auth/              # Authentication endpoints
│   ├── blogs/             # Blog CRUD operations
│   └── admin/             # Admin endpoints
├── lib/
│   ├── auth/              # JWT & middleware
│   ├── data/              # Data layer (users, blogs)
│   ├── db/                # SQLite connection & utils
│   ├── helpers/           # Response, validation, pagination
│   ├── types/             # TypeScript interfaces
│   └── config/            # Configuration
├── scripts/
│   └── init-database.sql  # Database schema & seed data
├── data/
│   └── blog.db            # SQLite database file
├── .env.example           # Environment variables template
└── 📖 Documentation files
```

---

## 🛠️ Tech Stack

- **Runtime:** Node.js 18+
- **Framework:** Next.js 16
- **Database:** SQLite 3 (better-sqlite3)
- **Authentication:** JWT (jsonwebtoken)
- **Security:** bcryptjs
- **IDs:** UUID v4
- **Language:** TypeScript

---

## 🚀 Deployment

### Environment Variables

Create `.env.local`:
```env
DATABASE_URL=data/blog.db
JWT_SECRET=your-secret-key-here-change-in-production
NODE_ENV=development
```

### Build & Run

```bash
# Development
npm run dev

# Production build
npm run build
npm start
```

### Database Setup

1. Initialize database:
```bash
curl -X POST http://localhost:3000/api/v1/admin/init-db
```

2. Or manually:
```bash
sqlite3 data/blog.db < scripts/init-database.sql
```

---

## 📊 Example Workflow

```bash
# 1. Initialize database
curl -X POST http://localhost:3000/api/v1/admin/init-db

# 2. Login
TOKEN=$(curl -s -X POST http://localhost:3000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin123"}' | jq -r '.data.accessToken')

# 3. Get all blogs
curl http://localhost:3000/api/v1/blogs

# 4. Search blogs
curl "http://localhost:3000/api/v1/blogs?q=nodejs&tags=tutorial&page=1"

# 5. Create blog
curl -X POST http://localhost:3000/api/v1/blogs \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "My First Blog",
    "content": "This is amazing!",
    "tags": ["nodejs", "api"],
    "status": "published"
  }'

# 6. Update blog
curl -X PUT http://localhost:3000/api/v1/blogs/{id} \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"status": "draft"}'

# 7. Delete blog
curl -X DELETE http://localhost:3000/api/v1/blogs/{id} \
  -H "Authorization: Bearer $TOKEN"
```

---

## 🔒 Security Features

✅ JWT authentication with 7-day expiration
✅ Bcrypt password hashing (10 rounds)
✅ Role-based access control
✅ Parameterized SQL queries (SQL injection prevention)
✅ Foreign key constraints
✅ CORS headers
✅ Input validation & sanitization

---

## 📈 Performance

- ✅ No network latency (local SQLite)
- ✅ Fast query execution
- ✅ Optimized indexes
- ✅ Efficient pagination
- ✅ Minimal dependencies

---

## 🧪 Testing

### Test All Endpoints

See [SETUP_CHECKLIST.md](./SETUP_CHECKLIST.md) for complete testing checklist.

### Manual Testing

```bash
# List blogs with filtering
curl "http://localhost:3000/api/v1/blogs?page=1&limit=5&status=published&sortBy=created_at&sortOrder=desc"

# Search
curl "http://localhost:3000/api/v1/blogs?q=nodejs&tags=tutorial,api"

# Get single blog
curl http://localhost:3000/api/v1/blogs/1

# Unauthorized access (should fail)
curl -X POST http://localhost:3000/api/v1/blogs
```

---

## 🐛 Troubleshooting

### Database file not found
```bash
npm run dev
curl -X POST http://localhost:3000/api/v1/admin/init-db
```

### Connection error
```bash
# Check if database file exists
ls -la data/blog.db

# Check file permissions
chmod 644 data/blog.db
```

### Token expired
Login again with valid credentials.

See [SQL_SETUP_GUIDE.md](./SQL_SETUP_GUIDE.md) for more troubleshooting.

---

## 📝 API Response Format

All responses follow standard format:

**Success:**
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Success message",
  "data": { /* response data */ },
  "meta": { /* pagination info */ },
  "timestamp": "2024-01-20T10:00:00.000Z"
}
```

**Error:**
```json
{
  "success": false,
  "statusCode": 400,
  "message": "Error message",
  "errors": [{ "field": "username", "message": "Required" }],
  "timestamp": "2024-01-20T10:00:00.000Z"
}
```

---

## 🎓 Learning Resources

- [Node.js Documentation](https://nodejs.org/docs/)
- [Next.js Documentation](https://nextjs.org/docs)
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [JWT Guide](https://jwt.io/introduction)
- [REST API Best Practices](https://restfulapi.net/)

---

## 📄 License

This project is open source and available under the MIT License.

---

## 🤝 Contributing

Feel free to fork, modify, and use this project for your own purposes.

---

## 📞 Support

For issues or questions:
1. Check the [documentation files](./00_START_HERE.md)
2. Review [SETUP_CHECKLIST.md](./SETUP_CHECKLIST.md)
3. See [SQL_SETUP_GUIDE.md](./SQL_SETUP_GUIDE.md) troubleshooting

---

## 🎉 Getting Started

**👉 [Read 00_START_HERE.md](./00_START_HERE.md) - Start here!**

Then:
```bash
npm install
npm run dev
curl -X POST http://localhost:3000/api/v1/admin/init-db
```

Happy coding! 🚀

---

**Version:** 1.0  
**Status:** ✅ Production Ready  
**Database:** SQLite  
**Last Updated:** 2024-01-20
