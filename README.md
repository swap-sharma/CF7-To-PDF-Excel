# CF7 to PDF & Excel

> A custom WordPress plugin that automatically converts **Contact Form 7** submissions into a formatted **PDF document** and a structured **Excel spreadsheet**, then delivers both as email attachments — instantly, with zero manual effort.

---

## 📋 Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [Features](#features)
- [Screenshots](#screenshots)
- [Form Structure](#form-structure)
- [Generated Outputs](#generated-outputs)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Technical Details](#technical-details)
- [Live Demo](#live-demo)

---

## Overview

**CF7 to PDF & Excel** was built for a client running a *Virtual Estimator* onboarding workflow. When a prospect submits the onboarding form, the plugin:

1. Captures all field values from the CF7 submission
2. Generates a clean, multi-page **PDF summary document**
3. Generates a structured **Excel (.xlsx) spreadsheet**
4. Attaches both files to an automated **notification email**
5. Delivers the email to the configured recipient — no manual step required

The entire pipeline runs in under a second from form submission.

---

## How It Works

```
User submits CF7 form
        │
        ▼
wpcf7_mail_sent hook fires
        │
        ▼
Plugin captures all field values
        │
        ├──▶ Generates PDF  (form-{id}.pdf)
        │
        └──▶ Generates Excel (submission-{id}.xlsx)
                │
                ▼
        wp_mail() sends email with both files attached
```

---

## Features

| Feature | Description |
|---|---|
| 📄 **PDF Generation** | Converts submission data into a clean, multi-page PDF document |
| 📊 **Excel Export** | Builds a structured `.xlsx` file with labelled rows per data group |
| ✉️ **Auto Email Delivery** | Fires an email with both attachments the moment the form is submitted |
| ⚡ **CF7 Native** | Hooks directly into CF7 — no third-party bridges or extra config |
| 🔒 **Structured Data Mapping** | All form sections mapped precisely into both output formats |
| 🎯 **Client-Specific Schema** | Built to match the exact field structure of the Virtual Estimator form |

---

## Screenshots

### Frontend Form — Virtual Estimator
The full onboarding form built with Contact Form 7, collecting business details, login credentials, shop rates, and sublet provider information.

![Frontend Form](https://plugins.stagingwave.site/cf7-to-pdf-excel/Frontend_Form.png)

---

### Successful Submission
Confirmation state shown to the user after the form is submitted successfully.

![Successful Submission](screenshots/Form_Successfull_Submission.png)

---

### Email Received with Attachments
The automated notification email delivered to the configured recipient, with both the PDF and Excel file attached.

![Email Received](screenshots/Email_Recived_with_PDF_And_Excel.png)

---

## Form Structure

The plugin maps the following form sections into both output files:

### 🏢 Business Details
| Field | Description |
|---|---|
| Business Name | Company or trading name |
| Business Timezone | Timezone for operations |
| Business Address | Full street address |
| Business Operation Hours | Operating hours label |
| Main Contact Person | Primary contact name |
| Email | Main contact email |
| Phone | Contact phone number |
| Accounts Contact Name | Accounts department name |
| Accounts Contact Email | Accounts department email |

### 👥 Communicators
Up to 5 name/email pairs for team members who will communicate with the Virtual Estimator (via Slack).

### 🖥️ Virtual Estimator Login Details
| Platform | Fields |
|---|---|
| Estimating Software | Software name, Username, Password |
| AUDANET / AUDATEX | Username, Password |
| Other Login | Username, Password |
| Partscheck | Username, Password |
| Repair Connection | Username, Password |

### 🔐 BSR Admin Login Details
| Platform | Fields |
|---|---|
| Estimating Software (iBodyshop) | Username, Password |
| AUDANET / AUDATEX | Username, Password |

### 💰 Shop Rate
- Know Shop Rate (Yes/No)
- Repair Rate ($)
- Paint Rate ($)

### 🔧 Sublet Provider Names
For each sublet type, the form collects: **Supplier name**, **Cost option**, and **Price**.

| Sublet Type |
|---|
| Mechanical |
| Electrical |
| 1/4 Glass R&R |
| Front / Rear Screen R&R |
| Window Tint (per glass) |
| ADAS Calibration |
| Regas / Degas A/C |
| Wheel Repairs |
| Wheel Alignment |
| Wheel Balance |
| Tow Transfer to Sublet |

### 📝 Notes
Free-text additional notes field.

---

## Generated Outputs

### PDF — Form Submission Summary

A multi-page PDF report laid out with bold section headers, clearly labelled fields, and formatted values. Sections match the form structure above.

| Page | Content |
|---|---|
| Page 1 | Business Details · Communicators · Estimating Software · VE Logins |
| Page 2 | BSR Logins · Shop Rate · Sublet Providers (partial) |
| Page 3 | Sublet Providers (continued) · Notes |

![PDF Page 1](screenshots/pdf_ss_1.png)
![PDF Page 2](screenshots/pdf_ss_2.png)
![PDF Page 3](screenshots/pdf_ss_3.png)

---

### Excel — Structured Spreadsheet

A `.xlsx` file with all data written into clearly labelled rows using section headers in column A and values in column B (with some sections using columns C and D for cost and price).

| Rows | Content |
|---|---|
| 1–10 | Business Details |
| 12–20 | Communicators |
| 22–23 | Estimating Software |
| 25–30 | VE Logins |
| 32–34 | BSR Logins |
| 36–39 | Shop Rate |
| 41–52 | Sublet Providers |
| 54–55 | Notes |

![Excel Sheet 1](screenshots/excel_ss_1.png)
![Excel Sheet 2](screenshots/excel_ss_2.png)
![Excel Sheet 3](screenshots/excel_ss_3.png)

---

## Requirements

- WordPress **5.8+**
- Contact Form 7 **5.5+**
- PHP **7.4+**
- PHP spreadsheet library (e.g. [PhpSpreadsheet](https://github.com/PHPOffice/PhpSpreadsheet)) for Excel generation
- PDF generation library (e.g. [TCPDF](https://tcpdf.org/) or [DOMPDF](https://github.com/dompdf/dompdf))
- `wp_mail()` configured and working on the server (SMTP recommended)

---

## Installation

1. Clone or download this repository into your WordPress plugins directory:

```bash
cd wp-content/plugins/
git clone https://github.com/your-username/cf7-to-pdf-excel.git
```

2. Activate the plugin from **WordPress Admin → Plugins**.

3. Ensure Contact Form 7 is installed and activated.

4. Configure the recipient email address in the plugin settings (or directly in the plugin file — see [Configuration](#configuration)).

---

## Configuration

Open the main plugin file and update the following constants to match your setup:

```php
// Recipient email for the notification
define( 'CF7_PDF_EXCEL_RECIPIENT', 'your@email.com' );

// Email subject line
define( 'CF7_PDF_EXCEL_SUBJECT', 'Virtual Estimator Form Submission' );

// CF7 Form ID to target (find this in CF7 → Contact Forms)
define( 'CF7_PDF_EXCEL_FORM_ID', 123 );
```

> **Note:** If you use multiple CF7 forms, the plugin can be extended to target specific form IDs by checking `$submission->get_contact_form()->id()` inside the hook callback.

---

## Technical Details

### Hook

The plugin attaches to CF7's `wpcf7_mail_sent` action:

```php
add_action( 'wpcf7_mail_sent', 'cf7_generate_and_email_attachments' );
```

### PDF Generation

```php
function cf7_generate_pdf( $fields ) {
    // Uses TCPDF / DOMPDF to render section headers and field values
    // Saves the file temporarily to wp-content/uploads/cf7-exports/
    return $pdf_path;
}
```

### Excel Generation

```php
function cf7_generate_excel( $fields ) {
    // Uses PhpSpreadsheet to write structured rows
    // Saves the .xlsx file temporarily to wp-content/uploads/cf7-exports/
    return $xlsx_path;
}
```

### Email Dispatch

```php
wp_mail(
    $recipient,
    $subject,
    $body,
    $headers,
    [ $pdf_path, $xlsx_path ]  // Both files attached
);
```

After the email is sent, the temporary files are deleted from the server.

---

## Live Demo

🌐 Plugin page: [https://plugins.stagingwave.site/cf7-to-pdf-excel/](https://plugins.stagingwave.site/cf7-to-pdf-excel/)

---

## License

This plugin was developed as a **custom client project**. Please contact the author before reuse or redistribution.

