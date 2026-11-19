# Rent Flow - Property Management Application

A comprehensive rental management system with PWA support, cloud synchronization, and cross-device data access.

## Features Implemented

### 1. Payment Status Visuals
- **Pending/Due Bills**: Displayed in **RED** color
- **Paid Bills**: Displayed in **GREEN** color
- Status badges show clearly across all components (Dashboard, History, Bill Entry)

### 2. Cross-Platform Data Synchronization
- **Google Sheets Backend**: All data stored in centralized Google Sheets database
- **Cloud Sync**: Automatic synchronization every 30 seconds
- **Cross-Device Access**: Data saved on desktop is instantly accessible on mobile and vice versa
- **Single Database**: One central Google Sheets spreadsheet serves all devices

### 3. Desktop Mode Configuration
- **100% Zoom**: Default zoom level set to 100% for desktop browsers
- **Responsive Layout**: Optimized for desktop screen sizes
- **No Zoom Issues**: Prevented unwanted zoom behavior

### 4. Mobile Full-Screen Adjustment
- **PWA Support**: Installable as a mobile app
- **Full-Screen Display**: Automatically adjusts to mobile device screen size
- **Responsive Design**: CSS media queries ensure proper layout on all screen sizes
- **Touch-Optimized**: Mobile-friendly buttons and navigation
- **Viewport Configuration**: Proper viewport settings prevent scaling issues

### 5. Browser Tab Elements
- **Icon**: Rent Flow icon displayed in browser tab (favicon.ico)
- **Title**: "Rent Flow" displayed as tab title
- **Apple Touch Icon**: iOS home screen icon support
- **PWA Icons**: 192x192 and 512x512 icons for app installation

## Setup Instructions

### 1. Google Apps Script Setup

1. Create a new Google Sheet named "RentFlow Data"
2. Open **Extensions** → **Apps Script**
3. Copy the contents of `google-apps-script/code.gs` and paste into the script editor
4. Save the project
5. Run the `setupSheets()` function to initialize all database tables
6. Deploy as **Web App**:
   - Click **Deploy** → **New deployment**
   - Select type: **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - Click **Deploy**
7. Copy the deployment URL

### 2. Configure RentFlow App

1. Open the RentFlow application
2. Login with admin credentials:
   - Email: `admin@rentflow.com`
   - Password: `J0elroy1991$123`
3. Click on your profile icon → **App Settings**
4. Go to **Cloud Settings** tab
5. Paste your Google Apps Script URL
6. Click **Test Connection** to verify
7. Click **Save Settings**

### 3. Enable Cloud Sync

Cloud sync is now automatically enabled and will:
- Sync data every 30 seconds
- Work across all devices
- Store data persistently in Google Sheets
- Show sync status in the header

## Login Credentials

### Admin
- Email: `admin@rentflow.com`
- Password: `J0elroy1991$123` or `Admin@123$`

### Manager 1
- Email: `manager@rentflow.com`
- Password: `manager123`

### Manager 2
- Email: `manager2@rentflow.com`
- Password: `manager123`

### Tenant 1 (belongs to Manager 1)
- Email: `tenant@rentflow.com`
- Password: `tenant123`

### Tenant 2 (belongs to Manager 2)
- Email: `tenant2@rentflow.com`
- Password: `tenant123`

## Progressive Web App (PWA) Installation

### Desktop
1. Open RentFlow in Chrome, Edge, or another compatible browser
2. Click the install icon in the address bar
3. Click **Install** when prompted
4. The app will open as a standalone application

### Mobile (iOS)
1. Open RentFlow in Safari
2. Tap the **Share** button
3. Scroll and tap **Add to Home Screen**
4. Tap **Add**
5. The app icon will appear on your home screen

### Mobile (Android)
1. Open RentFlow in Chrome
2. Tap the menu icon (three dots)
3. Tap **Install app** or **Add to Home screen**
4. Tap **Install** when prompted
5. The app icon will appear on your home screen

## Key Features

- **Dashboard**: Overview of rooms, tenants, revenue, and pending payments
- **Admin Panel**: Manage rooms, renters, users, and permissions
- **Bill Entry**: Create monthly bills with electricity calculations
- **Bill History**: View and edit all bills with advanced filtering
- **Reports**: Analytics dashboard with charts and export functionality
- **Reminders**: Send payment reminders via email, SMS, or WhatsApp
- **Room Agreements**: Generate rental agreements with state selection
- **Support Chat**: Built-in support system with contact information
- **Role-Based Access**: Admin, Manager, and Tenant roles with permissions
- **Cloud Sync**: Real-time data synchronization across all devices
- **Offline Support**: Works without internet connection
- **PWA**: Install as native app on desktop and mobile

## Payment Status Colors

Throughout the application:
- ✅ **GREEN** = Paid bills (payment completed)
- ❌ **RED** = Pending/Due bills (payment pending)
- 🔴 **DARK RED** = Overdue bills (past due date)

## Technical Stack

- **Frontend**: Next.js 15.5.9, React, TypeScript
- **Styling**: Tailwind CSS, shadcn/ui components
- **Backend**: Google Apps Script + Google Sheets
- **PWA**: Service Workers, Web App Manifest
- **Storage**: Local Storage + Cloud Sync
- **Authentication**: Role-based access control

## Data Persistence

All data is stored in:
1. **Local Storage**: For offline access and fast loading
2. **Google Sheets**: For cloud backup and cross-device sync
3. **Automatic Sync**: Changes sync automatically every 30 seconds

When you update or redeploy on Vercel:
- ✅ Your data in Google Sheets remains intact
- ✅ No data loss on code updates
- ✅ Database separate from application code
- ✅ Data accessible from any device

## Browser Compatibility

- ✅ Chrome 90+
- ✅ Edge 90+
- ✅ Safari 14+
- ✅ Firefox 88+
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)

## Support

For issues or questions:
- **Phone**: +91 9876543210
- **Email**: support@rentflow.com
- **WhatsApp**: +91 9876543210 (Emergency)

## Development

\`\`\`bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start
\`\`\`

## Deployment

This app is optimized for deployment on Vercel:

1. Push your code to GitHub
2. Import project in Vercel
3. Deploy
4. Your data in Google Sheets will persist across all deployments

## License

Proprietary - All rights reserved
