# FarmX Backend Authentication System

## Setup Instructions

### Prerequisites
- Node.js (v16 or higher)
- MongoDB installed locally
- npm or yarn

### Installation

1. **Navigate to backend directory:**
   ```bash
   cd backend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start MongoDB locally:**
   ```bash
   # On Windows
   mongod --dbpath C:\data\db

   # On macOS/Linux
   mongod --dbpath /usr/local/var/mongodb
   ```

4. **Start the server:**
   ```bash
   # Development mode with auto-restart
   npm run dev

   # Or production mode
   npm start
   ```

The server will start on `http://localhost:5000`

### API Endpoints

#### Authentication Routes

**POST /signup**
- Register a new user
- Body: `{ name, email, password, role }`
- Returns: `{ message, token, user }`

**POST /login**
- Authenticate existing user
- Body: `{ email, password }`
- Returns: `{ message, token, user }`

**GET /verify-token**
- Verify JWT token validity
- Headers: `Authorization: Bearer <token>`
- Returns: `{ valid: true, user }`

**GET /dashboard**
- Protected route for dashboard data
- Headers: `Authorization: Bearer <token>`
- Returns: `{ message, user }`

**GET /health**
- Server health check
- Returns: `{ status, timestamp }`

### Database Schema

**User Model:**
```javascript
{
  name: String (required, 2-50 chars),
  email: String (required, unique, valid email),
  password: String (required, min 6 chars, hashed),
  role: String (enum: Farmer, Consumer, Retailer, Wholesaler),
  isActive: Boolean (default: true),
  lastLogin: Date,
  createdAt: Date (auto),
  updatedAt: Date (auto)
}
```

### Security Features

- **Password Hashing:** bcryptjs with 10 salt rounds
- **JWT Authentication:** 24-hour token expiration
- **Input Validation:** Server-side validation for all inputs
- **CORS Protection:** Configured for frontend origin
- **Error Handling:** Comprehensive error responses

### Environment Variables

Create a `.env` file in the backend directory:

```env
MONGODB_URI=mongodb://127.0.0.1:27017/authApp
JWT_SECRET=your_super_secure_jwt_secret_key
PORT=5000
NODE_ENV=development
FRONTEND_URL=http://localhost:5173
```

### Testing the API

You can test the API endpoints using curl or Postman:

```bash
# Health check
curl http://localhost:5000/health

# Register user
curl -X POST http://localhost:5000/signup \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe","email":"john@example.com","password":"password123","role":"Farmer"}'

# Login user
curl -X POST http://localhost:5000/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123"}'
```

### Troubleshooting

**MongoDB Connection Issues:**
- Ensure MongoDB is running locally
- Check if the database path exists
- Verify port 27017 is not blocked

**CORS Errors:**
- Ensure frontend is running on http://localhost:5173
- Check CORS configuration in server.js

**JWT Errors:**
- Verify JWT_SECRET is set
- Check token format in Authorization header