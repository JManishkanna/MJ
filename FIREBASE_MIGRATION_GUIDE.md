# Migration Guide: localStorage to Firebase

This guide shows how to update your application to use Firebase instead of localStorage.

## Files to Update

### 1. index.html
Add Firebase scripts to `<head>`:

```html
<head>
    <!-- ... existing styles ... -->
    
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>
```

Add Firebase scripts before other JS in `<body>` (before `<script src="js/main.js">`):

```html
<body>
    <!-- ... HTML content ... -->
    
    <!-- Firebase Configuration & Utilities -->
    <script src="js/firebase-config.js"></script>
    <script src="js/firebase-utils.js"></script>
    
    <!-- Other scripts -->
    <script src="js/data.js"></script>
    <script src="js/main.js"></script>
</body>
```

### 2. admin/index.html
Same Firebase scripts as index.html in `<head>` and before `admin.js`:

```html
<head>
    <!-- ... existing ... -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>

<body>
    <!-- ... HTML ... -->
    <script src="../js/firebase-config.js"></script>
    <script src="../js/firebase-utils.js"></script>
    <script src="../js/data.js"></script>
    <script src="../js/theater-admins.js"></script>
    <script src="../js/admin.js"></script>
</body>
```

### 3. booking.html, movies.html, ticket.html
Repeat the same Firebase script additions.

## Code Changes Needed

### Change 1: Initialize Firebase on App Load

**Before (localStorage):**
```javascript
function initializeApp() {
    initializeData();
    // ...
}
```

**After (Firebase):**
```javascript
async function initializeApp() {
    await initializeFirebaseData();
    // ...
}
```

### Change 2: Replace getMovies() calls

**Before (localStorage):**
```javascript
const movies = getMovies();
```

**After (Firebase):**
```javascript
const movies = await getMoviesFromFirebase();
```

Update all files:
- `js/main.js`
- `js/movies.js`
- `js/booking.js`
- `js/data.js`

### Change 3: Replace Theater Snacks

**Before (localStorage):**
```javascript
const snacks = getTheaterAdminSnacks(theaterId);
```

**After (Firebase):**
```javascript
const snacks = await getTheaterSnacksFromFirebase(theaterId);
```

### Change 4: Replace Bookings

**Before (localStorage):**
```javascript
const bookings = getTheaterBookings(theaterId);
```

**After (Firebase):**
```javascript
const bookings = await getTheaterBookingsFromFirebase(theaterId);
```

### Change 5: Add Movie to Theater

**Before (localStorage):**
```javascript
addMovieToTheater(theaterId, movieData);
```

**After (Firebase):**
```javascript
await addMovieToTheaterFirebase(theaterId, movieData);
```

### Change 6: Save Bookings

**Before (localStorage):**
```javascript
saveBooking(bookingData);
```

**After (Firebase):**
```javascript
await saveBookingToFirebase(bookingData);
```

## Step-by-Step Implementation

### Phase 1: Setup (Do First)
1. ✅ Create Firebase project (DONE - see FIREBASE_SETUP_GUIDE.md)
2. ✅ Add Firebase scripts to HTML files
3. ✅ Update firebase-config.js with your credentials
4. Test: Open console and verify no Firebase errors

### Phase 2: Update Main Functions
1. Update `initializeApp()` to use Firebase
2. Update `loadFeaturedMovies()` to use `getMoviesFromFirebase()`
3. Test: Movies should load from Firebase

### Phase 3: Update Admin Panel
1. Update admin login to use `authenticateTheaterAdminFirebase()`
2. Update movie addition to use `addMovieToTheaterFirebase()`
3. Update snack operations to use Firebase
4. Test: Add a movie and verify in Firestore

### Phase 4: Update Bookings
1. Update booking save to use `saveBookingToFirebase()`
2. Update booking retrieval to use `getTheaterBookingsFromFirebase()`
3. Test: Complete a booking and verify in Firestore

### Phase 5: Cleanup
1. Keep `data.js` for default data backup
2. Optional: Remove localStorage references once Firebase is stable
3. Test all features work with Firebase

## Hybrid Approach (Recommended for Transition)

Keep both localStorage and Firebase working:

```javascript
async function getMoviesHybrid() {
    // Try Firebase first
    try {
        return await getMoviesFromFirebase();
    } catch (error) {
        console.log('Firebase error, falling back to localStorage');
        // Fall back to localStorage
        return getMovies();
    }
}
```

This allows gradual migration without breaking functionality.

## Testing Checklist

- [ ] Firebase initializes without errors
- [ ] Default data loads from Firebase
- [ ] Movies display on homepage
- [ ] Admin can add movies
- [ ] New movies appear in Firestore
- [ ] Users can book tickets
- [ ] Bookings appear in Firestore
- [ ] Admin can view bookings
- [ ] Snacks can be added/managed
- [ ] All operations persist after page refresh

## Common Issues

### Issue: Async/Await Errors
**Solution:** Make sure parent functions are async:
```javascript
async function loadMovies() {
    const movies = await getMoviesFromFirebase();
    // ...
}
```

### Issue: CORS Errors
**Solution:** Firebase SDK handles this automatically, just ensure scripts are loaded in correct order.

### Issue: Data Not Syncing
**Solution:** Check:
1. Firebase credentials in firebase-config.js
2. Firestore security rules allow writes
3. Console for specific error messages

## Rollback Plan

If you need to rollback to localStorage:
1. Remove Firebase script tags from HTML
2. Remove `await` keywords from function calls
3. Use original `getMovies()`, `addMovieToTheater()`, etc. functions
4. All localStorage-based functions still work as before

## Performance Notes

- Firebase has built-in caching (offline persistence)
- Real-time updates mean no need to refresh
- Network requests are optimized automatically
- First load initializes database (~2-3 seconds)

## Next Steps

1. Follow FIREBASE_SETUP_GUIDE.md to create Firebase project
2. Update HTML files with Firebase scripts
3. Test Firebase connectivity (console should show no errors)
4. Implement Phase 1-5 changes above
5. Monitor Firestore usage in Firebase Console

Good luck with your Firebase integration! 🚀
