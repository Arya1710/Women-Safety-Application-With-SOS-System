# Web/Desktop Synchronization Guide

## 🎯 How It Works

The Safeguard application uses a **single SQLite database** that is shared between:
- **Desktop Access** (via terminal: `python app.py`)
- **Web Access** (via browser: `http://127.0.0.1:5000`)

Both interfaces read from and write to the **same database file** (`safeguard.db`), ensuring real-time synchronization.

## 🚀 Workflow

### Step 1: Start the Application (Terminal)
```bash
python app.py
```

**What happens:**
1. ✅ Flask server starts on `http://127.0.0.1:5000`
2. ✅ SQLite database is initialized/connected
3. 🌐 Browser opens automatically
4. 💻 Terminal shows server status

### Step 2: Access via Desktop (Terminal)
- The browser that opens is your **desktop interface**
- All data is saved to `safeguard.db`
- You can see terminal logs for all operations

### Step 3: Access via Web (Any Browser)
- Open any browser
- Go to: `http://127.0.0.1:5000`
- **Same database, same data**
- Changes made here appear in desktop and vice versa

## 🔄 Synchronization Examples

### Example 1: Register User
1. **Desktop**: Register a new user via terminal-opened browser
2. **Web**: Open `http://127.0.0.1:5000` in another browser
3. **Result**: User can login from web browser immediately ✅

### Example 2: Add Contact
1. **Web**: Add emergency contact via web browser
2. **Desktop**: Refresh terminal-opened browser
3. **Result**: Contact appears in desktop interface ✅

### Example 3: Location Tracking
1. **Desktop**: Save location via desktop interface
2. **Web**: Check location history via web browser
3. **Result**: Location data is synchronized ✅

## 📊 Database Structure

All data is stored in `safeguard.db`:

```
users
├── id, username, name, email, password, security_question, security_answer
└── created_at, updated_at

emergency_contacts
├── id, user_id, name, phone, relationship
└── created_at

safe_routes
├── id, user_id, name, address, latitude, longitude
└── created_at

location_history
├── id, user_id, latitude, longitude, accuracy, speed
└── timestamp

sync_status
├── id, last_sync, sync_type, device_type
└── (tracks all synchronization events)
```

## 🔧 Technical Details

### How Synchronization Works

1. **Single Database File**: `safeguard.db` in project root
2. **SQLite Concurrency**: SQLite handles multiple connections safely
3. **Real-time Updates**: Changes are immediately written to database
4. **No Conflicts**: SQLite's locking mechanism prevents data corruption

### API Endpoints

All operations go through Flask API:
- `/api/register` - User registration
- `/api/login` - User authentication
- `/api/contacts` - Contact management
- `/api/routes` - Route management
- `/api/location` - Location tracking
- `/api/sync-status` - Check sync status

### Frontend Integration

The `pg1.html` file:
- Detects if running via Flask server or direct file access
- Automatically connects to `http://127.0.0.1:5000/api`
- All operations sync to SQLite database

## ✅ Benefits

1. **Unified Data**: One database for all access methods
2. **Real-time Sync**: Changes appear immediately
3. **No Manual Sync**: Automatic synchronization
4. **Backup Friendly**: Single file to backup
5. **Cross-Platform**: Works on Windows, Mac, Linux

## 🐛 Troubleshooting

### Database Locked Error
- **Cause**: Multiple processes accessing database
- **Solution**: Close all browser tabs, restart Flask server

### Changes Not Appearing
- **Cause**: Browser cache or connection issue
- **Solution**: Refresh browser (F5 or Ctrl+R)

### Cannot Connect
- **Cause**: Flask server not running
- **Solution**: Run `python app.py` in terminal

### Port Already in Use
- **Cause**: Another Flask instance running
- **Solution**: 
  ```bash
  # Windows
  netstat -ano | findstr :5000
  taskkill /PID <PID> /F
  
  # Linux/Mac
  lsof -ti:5000 | xargs kill
  ```

## 📝 Notes

- ✅ Database is created automatically on first run
- ✅ All data persists between sessions
- ✅ No internet required (local database)
- ✅ Works offline after initial setup
- ✅ Sync status is logged for debugging

## 🎓 Usage Tips

1. **Always start Flask server first**: `python app.py`
2. **Use same port**: Default is 5000, change in `app.py` if needed
3. **Check terminal logs**: See all database operations
4. **Backup database**: Copy `safeguard.db` to backup location
5. **Monitor sync**: Check `/api/sync-status` endpoint

## 🔒 Security

- Passwords are hashed (SHA256)
- SQL injection protection
- Input validation on all endpoints
- Local database (not exposed to internet)
- CORS enabled for localhost only
