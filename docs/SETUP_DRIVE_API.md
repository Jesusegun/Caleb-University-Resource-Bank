# Google Drive API Setup Guide

This guide will walk you through setting up the Google Drive API for CURB.

## Prerequisites

- A Google account
- Your organized Google Drive folder structure
- About 15 minutes

## Step 1: Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click on the project dropdown at the top
3. Click **"New Project"**
4. Enter project name: `CURB Resource Bank`
5. Click **"Create"**
6. Wait for the project to be created (you'll see a notification)

## Step 2: Enable Google Drive API

1. In the Google Cloud Console, make sure your new project is selected
2. Go to **"APIs & Services"** > **"Library"** (from the left sidebar)
3. Search for **"Google Drive API"**
4. Click on **"Google Drive API"**
5. Click **"Enable"**
6. Wait for it to be enabled

## Step 3: Create API Credentials

1. Go to **"APIs & Services"** > **"Credentials"**
2. Click **"Create Credentials"** at the top
3. Select **"API Key"**
4. Your API key will be generated and displayed
5. **IMPORTANT:** Copy this key immediately - you'll need it!

## Step 4: Restrict Your API Key (Security)

1. After creating the key, click **"Restrict Key"** (or edit the key)
2. Under **"Application restrictions"**:
   - Select **"HTTP referrers (web sites)"**
   - Click **"Add an item"**
   - Add your website URL:
     - For testing: `http://localhost:*` or `http://127.0.0.1:*`
     - For production: `https://your-domain.vercel.app/*`
   - You can add multiple referrers
3. Under **"API restrictions"**:
   - Select **"Restrict key"**
   - Check **"Google Drive API"**
4. Click **"Save"**

## Step 5: Get Your Root Folder ID

1. Go to Google Drive
2. Navigate to your root folder (the one containing all department folders)
3. Look at the URL in your browser:
   ```
   https://drive.google.com/drive/folders/1aBcDeFgHiJkLmNoPqRsTuVwXyZ
                                            ^^^^^^^^^^^^^^^^^^^^^^^^^^
                                            This is your folder ID
   ```
4. Copy the folder ID from the URL

## Step 6: Configure Folder Sharing

1. Right-click your root folder in Google Drive
2. Click **"Share"**
3. Click **"Change to anyone with the link"**
4. Make sure it's set to **"Viewer"** (not Editor)
5. Click **"Copy link"** then **"Done"**

**Important:** All folders and files inside must also be shared. If you created the structure, they inherit sharing settings automatically.

## Step 7: Add Credentials to Your Project

### Option A: For Local Development

1. Open `index.html` in your code editor
2. Find the `window.ENV` section (around line 150)
3. Replace the placeholder values:
   ```javascript
   window.ENV = {
     GOOGLE_DRIVE_API_KEY: 'YOUR_ACTUAL_API_KEY',
     GOOGLE_DRIVE_ROOT_FOLDER_ID: 'YOUR_ACTUAL_FOLDER_ID'
   };
   ```

### Option B: For Vercel Deployment (Recommended)

1. Go to your Vercel project dashboard
2. Navigate to **"Settings"** > **"Environment Variables"**
3. Add these variables:
   - **Name:** `GOOGLE_DRIVE_API_KEY`
   - **Value:** Your API key
   - **Environment:** Production, Preview, Development
4. Add another:
   - **Name:** `GOOGLE_DRIVE_ROOT_FOLDER_ID`
   - **Value:** Your folder ID
   - **Environment:** Production, Preview, Development
5. Click **"Save"**
6. Redeploy your application

**Note:** For Vercel, you'll also need to update `index.html` to read from environment variables injected at build time, or use Vercel's API to fetch them.

## Step 8: Test the Connection

1. Open your website (locally or deployed)
2. Open browser Developer Tools (F12)
3. Check the Console for any errors
4. If setup correctly, you should see departments loading
5. Click on a department to verify the API is fetching data

## Troubleshooting

### Error: "API key not valid"
- Check that you copied the key correctly
- Verify the key is unrestricted or properly restricted
- Make sure Google Drive API is enabled

### Error: "The caller does not have permission"
- Verify your root folder is shared with "Anyone with the link"
- Check that all subfolders are also shared (inherit from parent)
- Make sure the folder ID is correct

### Error: "Quota exceeded"
- Free tier allows 1,000 requests per day per project
- This should be plenty for normal use
- Consider implementing better caching if hitting limits

### Files not showing up
- Check folder structure matches expected format
- Verify folder/file names are correct
- Make sure files are PDFs
- Check that files aren't in the trash

## API Quota Information

**Google Drive API Free Tier:**
- 1,000 queries per day per project
- 1,000 queries per 100 seconds per user
- Should be sufficient for normal use

**To check your usage:**
1. Go to Google Cloud Console
2. Navigate to **"APIs & Services"** > **"Dashboard"**
3. Click on **"Google Drive API"**
4. View **"Quotas"** tab

## Security Best Practices

1. ✅ Always restrict your API key to specific domains
2. ✅ Never commit API keys to public repositories
3. ✅ Use environment variables for sensitive data
4. ✅ Regularly rotate API keys (every 6-12 months)
5. ✅ Monitor API usage in Google Cloud Console
6. ✅ Set up alerts for unusual activity

## Need Help?

If you're stuck:
1. Check the Console in browser Developer Tools (F12)
2. Review error messages carefully
3. Verify each step above
4. Check that all folder/file permissions are correct
5. Try the API in [Google's API Explorer](https://developers.google.com/drive/api/v3/reference)

## Next Steps

Once the API is set up:
- [Deploy to Vercel](./DEPLOYMENT.md)
- [Set up Google Forms](./GOOGLE_FORM_SETUP.md)
- Start adding content to your Drive folders

