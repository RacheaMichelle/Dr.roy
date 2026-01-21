# Quick Setup Checklist ✅

## Google Sheets & Apps Script Setup

### ⭐ **STEP 1: Create Google Sheet** (2 minutes)
- [ ] Go to https://sheets.google.com
- [ ] Click **+ Create** → Blank Spreadsheet
- [ ] Name it: `Dr Roy Skincare Reviews`
- [ ] Add headers: `Timestamp`, `Name`, `Email`, `Rating`, `Review`, `Status`
- [ ] **Copy Sheet ID** from URL: `https://docs.google.com/spreadsheets/d/{SHEET_ID}/edit`

**Your Sheet ID:** ___________________________________

---

### ⭐ **STEP 2: Create & Deploy Apps Script** (5 minutes)
- [ ] Go to https://script.google.com
- [ ] Click **+ New Project**
- [ ] Name it: `Dr Roy Skincare Reviews API`
- [ ] Delete default code
- [ ] **Paste the full code from GOOGLE_SHEETS_SETUP.md** (the Apps Script section)
- [ ] Replace `'YOUR_SHEET_ID_HERE'` with your actual Sheet ID
- [ ] Replace `'drroyskincare@gmail.com'` with your email
- [ ] Click **Deploy** → **New Deployment**
  - Type: **Web app**
  - Execute as: **Your email**
  - Access: **Anyone**
- [ ] Click **Authorize** and grant permissions
- [ ] **Copy the deployment URL**

**Your Deployment URL:** 
```
https://script.google.com/macros/d/YOUR_DEPLOYMENT_ID/useTrusted
```

**Your Deployment ID:** ___________________________________

---

### ⭐ **STEP 3: Update Website** (1 minute)
- [ ] Open `index.html` in your editor
- [ ] Find line ~1682: `const GOOGLE_SHEETS_URL = '...'`
- [ ] Replace the entire URL with your deployment URL
- [ ] Save the file

**Before:**
```javascript
const GOOGLE_SHEETS_URL = 'https://script.google.com/macros/d/YOUR_DEPLOYMENT_ID/useTrusted';
```

**After:**
```javascript
const GOOGLE_SHEETS_URL = 'https://script.google.com/macros/d/YOUR_ACTUAL_ID_12345xyz/useTrusted';
```

---

### ⭐ **STEP 4: Test Everything** (5 minutes)
- [ ] Open your website in browser
- [ ] Scroll to "Share Your Experience" form
- [ ] Fill in all fields:
  - Name: (your name)
  - Email: (your email)
  - Rating: Click 5 stars
  - Review: Write something nice
- [ ] Click **Submit Your Review**
- [ ] Should see green success message
- [ ] Check your Google Sheet → new row appeared ✅
- [ ] Check your email → received notification ✅
- [ ] Refresh website → review appears in carousel ✅

---

## 📋 Your Configuration Summary

| Item | Value |
|------|-------|
| Google Sheet ID | ___________________ |
| Deployment ID | ___________________ |
| Admin Email | ___________________ |
| Deployment URL | ___________________ |
| Website File | index.html |
| Line to Update | ~1682 |

---

## 🔧 If Something Doesn't Work

**Reviews not appearing?**
1. Check the Deployment URL is correct in index.html
2. Open browser DevTools (F12) → Console
3. Look for any red error messages
4. Refresh the page

**Emails not sending?**
1. Go to your Apps Script project
2. Click the **Run** button in the editor to test manually
3. Check the Execution log for errors
4. Verify admin email is correct

**Can't deploy?**
1. Make sure you're logged into your Google account
2. Authorization popup may appear - grant all permissions
3. Try deploying again

---

## 📚 Full Documentation

For detailed steps, see: **GOOGLE_SHEETS_SETUP.md**

---

## ✨ What You've Built

✅ **Real-time Review System**
- Reviews stored in Google Sheets
- Available on ANY device
- Instant email notifications
- Professional carousel display

✅ **Email Notifications**
- Admin gets notified of each review
- Email contains full review details
- Link to Google Sheet for management

✅ **Global Sync**
- Changes in Google Sheet appear on website
- New submissions instantly saved
- No server costs (using Google's infrastructure)

---

**Questions?** Check the troubleshooting section in GOOGLE_SHEETS_SETUP.md
