# Production Deployment Guide

## Prerequisites
- Node.js v18+ installed
- MySQL 5.7+ or MariaDB installed
- npm or yarn package manager

## Backend Setup

### 1. Environment Configuration
Create a `server/.env` file with production values:

```env
PORT=4000
NODE_ENV=production
DB_HOST=your-namecheap-host.com
DB_PORT=3306
DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_DATABASE=your_database_name
JWT_SECRET=generate-a-strong-random-string-here
CORS_ORIGIN=https://your-domain.com,https://www.your-domain.com
```

### 2. Install Dependencies
```bash
cd server
npm install
```

### 3. Start Backend
```bash
npm start
```

The server will automatically create required database tables on startup.

## Frontend Setup

### 1. Environment Configuration
Create a `client/.env` file:

```env
VITE_API_BASE=https://api.your-domain.com/api
```

### 2. Install Dependencies
```bash
cd client
npm install
```

### 3. Build for Production
```bash
npm run build
```

This creates an optimized production build in the `dist/` folder.

### 4. Deploy Frontend
Deploy the `client/dist` folder to your hosting service (Vercel, Netlify, cPanel static hosting, etc.)

## Database Setup (Namecheap)

### Via phpMyAdmin
1. Log into your Namecheap hosting panel
2. Go to MySQL Databases
3. Create a new database
4. Note the hostname, username, password, and database name
5. Update `server/.env` with these values

### Initial Admin Setup
On first run, the backend will be ready to create the initial admin account. Use the `/api/auth/setup` endpoint or the login form will prompt you to set up if no admins exist.

## Production Best Practices

### Security
- ✅ HTTPS enabled (update CORS_ORIGIN to https://)
- ✅ Strong JWT_SECRET (minimum 32 characters)
- ✅ Database credentials stored securely in `.env`
- ✅ Rate limiting enabled for login and API endpoints
- ✅ Input validation on all endpoints
- ✅ CORS configured for your domain only

### Performance
- ✅ Database indexes on ticket search fields
- ✅ Connection pooling configured
- ✅ Frontend assets minified and gzipped
- ✅ Dark mode preference saved locally
- ✅ API response caching on frontend

### Logging
- All API requests logged with timestamp and method
- Errors logged with full stack traces in development
- Production errors logged without sensitive details

### Monitoring
Recommended additions for production:
- Error tracking (Sentry)
- Performance monitoring (New Relic, DataDog)
- Uptime monitoring (UptimeRobot)
- Log aggregation (LogRocket)

## Scaling

### Horizontal Scaling
- Deploy multiple server instances behind a load balancer
- Use environment variables for configuration
- Database connection pool automatically manages connections

### Database Optimization
- Indexes are created automatically on startup
- Consider archiving old tickets monthly
- Regular backups of MySQL database

## Troubleshooting

### Database Connection Errors
```
ECONNREFUSED - Check DB_HOST, DB_PORT, DB_USER, DB_PASSWORD in .env
```

### CORS Errors
- Update CORS_ORIGIN in `.env` to match your frontend domain
- Ensure https:// protocol if using HTTPS

### PDF Generation Errors
- Requires network access to fetch airline logos
- Gracefully falls back to airline name if logo unavailable

## Monitoring Health

Check API health endpoint:
```bash
curl https://api.your-domain.com/api/health
```

Response:
```json
{ "status": "ok", "timestamp": "2026-05-11T10:30:00.000Z" }
```

## Backup & Recovery

### Database Backups
```bash
mysqldump -h DB_HOST -u DB_USER -p DB_DATABASE > backup.sql
```

### Restore from Backup
```bash
mysql -h DB_HOST -u DB_USER -p DB_DATABASE < backup.sql
```

## Support

For issues or questions:
1. Check server logs: `npm start` output
2. Check browser console: F12 Developer Tools
3. Enable verbose logging in production for debugging
