# RentFlow Cloud Sync Setup - Google Apps Script

## Overview

RentFlow uses Google Apps Script as a backend to sync all your data across devices using Google Sheets. This guide provides step-by-step instructions.

## Why Google Apps Script?

- **No API Keys Needed**: Simple URL-based authentication
- **Multi-Device Access**: Access your rental data from phone, tablet, and desktop
- **Free**: Uses free Google Sheets and Apps Script
- **Automatic**: Data syncs every 30 seconds without manual action
- **Offline Support**: Works offline, syncs when reconnected

## Prerequisites

- Google account
- RentFlow app deployed on Vercel
- Modern browser with localStorage support

## Step-by-Step Setup

### 1. Create Google Sheet

1. Go to [Google Sheets](https://sheets.google.com/)
2. Click "Create" → "Blank spreadsheet"
3. Name it "RentFlow Data"

### 2. Open Apps Script Editor

1. In your Google Sheet, go to Extensions → Apps Script
2. Delete any default code
3. The editor is now ready for your script

### 3. Add the Backend Code

1. Copy the entire code from `google-apps-script/code.gs` file
2. Paste it into the Apps Script editor
3. Save the project (Ctrl+S or Cmd+S)
4. Name it "RentFlow Backend"

### 4. Initialize the Database

1. In the Apps Script editor, find the function dropdown at the top
2. Select `setupSheets` from the dropdown
3. Click the Run button (▶️)
4. Authorize the script:
   - Click "Review permissions"
   - Choose your Google account
   - Click "Advanced" → "Go to RentFlow Backend (unsafe)"
   - Click "Allow"
5. Wait for the function to complete
6. Check your sheet - you should now see multiple tabs (Users, Rooms, Bills, etc.)

### 5. Deploy as Web App

1. Click "Deploy" → "New deployment"
2. Click the gear icon ⚙️ next to "Select type"
3. Choose "Web app"
4. Configure:
   - Description: "RentFlow API"
   - Execute as: "Me"
   - Who has access: "Anyone"
5. Click "Deploy"
6. Copy the deployment URL (looks like: `https://script.google.com/macros/s/AKfycby.../exec`)
7. Click "Done"

### 6. Configure RentFlow App

#### Option A: In the App (Recommended)
1. Open RentFlow app
2. Click user menu → "App Settings"
3. Go to "Cloud" tab
4. Paste your Apps Script URL
5. Click "Test Connection"
6. You should see "Connection successful!"

#### Option B: In Vercel (Environment Variables)
1. Go to your Vercel project settings
2. Navigate to Environment Variables
3. Add:
   - Key: `NEXT_PUBLIC_GOOGLE_APPS_SCRIPT_URL`
   - Value: Your deployment URL
4. Redeploy the app

### 7. Enable Auto Sync

The app automatically syncs every 30 seconds. You can also:
- Click the sync indicator (top-right) to see sync status
- Click "Sync Now" for immediate sync

## Verifying Cloud Sync

1. Create a test bill or room in the app
2. Open your Google Sheet
3. Check the appropriate tab (Bills, Rooms, etc.)
4. You should see your data in the sheet
5. Open the app on another device
6. Log in with same account
7. Wait 30 seconds or click "Sync Now"
8. The new data should appear

## Updating the Script

If you need to update the backend code:

1. Open Apps Script editor (Extensions → Apps Script)
2. Make your changes
3. Save the file
4. Click "Deploy" → "Manage deployments"
5. Click the edit icon ✏️ on your deployment
6. Change "Version" to "New version"
7. Click "Deploy"
8. The URL stays the same - no need to update the app

## Troubleshooting

### "Connection failed" Error

**Check 1: Deployment URL**
- Verify you copied the entire URL
- URL should end with `/exec`
- Check for extra spaces
- Make sure deployment is active

**Check 2: Script Permissions**
- Open Apps Script editor
- Run `setupSheets` again
- Make sure authorization was granted

**Check 3: Deployment Settings**
- Execute as: "Me"
- Who has access: "Anyone"
- If set to "Only myself", change to "Anyone"

### Data Not Syncing

**Issue: Data stored locally but not in Google Sheet**

Solution:
1. Check internet connection
2. Click "Sync Now" button
3. Check browser console (F12) for errors
4. Verify deployment URL is correct

**Issue: Data appears on one device but not another**

Solution:
1. Log in on both devices with same account
2. Click "Sync Now" on both devices
3. Wait 30 seconds
4. Refresh browser (Ctrl+R or Cmd+R)

**Issue: "Script error" message**

Solution:
1. Open Apps Script editor
2. Check the "Executions" log (left sidebar)
3. Find recent errors
4. Common fixes:
   - Re-run `setupSheets`
   - Redeploy the web app
   - Check sheet permissions

### "Unauthorized" Error

This means the script needs authorization:
1. Open Apps Script editor
2. Run any function manually
3. Grant permissions when prompted
4. Try syncing again in the app

## Data Sync Behavior

### What Syncs
- ✓ All rooms and properties
- ✓ All renters and tenants
- ✓ All bills and payments
- ✓ User data (excluding passwords)
- ✓ Permissions and roles
- ✗ User passwords (stored locally only)
- ✗ Session tokens (not synced)

### Sync Timing
- Every 30 seconds (automatic)
- When you click "Sync Now"
- When app regains internet connection
- On page load if data is stale

### Conflict Resolution
If the same data is edited on multiple devices:
- Most recent cloud version wins
- Local changes are overwritten on sync
- No data is deleted (merged carefully)

## Advanced: Manual Sheet Management

You can view and edit data directly in Google Sheets:

1. Open your RentFlow Data sheet
2. Navigate to any tab (Rooms, Bills, etc.)
3. View or edit data directly
4. Changes sync to app within 30 seconds

**Note:** Use caution when editing directly - invalid data formats may cause sync errors.

## Disable Cloud Sync

If you want to disable cloud sync:

1. App Settings → Cloud
2. Clear the Apps Script URL
3. Click "Test Connection" (should fail)
4. Data stays in app, just won't sync

Note: Disabling sync doesn't delete data, just stops synchronization.

## Limits & Considerations

**Apps Script Quotas (Free Tier):**
- 20,000 URL fetch calls per day
- RentFlow uses ~1 call every 30 seconds = ~2,880 per day
- Well within free tier limits

**Sheet Size Limits:**
- Maximum 10 million cells per sheet
- RentFlow uses minimal space
- Can store thousands of records easily

**Execution Time:**
- Apps Script: 6 minutes max per execution
- RentFlow operations: < 5 seconds typically
- No issues for normal use

## Security Notes

1. **Apps Script URL**
   - Public URL but requires user authentication
   - Each request includes user verification
   - Only authorized users can access data

2. **Google Sheets Access**
   - Only you (sheet owner) can access the sheet directly
   - Apps Script acts as secure middleware
   - Data isolation per user account

3. **Best Practices**
   - Use strong Google account password
   - Enable 2FA on Google account
   - Don't share deployment URL publicly
   - Regularly audit user permissions in app

## Next Steps

1. Follow setup steps above
2. Test connection in Cloud settings
3. Create a test bill to verify sync
4. Check Google Sheet for data
5. Try on another device
6. Enable automatic sync

Your data is now synced across all devices!

---

For issues or questions, check the main SETUP_GUIDE.md or review error messages in browser console (F12).
