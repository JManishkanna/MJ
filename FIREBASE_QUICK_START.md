# Firebase Quick Start - 5 Minutes

## Fastest Way to Get Firebase Running

### Step 1: Create Firebase Project (2 min)
```
1. Go to firebase.google.com
2. Click "Get Started"
3. Click "Create a project"
4. Name: "CineBook"
5. Click "Create Project"
```

### Step 2: Create Firestore (1 min)
```
1. Click "Firestore Database" (under "Build")
2. Click "Create Database"
3. Select your region
4. Production mode → Create
```

### Step 3: Get Your Config (1 min)
```
1. Click ⚙️ (Settings icon)
2. Project Settings
3. Scroll to "SDK Setup and Configuration"
4. Copy the config object
```

### Step 4: Update Configuration (30 sec)
1. Open `js/firebase-config.js`
2. Replace the config with yours
3. Save file

### Step 5: Add Firebase to HTML (30 sec)

**Add to BOTH files in `<head>`:**

Add to: `index.html` and `admin/index.html`

```html
<head>
    <!-- Existing code... -->
    
    <!-- ADD THESE 3 LINES: -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>
```

**Add to BOTH files before closing `</body>`:**

Add BEFORE all other JavaScript includes:

```html
<body>
    <!-- Existing HTML... -->
    
    <!-- ADD THESE 2 LINES FIRST: -->
    <script src="js/firebase-config.js"></script>
    <script src="js/firebase-utils.js"></script>
    
    <!-- Then existing scripts: -->
    <script src="js/data.js"></script>
    <!-- etc -->
</body>
```

**Do the same for:**
- `booking.html`
- `movies.html`
- `ticket.html`

### Step 6: Test It!
1. Open browser console (F12 → Console)
2. Refresh page
3. Should see: `"Firebase initialized successfully"`
4. Add a movie in admin panel
5. Check [Firebase Console](https://console.firebase.google.com) → Firestore → theater_movies collection

## Common Issues

| Issue | Fix |
|-------|-----|
| "Firebase is not defined" | Add Firebase SDK scripts before firebase-config.js |
| "Cannot read property 'firestore' of undefined" | Check script load order: SDK → config → utils → data |
| Data not saving | Check Firestore security rules (should be published) |
| "Missing permissions" error | In Firestore Rules, change first match to `allow read, write: if true;` |

## Use As-Is WITHOUT Changes

The system works in hybrid mode:
- Tries Firebase first
- Falls back to localStorage if needed
- **No code changes required to existing files**
- Data automatically syncs to Firebase

Just add scripts and update config!

## Complete File Checklist

- ✅ `js/firebase-config.js` - **Update your credentials here only**
- ✅ `js/firebase-utils.js` - **No changes needed**
- ✅ `FIREBASE_SETUP_GUIDE.md` - Full guide with screenshots
- ✅ `FIREBASE_MIGRATION_GUIDE.md` - Step-by-step code changes

## Need Help?

1. **Check Firestore**: https://console.firebase.google.com
2. **Check Console Errors**: Press F12 in browser
3. **Read**: FIREBASE_SETUP_GUIDE.md for detailed instructions
4. **Test Command**: In console run: `await initializeFirebaseData();`

## What Gets Stored in Firebase

| Collection | Data |
|-----------|------|
| movies | Default movies (Avatar, Top Gun, Black Panther) |
| theaters | Cinema locations (Cineplex, Star, Galaxy) |
| theater_movies | Movies added by admins |
| theater_admins | Admin accounts |
| bookings | Ticket reservations |
| theater_snacks | Snacks added by theaters |
| snacks | Default snacks |
| parking | Parking rates |

## Done! 🎉

Your app now has a real database! Movies, bookings, and snacks are saved permanently and sync across all devices.

---

**Next:** Read FIREBASE_SETUP_GUIDE.md for production-level security setup.
