# Safeguard Flask Backend - Web/Desktop Synchronization

## 🎯 Overview

This Flask backend provides a unified database system that synchronizes data between:
- **Desktop Application** (accessed via terminal)
- **Web Application** (accessed via browser)

Both interfaces share the **same SQLite database** for seamless synchronization.

## 🚀 Quick Start

### Method 1: Using run.py (Recommended)
```bash
python run.py
```

### Method 2: Direct Flask
```bash
pip install -r requirements.txt
python app.py
```

The application will:
1. ✅ Initialize the SQLite database automatically
2. 🌐 Start Flask server on `http://127.0.0.1:5000`
3. 🌍 Open browser automatically
4. 💾 Both desktop and web share the same database

## 📊 Database Structure

The SQLite database (`safeguard.db`) contains:

- **users** - User accounts and authentication
- **emergency_contacts** - Emergency contact information
- **safe_routes** - Saved safe routes
- **location_history** - Location tracking history
- **sync_status** - Synchronization tracking

## 🔄 How Synchronization Works

1. **Single Database**: Both web and desktop access the same `safeguard.db` file
2. **Real-time Sync**: Changes made in web are immediately available in desktop and vice versa
3. **Sync Tracking**: All operations are logged in `sync_status` table
4. **No Conflicts**: SQLite handles concurrent access safely

## 🌐 Access Methods

### Desktop Access (Terminal)
```bash
python app.py
# or
python run.py
```
- Opens browser automatically
- Shows terminal output
- Server runs on `http://127.0.0.1:5000`

### Web Access
- Open browser and go to: `http://127.0.0.1:5000`
- Or use the link from terminal output
- Same database, same data

## 📡 API Endpoints

All endpoints are prefixed with `/api/`:

### Authentication
- `POST /api/register` - Register new user
- `POST /api/login` - User login
- `POST /api/verify-email` - Verify email
- `POST /api/reset-password` - Reset password

### Data Management
- `GET /api/contacts?user_id=<id>` - Get contacts
- `POST /api/contacts` - Add contact
- `DELETE /api/contacts/<id>` - Delete contact
- `PUT /api/contacts/<id>` - Update contact

- `GET /api/routes?user_id=<id>` - Get routes
- `POST /api/routes` - Add route

- `POST /api/location` - Save location

### System
- `GET /health` - Health check
- `GET /api/sync-status` - Get sync status

## 🔧 Configuration

### Change Port
Edit `app.py`:
```python
app.run(debug=True, host='127.0.0.1', port=5000)  # Change port here
```

### Database Location
The database file `safeguard.db` is created in the same directory as `app.py`.

## 🐛 Troubleshooting

### Port Already in Use
```bash
# Windows: Find and kill process on port 5000
netstat -ano | findstr :5000
taskkill /PID <PID> /F

# Linux/Mac: Find and kill process
lsof -ti:5000 | xargs kill
```

### Database Locked
- Close all connections to the database
- Restart the Flask server
- If issue persists, delete `safeguard.db` (will be recreated)

### Cannot Connect
1. Check Flask server is running
2. Verify port 5000 is accessible
3. Check firewall settings
4. Try `http://127.0.0.1:5000/health` in browser

## 📝 Notes

- ✅ Database is automatically created on first run
- ✅ All passwords are hashed (SHA256)
- ✅ CORS enabled for local development
- ✅ Debug mode enabled by default
- ✅ Automatic browser opening on startup
- ✅ Sync status tracking for all operations

## 🔒 Security

- Passwords are hashed before storage
- SQL injection protection via parameterized queries
- Input validation on all endpoints
- CORS configured for local development only

## 📦 Dependencies

- Flask 3.0.0
- flask-cors 4.0.0
- Python 3.7+

Install with:
```bash
pip install -r requirements.txt
```
