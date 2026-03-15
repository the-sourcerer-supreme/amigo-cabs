# Amigo Cab – Cab Sharing App

Amigo Cabs is a real-time cab-sharing platform that allows nearby passengers traveling in similar directions to share rides. The system reduces travel costs, minimizes traffic congestion, and improves urban mobility by efficiently matching riders using live location data.

## Problem Statement

Urban commuters often travel similar routes but still book separate rides, which leads to:

- **Increased travel costs**

- **Traffic congestion**

- **Inefficient vehicle usage**

- **Higher carbon emissions**

Amigo Cabs solves this problem by enabling nearby passengers to share rides in real time.

## Key Features

- **Real-Time Ride Matching**

  - Passengers can create or join rides based on nearby users traveling in similar directions.

- **Live Socket Communication**

  - Real-time updates for ride invitations, passenger joins, location updates, and ride status using Socket.IO.

- **Location-Based Rider Discovery**

  - Nearby riders are identified based on geographic proximity.

- **Group Ride Coordination**

  - Passengers confirm readiness before a driver is assigned.

- **Ride History**

  - Users can view completed rides and ride details.

- **Secure Authentication**

  - User registration and login with password hashing.

## Architecture Overview

### Tech Stack

- **Mobile app**
  - React Native `0.81.x`
  - React Navigation (stack + bottom tabs)
  - React Native Paper + Vector Icons for theming and components
  - React Native Maps + Google Places/Directions APIs
  - Socket.io client for realtime updates

- **Backend**
  - Node.js (requires **Node 20+**)
  - Express for HTTP APIs
  - Socket.io for realtime events
  - MongoDB via Mongoose for persistence
  - bcryptjs for password hashing

## Running the Project Locally

### Prerequisites

- Node.js **20+**
- npm (or yarn)
- MongoDB instance (local or hosted)
- Android Studio and/or Xcode (for running the app on emulator/physical device)
- A Google Cloud project with:
  - Maps SDK / Places API / Directions API enabled
  - A browser/Android/iOS key (see **Environment & API Keys** below)

### 1. Clone and install

```bash
git clone git remote set-url origin https://github.com/the-sourcerer-supreme/amigo-cabs.git
cd amigo-cabs

# Install mobile app dependencies
npm install

# Install server dependencies
cd server
npm install
cd ..
```

### 2. Configure backend environment

Create a `.env` file inside `server/`:

```bash
cd server
cp .env.example .env  # if present, otherwise create manually
```

Set values like:

```env
PORT=4000
MONGODB_URI=mongodb://localhost:27017/amigo-cab
CORS_ORIGIN=http://localhost:19006
NODE_ENV=development
```

Start the backend:

```bash
cd server
npm start
```

The server will listen on `http://localhost:4000` by default.

### 3. Configure API base URL for the app

The mobile app talks to the backend through `services/api.ts` and `services/socket.ts`. The base URL logic is:

- Use `process.env.API_BASE_URL` if provided.
- Otherwise:
  - In dev:
    - Android default: currently points to a hosted demo (`https://amigo-cabs.onrender.com`).
    - iOS default: `http://localhost:4000`.
  - In production: `https://amigo-cabs.onrender.com`.

For local development, you will usually want:

- iOS simulator: `API_BASE_URL=http://localhost:4000`
- Android emulator: `API_BASE_URL=http://10.0.2.2:4000`
- Physical device: `API_BASE_URL=http://<YOUR_COMPUTER_LAN_IP>:4000`

You can either:

- Define `API_BASE_URL` in your React Native environment setup, or
- Temporarily edit `services/api.ts` / `services/socket.ts` to hard‑code your local URL while developing.

### 4. Configure Google Maps / Places keys

The app uses a Google Maps API key for:

- Places Autocomplete (pickup/destination search)
- Directions API (route drawing and distance)

For production, you should:

- Store keys in a secure configuration (env or platform‑specific secrets).
- Restrict them by domain/app package.

For local testing, replace the placeholder key values where `GOOGLE_MAPS_KEY` is used in `SearchScreen.tsx` and `MatchScreen.tsx`.

### 5. Run the mobile app

From the project root:

```bash
# Start Metro bundler
npm start

# run Android
npx react-native run-android

# Or for iOS (on macOS with Xcode)
npm run ios
```

Make sure:

- The backend is running and reachable from your emulator/device.
- You have configured `API_BASE_URL` appropriately.

---

## API Endpoints

### Authentication
```
POST /auth/register
POST /auth/login
```
### User
```
PATCH /users/:userId
DELETE /users/:userId
```
### Location
```
POST /location
Ride History
GET /rides/history/:userId
```

## Project Structure

```
Amigo-cabs/
├── server/                 # Backend server
│   ├── models/            # MongoDB models (User, Ride)
│   ├── index.js           # Express server + Socket.io
│   └── package.json
├── screens/               # App screens
│   ├── AuthScreen.tsx    # Login/Register
│   ├── HomeScreen.tsx    # Main screen
│   ├── SearchScreen.tsx  # Search destinations
│   └── MatchScreen.tsx   # Ride matching
├── services/              # API and Socket services
│   ├── api.ts            # HTTP API client
│   └── socket.ts         # Socket.io client
├── contexts/             # React contexts
│   ├── AuthContext.tsx   # Authentication state
│   └── InviteContext.tsx # Ride invitations
└── components/           # Reusable components
```

## Future Improvements

- Real driver integration

- Payment gateway support

- Push notifications

- Ride route optimization

- Improved ride matching

- Driver mobile application

#Source Code Availability

The full source code for **Amigo Cabs** is not included in this repository.

This repository is intended to showcase the application's architecture, screenshots, and demo video for portfolio and evaluation purposes.

If you are a recruiter, collaborator, or reviewer interested in the complete implementation, please feel free to contact me.

