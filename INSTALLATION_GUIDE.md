# 🚀 Installation & Setup Guide

Complete step-by-step installation instructions for the Smart Attendance System with Face Recognition.

## Table of Contents
1. [System Requirements](#system-requirements)
2. [Quick Installation](#quick-installation)
3. [Detailed Installation](#detailed-installation)
4. [Configuration](#configuration)
5. [First-Time Setup](#first-time-setup)
6. [Troubleshooting Installation](#troubleshooting-installation)

---

## System Requirements

### Hardware Requirements

**Minimum:**
- CPU: Dual-core processor (Intel i3 or equivalent)
- RAM: 4GB
- Storage: 2GB free space
- Webcam: 720p (1280×720) or higher

**Recommended:**
- CPU: Quad-core processor (Intel i5/i7 or equivalent)
- RAM: 8GB or more
- Storage: 5GB free space (for growth)
- Webcam: 1080p (1920×1080) Full HD

**For Production (100+ students):**
- CPU: 4+ cores
- RAM: 16GB
- Storage: SSD with 20GB+
- Network: Stable broadband connection

### Software Requirements

**Operating System:**
- ✅ Windows 10/11
- ✅ macOS 10.14 (Mojave) or later
- ✅ Linux (Ubuntu 20.04+, Debian 10+, Fedora 33+)

**Required Software:**
- Python 3.8, 3.9, 3.10, or 3.11
- pip (Python package manager)
- Modern web browser:
  - Google Chrome 90+ (recommended)
  - Firefox 88+
  - Safari 14+ (macOS)
  - Edge 90+

**Development Tools (for building dlib):**
- CMake 3.8+
- C++ compiler (GCC, Clang, or MSVC)
- Build tools for your platform

---

## Quick Installation

For users who want to get started immediately:

```bash
# 1. Install Python 3.8+ if not already installed
python --version  # Check version

# 2. Navigate to project directory
cd /path/to/attendance-system

# 3. Install Python dependencies
pip install flask werkzeug opencv-python dlib numpy pillow pytz

# 4. Download dlib model files (see section below)

# 5. Run the application
python app.py

# 6. Open browser to http://localhost:5001
# Default login: admin / admin123
```

---

## Detailed Installation

### Step 1: Install Python

#### Windows

1. **Download Python:**
   - Visit: https://www.python.org/downloads/
   - Download Python 3.11 (or 3.8-3.10)

2. **Install Python:**
   - ✅ Check "Add Python to PATH"
   - Click "Install Now"
   - Wait for installation to complete

3. **Verify Installation:**
   ```cmd
   python --version
   pip --version
   ```

#### macOS

**Option 1: Using Homebrew (Recommended)**
```bash
# Install Homebrew if not installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Python
brew install python@3.11

# Verify
python3 --version
pip3 --version
```

**Option 2: Official Installer**
- Download from: https://www.python.org/downloads/macos/
- Install the .pkg file
- Verify installation

#### Linux (Ubuntu/Debian)

```bash
# Update package list
sudo apt update

# Install Python 3.11 and pip
sudo apt install python3.11 python3-pip python3.11-venv

# Verify installation
python3.11 --version
pip3 --version
```

#### Linux (Fedora/RHEL)

```bash
# Install Python
sudo dnf install python3.11 python3-pip

# Verify
python3.11 --version
```

---

### Step 2: Install Build Tools (Required for dlib)

#### Windows

1. **Install Visual Studio Build Tools:**
   - Download: https://visualstudio.microsoft.com/downloads/
   - Select "Build Tools for Visual Studio 2022"
   - Install these components:
     - ✅ Desktop development with C++
     - ✅ MSVC v143 - VS 2022 C++ build tools
     - ✅ Windows 10/11 SDK

2. **Install CMake:**
   - Download: https://cmake.org/download/
   - Choose "Windows x64 Installer"
   - ✅ Check "Add CMake to system PATH"

#### macOS

```bash
# Install Xcode Command Line Tools
xcode-select --install

# Install CMake
brew install cmake
```

#### Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install build-essential cmake
sudo apt install libboost-all-dev
```

#### Linux (Fedora/RHEL)

```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install cmake boost-devel
```

---

### Step 3: Download Project Files

**Option 1: Clone from Git**
```bash
git clone <repository-url>
cd attendance-system
```

**Option 2: Download ZIP**
1. Download project ZIP file
2. Extract to desired location
3. Open terminal/command prompt in extracted folder

---

### Step 4: Create Virtual Environment (Recommended)

Creating a virtual environment isolates project dependencies.

```bash
# Navigate to project directory
cd /path/to/attendance-system

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate

# macOS/Linux:
source venv/bin/activate

# Your prompt should now show (venv)
```

---

### Step 5: Install Python Dependencies

With virtual environment activated:

```bash
# Upgrade pip first
pip install --upgrade pip

# Install dependencies
pip install flask==3.0.0
pip install werkzeug==3.0.1
pip install opencv-python==4.8.1.78
pip install numpy==1.24.3
pip install pillow==10.1.0
pip install pytz==2023.3

# Install dlib (this may take 5-15 minutes)
pip install dlib==19.24.2
```

**If dlib installation fails, see Troubleshooting section below.**

---

### Step 6: Download dlib Model Files

These files are required for face recognition but are not included due to size.

**Required Files:**
1. `shape_predictor_68_face_landmarks.dat` (~95 MB)
2. `dlib_face_recognition_resnet_model_v1.dat` (~23 MB)

**Download Steps:**

1. **Download compressed files:**
   ```bash
   # Create models directory
   mkdir models
   cd models

   # Download shape predictor
   wget http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2

   # Download face recognition model
   wget http://dlib.net/files/dlib_face_recognition_resnet_model_v1.dat.bz2
   ```

   **Alternative (if wget not available):**
   - Visit: http://dlib.net/files/
   - Download both .bz2 files manually

2. **Extract files:**
   ```bash
   # Extract both files
   bzip2 -d shape_predictor_68_face_landmarks.dat.bz2
   bzip2 -d dlib_face_recognition_resnet_model_v1.dat.bz2

   # Move to project root
   mv *.dat ../
   cd ..
   ```

   **Windows extraction:**
   - Use 7-Zip or WinRAR to extract .bz2 files
   - Move .dat files to project root directory

3. **Verify files are in correct location:**
   ```
   attendance-system/
   ├── app.py
   ├── shape_predictor_68_face_landmarks.dat  ← Here
   ├── dlib_face_recognition_resnet_model_v1.dat  ← Here
   └── templates/
   ```

---

### Step 7: Initialize Database

The database will be created automatically when you first run the application.

```bash
# Run application (database will be created)
python app.py
```

You should see output like:
```
 * Serving Flask app 'app'
 * Debug mode: on
WARNING: This is a development server.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5001
 * Running on http://192.168.1.x:5001
```

**Database file created:** `attendance.db`

---

### Step 8: Access the Application

1. **Open web browser**
2. **Navigate to:** `http://localhost:5001`
3. **Login with default admin account:**
   - Username: `admin`
   - Password: `admin123`

**⚠️ IMPORTANT: Change default password immediately after first login!**

---

## Configuration

### Change Admin Password

1. Login as admin
2. *(Password change feature to be implemented)*
3. **Temporary method:** Update database directly:

```python
# Run this Python script
from werkzeug.security import generate_password_hash
import sqlite3

new_password = "YourNewSecurePassword123!"
hashed = generate_password_hash(new_password)

conn = sqlite3.connect('attendance.db')
c = conn.cursor()
c.execute("UPDATE admins SET password = ? WHERE username = 'admin'", (hashed,))
conn.commit()
conn.close()

print("Password updated successfully!")
```

### Change Server Port

Edit `app.py`, last line:

```python
# Change from 5001 to desired port
app.run(debug=True, host='0.0.0.0', port=8080)
```

### Configure Timezone

Edit `app.py`, near the top:

```python
# Change timezone as needed
PAKISTAN_TZ = pytz.timezone('Asia/Karachi')

# Examples for other regions:
# USA East: pytz.timezone('America/New_York')
# USA West: pytz.timezone('America/Los_Angeles')
# UK: pytz.timezone('Europe/London')
# India: pytz.timezone('Asia/Kolkata')
# UAE: pytz.timezone('Asia/Dubai')
```

### Adjust Face Recognition Tolerance

Edit `app.py`, find `compare_faces` function:

```python
def compare_faces(encoding1, encoding2, tolerance=0.6):
    # Lower = stricter (0.4-0.5)
    # Higher = more lenient (0.7-0.8)
    # Default 0.6 is recommended
```

### Change Upload Folder Location

Edit `app.py`:

```python
# Change upload directory
UPLOAD_FOLDER = 'custom/path/uploads'
```

---

## First-Time Setup

### 1. Register First Student

1. Login as admin
2. Click "Register Student" tab
3. Fill in details:
   - Student Name: John Doe
   - Roll Number: STU001
   - Password: student123
4. Click "Start Camera"
5. Position face clearly
6. Click "Capture Photo"
7. Click "Register Student"

### 2. Add Your First Subject

1. Go to "Manage Subjects" tab
2. Fill subject form:
   - Subject Name: Introduction to Programming
   - Subject Code: CS101
   - Day of Week: Monday
   - Start Time: 09:00
   - End Time: 10:30
   - Attendance Window: 15
3. Click "Add Subject"

### 3. Test Student Attendance

1. Logout from admin
2. Login as student:
   - Roll Number: STU001
   - Password: student123
3. Click "Mark Attendance" tab
4. Start camera and mark attendance

---

## Troubleshooting Installation

### Issue: Python not found

**Windows:**
```cmd
# Ensure Python is in PATH
# Re-run installer and check "Add to PATH"
```

**macOS/Linux:**
```bash
# Use python3 instead of python
python3 --version
pip3 install ...
```

### Issue: pip command not found

```bash
# Windows
python -m pip install ...

# macOS/Linux
python3 -m pip install ...
```

### Issue: dlib installation fails

**Error: "CMake must be installed"**

Solution: Install CMake (see Step 2 above)

**Error: "No C++ compiler found"**

**Windows:**
- Install Visual Studio Build Tools (see Step 2)

**macOS:**
```bash
xcode-select --install
```

**Linux:**
```bash
sudo apt install build-essential
```

**Error: "wheel failed building"**

```bash
# Upgrade pip and setuptools
pip install --upgrade pip setuptools wheel

# Try installing again
pip install dlib
```

**Alternative (pre-built wheels):**

**Windows only:**
```bash
# Visit https://github.com/z-mahmud22/Dlib_Windows_Python3.x
# Download appropriate .whl file for your Python version
pip install dlib-19.24.2-cp311-cp311-win_amd64.whl
```

### Issue: OpenCV installation fails

**Error: "Could not find a version"**

```bash
# Try specific version
pip install opencv-python==4.8.1.78

# Or install without version constraint
pip install opencv-python
```

### Issue: Model files not loading

**Error: "FileNotFoundError: shape_predictor_68_face_landmarks.dat"**

**Solution:**
1. Verify files are in project root directory
2. Check file names match exactly
3. Ensure files were extracted (not .bz2 anymore)

```bash
# Check if files exist
ls -la *.dat  # macOS/Linux
dir *.dat     # Windows
```

### Issue: Permission denied errors

**Linux/macOS:**
```bash
# Fix permissions
chmod +x app.py

# Or run with sudo (not recommended)
sudo python3 app.py
```

**Better solution:**
```bash
# Install packages to user directory
pip install --user package_name
```

### Issue: Port already in use

**Error: "Address already in use"**

**Solution 1: Kill process on port 5001**
```bash
# Windows
netstat -ano | findstr :5001
taskkill /PID <PID> /F

# macOS/Linux
lsof -ti:5001 | xargs kill -9
```

**Solution 2: Use different port**
```python
# Edit app.py
app.run(debug=True, host='0.0.0.0', port=8080)
```

### Issue: Virtual environment not activating

**Windows:**
```cmd
# Enable script execution
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Then activate
venv\Scripts\activate
```

**macOS/Linux:**
```bash
# Ensure using correct shell
source venv/bin/activate

# If using fish shell
source venv/bin/activate.fish
```

### Issue: Camera not working in browser

**Solutions:**
1. Grant camera permissions in browser settings
2. Use HTTPS (camera may require secure context)
3. Try different browser (Chrome recommended)
4. Check if camera is being used by another app
5. Restart browser

**Check camera access:**
- Chrome: Settings → Privacy and security → Site Settings → Camera
- Firefox: Settings → Privacy & Security → Permissions → Camera
- Safari: Preferences → Websites → Camera

---

## Verification Checklist

After installation, verify everything works:

- [ ] Python version 3.8+ installed
- [ ] All Python packages installed successfully
- [ ] dlib model files downloaded and in correct location
- [ ] Database file created (`attendance.db`)
- [ ] Application starts without errors
- [ ] Can access http://localhost:5001
- [ ] Can login as admin
- [ ] Camera works in browser
- [ ] Can register a test student
- [ ] Can add a test subject
- [ ] Student can login
- [ ] Student can mark attendance

---

## Next Steps

After successful installation:

1. **Read Usage Documentation:** [USAGE.md](USAGE.md)
2. **Configure System:** Set up subjects, add students
3. **Test Thoroughly:** Try marking attendance with different faces
4. **Customize:** Adjust settings for your institution
5. **Deploy:** Set up on production server (see Production Deployment)

---

## Production Deployment

For production use, additional steps are needed:

### Use WSGI Server

```bash
# Install Gunicorn
pip install gunicorn

# Run with Gunicorn
gunicorn -w 4 -b 0.0.0.0:5001 app:app
```

### Enable HTTPS

Use Nginx or Apache as reverse proxy with SSL certificate.

### Database Migration

For large installations, migrate to PostgreSQL:

```bash
# Install PostgreSQL adapter
pip install psycopg2-binary

# Update database connection in app.py
```

### Environment Variables

```bash
# Set production environment
export FLASK_ENV=production
export SECRET_KEY='generate-a-random-secure-key-here'
```

---

## Getting Help

If you encounter issues:

1. Check this troubleshooting section
2. Review error messages carefully
3. Search for error on Google/Stack Overflow
4. Check Python/package versions compatibility
5. Ensure all prerequisites are installed

---

**Installation Support:**
- Documentation: [README.md](ATTENDANCE_README.md)
- Issues: Create GitHub issue with:
  - Operating system
  - Python version
  - Error message
  - Steps to reproduce

**System Ready!** 🎉

You can now start using the Smart Attendance System.
