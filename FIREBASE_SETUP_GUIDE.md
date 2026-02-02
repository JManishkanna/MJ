# Firebase Setup Guide for CineBook

## Overview
This guide helps you set up Firebase (Firestore) to replace localStorage for persistent data storage.

## Step 1: Create Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Create a new project"**
3. Project name: `CineBook` (or any name you prefer)
4. Click **Create Project**
5. Wait for project creation to complete

## Step 2: Create Firestore Database

1. In Firebase Console, go to **Build → Firestore Database**
2. Click **Create Database**
3. Choose location (closest to your users)
4. Start in **Production Mode** (for security rules)
5. Click **Create**

## Step 3: Set Security Rules

1. Go to **Firestore Database → Rules** tab
2. Replace the default rules with:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow public read access to all collections
    match /{document=**} {
      allow read: if true;
    }
    
    // Allow authenticated writes
    match /movies/{document=**} {
      allow write: if request.auth != null;
    }
    match /theater_movies/{document=**} {
      allow write: if request.auth != null;
    }
    match /bookings/{document=**} {
      allow write: if request.auth != null;
    }
    match /theater_snacks/{document=**} {
      allow write: if request.auth != null;
    }
    match /theater_parking/{document=**} {
      allow write: if request.auth != null;
    }
  }
}
```

3. Click **Publish**

## Step 4: Get Firebase Config

1. In Firebase Console, click the gear icon ⚙️
2. Go to **Project Settings**
3. Scroll down to **Your apps**
4. Click on the web app icon (or create new if needed)
5. Copy the **Firebase SDK snippet** (NOT the `<script>` tags)
6. It looks like:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY_HERE",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

## Step 5: Update Configuration File

1. Open `js/firebase-config.js`
2. Replace the `firebaseConfig` object with your actual credentials from Step 4
3. Save the file

Example:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyDemoKeyFromYourProject12345",
  authDomain: "cinebook-project.firebaseapp.com",
  projectId: "cinebook-project",
  storageBucket: "cinebook-project.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890ab"
};
```

## Step 6: Add Firebase to HTML Files

Add these script tags to the `<head>` section of:
- `index.html`
- `admin/index.html`
- `booking.html`
- `movies.html`
- `ticket.html`

```html
<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>

<!-- Firebase Configuration & Utilities -->
<script src="js/firebase-config.js"></script>
<script src="js/firebase-utils.js"></script>
```

## Step 7: Initialize Data on First Load

The Firebase utilities will automatically initialize the database with default data on first use. This happens in:
1. When the app first loads
2. When a user logs into admin panel
3. When data is accessed

To manually initialize, run in browser console:
```javascript
await initializeFirebaseData();
```

## Data Structure

Firebase Firestore collections:

### movies
- Default movies available in system
- Fields: id, title, genre, duration, rating, poster, backdrop, description, cast, director, releaseDate, trailer, showtimes, price

### theater_movies
- Movies added by theater admins
- Fields: Same as above + theaterId, addedAt

### theater_admins
- Admin user accounts
- Fields: adminId, theaterId, email, password, theaterName, createdAt

### bookings
- Ticket reservations
- Fields: id, movieId, movieTitle, theaterId, theaterName, seats, snacks, totalAmount, bookingDate, showtime, status

### theater_snacks
- Snacks added by theaters
- Fields: id, name, price, image, category, theaterId, addedAt

### theaters
- Cinema locations
- Fields: id, name, location, image, interior, facilities, movies

### snacks
- Default snacks
- Fields: id, name, price, image, category

### parking
- Parking rates per theater
- Fields: car (hourly rate, max hours), bike (hourly rate, max hours)

## Testing the Setup

1. **Add a Movie (Admin):**
   - Login to admin panel
   - Add a new movie
   - Check Firebase Console → Firestore Database to see data appear

2. **Book a Ticket:**
   - Go to Movies page
   - Select a movie
   - Complete booking
   - Check Firestore → bookings collection

3. **Check Real-time Updates:**
   - Open admin panel in one tab
   - Add a movie
   - Refresh another browser tab
   - New movie appears (real-time sync)

## Troubleshooting

### Error: "Firebase is not defined"
- Check that Firebase SDK script tags are added to HTML
- Verify script order: Firebase SDK → firebase-config.js → firebase-utils.js → data.js

### Error: "Missing or insufficient permissions"
- Check security rules in Firestore
- Make sure rules are published

### Data not appearing in Firestore
- Open browser console (F12)
- Check for error messages
- Verify Firebase credentials are correct

### Movies not loading
```javascript
// Debug in console:
const movies = await getMoviesFromFirebase();
console.log('Firebase movies:', movies);
```

## Next Steps

1. Complete Firebase setup with your own credentials
2. Update HTML files with Firebase scripts
3. Test adding movies and bookings
4. Monitor Firestore usage in Firebase Console

## FAQ

**Q: Will localStorage still work?**
A: Yes, the system maintains localStorage for session data (current admin), but uses Firebase for all persistent data.

**Q: Can I go back to localStorage?**
A: Yes, the original `data.js` functions still work. Just remove Firebase scripts.

**Q: Is my data secure?**
A: With the provided security rules, data is readable by all but writable only to authenticated users. For production, implement proper authentication.

**Q: How much does Firebase cost?**
A: Firestore has a generous free tier (1GB storage, 50k reads/day). See [Firebase Pricing](https://firebase.google.com/pricing)

## Support

For issues:
1. Check browser console for errors
2. Verify Firebase config credentials
3. Check Firestore security rules
4. Review Firebase documentation: https://firebase.google.com/docs
