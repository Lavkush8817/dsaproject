# 🎯 Face Recognition Attendance System

A modern, AI-powered attendance tracking system that uses facial recognition technology to automatically mark attendance. Built with vanilla JavaScript, Face-API.js, and Tailwind CSS.

## 📋 Table of Contents

- [Features](#features)
- [Technologies Used](#technologies-used)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Usage](#usage)
- [How It Works](#how-it-works)
- [Browser Compatibility](#browser-compatibility)
- [Security & Privacy](#security--privacy)
- [Troubleshooting](#troubleshooting)

## ✨ Features

### Core Functionality
- **Face Registration**: Capture and register user faces with personal details
- **Automated Attendance**: Mark attendance using facial recognition
- **Real-time Recognition**: Instant face detection and matching
- **Dashboard Analytics**: View statistics and attendance trends
- **User Management**: Add, view, and remove registered users
- **Attendance Records**: View, filter, and export attendance data

### User Interface
- Modern, responsive design with Tailwind CSS
- Intuitive navigation with sidebar menu
- Real-time webcam preview
- Toast notifications for user feedback
- Loading states and animations
- Mobile-friendly interface

### Data Management
- Local storage for user data and attendance records
- CSV export functionality for attendance reports
- Date range filtering for records
- Duplicate attendance prevention

## 🛠 Technologies Used

### Frontend
- **HTML5**: Structure and semantic markup
- **CSS3**: Custom styling and animations
- **JavaScript (ES6+)**: Application logic and state management
- **Tailwind CSS**: Utility-first CSS framework

### AI/ML Libraries
- **Face-API.js v0.22.2**: Face detection and recognition
  - SSD MobileNet V1: Face detection model
  - Face Landmark 68: Facial landmark detection
  - Face Recognition: 128-dimensional face descriptors

### External Resources
- **Google Fonts**: Inter and Space Grotesk fonts
- **Lucide Icons**: SVG icon library

## 🏗 System Architecture

### Application Structure

```
Face-Attendance-System/
├── index.html              # Main application file
├── models/                 # Face-API.js model files
│   ├── ssd_mobilenetv1_model-*
│   ├── face_landmark_68_model-*
│   ├── face_recognition_model-*
│   └── *-weights_manifest.json
└── README.md              # Documentation
```

### State Management

The application uses a centralized state object:

```javascript
state = {
    currentView: 'dashboard',      // Active view
    users: [],                     // Registered users array
    attendance: [],                // Attendance records array
    isWebcamOn: false,            // Webcam status
    registerPhotoData: null,       // Captured photo data
    faceMatcher: null             // Face recognition matcher
}
```

### Data Models

**User Object:**
```javascript
{
    id: "uuid",                    // Unique identifier
    name: "string",                // Full name
    employeeId: "string",          // Student/Employee ID
    department: "string",          // Department name
    email: "string",               // Email address
    photo: "base64",               // Profile photo (base64)
    descriptor: Float32Array,      // 128D face embedding
    createdAt: "ISO8601"          // Registration timestamp
}
```

**Attendance Record:**
```javascript
{
    id: "uuid",                    // Unique identifier
    userId: "uuid",                // Reference to user
    userName: "string",            // User's name
    employeeId: "string",          // Student/Employee ID
    department: "string",          // Department name
    timestamp: "ISO8601",          // Attendance timestamp
    photo: "base64"                // Captured photo (base64)
}
```

## 📦 Installation

### Prerequisites
- Modern web browser (Chrome, Firefox, Edge, Safari)
- Webcam access
- Local web server (for development)

### Setup Steps

1. **Clone or download the repository**
   ```bash
   git clone https://github.com/Lavkush8817/dsaproject.git
   cd Face-Attendance-System
   ```

2. **Ensure model files are present**
   The `models/` directory should contain all Face-API.js model files.

3. **Start a local server**
   
   Using Python:
   ```bash
   python -m http.server 8000
   ```
   
   Using Node.js:
   ```bash
   npx http-server -p 8000
   ```
   
   Using PHP:
   ```bash
   php -S localhost:8000
   ```

4. **Open in browser**
   Navigate to `http://localhost:8000`

5. **Grant webcam permissions**
   Allow the browser to access your webcam when prompted.

## 🚀 Usage

### 1. Register a New User

1. Click **"Register User"** from the sidebar or dashboard
2. Position your face in the webcam frame
3. Click **"Capture Photo"** when ready
4. Fill in the registration form:
   - Full Name
   - Student ID (must be unique)
   - Department
   - Email Address
5. Click **"Register User"** to save

### 2. Mark Attendance

1. Click **"Mark Attendance"** from the sidebar or dashboard
2. Position your face in the webcam frame
3. Click **"Mark My Attendance"**
4. The system will:
   - Detect your face
   - Match it against registered users
   - Mark attendance if recognized
   - Prevent duplicate entries for the same day

### 3. View Attendance Records

1. Click **"View Records"** from the sidebar
2. Use date filters to narrow down results:
   - Select start date
   - Select end date
   - Click **"Filter"**
3. Export data by clicking **"Export CSV"**

### 4. Manage Users

1. Click **"Manage Users"** from the sidebar
2. View all registered users with their details
3. Delete users by clicking the **"Delete"** button
   - This also removes their attendance records

## 🔍 How It Works

### Face Recognition Pipeline

1. **Face Detection**
   - Uses SSD MobileNet V1 model
   - Detects faces in real-time from webcam feed
   - Returns bounding box coordinates

2. **Landmark Detection**
   - Identifies 68 facial landmarks
   - Used for face alignment and normalization

3. **Face Descriptor Extraction**
   - Generates 128-dimensional embedding vector
   - Unique numerical representation of the face
   - Invariant to lighting, angle, and expression changes

4. **Face Matching**
   - Compares new face descriptor with stored descriptors
   - Uses Euclidean distance calculation
   - Threshold: 0.6 (configurable)
   - Returns best match or "unknown"

### Recognition Accuracy

The system uses a distance threshold of **0.6**:
- **Distance < 0.6**: Face recognized (match)
- **Distance ≥ 0.6**: Face not recognized (unknown)

Lower threshold = stricter matching (fewer false positives)
Higher threshold = looser matching (more false positives)

### Data Storage

All data is stored in browser's **localStorage**:
- **Key**: `face-attendance-users` - User profiles and face descriptors
- **Key**: `face-attendance-records` - Attendance records

**Note**: Data persists across sessions but is browser-specific.

## 🌐 Browser Compatibility

### Supported Browsers
- ✅ Chrome 60+
- ✅ Firefox 55+
- ✅ Edge 79+
- ✅ Safari 11+
- ✅ Opera 47+

### Required Features
- WebRTC (getUserMedia API)
- Canvas API
- LocalStorage
- ES6+ JavaScript support
- WebAssembly (for Face-API.js)

## 🔒 Security & Privacy

### Data Privacy
- All data stored locally in browser
- No data sent to external servers
- Face descriptors are mathematical representations (not images)
- Users can clear data by clearing browser storage

### Best Practices
- Use HTTPS in production
- Implement user authentication for admin features
- Regular data backups
- Comply with local privacy regulations (GDPR, etc.)

### Limitations
- Browser storage has size limits (~5-10MB)
- Data not synchronized across devices
- No built-in backup mechanism

## 🐛 Troubleshooting

### Webcam Not Working
- **Check permissions**: Ensure browser has webcam access
- **Check hardware**: Verify webcam is connected and working
- **Try different browser**: Some browsers have better webcam support
- **HTTPS required**: Some browsers require HTTPS for webcam access

### Face Not Detected
- **Improve lighting**: Ensure adequate, even lighting
- **Face the camera**: Look directly at the camera
- **Remove obstructions**: Remove glasses, masks, or hats
- **Distance**: Position face 1-2 feet from camera

### Face Not Recognized
- **Re-register**: Try registering again with better photo
- **Consistent conditions**: Use similar lighting as registration
- **Clear photo**: Ensure registration photo is clear and well-lit
- **Adjust threshold**: Modify the 0.6 threshold in code if needed

### Models Not Loading
- **Check model files**: Ensure all model files are in `/models` directory
- **Check file paths**: Verify paths in code match actual file locations
- **Use local server**: Don't open HTML file directly (use http://localhost)
- **Check console**: Look for error messages in browser console

### Performance Issues
- **Close other tabs**: Free up browser resources
- **Update browser**: Use latest browser version
- **Reduce resolution**: Lower webcam resolution if needed
- **Clear storage**: Remove old attendance records

## 📊 Dashboard Statistics

The dashboard displays three key metrics:

1. **Total Users Registered**: Count of all registered users
2. **Attendance Marked Today**: Count of attendance records for current day
3. **Total Attendance Records**: Count of all attendance records

Statistics update automatically when:
- New user is registered
- Attendance is marked
- User is deleted

## 📤 Export Functionality

### CSV Export Format
```csv
Name,Student ID,Department,Timestamp
"John Doe","STU001","Computer Science","1/1/2024, 9:30:00 AM"
"Jane Smith","STU002","Engineering","1/1/2024, 9:35:00 AM"
```

### Export Features
- Includes all attendance records
- Properly formatted for Excel/Google Sheets
- Quoted fields for special characters
- Localized timestamp format

## 🎨 UI/UX Design System

### Color Palette
- **Primary**: Sky Blue (#0ea5e9, #0284c7)
- **Gray Scale**: Slate shades
- **Success**: Emerald (#10b981)
- **Warning**: Amber (#f59e0b)
- **Error**: Red (#ef4444)

### Typography
- **Headings**: Space Grotesk (500, 600, 700)
- **Body**: Inter (400, 500, 600, 700)

### Components
- Rounded corners (8px, 12px, 16px)
- Subtle shadows for depth
- Smooth transitions (0.2s)
- Hover effects on interactive elements

## 🔧 Configuration

### Adjusting Recognition Threshold

In the code, find this line:
```javascript
state.faceMatcher = new faceapi.FaceMatcher(labeledFaceDescriptors, 0.6);
```

Change `0.6` to:
- **0.4-0.5**: Stricter matching (recommended for high security)
- **0.6**: Default (balanced)
- **0.7-0.8**: Looser matching (more tolerant)

### Changing Model Path

If models are in a different directory:
```javascript
await faceapi.nets.ssdMobilenetv1.loadFromUri('/your-path/models');
```

## 📝 Future Enhancements

Potential features for future versions:
- Backend integration with database
- Multi-device synchronization
- Email notifications
- Attendance reports and analytics
- Role-based access control
- Bulk user import
- Mobile app version
- Anti-spoofing (liveness detection)

## 👨‍💻 Development

### Code Structure

**Main Sections:**
1. State Management
2. DOM Element References
3. Initialization
4. Navigation
5. Webcam Management
6. Database (LocalStorage)
7. Face Recognition Logic
8. Dashboard
9. Registration
10. Attendance Marking
11. View & Manage
12. Utilities

### Adding New Features

1. Update state object if needed
2. Add DOM elements and references
3. Implement feature logic
4. Update UI rendering
5. Add event listeners
6. Test thoroughly

## 📄 License

This project is open source and available for educational purposes.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## 📧 Contact

For questions or support, please contact the repository owner.

---

**Built with ❤️ using Face-API.js and modern web technologies**
