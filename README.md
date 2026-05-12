# Flight Booking Admin Panel

A full-stack web application for managing flight ticket bookings with PDF generation. Built with React + Vite (frontend) and Node.js + Express (backend) with MySQL database.

## Features

✨ **Core Features**
- 🔐 Secure admin authentication with JWT tokens
- 📋 Flight ticket data entry with passenger and itinerary details
- 📄 Automatic PDF generation matching exact PDF format
- 🔍 Searchable ticket history with filtering
- 📥 PDF preview and download functionality
- 👥 Create and manage multiple admin accounts
- 🌙 Dark mode toggle with persistent preference
- ✈️ Dynamic airline logo support via airline codes

⚡ **Production-Ready**
- Input validation on frontend and backend
- Rate limiting on login and API endpoints
- Security headers (Helmet.js)
- CORS protection
- Error handling with user-friendly messages
- Toast notifications for user feedback
- Database connection pooling
- Request logging and error tracking
- Optimized database queries with indexes

## Tech Stack

**Frontend**
- React 18
- Vite (build tool)
- Vanilla CSS (no dependencies)
- Context API for state management

**Backend**
- Node.js + Express
- JWT authentication
- MySQL database
- PDFKit for PDF generation
- Rate limiting & Helmet security
- Comprehensive input validation

## Quick Start

### Prerequisites
- Node.js v18+ and npm
- MySQL 5.7+ (or use Namecheap MySQL)

### Setup

1. **Clone repository**
   ```bash
   cd flight-record
   ```

2. **Install backend dependencies**
   ```bash
   cd server
   npm install
   cp .env.example .env
   ```

3. **Configure database** (update `server/.env`):
   ```env
   DB_HOST=localhost
   DB_USER=root
   DB_PASSWORD=yourpassword
   DB_DATABASE=flight_records
   JWT_SECRET=your-secret-key-here
   ```

4. **Start backend**
   ```bash
   npm start
   ```
   Server runs on `http://localhost:4000`

5. **Install frontend dependencies** (new terminal)
   ```bash
   cd client
   npm install
   ```

6. **Start frontend dev server**
   ```bash
   npm run dev
   ```
   Open `http://localhost:5173`

## Project Structure

```
flight-record/
├── server/                      # Node.js + Express backend
│   ├── middleware/              # Auth, rate limiting
│   ├── routes/                  # API endpoints
│   ├── utils/                   # Validation, logging, error handling
│   ├── db.js                    # MySQL connection pool
│   ├── pdfGenerator.js          # PDF creation
│   └── index.js                 # Server entry point
├── client/                      # React + Vite frontend
│   ├── src/
│   │   ├── components/          # React components
│   │   ├── context/             # Theme, Toast context
│   │   ├── utils/               # API, validation
│   │   ├── App.jsx              # Root component
│   │   └── App.css              # Tailored CSS
│   ├── index.html               # HTML template
│   └── vite.config.js           # Vite configuration
├── API.md                       # API documentation
├── DEPLOYMENT.md                # Production deployment guide
└── README.md                    # This file
```

## API Documentation

See [API.md](./API.md) for complete API reference with examples.

### Key Endpoints
- `POST /api/auth/login` - Admin sign in
- `GET /api/tickets` - List all tickets
- `POST /api/tickets` - Create new ticket
- `GET /api/tickets/:id/pdf` - Download ticket PDF
- `POST /api/admins` - Create new admin account

## Form Fields

### Ticket Information
- **Airline**: Name and 2-letter code
- **PNR & Booking ID**: Unique identifiers
- **Issue Date/Time**: When ticket was issued

### Passenger Details
- **Name**: Full passenger name
- **Type**: ADULT, CHILD, or INFANT
- **Passport**: Passport number
- **Baggage**: Check-in and cabin allowances

### Flight Segments (Multi-leg support)
- **Route**: Origin and destination airport codes
- **Dates & Times**: Departure and arrival
- **Flight Details**: Number, aircraft type
- **Airports**: Full airport names and terminals
- **Duration**: Flight duration

## PDF Output

Generated PDFs match the provided sample format exactly:
- Header with airline PNR and booking ID
- Passenger information section
- Multi-segment flight itinerary
- Important reminders and information
- Professional styling and formatting

## Features in Detail

### Dark Mode
- Toggle in navbar (Light mode / Dark mode button)
- Preference saved to browser localStorage
- All colors automatically adjust

### Ticket History
- Search by PNR, booking ID, passenger name, or airline
- Real-time filtering with database query
- Preview and download PDFs
- Latest tickets shown first (reverse chronological)

### Admin Management
- Create new admin accounts from dashboard
- Email-based authentication
- Secure password hashing with bcrypt
- JWT tokens with 8-hour expiration

### Airline Logos
- Automatically fetched from DaisynCon API
- Falls back to airline name if unavailable
- Dynamically loads based on airline code

## Authentication & Security

- Passwords hashed with bcrypt (salted, 10 rounds)
- JWT tokens for session management
- CORS configured for trusted domains only
- Rate limiting on login (5 attempts per 15 min)
- Input validation on all fields
- SQL injection prevention via parameterized queries
- XSS protection with helmet security headers

## Deployment

For production deployment instructions, see [DEPLOYMENT.md](./DEPLOYMENT.md).

Quick production steps:
1. Set up MySQL database on Namecheap or hosting provider
2. Update `server/.env` with production values
3. Run `cd server && npm install && npm start`
4. Run `cd client && npm install && npm run build`
5. Deploy `client/dist` to static hosting
6. Point domain to backend and frontend servers

## Performance

- Optimized database queries with indexes
- Connection pooling for MySQL
- Frontend assets minified with Vite
- Dark mode preference cached locally
- Lazy token refresh on 401 response
- Rate limiting prevents abuse

## Browser Support

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile browsers (iOS Safari 14+, Chrome Android)

## Troubleshooting

### Database Connection Error
```
ECONNREFUSED: Update DB_HOST, DB_PORT, DB_USER, DB_PASSWORD in server/.env
```

### CORS Error
- Update `CORS_ORIGIN` in `server/.env` to match frontend domain
- Ensure frontend and backend are properly configured

### PDF Download Not Working
- Check browser console for errors
- Ensure backend is running and accessible
- Verify JWT token is valid (check localStorage)

### Login Issues
- Clear localStorage: Open DevTools → Application → Clear Storage
- Check server logs for validation errors
- Verify email and password format

## Support

For issues or questions:
1. Check [API.md](./API.md) and [DEPLOYMENT.md](./DEPLOYMENT.md)
2. Review server logs in terminal
3. Open browser DevTools (F12) for frontend errors
4. Check database connectivity with `mysql -h host -u user -p`

## License

This project is proprietary and confidential.

## Version

- Frontend: 0.0.1
- Backend: 0.0.1
- Last Updated: May 2026
