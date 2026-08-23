# 📊 Google Sheets Dynamic Dashboard & Data Validator

[![Google Apps Script](https://img.shields.io/badge/Google%20Apps%20Script-4285F4?style=flat&logo=google&logoColor=white)](https://developers.google.com/apps-script)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)]()

An optimized, universal Google Apps Script automation tool designed for large-scale data workflows. It dynamically scans any Google Spreadsheet, creates an interactive Index/Dashboard tab, generates direct tab hyperlinks, counts actual data rows (excluding headers), and performs real-time row-count validation against target numbers.

---

## 📸 Overview

Managing multi-tab spreadsheets (50+ sheets) often leads to navigation friction and data auditing issues. This tool automates navigation and auditing by providing:

1. **Quick Jump Links:** Direct sheet-level URLs for 1-click tab switching.
2. **Data Row Tracking:** Automated non-empty row extraction excluding header rows.
3. **Target Validation:** Instant visual feedback comparing expected target rows vs. actual data rows.

---

## ✨ Features

- **🚀 100% Dynamic & Scalable:** Works out-of-the-box on any Google Sheet without hardcoding tab names, IDs, or spreadsheet URLs.
- **⚡ Fast Batch Processing:** Implements single-batch array operations (`setValues()`, `setBackgrounds()`) to process 50+ tabs in under 2 seconds without hitting Google Apps Script quotas.
- **🎯 Visual Verification Badges:**
  - `✅ MATCH` (Row count matches target)
  - `❌ MISMATCH` (Highlights exact missing or extra rows)
  - `⚠️ Enter Target` (Prompts user input for auditing)
- **💾 State Retention:** Retains manually entered target row values across multiple sync cycles without data loss.
- **🎨 Modern Slate Dashboard:** Styled with high-contrast slate headers (`#1E293B`), alternate zebra striping (`#F8FAFC`), and clean borders.
- **🧭 Custom Top Menu:** Adds a `📊 Dashboard Manager` menu directly to the Google Sheets UI for instant manual syncs.

---

## 🛠️ Installation & Setup

1. Open your **Google Spreadsheet**.
2. Navigate to **Extensions** ➔ **Apps Script**.
3. Delete any default code in the editor and paste the code from [`Code.js`](./Code.js).
4. Click **Save (💾)**.
5. In the toolbar dropdown, select the function **`createDashboard`** and click **Run (▶)**.
6. Grant the necessary permissions on the first run:
   - Click **Review Permissions** ➔ Choose your Google Account ➔ Click **Advanced** ➔ Click **Go to Untitled project (unsafe)** ➔ **Allow**.
7. Return to your spreadsheet. The **Dashboard** tab will appear at the first position.

---

## 📋 How to Use

1. **Navigate:** Click on any `Open ➜ [Sheet Name]` link in Column C to jump directly to that tab.
2. **Set Targets:** In Column D (`Expected Rows`), enter your expected target row counts.
3. **Re-sync:** Use the top menu bar **`📊 Dashboard Manager` ➔ `🔄 Refresh Dashboard`** to re-evaluate the verification badges.

---

## 💻 Tech Stack

- **Scripting:** JavaScript (ES6+ / Google Apps Script V8 Engine)
- **Environment:** Google Sheets API / Workspace Platform
