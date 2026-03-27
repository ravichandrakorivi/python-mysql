# Google Sheets API Access using Service Account (Python)

##  Overview

To access Google Sheets data from Python without manual login, we use a **Service Account** from Google Cloud and authenticate using a **JSON credentials file**.

---

## Step 1: Open Google Cloud Console

Go to:

* Google Cloud Console (https://console.cloud.google.com)

---

## Step 2: Create / Select Project

* Click **Select Project → New Project**
* Give a name (e.g., `python-sheet-project`)
* Click **Create**

---

## 🔌 Step 3: Enable Required APIs

Go to **APIs & Services → Library**, enable:

* Google Sheets API
* Google Drive API

**Note** : Google Drive API is required because Sheets are stored in Drive.

---

## Step 4: Create Service Account

* Go to **APIs & Services → Credentials**
* Click **Create Credentials → Service Account**
* Enter name (e.g., `python-sheet-reader`)
* Click **Done**

**Note**: This name is just a label and does NOT affect the file name.

---

## Step 5: Create Service Account Key (IMPORTANT)

* Open the created service account
* Go to **Keys tab**
* Click **Add Key → Create new key**
* Select **JSON**
* Click **Create**

A JSON file will be downloaded automatically.

Example file name:
`project-name-123456-abcde12345.json`

---

## Step 6: Rename the JSON File (IMPORTANT)

The downloaded file name is auto-generated and not meaningful. Rename it to something simple:

* `service_account.json` or
* `python-sheet-reader.json`

Renaming is SAFE and recommended. Only the filename changes, not the contents.

---

## Step 7: Place File in Project

Place the renamed json file in your project folder.

## Step 8: Share Google Sheet with Service Account

Open the JSON file and copy:

"client_email": "[xxxx@project-id.iam.gserviceaccount.com](mailto:xxxx@project-id.iam.gserviceaccount.com)"

Steps:

* Open your Google Sheet
* Click **Share**
* Paste the email
* Give Viewer/Editor access

Without this step → you will get "Spreadsheet not found" error

---

## Step 9: Use in Python (gspread)

```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials

scope = ["https://www.googleapis.com/auth/spreadsheets.readonly"]

creds = ServiceAccountCredentials.from_json_keyfile_name(
    "service_account.json",
    scope
)

client = gspread.authorize(creds)

sheet = client.open_by_key("YOUR_SHEET_ID")
```

---

## Key Concepts

### 🔹 Service Account

* A non-human Google account used for backend/API access

### 🔹 Service Account Key

* Authentication credential created for the service account

### 🔹 JSON Credentials File

* Contains private key + client email
* Used by Python to authenticate

---

## Important Notes

### 1. Never upload JSON file to GitHub

Add to `.gitignore`:

`service_account.json`

---

### 2. If file is lost

* You cannot download the same key again
* Create a new key:
  Service Account → Keys → Create new key

---

## Best Practices

### Use .env file (avoid hardcoding)

.env:

```
GOOGLE_CREDS=service_account.json
```

Python:

```python
import os
creds_path = os.getenv("GOOGLE_CREDS")
```
---