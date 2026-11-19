# Google Apps Script Setup Guide

You **DO NOT** need a Google API key! Since you've already deployed the Google Apps Script, follow these simple steps:

## Your Apps Script URL
\`\`\`
https://script.google.com/macros/s/AKfycbz9kJttpb488bnwjiEHz71501KUSTpGE3Glfoml3qYN1e8jSZVAp7_kI9bYFFsCNuJw/exec
\`\`\`

## How to Connect:

1. **Open RentFlow App**
2. **Go to Settings** (gear icon in header)
3. **Click on "Cloud" tab**
4. **Paste your Apps Script URL** in the input field
5. **Click "Test & Save"**
6. **Done!** Your data will now sync across all devices

## No API Keys Required

The Apps Script approach is much simpler than the Google Sheets API:
- No API keys needed
- No Google Cloud Console setup
- No authentication configuration
- Just deploy and use the URL

## Verify Your Deployment

Make sure your Apps Script is deployed correctly:

1. Open your Google Sheet
2. Go to Extensions → Apps Script
3. Click Deploy → Manage deployments
4. Verify settings:
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy the Web app URL

## Features Enabled

With the Apps Script connected, you get:

- **Cross-device sync** - Access data from any device
- **Real-time updates** - Changes sync every 30 seconds
- **Offline support** - Works without internet, syncs when connected
- **Data persistence** - Never lose your data
- **Role-based access** - Admins, Managers, and Tenants with proper permissions
- **Recycle bin** - Restore deleted items
- **Cloud backup** - All data stored in Google Sheets

## Troubleshooting

If connection test fails:

1. **Check URL** - Make sure it ends with `/exec`
2. **Verify deployment** - Must be set to "Anyone" access
3. **Run setupSheets()** - Initialize the database tables
4. **Check permissions** - Allow the script to access your Google Sheet
5. **Try redeploying** - Sometimes a fresh deployment helps

## Need Help?

If you're still having issues:
1. Open the Apps Script editor
2. Check the Execution log for errors
3. Make sure all sheets (Users, Rooms, Bills, etc.) were created by `setupSheets()`
4. Verify the script has permissions to read/write the sheet
