# Talkstra Chat App

A modern, real-time chat application built with React and Node.js, featuring instant messaging, online status, image sharing, and customizable themes.

## Features

- 🔐 **User Authentication** - Secure signup and login with JWT tokens stored in HTTP-only cookies
- 💬 **Real-time Messaging** - Instant messaging using Socket.IO with live message delivery
- 👥 **Online Status** - Real-time online/offline status tracking with visual indicators
- 🖼️ **Image Sharing** - Send and receive images in chat using Cloudinary integration
- 🎨 **33 Beautiful Themes** - Choose from 33+ pre-configured themes using DaisyUI
- 📱 **Responsive Design** - Fully responsive design that works on desktop and mobile devices
- 👤 **Profile Management** - Update profile picture and view account information
- 🌐 **Online Users Filter** - Filter sidebar to show only online users

## Tech Stack

### Frontend
- React 18
- Vite (Build tool)
- Zustand (State Management)
- React Router DOM (Routing)
- Socket.IO Client (Real-time communication)
- Tailwind CSS + DaisyUI (Styling)
- Axios (HTTP client)
- React Hot Toast (Notifications)
- Lucide React (Icons)

### Backend
- Node.js
- Express.js (Web framework)
- Socket.IO (Real-time WebSocket)
- MongoDB with Mongoose (Database)
- JWT (JSON Web Tokens for authentication)
- Cloudinary (Image storage and CDN)
- Bcryptjs (Password hashing)
- Cookie Parser (Cookie handling)
- CORS (Cross-origin resource sharing)

## Prerequisites

- Node.js (v14 or higher)
- MongoDB (local installation or MongoDB Atlas)
- Cloudinary account (for image uploads)

## Installation

1. Clone the repository
```bash
git clone <repository-url>
cd Talkstra-chat-app-main/Talkstra-chat-app-main/Talkstra-chat-app-main
```

2. Install dependencies
```bash
# Install root dependencies (optional, for build scripts)
npm install

# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

## Environment Variables

Create a `.env` file in the `backend` directory:

```env
PORT=5001
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
NODE_ENV=development
```

## Running the Application

### Development Mode

1. Start the backend server:
```bash
cd backend
npm run dev
```

2. Start the frontend (in a new terminal):
```bash
cd frontend
npm run dev
```

The backend will run on `http://localhost:5001` and the frontend on `http://localhost:5173`.

### Production Mode

Build and start the application:
```bash
npm run build
npm start
```

## Database Seeding

To seed the database with sample users (16 pre-configured users), run:
```bash
cd backend
node src/seeds/user.seed.js
```

**Note:** The seed file creates 16 sample users (8 female and 8 male users) with default password `123456`. All users have profile pictures from randomuser.me API.

## Project Structure

```
Talkstra-chat-app-main/
├── backend/
│   ├── src/
│   │   ├── controllers/        # Request handlers
│   │   │   ├── auth.controller.js
│   │   │   └── message.controller.js
│   │   ├── models/             # MongoDB schemas
│   │   │   ├── user.model.js
│   │   │   └── message.model.js
│   │   ├── routes/             # API routes
│   │   │   ├── auth.route.js
│   │   │   └── message.route.js
│   │   ├── middleware/         # Authentication middleware
│   │   │   └── auth.middleware.js
│   │   ├── lib/                # Utilities
│   │   │   ├── db.js           # MongoDB connection
│   │   │   ├── socket.js       # Socket.IO setup
│   │   │   ├── cloudinary.js   # Cloudinary config
│   │   │   └── utils.js        # JWT utilities
│   │   ├── seeds/              # Database seed files
│   │   │   └── user.seed.js
│   │   └── index.js            # Server entry point
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/         # React components
│   │   │   ├── Navbar.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   ├── ChatContainer.jsx
│   │   │   ├── ChatHeader.jsx
│   │   │   ├── MessageInput.jsx
│   │   │   ├── NoChatSelected.jsx
│   │   │   ├── AuthImagePattern.jsx
│   │   │   └── skeletons/      # Loading skeletons
│   │   ├── pages/              # Page components
│   │   │   ├── HomePage.jsx
│   │   │   ├── LoginPage.jsx
│   │   │   ├── SignupPage.jsx
│   │   │   ├── ProfilePage.jsx
│   │   │   └── SettingsPage.jsx
│   │   ├── store/              # Zustand stores
│   │   │   ├── useAuthStore.js
│   │   │   ├── useChatStore.js
│   │   │   └── useThemeStore.js
│   │   ├── lib/                # Utilities
│   │   │   ├── axios.js        # Axios instance
│   │   │   └── utils.js        # Helper functions
│   │   ├── constants/          # Constants
│   │   │   └── index.js        # Theme list
│   │   ├── App.jsx             # Main app component
│   │   ├── main.jsx            # React entry point
│   │   └── index.css           # Global styles
│   ├── public/                 # Static assets
│   ├── index.html
│   ├── package.json
│   ├── vite.config.js
│   ├── tailwind.config.js
│   └── postcss.config.js
└── package.json
```

## API Endpoints

### Authentication
All authentication endpoints use JWT tokens stored in HTTP-only cookies.

- `POST /api/auth/signup` - Create new account
  - Body: `{ fullName, email, password }`
  - Returns: User object (without password)
  
- `POST /api/auth/login` - Login user
  - Body: `{ email, password }`
  - Returns: User object (without password)
  
- `POST /api/auth/logout` - Logout user
  - Clears authentication cookie
  
- `GET /api/auth/check` - Check authentication status
  - Protected route
  - Returns: Current user object (without password)
  
- `PUT /api/auth/update-profile` - Update user profile
  - Protected route
  - Body: `{ profilePic: base64Image }`
  - Returns: Updated user object

### Messages
All message endpoints require authentication (protected routes).

- `GET /api/messages/users` - Get all users for sidebar
  - Returns: Array of all users except the logged-in user
  
- `GET /api/messages/:id` - Get messages with a user
  - `:id` - User ID to chat with
  - Returns: Array of messages between logged-in user and specified user
  
- `POST /api/messages/send/:id` - Send a message to a user
  - `:id` - Receiver user ID
  - Body: `{ text?: string, image?: base64Image }`
  - Returns: Created message object
  - Emits Socket.IO event to receiver if online

## Socket.IO Events

### Client Events
- `connection` - Client connects with `userId` query parameter
- `disconnect` - Client disconnects

### Server Events
- `getOnlineUsers` - Emitted when user connects/disconnects (returns array of online user IDs)
- `newMessage` - Emitted to receiver when a new message is sent

## Available Themes

The application includes 33 pre-configured themes:
light, dark, cupcake, bumblebee, emerald, corporate, synthwave, retro, cyberpunk, valentine, halloween, garden, forest, aqua, lofi, pastel, fantasy, wireframe, black, luxury, dracula, cmyk, autumn, business, acid, lemonade, night, coffee, winter, dim, nord, sunset

Users can change themes from the Settings page, and the preference is saved in localStorage.

## Development Notes

- The backend uses ES6 modules (type: "module" in package.json)
- The frontend uses Vite for fast development and building
- All API requests include credentials for cookie-based authentication
- Socket.IO connection is established automatically after successful authentication
- Profile pictures and message images are uploaded to Cloudinary
- Messages support both text and image content

## License

ISC

