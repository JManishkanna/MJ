# Booking Debug Guide

## Testing the Movie Booking Flow

This guide helps you debug why newly added movies might not be bookable.

### Step 1: Check Browser Console
1. Open your browser (Chrome, Firefox, Edge)
2. Press `F12` to open Developer Tools
3. Go to the **Console** tab
4. Open the admin panel and add a new movie
5. Watch the console for any errors

### Step 2: Test Data Storage
In the Console, run these commands to check if movie data is being stored:

```javascript
// Check if movies are in localStorage
console.log('Movies:', JSON.parse(localStorage.getItem('movies')));

// Check if theater movies are stored
console.log('Theater Movies:', JSON.parse(localStorage.getItem('theaterMovies')));

// Check what getMovies() returns
console.log('All Movies from getMovies():', getMovies());
```

### Step 3: Add a Test Movie
1. Go to Admin Panel
2. Login with: `cineplex@theater.com` / `theater123`
3. Click "Add Movie" button
4. Fill in ALL fields:
   - **Title**: "Test Movie 2026"
   - **Genre**: "Action"
   - **Duration**: "2h 30m"
   - **Rating**: "8.5"
   - **Cast**: "Actor1, Actor2, Actor3"
   - **Director**: "Test Director"
   - **Release Date**: Pick today or a future date
   - **Poster URL**: `https://images.unsplash.com/photo-1485846234645-a62644f84728?ixlib=rb-4.0.3&auto=format&fit=crop&w=800&q=80`
   - **Backdrop URL**: `https://images.unsplash.com/photo-1485846234645-a62644f84728?ixlib=rb-4.0.3&auto=format&fit=crop&w=1920&q=80`
   - **Trailer URL**: (leave empty or add a YouTube embed URL)
   - **Showtimes**: "10:00 AM, 1:30 PM, 5:00 PM, 8:30 PM"
   - **Description**: "A test movie description"
   - **Price**: "299"
4. Click "Add Movie"
5. You should see "Movie added successfully!" message

### Step 4: Verify Movie Appears on Movies Page
1. Go to **Movies** page
2. Look for your newly added movie
3. If you see it:
   - Check the "Book Now" button works
   - Click "Book Now"
4. If you DON'T see it:
   - Open console and check the output from Step 2

### Step 5: Booking Page Debugging
When you click "Book Now":
1. In Console, check logs for:
   - `Loading booking page with movieId: [ID]`
   - `Retrieved movie: [movie object]`
2. If you see `Movie not found` error:
   - Check that the movie ID is being passed correctly
   - Check that getMovieById() can find the movie

### Step 6: Critical Fields Check
If booking page loads but doesn't display properly, check that admin-added movie has these fields:
- `title` ✓
- `poster` ✓
- `backdrop` ✓
- `duration` ✓
- `rating` ✓
- `price` ✓
- `genre` ✓
- `description` ✓
- `cast` (array) ✓
- `director` ✓
- `releaseDate` ✓
- `showtimes` (array) ✓

### Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| Movie doesn't appear in Movies page | Verify all required fields are filled in the form |
| "Movie not found" error on booking page | Check browser console for movie ID mismatch |
| Booking page loads but looks broken | Check that cast and showtimes are properly comma-separated |
| Can see movie in admin dashboard but not on Movies page | Check localStorage keys in console |

### Console Debug Commands

```javascript
// Get the first newly added movie
const allMovies = getMovies();
const newMovie = allMovies.find(m => m.id && m.id.startsWith('TMOV'));
console.log('New movie:', newMovie);

// Try to book it manually
if (newMovie) {
    window.location.href = `booking.html?movie=${newMovie.id}`;
}

// Check specific movie by ID
const movie = getMovieById('TMOV1234567890');
console.log('Found movie:', movie);
```

### If Still Not Working

Please share in the console:
1. Output of `getMovies()` 
2. Output of `JSON.parse(localStorage.getItem('theaterMovies'))`
3. Any error messages shown in Console
4. Steps you took to add the movie
