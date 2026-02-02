# Theater Admin System - CineBook

## Overview

The Theater Admin System is a complete role-based administration panel that allows theater owners/managers to control their specific theater operations through the CineBook platform. Each theater has its own admin account with a unique Admin ID for tracking and management purposes.

## Features

### 1. **Authentication & Login**
- **Secure Login Page**: Theater admins must log in with their unique email and password
- **Demo Credentials** (Pre-configured):
  - **Cineplex Grand**: Email: `cineplex@theater.com` | Password: `theater123` | Admin ID: `ADM001`
  - **Star Cinema**: Email: `star@theater.com` | Password: `theater123` | Admin ID: `ADM002`
  - **Galaxy Theaters**: Email: `galaxy@theater.com` | Password: `theater123` | Admin ID: `ADM003`

### 2. **Dashboard**
- Real-time statistics displaying:
  - Total movies managed by the theater
  - Theater count (1 - their own theater)
  - Total bookings for the theater
  - Total revenue generated from bookings

### 3. **Movie Management**
- **Add Movies**: Theater admins can add new movies to their theater
- **Remove Movies**: Delete movies from their theater inventory
- **View Movie Details**: See ratings, duration, genre, price, and description
- Movies are stored separately for each theater

### 4. **Snacks Management**
- **Add Snacks**: Create snack items specific to their theater
- **Remove Snacks**: Delete snacks from inventory
- **Snack Categories**: Popcorn, Drinks, Candy, Food, Desserts
- Price management for each snack item

### 5. **Parking Management**
- **Separate Settings**: Each theater can set their own parking prices
- **Vehicle Types**:
  - Car Parking (with individual pricing)
  - Bike/Two-wheeler Parking (with individual pricing)
- **Real-time Updates**: Prices can be updated anytime

### 6. **Ticket Booking Management**
- **View Theater Bookings**: See all tickets booked for their theater
- **Booking Details**: 
  - Booking ID
  - Movie Title
  - Showtime
  - Number of Seats
  - Total Amount
  - Status (Confirmed, Completed, Cancelled)
- **Filter Options**:
  - Filter by Status (All, Confirmed, Completed, Cancelled)
  - Filter by Date
- **Booking Actions**:
  - View complete booking details
  - Cancel bookings (if needed)

### 7. **Settings & Updates**
- **Parking Price Management**: Adjust car and bike parking prices
- **Upcoming Movies**: Add upcoming movie announcements for their theater
- **Theater-specific Configuration**: All settings are isolated per theater

## File Structure

```
candy man/
├── admin/
│   └── index.html              # Main admin panel HTML
├── js/
│   ├── data.js                 # Shared data functions
│   ├── admin.js                # Theater admin dashboard logic
│   └── theater-admins.js       # Theater admin authentication & management
├── styles/
│   └── admin.css               # Admin panel styling (includes login page)
```

## Key Functions

### Authentication Functions (theater-admins.js)
- `initializeTheaterAdmins()` - Set up default theater admin accounts
- `authenticateTheaterAdmin(email, password)` - Verify login credentials
- `getCurrentAdmin()` - Get currently logged-in admin
- `setCurrentAdmin(admin)` - Store current admin session
- `clearCurrentAdmin()` - Logout function

### Movie Management Functions
- `addMovieToTheater(theaterId, movieData)` - Add movie to specific theater
- `removeMovieFromTheater(theaterId, movieId)` - Delete movie from theater
- `getTheaterAdminMovies(theaterId)` - Fetch all movies for a theater

### Snacks Management Functions
- `addSnackToTheater(theaterId, snackData)` - Add snack to theater
- `removeSnackFromTheater(theaterId, snackId)` - Delete snack from theater
- `getTheaterAdminSnacks(theaterId)` - Fetch all snacks for theater

### Parking Management Functions
- `updateTheaterAdminParking(theaterId, carPrice, bikePrice)` - Update parking prices
- `getTheaterAdminParking(theaterId)` - Get current parking prices

### Booking Management Functions
- `getTheaterBookings(theaterId)` - Get all bookings for a theater
- `filterTheaterBookings(theaterId)` - Filter bookings by status and date

## Data Storage

All data is stored in browser's `localStorage` with separate namespaces for each theater:

- `theaterAdmins` - Admin user accounts
- `theaterMovies` - Movies managed by each theater (format: `{theaterId: [movies]}`)
- `theaterSnacks` - Snacks for each theater (format: `{theaterId: [snacks]}`)
- `theaterParking` - Parking settings for each theater (format: `{theaterId: {car: {...}, bike: {...}}}`)
- `currentTheaterAdmin` - Currently logged-in admin session

## How to Use

### For Theater Admins:

1. **Access Admin Panel**: Navigate to `/admin/index.html`

2. **Login**: 
   - Enter email and password
   - Use demo credentials if available

3. **Manage Movies**:
   - Click "Movies" tab
   - Click "+ Add New Movie" button
   - Fill in movie details (title, genre, duration, rating, URLs, etc.)
   - Movies are now available for your theater

4. **Manage Snacks**:
   - Click "Snacks" tab
   - Click "+ Add New Snack" button
   - Enter snack details (name, price, category, image URL)
   - View and delete snacks as needed

5. **View Bookings**:
   - Click "Bookings" tab
   - Filter by status or date if needed
   - Click "View" to see booking details
   - Click "Cancel" to cancel a booking if necessary

6. **Update Settings**:
   - Click "Settings" tab
   - Update parking prices for cars and bikes
   - Add upcoming movie announcements
   - Changes are saved immediately

7. **Logout**: Click "Logout" button in header to exit

## Admin ID System

Each theater admin has a unique Admin ID:
- Format: `ADM###` (e.g., ADM001, ADM002, ADM003)
- Used for tracking all admin actions
- Visible in the admin panel header
- Associated with upcoming movie announcements and updates

## Security Notes

⚠️ **Production Implementation**: 
- Currently uses plain-text passwords for demo purposes
- Passwords should be hashed (e.g., bcrypt) in production
- Implement server-side authentication
- Use HTTPS for secure communication
- Implement role-based access control (RBAC)
- Add audit logging for all admin actions

## Theater Management Hierarchy

```
Theater Admin (ADM001) → Cineplex Grand (ID: 1)
  ├── Movies (Theater-specific)
  ├── Snacks (Theater-specific)
  ├── Parking Settings (Theater-specific)
  └── Bookings (Theater-specific)

Theater Admin (ADM002) → Star Cinema (ID: 2)
  ├── Movies (Theater-specific)
  ├── Snacks (Theater-specific)
  ├── Parking Settings (Theater-specific)
  └── Bookings (Theater-specific)

Theater Admin (ADM003) → Galaxy Theaters (ID: 3)
  ├── Movies (Theater-specific)
  ├── Snacks (Theater-specific)
  ├── Parking Settings (Theater-specific)
  └── Bookings (Theater-specific)
```

## Booking Management Details

### Booking Status Types:
- **Confirmed**: Booking is confirmed and valid
- **Completed**: Show has ended, booking is complete
- **Cancelled**: Booking has been cancelled by user or admin

### Booking Information Tracked:
- Booking ID
- Movie Title
- Theater Name
- Showtime
- Number of Seats
- Total Amount (₹)
- Booking Status
- Booking Date

## Future Enhancements

1. ✅ Add seat layout management
2. ✅ Implement real-time occupancy tracking
3. ✅ Add revenue analytics and reports
4. ✅ Implement showtime management
5. ✅ Add email notifications for bookings
6. ✅ Implement multi-language support
7. ✅ Add mobile app version
8. ✅ Implement backup and recovery system

## Troubleshooting

**Q: Why can't I login?**
A: Check your email and password. Make sure you're using the correct credentials for your theater.

**Q: How do I add movies to my theater?**
A: Click the "Movies" tab, then click "+ Add New Movie" and fill in the required details.

**Q: Can I see bookings from other theaters?**
A: No, you can only see bookings for your specific theater for security and data isolation.

**Q: How do I change parking prices?**
A: Go to the "Settings" tab, update the prices, and click "Update Parking".

**Q: What happens when I cancel a booking?**
A: The booking status changes to "cancelled" and the customer receives a notification (if email integration is enabled).

## Support

For issues or questions about the Theater Admin System, please contact the development team or refer to the main CineBook documentation.
