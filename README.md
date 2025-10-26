[README.md](https://github.com/user-attachments/files/23150506/README.md)
# Elderbloom Support Website

A modern, responsive website for Elderbloom Support services with a complete backend API system.

## Features

### Frontend
- Modern, responsive design with mobile-first approach
- Interactive contact and support request forms
- Service information and pricing
- Admin dashboard for managing requests
- Real-time form validation
- Smooth animations and transitions

### Backend
- RESTful API with Express.js
- MongoDB database with Mongoose ODM
- JWT authentication for admin access
- Email notifications using Nodemailer
- Rate limiting and security middleware
- Comprehensive error handling
- Admin panel for managing contacts and support requests

## Project Structure

```
web/
├── frontend/
│   ├── index.html          # Main HTML file
│   ├── styles.css          # CSS styles
│   └── script.js           # JavaScript functionality
├── backend/
│   ├── config/
│   │   └── database.js     # MongoDB connection
│   ├── models/
│   │   ├── Contact.js      # Contact message model
│   │   ├── SupportRequest.js # Support request model
│   │   └── Admin.js        # Admin user model
│   ├── routes/
│   │   ├── contact.js      # Contact API routes
│   │   ├── support.js      # Support request API routes
│   │   ├── services.js     # Services API routes
│   │   └── admin.js        # Admin API routes
│   ├── utils/
│   │   └── email.js        # Email utility functions
│   ├── server.js           # Main server file
│   ├── package.json        # Backend dependencies
│   └── env.example         # Environment variables example
└── README.md               # This file
```

## Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- MongoDB (local or cloud)
- Git

### Local Development

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd web
   ```

2. **Install backend dependencies**
   ```bash
   cd backend
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp env.example .env
   # Edit .env with your configuration
   ```

4. **Start MongoDB**
   - Local: Start your local MongoDB service
   - Cloud: Use MongoDB Atlas or similar service

5. **Run the backend server**
   ```bash
   npm run dev
   # or
   npm start
   ```

6. **Open the frontend**
   - Open `frontend/index.html` in your browser
   - Or serve it with a local server

### Production Deployment on Hostinger VPS

#### 1. VPS Setup
- Choose Hostinger VPS with Ubuntu 22.04 + Node.js template
- Ensure Node.js 16+ and MongoDB are installed

#### 2. Upload Files
```bash
# Upload your project files to the VPS
scp -r . user@your-vps-ip:/var/www/elderbloom-support
```

#### 3. Install Dependencies
```bash
cd /var/www/elderbloom-support/backend
npm install --production
```

#### 4. Configure Environment
```bash
cd /var/www/elderbloom-support/backend
cp env.example .env
nano .env
# Configure your production environment variables
```

#### 5. Set up MongoDB
```bash
# Install MongoDB (if not already installed)
sudo apt update
sudo apt install mongodb

# Start MongoDB
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

#### 6. Install PM2 for Process Management
```bash
npm install -g pm2
```

#### 7. Start the Application
```bash
cd /var/www/elderbloom-support/backend
pm2 start server.js --name "elderbloom-api"
pm2 save
pm2 startup
```

#### 8. Configure Nginx (if using)
```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        root /var/www/elderbloom-support/frontend;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    location /api {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }
}
```

#### 9. Set up SSL Certificate
```bash
# Using Let's Encrypt
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

## Environment Variables

Create a `.env` file in the backend directory with the following variables:

```env
NODE_ENV=production
PORT=5000
FRONTEND_URL=https://your-domain.com

# Database
MONGODB_URI=mongodb://localhost:27017/elderbloom_support

# JWT
JWT_SECRET=your-super-secret-jwt-key

# Email
EMAIL_SERVICE=gmail
EMAIL_USER=your-email@gmail.com
EMAIL_PASS=your-app-password
EMAIL_FROM=noreply@elderbloom.com
ADMIN_EMAIL=admin@elderbloom.com

# Security
BCRYPT_ROUNDS=12
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

## API Endpoints

### Public Endpoints
- `POST /api/contact` - Submit contact form
- `POST /api/support` - Submit support request
- `GET /api/services` - Get all services
- `GET /api/services/:id` - Get specific service

### Admin Endpoints (Require Authentication)
- `POST /api/admin/login` - Admin login
- `GET /api/admin/dashboard` - Dashboard statistics
- `GET /api/admin/contacts` - Get all contacts
- `GET /api/admin/support-requests` - Get all support requests
- `PUT /api/admin/contacts/:id` - Update contact
- `PUT /api/admin/support-requests/:id` - Update support request

## Default Admin Account

After first deployment, a default admin account is created:
- Email: admin@elderbloom.com
- Password: admin123

**Important**: Change the default password immediately after deployment!

## Features Overview

### Contact Management
- Contact form with validation
- Email notifications
- Admin panel for managing contacts
- Status tracking (new, read, replied, closed)

### Support Request System
- Support request form with service selection
- Urgency levels (low, medium, high, emergency)
- Email confirmations
- Admin assignment and tracking
- Notes and progress tracking

### Admin Dashboard
- Real-time statistics
- Contact and support request management
- User management
- Email notifications

## Security Features

- JWT authentication
- Password hashing with bcrypt
- Rate limiting
- Input validation and sanitization
- CORS protection
- Helmet.js security headers
- SQL injection prevention (MongoDB)

## Monitoring & Maintenance

### PM2 Commands
```bash
pm2 status              # Check application status
pm2 logs elderbloom-api # View logs
pm2 restart elderbloom-api # Restart application
pm2 stop elderbloom-api    # Stop application
```

### Database Backup
```bash
# Create backup
mongodump --db elderbloom_support --out /backup/$(date +%Y%m%d)

# Restore backup
mongorestore --db elderbloom_support /backup/20240101/elderbloom_support
```

## Support

For technical support or questions about this project, please contact the development team.

## License

This project is licensed under the MIT License.
