# RentFlow - Complete Setup Guide

## Overview

RentFlow is now a **fully-featured Progressive Web App (PWA)** with cloud synchronization, cross-device data sync, and offline support. This guide covers all features and setup instructions.

## Features Implemented

### 1. Progressive Web App (PWA) Support
- **Offline Functionality**: Full offline support with Service Worker caching
- **Mobile Installation**: Install as an app on iOS, Android, and desktop
- **Auto-Sync**: Automatic background synchronization every 30 seconds
- **Installable**: Add to home screen with app icon and splash screen

### 2. Cloud Synchronization (Google Apps Script)
- **Real-time Sync**: Data syncs automatically across all devices
- **Conflict Resolution**: Remote data wins on sync conflicts
- **Offline-first**: Works offline and syncs when reconnected
- **Multiple Data Types**: Rooms, renters, and bills all sync

### 3. Enhanced Bill Management
- **Bill Editing**: Edit bills directly from history view
- **Payment Tracking**: Mark bills as paid/pending/overdue
- **Payment Status Override**: Update payment status even after bill creation
- **Real-time Balance Calculation**: Automatic balance updates

### 4. Role-Based Access Control
- **Admin**: Full access to all data and features
- **Manager**: Access only to their own data
- **Tenant**: Access only to their own bills and agreements
- **Dynamic Permissions**: Admins can grant specific permissions per user

### 5. Bundle Size Optimization
- **SWC Minification**: Optimized JavaScript compression
- **Tree Shaking**: Unused code removal
- **Code Splitting**: Lazy loading of components
- **Production Build**: No source maps in production

---

## Getting Started

### Prerequisites
- Node.js 18+ and npm/yarn
- Google Account
- Modern browser with PWA support

### Step 1: Clone and Install

\`\`\`bash
git clone <your-repo-url>
cd rentflow
npm install
\`\`\`

### Step 2: Set Up Google Apps Script Backend

1. **Create Google Sheet**
   - Go to https://sheets.google.com/
   - Create a new sheet named "RentFlow Data"
   - Open Extensions → Apps Script

2. **Deploy Apps Script**
   - Copy the code from `google-apps-script/code.gs`
   - Paste it into the Apps Script editor
   - Run the `setupSheets()` function to initialize tables
   - Click Deploy → New deployment
   - Select type: Web app
   - Execute as: Me
   - Who has access: Anyone
   - Copy the deployment URL

3. **Configure App**
   - Add the Apps Script URL to your Vercel environment variables:
     - `NEXT_PUBLIC_GOOGLE_APPS_SCRIPT_URL`: Your deployment URL

### Step 3: Deploy to Vercel

\`\`\`bash
npm run build
git push  # Deploy to Vercel
\`\`\`

---

## Using Cloud Sync

### Enable Cloud Sync
1. Open App Settings (click user menu → App Settings)
2. Go to the "Cloud" tab
3. Enter your Google Apps Script URL
4. Click "Test Connection"
5. Data will auto-sync every 30 seconds

### Manual Sync
- Click the sync indicator in the top-right of the header
- Select "Sync Now" to force an immediate sync

### Offline Mode
- App works fully offline
- Changes are stored locally
- When reconnected, changes sync automatically
- Indicator shows "Offline" when network unavailable

---

## Cross-Device Synchronization

### How It Works
1. **Local Storage**: Data stored in browser's localStorage
2. **Cloud Backup**: Every 30 seconds, data syncs to Google Sheets
3. **Multi-Device**: Open the app on any device to see synced data
4. **Conflict Resolution**: Most recent data from cloud wins

### What Syncs
- All rooms and property information
- All renters and tenant data
- All bills and payment records
- App settings and preferences

### Testing Cross-Device Sync
1. Open RentFlow on Device A
2. Create a new bill/room/renter
3. Open RentFlow on Device B
4. Wait 30 seconds or click "Sync Now"
5. New data appears on Device B

---

## Mobile Installation

### iOS
1. Open RentFlow in Safari
2. Tap the Share button
3. Select "Add to Home Screen"
4. Name it and tap "Add"

### Android
1. Open RentFlow in Chrome
2. Tap the menu button (three dots)
3. Select "Install app"
4. Tap "Install"

### Desktop
1. Open RentFlow in Chrome/Edge
2. Click the install icon in address bar
3. Select "Install"

---

## Bill Editing & Payment Status

### Edit a Bill
1. Go to Bills History
2. Click "View & Edit" on any bill
3. Click "Edit Bill"
4. Update payment information:
   - Payment Received
   - Advance Adjustment
   - Status
5. Click "Calculate Balance" to auto-update
6. Click "Save & Sync" to save

### Mark Bill as Paid
1. Open bill in history
2. Click "Edit Bill"
3. Enter full payment amount
4. Status automatically changes to "Paid"
5. Save and sync

---

## Storage & Data Persistence

### Local Storage
- Data stored in browser's localStorage
- Persists across browser sessions
- Cleared only when explicitly deleted

### Vercel Deployment
- App code deploys to Vercel
- Data remains in Google Sheets
- Database completely separate from Vercel scripts
- Data persists even after app updates

### Backup & Export
1. Go to App Settings → Data tab
2. Click "Export All Data" to download JSON backup
3. Click "Import Data" to restore from backup

---

## Troubleshooting

### Sync Not Working
- Check internet connection
- Verify Apps Script URL in Cloud Settings
- Ensure Google Sheet script is deployed
- Check browser console for errors (F12)

### Data Not Appearing on Another Device
- Ensure cloud sync is enabled
- Click "Sync Now" button
- Verify Apps Script URL is correct
- Check that script has proper permissions

### PWA Not Installing
- Use Chrome, Edge, or Safari (iOS)
- Must be served over HTTPS (automatic on Vercel)
- Try a different browser if issues persist

### App Slow or Large Size
- Clear browser cache
- Disable unused notifications in App Settings
- Export and reimport data to clean up
- Update browser to latest version

---

## Admin Features

### Role-Based Access Control
1. Go to Admin Panel → Permissions
2. Select a user or manager
3. Grant/revoke specific permissions
4. Changes apply immediately

### Recycle Bin
1. Go to Admin Panel → Recycle Bin
2. View all deleted items
3. Click "Restore" to recover
4. Click "Permanently Delete" to remove forever

### Admin Content
1. Go to Admin Panel → Admin Content
2. Create announcements/notices visible only to admins
3. Set priority and expiration date
4. Managers and tenants won't see this content

---

## Login Credentials (Demo)

**Admin:**
- Email: `admin@rentflow.com`
- Password: `J0elroy1991$123` or `Admin@123$`

**Manager 1:**
- Email: `manager@rentflow.com`
- Password: `manager123`

**Manager 2:**
- Email: `manager2@rentflow.com`
- Password: `manager123`

**Tenant 1:**
- Email: `tenant@rentflow.com`
- Password: `tenant123`

---

## Performance Optimization

### Bundle Size Reduction
- Next.js SWC minification enabled
- Unused exports removed via tree-shaking
- Component lazy loading implemented
- Source maps disabled in production

### Caching Strategy
- Service Worker: Network-first with cache fallback
- Static assets: Long-term caching
- API responses: 30-second sync interval
- Local data: Browser localStorage

### Recommended Limits
- Maximum 10,000 bills per sheet
- Maximum 1,000 rooms per instance
- Maximum 5,000 renters per instance

---

## Security Considerations

### Data Protection
- Apps Script URL is public but requires authentication
- Google Sheets access controlled by script
- No sensitive data in URLs
- HTTPS enforced on Vercel

### Best Practices
- Change admin password regularly
- Don't share Apps Script deployment URL publicly
- Use strong passwords
- Enable 2FA on Google Account
- Audit user permissions regularly

---

## Support & Troubleshooting

### Getting Help
- Check App Settings → Cloud tab for status
- Review browser console logs (F12)
- Test with demo credentials first
- Export data before major changes

### Reporting Issues
- Note exact error message
- Check browser/device model
- Verify internet connection
- Document steps to reproduce

---

## FAQ

**Q: Does data sync work offline?**
A: Yes! Data saves locally and syncs when reconnected.

**Q: Can I use this on multiple devices?**
A: Yes! Login on any device and sync automatically.

**Q: Is my data safe?**
A: Data stored in your Google Sheet with your Google account's security.

**Q: Can I export my data?**
A: Yes! App Settings → Data → Export All Data

**Q: How often does it sync?**
A: Every 30 seconds automatically, or click "Sync Now" for manual sync.

**Q: Can I use it as a mobile app?**
A: Yes! Install on iOS, Android, or desktop via browser.

---

## Version Information

- Next.js: 15.5.9
- React: 19.2.0
- Tailwind CSS: 3.4.17
- Node version: 18+

---

## License & Attribution

RentFlow - Complete Rental Management System
Built with Next.js, React, and Tailwind CSS
PWA support via Service Workers and manifest.json
Cloud sync via Google Apps Script

---

Last Updated: 2025-12-14
