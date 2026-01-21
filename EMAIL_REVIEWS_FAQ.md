# Dr Roy Skincare - Reviews & Email System FAQ

## 📧 Email System - How It Works

### **IMPORTANT: Setup Required!**
The email features will NOT work until you set up EmailJS. See the end of this document for setup instructions.

---

## 🌟 Reviews System

### **Question: Can other people see reviews?**
**Answer: YES! ✓**
- All reviews submitted on the website are **publicly visible** to other visitors
- Reviews appear immediately in the "Client Reviews & Ratings" section
- Reviews are stored in your browser's local storage (persistent)
- This builds trust and social proof for your clinic

### **Question: Does the doctor receive review notifications?**
**Answer: YES! ✓** (After EmailJS setup)
- When a client submits a review, the doctor receives an **email notification** to `drroyskincare@gmail.com`
- Email includes: Client name, rating (stars), review text, and email address
- This allows the doctor to respond to feedback and build relationships

### **Review Display Features:**
- ⭐ Star rating (1-5 stars)
- 💬 Review text from the client
- 📝 Client's name
- 📅 Date submitted
- 🎨 Beautiful card design with gradient backgrounds
- ✨ Hover animations for engagement

---

## 📧 Appointment Booking System

### **Question: Does the doctor receive appointment requests?**
**Answer: YES! ✓** (After EmailJS setup)
- When a client books an appointment, an **email is sent to the clinic**
- Email includes:
  - Client's name
  - Phone number
  - Preferred branch (Kampala or Mbarara)
  - Any additional message
- The clinic can then contact the client to confirm

### **Appointment Request Features:**
- 📝 Name field
- 📞 Phone number field
- 🏢 Branch selection dropdown
- 💬 Optional message field
- ✉️ Email icon shows it sends via email
- ✅ Success confirmation message

---

## ⚙️ How to Set Up Emails (CRITICAL!)

### **Step 1: Create EmailJS Account**
1. Visit [emailjs.com](https://emailjs.com)
2. Click **Sign Up**
3. Complete registration

### **Step 2: Get Your Public Key**
1. Log into EmailJS
2. Go to **Account → API Keys**
3. Copy your **Public Key**
4. In your HTML file, find this line (around line 2 of the script):
   ```javascript
   emailjs.init('YOUR_PUBLIC_KEY');
   ```
5. Replace with your actual key:
   ```javascript
   emailjs.init('a1b2c3d4e5f6g7h8i9');
   ```

### **Step 3: Set Up Email Service**
1. In EmailJS, go to **Email Services**
2. Click **Add New Service**
3. Choose your email provider (Gmail is easiest)
4. Follow the setup
5. Copy your **Service ID** (e.g., `service_abc123`)

### **Step 4: Create Email Templates**
You need 2 templates - one for appointments, one for reviews:

#### **Template 1: Appointment Notifications**
1. Go to **Email Templates** in EmailJS
2. Click **Create New Template**
3. Name it: `appointment_notification`
4. Set up the email:

**Subject:**
```
New Appointment Request from {{from_name}}
```

**Body:**
```
Dear Dr Roy,

You have received a new appointment request:

Client Name: {{from_name}}
Phone Number: {{from_email}}
Preferred Branch: {{branch}}
Message: {{message}}

Please contact them to confirm the appointment.

Thank you!
```

5. Copy the **Template ID** that appears (e.g., `template_abc123`)

#### **Template 2: Review Notifications**
1. Create another new template
2. Name it: `review_notification`
3. Set up the email:

**Subject:**
```
New Review Submitted - {{from_name}} ({{rating}} Stars)
```

**Body:**
```
Dear Dr Roy,

A new review has been submitted to your website:

Client Name: {{from_name}}
Email: {{from_email}}
Rating: {{rating}}/5 Stars

Review Text:
"{{review_text}}"

This review is now visible on your website.

Thank you!
```

5. Copy the **Template ID**

### **Step 5: Update HTML with IDs**
In your HTML file, find these lines (around 130-145):
```javascript
emailjs.send('service_YOUR_SERVICE_ID', 'template_YOUR_TEMPLATE_ID', templateParams)
```

Replace with your actual IDs (there are 2 locations - one for appointments, one for reviews):
```javascript
emailjs.send('service_abc123', 'template_xyz789', templateParams)
```

---

## 🧪 Testing

### **Test Review Submission:**
1. Go to the Reviews section on your website
2. Fill in the review form with test data
3. Click stars to rate
4. Click "Submit Review"
5. You should see a success message
6. Check your email for a notification (if EmailJS is set up)
7. Refresh the page - the review should still be there (localStorage)

### **Test Appointment Booking:**
1. Go to the Contact section
2. Fill in the appointment form
3. Click "Send Appointment Request"
4. You should see a success message
5. Check your email for the appointment notification (if EmailJS is set up)

---

## 💾 Local Storage - Reviews Are Saved Locally

- Reviews are stored in your **browser's local storage**
- This means reviews persist even after closing the browser
- Reviews are stored on each visitor's browser
- To clear reviews, users can clear their browser cache

### **To Migrate to Database (Future):**
To make reviews permanent across all users, you'd need:
- A backend server (Node.js, Python, etc.)
- A database (MongoDB, MySQL, etc.)
- An API to save/retrieve reviews

---

## 🔄 Flow Diagram

### **Review Submission Flow:**
```
User fills review form
        ↓
User clicks "Submit Review"
        ↓
Review saved to browser's localStorage
        ↓
Review displayed on website immediately
        ↓
Email sent to doctor (if EmailJS configured)
        ↓
Page refreshes and shows new review
```

### **Appointment Request Flow:**
```
User fills appointment form
        ↓
User clicks "Send Appointment Request"
        ↓
Email sent to clinic (if EmailJS configured)
        ↓
Success message shown to user
        ↓
Form cleared
        ↓
Doctor receives email with contact info
```

---

## ❓ Common Issues & Solutions

### **Problem: Emails not sending**
- ✓ Check EmailJS account is active
- ✓ Verify Service ID is correct
- ✓ Verify Template ID is correct
- ✓ Check email service is connected in EmailJS
- ✓ Look for errors in browser console (F12)

### **Problem: Reviews disappear after refresh**
- **This is normal if using localStorage**
- To keep reviews permanently, implement a database backend

### **Problem: Can't see review form**
- ✓ Make sure you're scrolled to the Reviews section
- ✓ Try in a different browser
- ✓ Clear browser cache
- ✓ Check JavaScript is enabled

---

## 🎯 Best Practices

1. **Regular Email Checks**: Check the email account daily for new appointments and reviews
2. **Respond Quickly**: Reply to appointment requests within 24 hours
3. **Engage with Reviews**: Thank clients for positive reviews
4. **Database Backup**: Plan to migrate to a database for long-term storage
5. **Monitor Ratings**: Keep track of average ratings and trends

---

## 📞 Support

- **EmailJS Docs**: https://www.emailjs.com/docs/
- **Browser Console Errors**: Press F12 to see detailed error messages
- **Contact Details**: Check if stored correctly in footer

---

**Last Updated**: January 2024
**Status**: Ready for production (after EmailJS setup)
