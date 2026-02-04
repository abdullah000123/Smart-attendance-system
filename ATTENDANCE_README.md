# 🎓 Smart Attendance System with Face Recognition

A comprehensive web-based attendance management system using facial recognition technology. Built with Flask, dlib, and modern web technologies to automate attendance tracking for educational institutions.

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![Flask](https://img.shields.io/badge/flask-3.0.0-green)
![dlib](https://img.shields.io/badge/dlib-face%20recognition-orange)
![License](https://img.shields.io/badge/license-MIT-purple)

## ✨ Key Features

### 👨‍💼 Admin Dashboard
- **Student Registration**: Register students with face encoding
- **Subject Management**: Create and schedule subjects with specific time slots
- **Location-based Attendance**: Configure WiFi and GPS-based location verification
- **Attendance Records**: View comprehensive attendance reports
- **Student Management**: Edit, update, or remove student records
- **Real-time Statistics**: Monitor attendance trends and metrics

### 👨‍🎓 Student Portal
- **Self-Service Attendance**: Mark attendance using facial recognition
- **Time-Aware System**: Attendance only allowed during class hours
- **Personal Dashboard**: View individual attendance history
- **Attendance Statistics**: Track attendance rate and monthly records
- **Automated Verification**: Face matching against registered profile

### 🔐 Security & Authentication
- **Dual Login System**: Separate portals for admin and students
- **Password Protection**: Secure password hashing with werkzeug
- **Session Management**: Flask session-based authentication
- **Password Reset**: Token-based password recovery system
- **Face Verification**: dlib-powered facial recognition

### 📅 Schedule Management
- **Day-wise Scheduling**: Assign subjects to specific days
- **Time Slots**: Define class start and end times
- **Attendance Windows**: Configurable grace periods after class
- **Automatic Validation**: System prevents attendance outside allowed times

### 📊 Advanced Features
- **SQLite Database**: Lightweight, file-based database
- **Pakistan Time Zone**: Built-in timezone support (Asia/Karachi)
- **Attendance Analytics**: Monthly and daily statistics
- **Subject-wise Filtering**: Filter attendance by subject
- **Duplicate Prevention**: One attendance per subject per day

## 🚀 Quick Start

### Prerequisites

```bash
# Required software
- Python 3.8 or higher
- pip (Python package manager)
- Webcam (for face capture)
- Modern web browser with camera access
```

### Installation

1. **Clone or download the project**
   ```bash
   cd /path/to/attendance-system
   ```

2. **Install Python dependencies**
   ```bash
   pip install flask werkzeug opencv-python dlib numpy pillow pytz
   ```

3. **Download dlib models** (Required for face recognition)
   
   Download these two files and place them in the project root:
   - `shape_predictor_68_face_landmarks.dat`
   - `dlib_face_recognition_resnet_model_v1.dat`
   
   Download from: http://dlib.net/files/

4. **Initialize the database**
   ```bash
   python app.py
   ```
   The database and tables will be created automatically on first run.

5. **Access the application**
   - Open browser: `http://localhost:5001`
   - Default admin login:
     - Username: `admin`
     - Password: `admin123`

## 📖 Usage Guide

### Admin Workflow

#### 1. Register Students

1. Login as admin
2. Navigate to "Register Student" tab
3. Fill in student details:
   - Student Name
   - Roll Number (must be unique)
   - Default Password
4. Click "Start Camera"
5. Position student's face clearly in frame
6. Click "Capture Photo"
7. Click "Register Student"

**Face Capture Tips:**
- Good lighting is essential
- Face should be front-facing
- Remove glasses if possible
- Maintain neutral expression
- Avoid shadows on face

#### 2. Add Subjects

1. Go to "Manage Subjects" tab
2. Fill in subject details:
   - Subject Name (e.g., "Mathematics")
   - Subject Code (e.g., "MATH101")
   - Day of Week
   - Start Time
   - End Time
   - Attendance Window (minutes after class ends)
3. Click "Add Subject"

**Example:**
```
Subject: Database Systems
Code: CS301
Day: Monday
Start Time: 09:00
End Time: 10:30
Window: 15 minutes
```
Students can mark attendance from 09:00 to 10:45 (class end + 15 min window)

#### 3. Configure Locations (Optional)

1. Navigate to "Locations" tab
2. Add location details:
   - Location Name (e.g., "Main Campus")
   - WiFi SSID (optional)
   - IP Range (optional)
   - GPS Coordinates (optional)
   - Radius in meters
3. Click "Add Location"

#### 4. View Attendance Records

1. Go to "View Records" tab
2. Select date using date picker
3. Filter by subject (optional)
4. View attendance statistics:
   - Total students
   - Present today
   - Total subjects
   - Monthly attendance count

### Student Workflow

#### 1. Login

1. Navigate to `http://localhost:5001`
2. Click "Student" button
3. Enter Roll Number
4. Enter Password
5. Click "Login"

#### 2. Mark Attendance

1. Dashboard shows current class (if any)
2. Click "Start Camera"
3. Position your face clearly in frame
4. Click "Mark Attendance"
5. System verifies face and marks attendance

**Important:**
- Attendance only works during class hours
- Face must match registered profile
- Only one attendance per subject per day
- Attendance window includes grace period after class

#### 3. View Attendance History

1. Click "My Attendance" tab
2. View statistics:
   - Total days present
   - This month's attendance
   - Attendance rate percentage
3. See detailed attendance log with:
   - Date and time
   - Subject name
   - Status

## 🏗️ Project Structure

```
attendance-system/
├── app.py                              # Main Flask application
├── attendance.db                       # SQLite database (auto-created)
├── face_encodings.pkl                  # Face encodings storage
├── shape_predictor_68_face_landmarks.dat    # dlib model (download required)
├── dlib_face_recognition_resnet_model_v1.dat # dlib model (download required)
├── templates/
│   ├── login.html                      # Login page
│   ├── admin.html                      # Admin dashboard
│   ├── student.html                    # Student portal
│   └── forgot_password.html            # Password reset page
├── static/
│   └── uploads/                        # Student photos
├── README.md                           # This file
└── requirements.txt                    # Python dependencies
```

## 🗄️ Database Schema

### Tables

**admins**
```sql
- id: INTEGER PRIMARY KEY
- username: TEXT UNIQUE
- password: TEXT (hashed)
- created_date: TIMESTAMP
```

**students**
```sql
- id: INTEGER PRIMARY KEY
- name: TEXT
- roll_number: TEXT UNIQUE
- password: TEXT (hashed)
- encoding: BLOB (face encoding)
- photo_path: TEXT
- registered_date: TIMESTAMP
```

**subjects**
```sql
- id: INTEGER PRIMARY KEY
- name: TEXT
- code: TEXT UNIQUE
- day_of_week: TEXT
- start_time: TEXT
- end_time: TEXT
- attendance_window: INTEGER (minutes)
- created_date: TIMESTAMP
```

**attendance**
```sql
- id: INTEGER PRIMARY KEY
- student_id: INTEGER (FK → students)
- subject_id: INTEGER (FK → subjects)
- timestamp: TIMESTAMP
- status: TEXT (default: 'Present')
- location_info: TEXT
```

**locations**
```sql
- id: INTEGER PRIMARY KEY
- name: TEXT
- wifi_ssid: TEXT
- ip_range: TEXT
- latitude: REAL
- longitude: REAL
- radius: INTEGER (meters)
- is_active: INTEGER (boolean)
```

## 🔧 Configuration

### Change Server Port

Edit the last line in `app.py`:
```python
app.run(debug=True, host='0.0.0.0', port=5001)  # Change port here
```

### Change Timezone

Edit the timezone constant in `app.py`:
```python
PAKISTAN_TZ = pytz.timezone('Asia/Karachi')  # Change to your timezone
```

Common timezones:
- USA East: `America/New_York`
- USA West: `America/Los_Angeles`
- UK: `Europe/London`
- India: `Asia/Kolkata`
- UAE: `Asia/Dubai`

### Adjust Face Recognition Tolerance

Edit the tolerance in `compare_faces()` function:
```python
def compare_faces(encoding1, encoding2, tolerance=0.6):  # Lower = stricter
```
- **0.4**: Very strict (may reject valid faces)
- **0.6**: Balanced (recommended)
- **0.8**: Lenient (may accept similar faces)

### Change Secret Key

In `app.py`, update:
```python
app.secret_key = 'your-secret-key-change-this-in-production'
```

Generate a secure key:
```python
import secrets
print(secrets.token_hex(32))
```

## 📡 API Endpoints

### Authentication
- `POST /auth/login` - User login
- `POST /auth/forgot-password` - Request password reset
- `POST /auth/reset-password` - Reset password with code
- `GET /logout` - Logout current user

### Admin Operations
- `POST /register` - Register new student
- `POST /subjects/add` - Add subject
- `GET /subjects/list` - List all subjects
- `PUT /subjects/update/<id>` - Update subject
- `DELETE /subjects/delete/<id>` - Delete subject
- `POST /locations/add` - Add location
- `GET /locations/list` - List locations
- `PUT /locations/update/<id>` - Update location
- `DELETE /locations/delete/<id>` - Delete location
- `GET /get_students` - Get all students
- `PUT /students/update/<id>` - Update student
- `DELETE /students/delete/<id>` - Delete student
- `GET /get_stats` - Get attendance statistics

### Student Operations
- `GET /get_current_subject` - Get active class
- `POST /mark_attendance` - Mark attendance with face
- `GET /get_attendance` - Get attendance records
- `GET /get_my_attendance` - Get personal attendance history

## 🛡️ Security Features

### Password Security
- Passwords hashed using werkzeug's `generate_password_hash()`
- Uses PBKDF2-SHA256 algorithm
- Individual salts for each password
- Never stores plain text passwords

### Session Security
- Flask session-based authentication
- Secret key for session encryption
- Session timeout on browser close
- Role-based access control (admin/student)

### Face Recognition Security
- Face encodings stored as binary blobs
- 128-dimensional face descriptors
- Distance-based matching algorithm
- Configurable tolerance threshold
- One face per registration

### Input Validation
- SQL injection prevention via parameterized queries
- Form validation on frontend and backend
- File type validation for images
- Size limits on uploaded files

## 🚨 Troubleshooting

### Camera Not Working

**Issue**: "Error accessing camera"

**Solutions:**
1. Grant browser camera permissions
2. Check if camera is in use by another app
3. Try different browser (Chrome/Firefox recommended)
4. Restart browser
5. Check system privacy settings

### Face Not Detected

**Issue**: "No face detected in image"

**Solutions:**
1. Ensure good lighting
2. Face directly towards camera
3. Move closer to camera
4. Remove glasses/hats
5. Avoid extreme angles
6. Check if face is clearly visible

### Face Not Recognized

**Issue**: "Face not recognized" or "Face does not match"

**Solutions:**
1. Ensure lighting similar to registration
2. Register again with better photo
3. Adjust tolerance in code (increase to 0.7-0.8)
4. Verify correct student is attempting attendance
5. Check for significant appearance changes

### dlib Installation Errors

**Issue**: "Cannot install dlib" or "dlib not found"

**Solutions:**

**Windows:**
```bash
# Install Visual Studio Build Tools first
# Then install CMake
pip install cmake
pip install dlib
```

**macOS:**
```bash
brew install cmake
pip install dlib
```

**Linux (Ubuntu/Debian):**
```bash
sudo apt-get install build-essential cmake
sudo apt-get install libboost-all-dev
pip install dlib
```

### Model Files Missing

**Issue**: "FileNotFoundError: shape_predictor_68_face_landmarks.dat"

**Solution:**
1. Download from: http://dlib.net/files/
2. Files needed:
   - `shape_predictor_68_face_landmarks.dat.bz2` (extract after download)
   - `dlib_face_recognition_resnet_model_v1.dat.bz2` (extract after download)
3. Place extracted `.dat` files in project root directory

### Port Already in Use

**Issue**: "Address already in use" on port 5001

**Solution:**
```bash
# Find process using port
lsof -ti:5001 | xargs kill -9  # macOS/Linux
netstat -ano | findstr :5001   # Windows

# Or change port in app.py
app.run(debug=True, host='0.0.0.0', port=5002)
```

### Database Locked

**Issue**: "database is locked"

**Solution:**
1. Close all connections to database
2. Restart application
3. Check for multiple app instances
4. Delete `attendance.db` to reset (WARNING: loses all data)

## 📊 Performance Optimization

### Speed Improvements

1. **Reduce Image Size**
   ```python
   # In startCamera() function
   video: { width: 320, height: 240 }  # Lower resolution
   ```

2. **Optimize Face Detection**
   ```python
   # In get_face_encoding()
   faces = detector(rgb, 0)  # Change from 1 to 0 (faster, less accurate)
   ```

3. **Database Indexing**
   ```sql
   CREATE INDEX idx_student_roll ON students(roll_number);
   CREATE INDEX idx_attendance_date ON attendance(timestamp);
   ```

### Memory Optimization

- Clear old attendance records periodically
- Compress student photos
- Use database vacuuming:
  ```sql
  VACUUM;
  ```

## 📱 Mobile Compatibility

The system is mobile-responsive and works on:
- ✅ Modern smartphones (Android/iOS)
- ✅ Tablets
- ✅ Desktop browsers

**Mobile Tips:**
- Use Chrome or Safari for best camera support
- Ensure camera permissions are granted
- Stable WiFi connection recommended
- Portrait orientation works best

## 🔄 Backup & Recovery

### Backup Database

```bash
# Copy database file
cp attendance.db attendance_backup_$(date +%Y%m%d).db

# Or use SQLite backup
sqlite3 attendance.db ".backup attendance_backup.db"
```

### Restore Database

```bash
# Replace current database
cp attendance_backup.db attendance.db
```

### Export Attendance Data

```python
import sqlite3
import csv

conn = sqlite3.connect('attendance.db')
cursor = conn.cursor()
cursor.execute("""
    SELECT s.name, s.roll_number, a.timestamp, sub.name 
    FROM attendance a
    JOIN students s ON a.student_id = s.id
    LEFT JOIN subjects sub ON a.subject_id = sub.id
""")

with open('attendance_export.csv', 'w', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(['Name', 'Roll Number', 'Timestamp', 'Subject'])
    writer.writerows(cursor.fetchall())

conn.close()
```

## 🌐 Deployment

### Production Deployment

**NOT recommended to use built-in Flask server in production**

Use WSGI server instead:

1. **Install Gunicorn**
   ```bash
   pip install gunicorn
   ```

2. **Run with Gunicorn**
   ```bash
   gunicorn -w 4 -b 0.0.0.0:5001 app:app
   ```

3. **Use Nginx as reverse proxy** (recommended)

4. **Enable HTTPS** for security

5. **Set environment variables**
   ```bash
   export FLASK_ENV=production
   export SECRET_KEY='your-production-secret-key'
   ```

### Docker Deployment

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 5001

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5001", "app:app"]
```

## 📄 Requirements.txt

```txt
Flask==3.0.0
werkzeug==3.0.1
opencv-python==4.8.1.78
dlib==19.24.2
numpy==1.24.3
Pillow==10.1.0
pytz==2023.3
```

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create feature branch
3. Make changes
4. Test thoroughly
5. Submit pull request

## 📜 License

This project is licensed under the MIT License.

## 🙏 Acknowledgments

- **dlib** - Face recognition library by Davis King
- **Flask** - Micro web framework
- **OpenCV** - Computer vision library
- **SQLite** - Embedded database

## 📞 Support

For issues, questions, or feature requests:
- Review troubleshooting section above
- Check existing GitHub issues
- Create new issue with detailed description

## 🔮 Future Enhancements

- [ ] Email notifications for attendance
- [ ] SMS alerts for absent students
- [ ] Attendance reports export (PDF/Excel)
- [ ] Multi-camera support
- [ ] QR code attendance option
- [ ] Mobile app (Android/iOS)
- [ ] Biometric fingerprint support
- [ ] Advanced analytics dashboard
- [ ] Parent portal access
- [ ] Integration with LMS platforms

---

**Made with ❤️ for educational institutions**

*Last Updated: February 2026*
