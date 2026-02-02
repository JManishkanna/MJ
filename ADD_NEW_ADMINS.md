# How to Add New Theater Admins

This guide explains how to add new theater admin accounts to the system.

## Method 1: Programmatically (JavaScript Console)

Open your browser's Developer Console (F12 or Right-click → Inspect → Console) and run:

### Add a New Theater Admin

```javascript
// Example: Add a new theater admin
const newAdmin = addTheaterAdmin(
  'newtheater@email.com',      // Email
  'securepassword123',          // Password
  'New Theater Name',           // Theater Name
  4                             // Theater ID
);

console.log('New admin created:', newAdmin);
// Output: {adminId: 'ADM004', theaterId: 4, email: 'newtheater@email.com', ...}
```

### Verify All Theater Admins

```javascript
// Get all theater admins
const allAdmins = getTheaterAdmins();
console.table(allAdmins);

// Output will show all admin accounts with their details
```

## Method 2: Direct Modification (Advanced)

If you need to modify the `localStorage` directly:

```javascript
// Get current admins
const admins = JSON.parse(localStorage.getItem('theaterAdmins'));

// Add new admin
admins.push({
  adminId: 'ADM004',
  theaterId: 4,
  email: 'newtheater@email.com',
  password: 'securepassword123',
  theaterName: 'New Theater Name',
  createdAt: new Date().toISOString()
});

// Save back to localStorage
localStorage.setItem('theaterAdmins', JSON.stringify(admins));

console.log('Admin added successfully!');
```

## Method 3: Adding Multiple Admins at Once

```javascript
// Array of new admins to add
const newAdminsToAdd = [
  {
    adminId: 'ADM004',
    theaterId: 4,
    email: 'theater4@email.com',
    password: 'password1',
    theaterName: 'Theater 4'
  },
  {
    adminId: 'ADM005',
    theaterId: 5,
    email: 'theater5@email.com',
    password: 'password2',
    theaterName: 'Theater 5'
  }
];

const admins = getTheaterAdmins();
admins.push(...newAdminsToAdd);
localStorage.setItem('theaterAdmins', JSON.stringify(admins));

console.log('Multiple admins added!');
```

## Current Theater Admins

Here are the pre-configured theater admins:

| Admin ID | Theater Name | Email | Password |
|----------|--------------|-------|----------|
| ADM001 | Cineplex Grand | cineplex@theater.com | theater123 |
| ADM002 | Star Cinema | star@theater.com | theater123 |
| ADM003 | Galaxy Theaters | galaxy@theater.com | theater123 |

## Adding Admin with Backend API (Future)

Once you implement backend:

```javascript
// This is how it would work with a backend API
async function addTheaterAdminViaAPI(adminData) {
  try {
    const response = await fetch('/api/admin/create', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(adminData)
    });
    
    const result = await response.json();
    console.log('Admin created:', result);
    return result;
  } catch (error) {
    console.error('Error creating admin:', error);
  }
}

// Usage:
addTheaterAdminViaAPI({
  email: 'newtheater@email.com',
  password: 'hashedPassword',
  theaterName: 'New Theater',
  theaterId: 4
});
```

## Admin ID Generation

The system automatically generates Admin IDs in the format `ADM###`:

```javascript
// How the system generates Admin IDs
function generateAdminId(adminCount) {
  return 'ADM' + String(adminCount + 1).padStart(3, '0');
}

// Examples:
// 1 admin → ADM001
// 2 admins → ADM002
// 3 admins → ADM003
// 10 admins → ADM010
// 100 admins → ADM100
```

## Verifying Admin Login

```javascript
// Test if credentials are correct
const email = 'cineplex@theater.com';
const password = 'theater123';

const admin = authenticateTheaterAdmin(email, password);

if (admin) {
  console.log('Login successful:', admin);
} else {
  console.log('Login failed: Invalid credentials');
}
```

## Resetting All Admins to Default

```javascript
// Reset to default state
localStorage.removeItem('theaterAdmins');

// Re-initialize with defaults
initializeTheaterAdmins();

console.log('All admins reset to default');
```

## Admin Registration Form (For Future Implementation)

Here's a template for an admin registration form:

```html
<!-- Admin Registration Form -->
<form id="adminRegistrationForm">
  <div class="form-group">
    <label for="adminEmail">Email:</label>
    <input type="email" id="adminEmail" name="email" required>
  </div>
  
  <div class="form-group">
    <label for="adminPassword">Password:</label>
    <input type="password" id="adminPassword" name="password" required>
  </div>
  
  <div class="form-group">
    <label for="theaterName">Theater Name:</label>
    <input type="text" id="theaterName" name="theaterName" required>
  </div>
  
  <div class="form-group">
    <label for="theaterId">Theater ID:</label>
    <input type="number" id="theaterId" name="theaterId" required>
  </div>
  
  <button type="submit">Register Admin</button>
</form>

<script>
document.getElementById('adminRegistrationForm').addEventListener('submit', function(e) {
  e.preventDefault();
  
  const formData = new FormData(this);
  
  const newAdmin = addTheaterAdmin(
    formData.get('email'),
    formData.get('password'),
    formData.get('theaterName'),
    parseInt(formData.get('theaterId'))
  );
  
  alert('Admin registered: ' + newAdmin.adminId);
  this.reset();
});
</script>
```

## Important Notes

⚠️ **Security Reminders:**

1. **Passwords in Production**
   - Never store plain-text passwords
   - Use hashing algorithms (bcrypt, Argon2)
   - Implement password requirements

2. **Admin Creation**
   - Only authorized users should create admins
   - Verify theater ownership before creating admin
   - Send welcome email with temporary credentials

3. **Admin ID Format**
   - Never duplicate Admin IDs
   - Keep format consistent (ADM###)
   - Store Admin ID with all actions for audit trail

4. **Data Validation**
   - Validate email format
   - Ensure unique email per theater
   - Validate theater ID exists

## API Endpoints (For Backend Integration)

```
POST /api/admin/register
  - Create new theater admin
  - Body: {email, password, theaterName, theaterId}
  - Returns: {adminId, ...}

GET /api/admin/list
  - Get all theater admins
  - Headers: {Authorization: token}
  - Returns: [{admin1}, {admin2}, ...]

POST /api/admin/verify
  - Verify admin credentials
  - Body: {email, password}
  - Returns: {adminId, theaterId, ...}

PUT /api/admin/{adminId}
  - Update admin details
  - Headers: {Authorization: token}
  - Body: {email, theaterName, ...}

DELETE /api/admin/{adminId}
  - Delete admin account
  - Headers: {Authorization: token}
```

## Database Schema (For Backend)

```sql
-- Theater Admins Table
CREATE TABLE theater_admins (
  id INT PRIMARY KEY AUTO_INCREMENT,
  admin_id VARCHAR(10) UNIQUE NOT NULL,
  theater_id INT NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  theater_name VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  is_active BOOLEAN DEFAULT TRUE,
  FOREIGN KEY (theater_id) REFERENCES theaters(id)
);

-- Admin Activity Log Table
CREATE TABLE admin_activity_log (
  id INT PRIMARY KEY AUTO_INCREMENT,
  admin_id VARCHAR(10) NOT NULL,
  action VARCHAR(100) NOT NULL,
  details JSON,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (admin_id) REFERENCES theater_admins(admin_id)
);
```

---

**Need help?** Refer to the main Theater Admin System documentation or contact support.
