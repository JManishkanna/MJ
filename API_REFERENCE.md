# Theater Admin System - API Reference

Complete API documentation for the Theater Admin System.

## Authentication Functions

### `initializeTheaterAdmins()`
Initializes default theater admin accounts and storage structures.

```javascript
initializeTheaterAdmins();
```

**Returns**: `void`  
**Side Effects**: Creates localStorage entries if they don't exist

**Example**:
```javascript
initializeTheaterAdmins();
// Creates 3 default admin accounts
```

---

### `authenticateTheaterAdmin(email, password)`
Verifies admin login credentials.

```javascript
const admin = authenticateTheaterAdmin(email, password);
```

**Parameters**:
- `email` (string): Admin email address
- `password` (string): Admin password

**Returns**: 
- Admin object if credentials valid: `{adminId, theaterId, email, password, theaterName, createdAt}`
- `null` if credentials invalid

**Example**:
```javascript
const admin = authenticateTheaterAdmin('cineplex@theater.com', 'theater123');
if (admin) {
  console.log('Login successful:', admin.adminId);
} else {
  console.log('Login failed');
}
```

---

### `getCurrentAdmin()`
Gets currently logged-in admin from session.

```javascript
const currentAdmin = getCurrentAdmin();
```

**Returns**:
- Admin object if logged in
- `null` if not logged in

**Example**:
```javascript
const admin = getCurrentAdmin();
if (admin) {
  console.log('Currently logged in as:', admin.theaterName);
}
```

---

### `setCurrentAdmin(admin)`
Creates a session for an admin user.

```javascript
setCurrentAdmin(admin);
```

**Parameters**:
- `admin` (object): Admin object to store in session

**Returns**: `void`

**Example**:
```javascript
const admin = authenticateTheaterAdmin(email, password);
setCurrentAdmin(admin);
```

---

### `clearCurrentAdmin()`
Logs out the current admin user.

```javascript
clearCurrentAdmin();
```

**Returns**: `void`  
**Side Effects**: Removes session from localStorage

**Example**:
```javascript
clearCurrentAdmin();
// Admin is logged out
```

---

### `addTheaterAdmin(email, password, theaterName, theaterId)`
Creates a new theater admin account.

```javascript
const newAdmin = addTheaterAdmin(email, password, theaterName, theaterId);
```

**Parameters**:
- `email` (string): Admin email
- `password` (string): Admin password
- `theaterName` (string): Theater name
- `theaterId` (number): Theater ID

**Returns**: New admin object with auto-generated `adminId`

**Example**:
```javascript
const admin = addTheaterAdmin(
  'newtheater@email.com',
  'secure123',
  'New Theater',
  4
);
console.log(admin.adminId); // 'ADM004'
```

---

### `getTheaterAdmins()`
Gets all theater admin accounts.

```javascript
const allAdmins = getTheaterAdmins();
```

**Returns**: Array of all admin objects

**Example**:
```javascript
const admins = getTheaterAdmins();
console.table(admins);
```

---

## Movie Management Functions

### `addMovieToTheater(theaterId, movieData)`
Adds a movie to a specific theater's inventory.

```javascript
const movie = addMovieToTheater(theaterId, movieData);
```

**Parameters**:
- `theaterId` (number): Theater ID
- `movieData` (object): Movie details
  - `title` (string): Movie title
  - `genre` (string): Movie genre
  - `duration` (string): Duration (e.g., "2h 30m")
  - `rating` (number): Rating (0-10)
  - `poster` (string): Poster image URL
  - `backdrop` (string): Backdrop image URL
  - `description` (string): Movie description
  - `price` (number): Ticket price in ₹

**Returns**: Created movie object with auto-generated `id`

**Example**:
```javascript
const movie = addMovieToTheater(1, {
  title: "Test Movie",
  genre: "Action",
  duration: "2h 30m",
  rating: 8.5,
  poster: "https://...",
  backdrop: "https://...",
  description: "Test description",
  price: 299
});
```

---

### `getTheaterAdminMovies(theaterId)`
Retrieves all movies for a theater.

```javascript
const movies = getTheaterAdminMovies(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: Array of movie objects for the theater

**Example**:
```javascript
const movies = getTheaterAdminMovies(1);
console.log(`Theater has ${movies.length} movies`);
```

---

### `removeMovieFromTheater(theaterId, movieId)`
Deletes a movie from a theater's inventory.

```javascript
const success = removeMovieFromTheater(theaterId, movieId);
```

**Parameters**:
- `theaterId` (number): Theater ID
- `movieId` (string): Movie ID to delete

**Returns**: `true` if successful, `false` if not found

**Example**:
```javascript
const removed = removeMovieFromTheater(1, 'TMOV1738528000000');
if (removed) {
  console.log('Movie deleted');
}
```

---

## Snack Management Functions

### `addSnackToTheater(theaterId, snackData)`
Adds a snack item to a theater's inventory.

```javascript
const snack = addSnackToTheater(theaterId, snackData);
```

**Parameters**:
- `theaterId` (number): Theater ID
- `snackData` (object): Snack details
  - `name` (string): Snack name
  - `price` (number): Price in ₹
  - `category` (string): Category (Popcorn, Drinks, Candy, Food, Desserts)
  - `image` (string): Image URL

**Returns**: Created snack object with auto-generated `id`

**Example**:
```javascript
const snack = addSnackToTheater(1, {
  name: "Large Popcorn",
  price: 199,
  category: "Popcorn",
  image: "https://..."
});
```

---

### `getTheaterAdminSnacks(theaterId)`
Retrieves all snacks for a theater.

```javascript
const snacks = getTheaterAdminSnacks(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: Array of snack objects

**Example**:
```javascript
const snacks = getTheaterAdminSnacks(1);
snacks.forEach(snack => {
  console.log(`${snack.name}: ₹${snack.price}`);
});
```

---

### `removeSnackFromTheater(theaterId, snackId)`
Deletes a snack from a theater's inventory.

```javascript
const success = removeSnackFromTheater(theaterId, snackId);
```

**Parameters**:
- `theaterId` (number): Theater ID
- `snackId` (string): Snack ID to delete

**Returns**: `true` if successful, `false` if not found

**Example**:
```javascript
const removed = removeSnackFromTheater(1, 'TSNK1738528000000');
```

---

## Parking Management Functions

### `getTheaterAdminParking(theaterId)`
Retrieves parking settings for a theater.

```javascript
const parking = getTheaterAdminParking(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: Parking object with car and bike settings

**Example**:
```javascript
const parking = getTheaterAdminParking(1);
console.log(`Car: ₹${parking.car.price}`);
console.log(`Bike: ₹${parking.bike.price}`);
```

**Returns Object Structure**:
```javascript
{
  car: {
    price: 99,
    total: 100,
    available: 85
  },
  bike: {
    price: 49,
    total: 50,
    available: 32
  }
}
```

---

### `updateTheaterAdminParking(theaterId, carPrice, bikePrice)`
Updates parking prices for a theater.

```javascript
const parking = updateTheaterAdminParking(theaterId, carPrice, bikePrice);
```

**Parameters**:
- `theaterId` (number): Theater ID
- `carPrice` (number): New car parking price in ₹
- `bikePrice` (number): New bike parking price in ₹

**Returns**: Updated parking object

**Example**:
```javascript
const parking = updateTheaterAdminParking(1, 149, 79);
console.log('Prices updated successfully');
```

---

## Booking Management Functions

### `getTheaterBookings(theaterId)`
Retrieves all bookings for a theater.

```javascript
const bookings = getTheaterBookings(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: Array of booking objects for the theater

**Example**:
```javascript
const bookings = getTheaterBookings(1);
console.log(`${bookings.length} bookings for this theater`);
```

**Booking Object Structure**:
```javascript
{
  id: "BK001",
  movieTitle: "Movie Name",
  theaterName: "Theater Name",
  theaterId: 1,
  showtime: "7:30 PM",
  seats: [1, 2, 3],
  totalAmount: 897,
  status: "confirmed",
  bookingDate: "2026-02-02"
}
```

---

### `getTheaterNameById(theaterId)`
Gets theater name by ID.

```javascript
const name = getTheaterNameById(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: Theater name string

**Example**:
```javascript
const name = getTheaterNameById(1);
console.log(name); // "Cineplex Grand"
```

---

## Dashboard Functions

### `loadTheaterDashboardStats(theaterId)`
Loads and displays dashboard statistics for a theater.

```javascript
loadTheaterDashboardStats(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: `void`  
**Side Effects**: Updates HTML elements with current stats

**Example**:
```javascript
loadTheaterDashboardStats(1);
// Updates #totalMovies, #totalTheaters, #totalBookings, #totalRevenue
```

---

### `loadTheaterMovies(theaterId)`
Loads and displays movies for a theater.

```javascript
loadTheaterMovies(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: `void`  
**Side Effects**: Updates #moviesList with movie cards

**Example**:
```javascript
loadTheaterMovies(1);
```

---

### `loadTheaterSnacks(theaterId)`
Loads and displays snacks for a theater.

```javascript
loadTheaterSnacks(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: `void`  
**Side Effects**: Updates #snacksList with snack cards

**Example**:
```javascript
loadTheaterSnacks(1);
```

---

### `loadTheaterBookings(theaterId)`
Loads and displays bookings for a theater.

```javascript
loadTheaterBookings(theaterId);
```

**Parameters**:
- `theaterId` (number): Theater ID

**Returns**: `void`  
**Side Effects**: Updates #bookingsTableBody with booking rows

**Example**:
```javascript
loadTheaterBookings(1);
```

---

## UI Functions

### `switchTab(tabId)`
Switches between dashboard tabs.

```javascript
switchTab(tabId);
```

**Parameters**:
- `tabId` (string): Tab ID (movies, snacks, bookings, settings)

**Returns**: `void`

**Example**:
```javascript
switchTab('movies');  // Show Movies tab
switchTab('bookings'); // Show Bookings tab
```

---

### `openAddMovieModal()`
Opens the add movie modal form.

```javascript
openAddMovieModal();
```

**Returns**: `void`

**Example**:
```javascript
openAddMovieModal();
```

---

### `closeModal(modalId)`
Closes a modal by ID.

```javascript
closeModal(modalId);
```

**Parameters**:
- `modalId` (string): Modal element ID

**Returns**: `void`

**Example**:
```javascript
closeModal('addMovieModal');
```

---

### `logoutAdminPanel()`
Logs out the current admin with confirmation.

```javascript
logoutAdminPanel();
```

**Returns**: `void`  
**Side Effects**: Shows confirmation dialog, clears session if confirmed

**Example**:
```javascript
logoutAdminPanel();
// Shows: "Are you sure you want to logout?"
```

---

## Booking Action Functions

### `viewBookingDetails(bookingId)`
Displays complete booking details.

```javascript
viewBookingDetails(bookingId);
```

**Parameters**:
- `bookingId` (string): Booking ID

**Returns**: `void`  
**Side Effects**: Shows alert with booking details

**Example**:
```javascript
viewBookingDetails('BK001');
```

---

### `cancelBooking(bookingId)`
Cancels a booking with confirmation.

```javascript
cancelBooking(bookingId);
```

**Parameters**:
- `bookingId` (string): Booking ID to cancel

**Returns**: `void`  
**Side Effects**: Changes status to "cancelled" and updates display

**Example**:
```javascript
cancelBooking('BK001');
// Shows: "Are you sure you want to cancel this booking?"
```

---

## Data Validation Functions

### Input Validation
```javascript
// Example validation checks performed:
- Email format validation
- Password length validation
- Number range validation
- URL format validation
- Required field validation
```

---

## localStorage Keys Reference

```javascript
{
  // Admin data
  "theaterAdmins": Array<AdminObject>,
  "currentTheaterAdmin": AdminObject | null,
  
  // Theater-specific data
  "theaterMovies": {
    "1": Array<MovieObject>,
    "2": Array<MovieObject>,
    "3": Array<MovieObject>
  },
  
  "theaterSnacks": {
    "1": Array<SnackObject>,
    "2": Array<SnackObject>,
    "3": Array<SnackObject>
  },
  
  "theaterParking": {
    "1": ParkingObject,
    "2": ParkingObject,
    "3": ParkingObject
  },
  
  // Updates
  "upcomingUpdates": Array<UpdateObject>
}
```

---

## Error Handling

### Common Errors and Recovery

```javascript
// Invalid login
if (!admin) {
  console.error('Authentication failed');
  showAlert('Invalid email or password');
}

// Movie not found
const movies = getTheaterAdminMovies(theaterId);
const movie = movies.find(m => m.id === movieId);
if (!movie) {
  console.error('Movie not found');
}

// Booking not found
const bookings = getTheaterBookings(theaterId);
const booking = bookings.find(b => b.id === bookingId);
if (!booking) {
  console.error('Booking not found');
}
```

---

## Performance Considerations

### Optimization Tips
```javascript
// Cache frequently accessed data
const movies = getTheaterAdminMovies(theaterId);
movies.forEach(movie => {
  // Process movies
});

// Batch updates
const theaterMovies = JSON.parse(localStorage.getItem('theaterMovies'));
// Make multiple changes
localStorage.setItem('theaterMovies', JSON.stringify(theaterMovies));

// Minimize re-renders
loadTheaterAdminDashboard(); // Updates all at once
```

---

## Migration Guide

### From Old Admin System
```javascript
// Old: Global admin
if (isAdmin()) { /* ... */ }

// New: Theater-specific admin
const admin = getCurrentAdmin();
if (admin) {
  const movies = getTheaterAdminMovies(admin.theaterId);
}
```

---

## Version History

**v1.0.0** - Initial Release
- Authentication system
- Movie management
- Snacks management
- Parking management
- Booking management

---

**API Reference Last Updated**: February 2, 2026
