# Google Sheets & Email Setup Guide for Dr Roy Skincare

This guide will walk you through setting up Google Sheets to store reviews and Google Apps Script to handle email notifications.

---

## Part 1: Create Google Sheet for Reviews

### Step 1: Create a New Google Sheet
1. Go to [Google Sheets](https://sheets.google.com)
2. Click **+ Create** → **Blank Spreadsheet**
3. Name it: `Dr Roy Skincare Reviews`

### Step 2: Set Up Sheet Columns
Create the following column headers in Row 1:
- **A1:** `Timestamp`
- **B1:** `Name`
- **C1:** `Email`
- **D1:** `Rating`
- **E1:** `Review`
- **F1:** `Status` (Optional - for marking reviewed)

Your sheet should look like:
```
| Timestamp | Name | Email | Rating | Review | Status |
|-----------|------|-------|--------|--------|--------|
```

### Step 3: Share Sheet with Apps Script
- Click **Share** (top right)
- Get the **Sheet ID** from the URL: `https://docs.google.com/spreadsheets/d/{SHEET_ID}/edit`
- Save this ID - you'll need it later

---

## Part 2: Create Google Apps Script

### Step 1: Open Apps Script Editor
1. Go to [Google Apps Script](https://script.google.com)
2. Click **+ New Project**
3. Name it: `Dr Roy Skincare Reviews API`

### Step 2: Add the Code

Delete any default code and paste this complete script:

```javascript
// ==================== Configuration ====================
const SHEET_ID = 'YOUR_SHEET_ID_HERE'; // Replace with your Google Sheet ID
const SHEET_NAME = 'Sheet1'; // Default sheet name, change if needed
const ADMIN_EMAIL = 'drroyskincare@gmail.com'; // Your email for notifications

// ==================== Fetch All Reviews ====================
function doGet(e) {
  const action = e.parameter.action;
  
  try {
    if (action === 'getReviews') {
      return getReviews();
    } else {
      return sendError('Invalid action');
    }
  } catch (error) {
    return sendError('Error: ' + error.toString());
  }
}

function getReviews() {
  const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName(SHEET_NAME);
  const data = sheet.getDataRange().getValues();
  
  // Skip header row and format data
  const reviews = [];
  for (let i = 1; i < data.length; i++) {
    if (data[i][1]) { // Check if name exists
      reviews.push({
        timestamp: data[i][0],
        name: data[i][1],
        email: data[i][2],
        rating: parseInt(data[i][3]) || 0,
        text: data[i][4],
        status: data[i][5] || 'new'
      });
    }
  }
  
  // Return in reverse order (newest first)
  return ContentService
    .createTextOutput(JSON.stringify({
      success: true,
      reviews: reviews.reverse()
    }))
    .setMimeType(ContentService.MimeType.JSON);
}

// ==================== Add New Review ====================
function doPost(e) {
  const action = e.parameter.action;
  
  try {
    if (action === 'addReview') {
      const review = JSON.parse(e.postData.contents);
      return addReview(review);
    } else {
      return sendError('Invalid action');
    }
  } catch (error) {
    return sendError('Error: ' + error.toString());
  }
}

function addReview(review) {
  const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName(SHEET_NAME);
  const timestamp = new Date().toLocaleString();
  
  // Add new row with review data
  sheet.appendRow([
    timestamp,
    review.name,
    review.email,
    review.rating,
    review.text,
    'new'
  ]);
  
  // Send email notification to admin
  sendAdminNotification(review);
  
  return ContentService
    .createTextOutput(JSON.stringify({
      success: true,
      message: 'Review added successfully'
    }))
    .setMimeType(ContentService.MimeType.JSON);
}

// ==================== Send Admin Email Notification ====================
function sendAdminNotification(review) {
  try {
    const subject = `New Review: ${review.rating}⭐ from ${review.name}`;
    const body = `
New Review Submitted!

Name: ${review.name}
Email: ${review.email}
Rating: ${review.rating} Stars
Date: ${review.date}

Review:
"${review.text}"

---
Click here to view all reviews:
https://docs.google.com/spreadsheets/d/${SHEET_ID}/edit
    `;
    
    GmailApp.sendEmail(ADMIN_EMAIL, subject, body);
  } catch (error) {
    Logger.log('Error sending email: ' + error);
  }
}

// ==================== Error Response ====================
function sendError(message) {
  return ContentService
    .createTextOutput(JSON.stringify({
      success: false,
      error: message
    }))
    .setMimeType(ContentService.MimeType.JSON);
}

// ==================== Test Function ====================
function testAddReview() {
  const testReview = {
    name: "Test User",
    email: "test@example.com",
    rating: 5,
    text: "This is a test review",
    date: new Date().toLocaleDateString()
  };
  
  addReview(testReview);
  Logger.log("Test review added successfully!");
}
```

### Step 3: Replace Sheet ID
1. Find this line: `const SHEET_ID = 'YOUR_SHEET_ID_HERE';`
2. Replace with your actual Google Sheet ID from Part 1, Step 3

### Step 4: Update Admin Email
1. Find this line: `const ADMIN_EMAIL = 'drroyskincare@gmail.com';`
2. Replace with your actual email address

---

## Part 3: Deploy Apps Script

### Step 1: Deploy as Web App
1. Click **Deploy** (top right) → **New Deployment**
2. Select deployment type: **Web app**
3. Configure:
   - Execute as: **Your email**
   - Who has access: **Anyone**
4. Click **Deploy**
5. Click **Authorize** and grant permissions

### Step 2: Copy Deployment URL
1. After deployment, you'll see a URL like:
   ```
   https://script.google.com/macros/d/YOUR_DEPLOYMENT_ID/useTrusted
   ```
2. Copy the **entire URL** - you'll need this for the website

### Step 3: Save Your Deployment ID
Extract the ID from the URL (the part between `/d/` and `/useTrusted`):
```
YOUR_DEPLOYMENT_ID = abc123xyz...
```

---

## Part 4: Update Your Website

### Find the Google Sheets URL in your code:
Locate this line in `index.html` (around line 1665):
```javascript
const GOOGLE_SHEETS_URL = 'https://script.google.com/macros/d/YOUR_DEPLOYMENT_ID/useTrusted';
```

### Replace with your deployment URL:
```javascript
const GOOGLE_SHEETS_URL = 'https://script.google.com/macros/d/YOUR_ACTUAL_DEPLOYMENT_ID/useTrusted';
```

---

## Part 5: Test the Setup

### Test 1: Submit a Review via Website
1. Open your website
2. Go to "Share Your Experience" section
3. Fill in all fields and submit
4. You should see a success message

### Test 2: Check Google Sheet
1. Open your Google Sheet
2. A new row should appear with the review data
3. You should receive an email notification

### Test 3: Check Carousel Display
1. Refresh the website
2. New review should appear in the carousel

---

## Troubleshooting

### Reviews not appearing?
1. Check browser console (F12) for JavaScript errors
2. Verify GOOGLE_SHEETS_URL is correct
3. Ensure Google Sheet has proper column headers
4. Check that Apps Script was deployed as "Anyone" access

### Emails not sending?
1. Verify ADMIN_EMAIL is correct in Apps Script
2. Check Apps Script execution permissions
3. Look in Google Sheet tab to see if data was saved
4. Check Gmail spam folder

### CORS/Permission Errors?
1. Re-deploy Apps Script (Deploy → New Deployment)
2. Ensure "Who has access" is set to "Anyone"
3. Clear browser cache and try again

### Reviews not loading on page load?
1. The script uses `fetchReviewsFromSheet()` on page load
2. Check browser Network tab for failed API calls
3. Verify the deployment URL is accessible

---

## Important Security Notes

⚠️ **This setup uses ANYONE access for security reasons:**
- Data is read-only for public viewers
- Only Apps Script can write new reviews
- Verify email is added to spam filter to prevent loss of notifications

### To add extra security later:
1. Implement a CAPTCHA on the form
2. Add rate limiting to prevent spam
3. Require email verification before publishing

---

## Google Sheets URL Reference

Keep these IDs in a safe place:
- **Sheet ID:** `_____________________________`
- **Deployment ID:** `_____________________________`
- **Admin Email:** `_____________________________`

---

## Need Help?

If reviews aren't syncing:
1. Check Apps Script logs (Apps Script console → Execution log)
2. Run the `testAddReview()` function to test manually
3. Verify your deployment URL is correct

For email issues:
1. Check that ADMIN_EMAIL matches a valid Gmail account
2. Apps Script needs permission to send emails
3. Gmail may require "Less secure app access" if using custom domain

---

## Next Steps

✅ Create Google Sheet
✅ Set up Apps Script
✅ Deploy as Web App
✅ Update website URL
✅ Test submission
✅ Verify email notifications

Once everything is working:
- Your reviews will sync across all devices
- New reviews appear immediately on the website
- You get email notifications for each submission
- Data is safely stored in Google Sheets
