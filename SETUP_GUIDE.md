# Dr Roy Skincare Website - Setup Guide

## 🎉 Modernization Complete!

Your website has been upgraded with:
- ✅ **Modern, responsive design** with improved styling
- ✅ **Working reviews & ratings system** with persistent storage
- ✅ **EmailJS integration** for automated email sending
- ✅ **Functional appointment booking** with email notifications
- ✅ **Enhanced UX** with animations and interactive elements

---

## 📧 EmailJS Setup (REQUIRED)

To enable email sending for appointments and review notifications, you need to set up EmailJS:

### Step 1: Create EmailJS Account
1. Go to [https://www.emailjs.com/](https://www.emailjs.com/)
2. Click **Sign Up** (it's free!)
3. Create your account with email and password

### Step 2: Get Your Public Key
1. After login, go to **Account → API Keys**
2. Copy your **Public Key** (looks like: `a1b2c3d4e5f6g7h8i9`)
3. In `index.html`, find this line (around line 2):
   ```javascript
   emailjs.init('YOUR_PUBLIC_KEY');
   ```
4. Replace `'YOUR_PUBLIC_KEY'` with your actual public key:
   ```javascript
   emailjs.init('a1b2c3d4e5f6g7h8i9');
   ```

### Step 3: Create an Email Service
1. In EmailJS, go to **Email Services**
2. Click **Add New Service**
3. Select your email provider (Gmail, Outlook, or use EmailJS's default)
4. Follow the setup instructions
5. Copy your **Service ID** (looks like: `service_abc123def456`)
6. In `index.html`, find these lines (around lines 140-145):
   ```javascript
   emailjs.send('service_YOUR_SERVICE_ID', 'template_YOUR_TEMPLATE_ID', templateParams)
   ```
7. Replace `'service_YOUR_SERVICE_ID'` with your actual Service ID in ALL locations (there are 2):
   - One in the review form handler
   - One in the appointment form handler

### Step 4: Create Email Templates

#### Template 1: For Appointments
1. In EmailJS, go to **Email Templates**
2. Click **Create New Template**
3. Set **Template Name**: `appointment_notification`
4. Set **Service**: Select the service you created in Step 3
5. Copy this as your **Template ID** (looks like: `template_xyz789`)
6. In the email template, set up like this:

**Subject:**
```
New Appointment Request from {{from_name}}
```

**Email Body:**
```
New Appointment Request

Name: {{from_name}}
Phone: {{from_email}}
Preferred Branch: {{branch}}
Message: {{message}}

Please contact them to confirm the appointment.
```

7. Copy the **Template ID** and replace in `index.html` (around lines 140-145):
   ```javascript
   emailjs.send('service_abc123', 'template_YOUR_TEMPLATE_ID', templateParams)
   ```
   becomes:
   ```javascript
   emailjs.send('service_abc123', 'template_xyz789', templateParams)
   ```

#### Template 2: For Reviews
Create another template with:

**Template Name**: `review_notification`

**Subject:**
```
New Review Submitted - {{from_name}} ({{rating}} Stars)
```

**Email Body:**
```
New Client Review

Name: {{from_name}}
Email: {{from_email}}
Rating: {{rating}}/5 Stars
Review: {{review_text}}

This review has been automatically added to your website.
```

8. Copy this Template ID and use it in the same locations (lines 140-145)

---

## 🌟 Features Overview

### 1. **Reviews & Ratings**
- ⭐ Dynamic 5-star rating system
- 💾 Persistent storage using browser's localStorage
- 📊 Automatic average rating calculation
- 🎨 Modern card-based design
- 📱 Fully responsive

**To test reviews:**
1. Go to the Reviews section
2. Enter your name, email, and feedback
3. Click stars to rate
4. Submit your review
5. Review appears immediately and is saved locally

### 2. **Email Notifications**
- 📧 Automated appointment confirmations
- 📧 Review submission notifications to admin
- 🔄 Real-time email sending via EmailJS

### 3. **Appointment Booking**
- 📋 Modern form with branch selection
- ✉️ Automatic email to clinic
- ✅ Success confirmation to user
- 📱 Fully responsive design

---

## 🧪 Testing Checklist

- [ ] Reviews section displays correctly
- [ ] Can submit a review
- [ ] Stars light up when clicking
- [ ] Appointment form submits
- [ ] Receive email notification (after EmailJS setup)
- [ ] Website is responsive on mobile
- [ ] All navigation links work
- [ ] Smooth scrolling functions

---

## 📱 Mobile Responsiveness

The website is fully responsive with:
- Mobile hamburger menu
- Touch-friendly buttons
- Optimized spacing for small screens
- Adaptive grid layouts

---

## 🎨 Customization

### Change Colors
Edit these CSS variables (around line 11):
```css
:root {
    --primary: #2a6e8f;      /* Main blue */
    --secondary: #6db3c6;    /* Light blue */
    --accent: #ff7e8a;       /* Pink accent */
}
```

### Add More Reviews
Edit the initial reviews array in the script section or submit through the form.

### Update Contact Information
Search for the contact details in the HTML and update:
- Phone numbers
- Email addresses
- Branch locations
- Social media links

---

## 🔧 Troubleshooting

### Emails Not Sending?
1. Check EmailJS public key is correct
2. Verify Service ID is correct
3. Check Template IDs match
4. Ensure email service is activated in EmailJS

### Reviews Not Saving?
- Check browser allows localStorage
- Try a different browser
- Clear browser cache and try again

### Mobile Menu Not Working?
- Clear browser cache
- Try in incognito/private mode
- Check JavaScript is enabled

---

## 📞 Support

For EmailJS issues:
- Visit: https://www.emailjs.com/docs/
- Check your email service configuration

For website issues:
- Open browser console (F12) to see errors
- Check file paths for images

---

## 🚀 Next Steps

1. **Set up EmailJS** (most important!)
2. **Test appointment form** to ensure emails work
3. **Test review submission**
4. **Customize colors and images** to match branding
5. **Deploy website** to hosting

---

## 📝 Notes

- Reviews are stored in browser localStorage (local storage only)
- For production, consider migrating to a database
- Add your social media links in the footer
- Update all placeholder text with actual information

---

**Last Updated**: January 2024
**Version**: 2.0 (Modernized)
