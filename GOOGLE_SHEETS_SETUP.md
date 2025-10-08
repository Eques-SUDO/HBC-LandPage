# Google Sheets Setup Guide

Follow these steps to connect your form to Google Sheets:

## Step 1: Create a Google Sheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet
3. Name it "Spanish Glow Form Submissions" (or any name you prefer)
4. In the first row, add these column headers:
   - **A1**: Timestamp
   - **B1**: Nom Complet
   - **C1**: Téléphone
   - **D1**: Remarques

## Step 2: Create Google Apps Script

1. In your Google Sheet, click on **Extensions** → **Apps Script**
2. Delete any existing code
3. Paste this code:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = e.parameter;

  // Add timestamp and form data
  sheet.appendRow([
    new Date(),
    data.nom,
    data.telephone,
    data.remarques || ''
  ]);

  // Return success response
  return ContentService
    .createTextOutput(JSON.stringify({ 'result': 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Click the **Save** icon (💾) and name the project "Form to Sheets"

## Step 3: Deploy the Script

1. Click **Deploy** → **New deployment**
2. Click the gear icon ⚙️ next to "Select type"
3. Choose **Web app**
4. Configure the deployment:
   - **Description**: Form Handler (optional)
   - **Execute as**: Me
   - **Who has access**: Anyone
5. Click **Deploy**
6. Click **Authorize access** and grant the necessary permissions
7. **Copy the Web app URL** - it looks like:
   `https://script.google.com/macros/s/XXXXX/exec`

## Step 4: Update Your HTML File

1. Open `index.html`
2. Find line 255 where it says:
   ```javascript
   const scriptURL = 'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE';
   ```
3. Replace `'YOUR_GOOGLE_APPS_SCRIPT_URL_HERE'` with your copied URL:
   ```javascript
   const scriptURL = 'https://script.google.com/macros/s/XXXXX/exec';
   ```
4. Save the file

## Step 5: Test the Form

1. Open your `index.html` file in a web browser
2. Scroll to the form section
3. Fill in the form fields
4. Click "Envoyer"
5. Check your Google Sheet - the data should appear in a new row!

## Troubleshooting

- **Form not submitting**: Make sure you copied the complete URL including `/exec` at the end
- **Permission errors**: Ensure you authorized the script to access your Google account
- **Data not appearing**: Check that your sheet column headers match exactly (Nom Complet, Téléphone, Remarques)

## Notes

- Form submissions are added to the sheet in real-time
- The timestamp is automatically added in Morocco timezone
- All form data is stored securely in your Google Sheet
