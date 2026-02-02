# Theater Admin System - Testing Guide

## Quick Testing Checklist

Use this checklist to verify all features are working correctly.

### ✅ Setup
- [ ] Open `admin/index.html` in browser
- [ ] Verify login page appears
- [ ] Check demo credentials are displayed

### ✅ Login & Authentication

#### Test Case 1: Valid Login
```
Email: cineplex@theater.com
Password: theater123
Expected: Dashboard loads, shows "Cineplex Grand" and "Admin ID: ADM001"
```
- [ ] Email field accepts input
- [ ] Password field masks characters
- [ ] Login button is clickable
- [ ] Valid credentials allow login
- [ ] Dashboard loads successfully
- [ ] Theater name displays correctly
- [ ] Admin ID displays correctly

#### Test Case 2: Invalid Credentials
```
Email: test@invalid.com
Password: wrongpass
Expected: Error alert "Invalid email or password"
```
- [ ] Error message appears
- [ ] Login page remains visible
- [ ] User can retry login

#### Test Case 3: Empty Fields
```
Leave fields empty and click Login
Expected: Browser validation prevents submission
```
- [ ] "required" validation triggers
- [ ] Error messages appear for empty fields

### ✅ Dashboard Stats

After logging in:
- [ ] Total Movies count displays (should be 0 initially)
- [ ] Theaters count shows 1
- [ ] Total Bookings count displays
- [ ] Revenue shows ₹0 (or actual value)
- [ ] Stats update when data changes

### ✅ Tab Navigation

- [ ] Click "Movies" tab → Movies section shows
- [ ] Click "Snacks" tab → Snacks section shows
- [ ] Click "Parking" tab (in Settings) → Parking settings show
- [ ] Click "Bookings" tab → Bookings table shows
- [ ] Click "Settings" tab → Settings section shows
- [ ] Active tab is highlighted

### ✅ Movie Management

#### Add Movie
```
1. Click Movies tab
2. Click "+ Add New Movie"
3. Fill in all fields:
   - Title: "Test Movie"
   - Genre: "Action"
   - Duration: "2h 30m"
   - Rating: "8.5"
   - Poster: "https://via.placeholder.com/400x600"
   - Backdrop: "https://via.placeholder.com/1920x1080"
   - Description: "Test movie description"
   - Price: "299"
4. Click "Add Movie"
```
- [ ] Modal appears
- [ ] All form fields display
- [ ] Form validation works
- [ ] Movie added successfully (alert shows)
- [ ] Modal closes after submission
- [ ] New movie appears in Movies list
- [ ] Dashboard stat updates (Total Movies increases)

#### View Movie Details
- [ ] Movie card displays with image
- [ ] Title shows correctly
- [ ] Rating displays (⭐ X.X/10)
- [ ] Duration shows (🕐)
- [ ] Genre shows (🏷️)
- [ ] Price shows (₹)
- [ ] Description preview shows

#### Delete Movie
```
1. Click delete button on any movie
2. Click OK in confirmation
```
- [ ] Confirmation dialog appears
- [ ] Movie removed from list
- [ ] Dashboard stat updates (decreases)
- [ ] Success message appears

### ✅ Snacks Management

#### Add Snack
```
1. Click Snacks tab
2. Click "+ Add New Snack"
3. Fill in prompts:
   - Name: "Large Popcorn"
   - Price: "199"
   - Category: "Popcorn"
   - Image URL: "https://via.placeholder.com/300"
```
- [ ] First prompt for snack name appears
- [ ] Price prompt appears
- [ ] Category prompt appears
- [ ] Image URL prompt appears
- [ ] Snack added successfully
- [ ] New snack appears in list
- [ ] Dashboard updates

#### View Snack Details
- [ ] Snack card displays with image
- [ ] Name shows correctly
- [ ] Category displays
- [ ] Price displays with ₹ symbol

#### Delete Snack
- [ ] Confirmation dialog appears
- [ ] Snack removed from list
- [ ] Success message appears

### ✅ Booking Management

#### View All Bookings
```
1. Click Bookings tab
2. Look at table
```
- [ ] Booking table displays
- [ ] Headers show: ID, Movie, Theater, Showtime, Seats, Total, Status, Actions
- [ ] Bookings display (if any exist)

#### Filter by Status
```
1. Select "Confirmed" from Status filter
2. Click or wait for update
```
- [ ] Only confirmed bookings show
- [ ] Changing status filter updates table

#### Filter by Date
```
1. Click date input
2. Select a date
```
- [ ] Calendar picker appears
- [ ] Bookings filter by selected date

#### View Booking Details
```
1. Click "View" button on any booking
```
- [ ] Alert displays booking information:
  - Booking ID
  - Movie Title
  - Theater Name
  - Showtime
  - Seats
  - Total Amount
  - Status

#### Cancel Booking
```
1. Click "Cancel" button
2. Confirm cancellation
```
- [ ] Confirmation dialog appears
- [ ] Booking status changes to "cancelled"
- [ ] Table updates to show new status

### ✅ Parking Management

#### View Current Settings
```
1. Click Settings tab
2. Look for "Parking Management" section
```
- [ ] Car Parking Price shows current value
- [ ] Bike Parking Price shows current value

#### Update Parking Prices
```
1. Enter new Car Price: "149"
2. Enter new Bike Price: "79"
3. Click "Update Parking"
```
- [ ] Input fields accept numbers
- [ ] Success message appears
- [ ] Values persist after page refresh

### ✅ Upcoming Movies

#### Add Upcoming Movie
```
1. Click Settings tab
2. Fill in "Updates Management" section:
   - Title: "Upcoming Movie Title"
   - Date: [Select future date]
3. Click "Add Upcoming Movie"
```
- [ ] Input fields display
- [ ] Date picker appears
- [ ] Success message appears
- [ ] Form clears after submission

### ✅ Logout

#### Logout Process
```
1. Click "Logout" button (top right)
2. Click OK in confirmation
```
- [ ] Confirmation dialog appears
- [ ] Redirected to login page
- [ ] All data preserved (can login again)
- [ ] Session cleared properly

### ✅ Responsive Design

#### Desktop (1920x1080)
- [ ] All elements display correctly
- [ ] No overflow issues
- [ ] Buttons are easily clickable

#### Tablet (768x1024)
- [ ] Layout adjusts properly
- [ ] Tab navigation remains functional
- [ ] Cards stack appropriately

#### Mobile (375x667)
- [ ] Login page is readable
- [ ] Dashboard fits screen
- [ ] Forms are usable
- [ ] Table data is visible (with scroll)

### ✅ Data Persistence

#### Test After Page Refresh
```
1. Add a movie
2. Note the movie details
3. Refresh page (F5)
4. Login again
```
- [ ] Movie still exists
- [ ] Dashboard stats match

#### Test After Logout/Login
```
1. Add a snack
2. Logout
3. Login again
4. Check Snacks tab
```
- [ ] Snack data persists
- [ ] Same user data shows

### ✅ Theater Isolation

#### Test with Different Admins
```
1. Login as Cineplex (cineplex@theater.com)
2. Add a movie
3. Logout
4. Login as Star Cinema (star@theater.com)
5. Check Movies tab
```
- [ ] Cineplex's movie does NOT appear in Star's dashboard
- [ ] Each theater has separate data
- [ ] No data leakage between theaters

### ✅ Error Handling

#### Try Invalid Inputs
```
1. Try adding movie with empty Title
2. Try negative price
3. Try invalid URL
```
- [ ] Form validation prevents submission
- [ ] Error messages are clear
- [ ] User can correct and resubmit

#### Try XSS/Injection Attacks
```
1. Add movie with title: <script>alert('xss')</script>
2. Check if script executes
```
- [ ] Script should NOT execute
- [ ] Text displays as-is
- [ ] System is protected

### ✅ Performance

#### Load Testing
```
1. Add 20+ movies to a theater
2. Add 20+ snacks
3. Load Bookings tab with 50+ bookings
```
- [ ] Dashboard loads within 2 seconds
- [ ] No freezing or lag
- [ ] Scroll remains smooth
- [ ] Filters respond quickly

### ✅ Browser Compatibility

Test on:
- [ ] Chrome/Chromium (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest)
- [ ] Edge (latest)

Each browser should:
- [ ] Display UI correctly
- [ ] Handle all interactions
- [ ] Support localStorage

### 🧪 Functional Test Script

Run this in browser console to test programmatically:

```javascript
// Test 1: Check if theater-admins.js is loaded
console.log('getTheaterAdmins:', typeof getTheaterAdmins);
console.log('addMovieToTheater:', typeof addMovieToTheater);

// Test 2: Get all admins
console.log('All admins:', getTheaterAdmins());

// Test 3: Try authentication
const admin = authenticateTheaterAdmin('cineplex@theater.com', 'theater123');
console.log('Authentication test:', admin ? 'PASS' : 'FAIL');

// Test 4: Add test movie
if (admin) {
  const movie = addMovieToTheater(admin.theaterId, {
    title: 'Test Movie',
    genre: 'Action',
    rating: 8.5,
    price: 299,
    duration: '2h 30m',
    poster: 'https://via.placeholder.com/400',
    backdrop: 'https://via.placeholder.com/1920',
    description: 'Test'
  });
  console.log('Movie added:', movie);
}

// Test 5: Get theater movies
const movies = getTheaterAdminMovies(1);
console.log('Theater 1 movies:', movies);

// Test 6: Check localStorage
console.log('Current session:', getCurrentAdmin());
```

## Test Data

### Sample Movie Data
```javascript
{
  title: "Avatar: The Way of Water",
  genre: "Sci-Fi/Adventure",
  duration: "3h 12m",
  rating: 7.6,
  poster: "https://via.placeholder.com/400x600",
  backdrop: "https://via.placeholder.com/1920x1080",
  description: "Jake Sully lives with his newfound family formed on the extrasolar moon Pandora.",
  price: 299
}
```

### Sample Snack Data
```javascript
{
  name: "Large Popcorn",
  price: 199,
  category: "Popcorn",
  image: "https://via.placeholder.com/300"
}
```

## Regression Test Checklist

After making any changes:

- [ ] Login works for all 3 demo admins
- [ ] Can add/delete movies
- [ ] Can add/delete snacks
- [ ] Can view bookings
- [ ] Can update parking prices
- [ ] Theater isolation works
- [ ] Data persists after refresh
- [ ] No console errors
- [ ] UI looks correct on desktop
- [ ] UI looks correct on mobile

## Bug Report Template

If you find an issue:

```
Title: [Brief description]

Steps to Reproduce:
1. 
2. 
3. 

Expected Result:
[What should happen]

Actual Result:
[What actually happened]

Browser:
[Chrome/Firefox/Safari/etc]

Screenshots:
[If applicable]

Console Errors:
[If applicable]
```

---

## Test Environments

### Development
```
URL: http://localhost/admin/
Data Storage: Browser localStorage
Database: None (mock data)
```

### Production (Future)
```
URL: https://cinebase.com/admin/
Data Storage: Backend Database
Database: MySQL/PostgreSQL
Authentication: JWT Tokens
```

---

**All tests passing? Great! The system is ready for deployment! 🎉**
