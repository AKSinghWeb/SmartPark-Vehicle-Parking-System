# SmartPark - Vehicle Parking Management System

A comprehensive smart parking management system that uses AI-powered Automatic Number Plate Recognition (ANPR) technology to automate vehicle entry and exit management. The system provides real-time parking space monitoring, booking capabilities, and seamless payment tracking.

## 🚀 Features

- **AI-Powered ANPR**: Automated vehicle number plate recognition using YOLO and PaddleOCR
- **Real-time Monitoring**: Live parking space availability tracking with WebSocket support
- **Parking Space Booking**: Users can reserve parking spaces in advance
- **Vehicle Entry/Exit Management**: Automated tracking of vehicle entry and exit times
- **Transaction Management**: Complete parking transaction history and billing
- **User Authentication**: Secure JWT-based authentication system
- **Admin Dashboard**: Comprehensive admin panel for managing parking operations
- **Responsive UI**: Modern, mobile-friendly interface built with React
- **Real-time Updates**: Socket.IO integration for live updates across the system

## 🛠️ Technology Stack

### Backend
- **Flask**: Python web framework for RESTful API
- **Flask-SocketIO**: Real-time bidirectional communication
- **SQLAlchemy**: SQL toolkit and ORM
- **Flask-JWT-Extended**: JWT token management
- **YOLO (Ultralytics)**: Object detection for number plate recognition
- **PaddleOCR**: Optical character recognition for license plates
- **OpenCV**: Computer vision processing
- **PostgreSQL**: Database (configurable)

### Frontend
- **React 18**: UI library
- **Vite**: Build tool and development server
- **React Router**: Navigation and routing
- **Axios**: HTTP client
- **Socket.IO Client**: Real-time communication
- **TailwindCSS**: Utility-first CSS framework
- **Radix UI**: Accessible component primitives
- **Lucide React**: Icon library
- **AOS**: Animate on scroll library

## 📁 Project Structure

```
SmartPark-Vehicle-Parking-System/
├── Flask_Server/              # Backend API server
│   ├── app/
│   │   ├── __init__.py       # Flask app initialization
│   │   ├── config.py         # Configuration settings
│   │   ├── models.py         # Database models
│   │   ├── sockets.py        # WebSocket event handlers
│   │   ├── resources/        # REST API resources
│   │   │   ├── user.py       # User management endpoints
│   │   │   ├── parking.py    # Parking management endpoints
│   │   │   └── booking.py    # Booking management endpoints
│   │   └── .env.example      # Environment variables template
│   ├── ANPR_Model/           # YOLO model for license plate detection
│   │   └── best.pt           # Trained YOLO model weights
│   ├── requirements.txt      # Python dependencies
│   └── run.py               # Application entry point
│
└── Client_Interface/         # Frontend React application
    ├── src/
    │   ├── components/       # React components
    │   │   ├── CameraComponent/        # Camera integration
    │   │   ├── ParkingLiveMap/         # Live parking map
    │   │   ├── ParkingStatistics/      # Statistics dashboard
    │   │   ├── VehicleEntryExitForm/   # Entry/exit forms
    │   │   ├── UserProfile/            # User profile management
    │   │   └── ui/                     # UI components library
    │   ├── pages/            # Application pages
    │   │   ├── Home.jsx              # Landing page
    │   │   ├── AdminHome.jsx         # Admin dashboard
    │   │   ├── AdminEntry.jsx        # Vehicle entry page
    │   │   ├── AdminExit.jsx         # Vehicle exit page
    │   │   ├── MyBookings.jsx        # User bookings
    │   │   ├── About.jsx             # About/team page
    │   │   └── ContactUs.jsx         # Contact page
    │   ├── services/         # API service layer
    │   │   └── apiService.js         # API integration
    │   ├── utils/            # Utility functions
    │   ├── socket.js         # Socket.IO client setup
    │   └── App.jsx           # Root application component
    ├── package.json          # Node.js dependencies
    └── vite.config.js        # Vite configuration
```

## 🔧 Installation and Setup

### Prerequisites
- Python 3.8 or higher
- Node.js 16 or higher
- PostgreSQL (or any compatible SQL database)
- CUDA-compatible GPU (recommended for optimal ANPR performance)

### Backend Setup

1. **Navigate to the Flask Server directory**
   ```bash
   cd Flask_Server
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**
   
   Create a `.env` file in the `Flask_Server/app/` directory:
   ```env
   SECRET_KEY=your-secret-key-here
   DATABASE_URI=postgresql://username:password@localhost:5432/smartpark
   ```

5. **Initialize the database**
   ```bash
   python run.py
   ```
   This will create all necessary database tables on first run.

### Frontend Setup

1. **Navigate to the Client Interface directory**
   ```bash
   cd Client_Interface
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Configure API endpoint**
   
   Update the `apiUrl` in `src/services/apiService.js` and `src/socket.js` to match your backend server URL:
   ```javascript
   export const apiUrl = 'http://127.0.0.1:5000'
   ```

## 🚀 Running the Application

### Start the Backend Server

```bash
cd Flask_Server
source venv/bin/activate  # On Windows: venv\Scripts\activate
python run.py
```

The Flask server will start on `http://127.0.0.1:5000`

### Start the Frontend Development Server

```bash
cd Client_Interface
npm run dev
```

The Vite development server will start on `http://localhost:5173`

### Build for Production

**Backend**: The Flask application serves the React build from the `client/dist` folder.

**Frontend**:
```bash
cd Client_Interface
npm run build
```

Copy the generated `dist` folder to `Flask_Server/app/client/`

## 📡 API Endpoints

### Authentication
- `POST /api/auth/signup` - User registration
- `POST /api/auth/login` - User login

### User Management
- `GET /api/users` - Get all users (admin)
- `GET /api/users/<user_id>` - Get user by ID
- `GET /api/users/my-profile` - Get current user profile

### Parking Management
- `GET /api/parking` - Get all parking spaces
- `GET /api/parking/stats` - Get parking statistics
- `POST /api/parking/entry` - Register vehicle entry
- `POST /api/parking/exit` - Register vehicle exit
- `POST /api/parking/search` - Search for vehicle

### Parking Transactions
- `GET /api/parking/transactions` - Get all transactions
- `GET /api/parking/transactions/day/<date>` - Get transactions by date

### Booking Management
- `POST /api/parking/book` - Book a parking space
- `GET /api/parking/book` - Get user bookings

### WebSocket Events
- `connect` - Client connection
- `frame` - Send camera frame for entry ANPR
- `exit_frame` - Send camera frame for exit ANPR
- `ocr` - Receive recognized plate number (entry)
- `exit_ocr` - Receive recognized plate number (exit)

## 🎯 Key Features Explained

### ANPR System
The system uses a custom-trained YOLO model to detect license plates in real-time from camera feeds. Once detected, PaddleOCR extracts the text from the plate image. The system supports multiple Indian license plate formats.

### Real-time Updates
Socket.IO enables instant updates across all connected clients when parking space availability changes, ensuring accurate real-time information.

### Parking Space Booking
Users can reserve parking spaces in advance. The system prevents double-booking and manages reservation timeouts.

### Transaction Tracking
Every vehicle entry and exit is logged with timestamps. The system automatically calculates parking duration and applicable charges.

## 👥 Team

This project was developed by students from North-Eastern Hill University, Shillong.

**Project Lead & Backend Developer**: Anupam Kumar Singh
- GitHub: [@AKSinghWeb](https://github.com/aksinghweb)
- LinkedIn: [aksinghweb](https://www.linkedin.com/in/aksinghweb/)

**Frontend Developers**:
- Suraj Bhagat
- Savelyness Iawphniaw

**UI/UX Designer & QA Tester**: Lota Ingtipi
- LinkedIn: [lota-ingtipi](https://www.linkedin.com/in/lota-ingtipi/)

**Project Supervisor**: Prof./Dr. from North-Eastern Hill University, Shillong

## 📝 License

This project is part of an academic initiative at North-Eastern Hill University, Shillong.

## 🤝 Contributing

This is an academic project. For suggestions or issues, please contact the team members.

## 📧 Contact

For any queries or support, please visit the contact page in the application or reach out to the team members through their respective profiles.

---

**Note**: Make sure to configure your database and API endpoints correctly before running the application. The ANPR model requires adequate computational resources for optimal performance.
