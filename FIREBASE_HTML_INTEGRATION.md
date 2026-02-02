# Firebase HTML Integration Examples

## How to Update Your HTML Files

### Example 1: index.html

**BEFORE (Current):**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="styles/main.css">
    <!-- No Firebase -->
</head>
<body>
    <!-- HTML content -->
    
    <script src="js/data.js"></script>
    <script src="js/main.js"></script>
</body>
</html>
```

**AFTER (With Firebase):**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="styles/main.css">
    
    <!-- ADD FIREBASE SCRIPTS -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>
<body>
    <!-- HTML content -->
    
    <!-- ADD FIREBASE CONFIG & UTILS FIRST -->
    <script src="js/firebase-config.js"></script>
    <script src="js/firebase-utils.js"></script>
    
    <!-- THEN add existing scripts -->
    <script src="js/data.js"></script>
    <script src="js/main.js"></script>
</body>
</html>
```

### Example 2: admin/index.html

**BEFORE (Current):**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="../styles/admin.css">
    <!-- No Firebase -->
</head>
<body>
    <!-- Admin HTML -->
    
    <script src="../js/data.js"></script>
    <script src="../js/theater-admins.js"></script>
    <script src="../js/admin.js"></script>
</body>
</html>
```

**AFTER (With Firebase):**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="../styles/admin.css">
    
    <!-- ADD FIREBASE SCRIPTS -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>
<body>
    <!-- Admin HTML -->
    
    <!-- ADD FIREBASE CONFIG & UTILS FIRST -->
    <script src="../js/firebase-config.js"></script>
    <script src="../js/firebase-utils.js"></script>
    
    <!-- THEN add existing scripts -->
    <script src="../js/data.js"></script>
    <script src="../js/theater-admins.js"></script>
    <script src="../js/admin.js"></script>
</body>
</html>
```

### Example 3: booking.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="styles/booking.css">
    
    <!-- ADD FIREBASE SCRIPTS -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>
<body>
    <!-- Booking HTML -->
    
    <!-- ADD FIREBASE SCRIPTS FIRST -->
    <script src="js/firebase-config.js"></script>
    <script src="js/firebase-utils.js"></script>
    
    <!-- THEN other scripts -->
    <script src="js/data.js"></script>
    <script src="js/booking.js"></script>
</body>
</html>
```

## Files That Need Firebase Scripts

Update ALL of these by adding the 3 Firebase SDK scripts in `<head>` and 2 Firebase config scripts before other JS:

1. ✅ `index.html`
2. ✅ `admin/index.html`
3. ✅ `booking.html`
4. ✅ `movies.html`
5. ✅ `ticket.html`
6. ✅ `theater.html`
7. ✅ `snacks.html`
8. ✅ `parking.html`
9. ✅ `history.html`
10. ✅ `ott.html`
11. ✅ `updates.html`

## Quick Copy-Paste Template

Use this for all HTML files:

```html
<!DOCTYPE html>
<html>
<head>
    <!-- Your existing meta tags and styles -->
    
    <!-- PASTE THESE 3 LINES IN <head> -->
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-firestore.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-auth.js"></script>
</head>
<body>
    <!-- Your HTML content -->
    
    <!-- PASTE THESE 2 LINES FIRST, before other <script> tags -->
    <script src="js/firebase-config.js"></script>
    <script src="js/firebase-utils.js"></script>
    
    <!-- Your existing script tags go here -->
    <script src="js/data.js"></script>
    <!-- etc -->
</body>
</html>
```

## Path Notes

From `admin/index.html` (which is in a subdirectory), paths need `../`:
- Firebase config: `../js/firebase-config.js`
- Firebase utils: `../js/firebase-utils.js`

From root HTML files (index.html, etc.), use:
- Firebase config: `js/firebase-config.js`
- Firebase utils: `js/firebase-utils.js`

## Verification Checklist

After updating each HTML file:

- [ ] Firebase SDK scripts (3) are in `<head>`
- [ ] Firebase config script is before other JS
- [ ] Firebase utils script is before other JS
- [ ] Paths are correct (../ if in subdirectory)
- [ ] No duplicate script tags
- [ ] Page loads without console errors

## Test After Updates

1. Open page in browser
2. Press F12 (open console)
3. Look for: `"Firebase initialized successfully"`
4. Should NOT see errors like "Firebase is not defined"
5. Existing functionality should work unchanged

## If You See Errors

### Error: "Firebase is not defined"
**Fix:** Make sure Firebase SDK scripts are loaded BEFORE firebase-config.js

### Error: "db is not defined"
**Fix:** Make sure firebase-config.js is loaded BEFORE any function that uses `db`

### Error: "Failed to load resource"
**Fix:** Check that script paths are correct:
- From root: `js/firebase-config.js`
- From admin/: `../js/firebase-config.js`

## Optional: Minify for Production

For production, use minified versions:
```html
<script src="https://www.gstatic.com/firebasejs/10.7.0/firebase-app.min.js"></script>
```

But for development, non-minified is fine.

## Need Help?

1. Check browser console (F12) for specific errors
2. Verify file paths are correct
3. Make sure firebase-config.js has correct credentials
4. See FIREBASE_QUICK_START.md for more help

---

**That's it!** After updating all HTML files with Firebase scripts, your app will automatically start using Firebase for data storage.
