# Invoice Entry Bot 🤖🧾

An **unattended UiPath RPA bot** that automates invoice data entry into [Gimbooks](https://gimbooks.com/) (a billing/accounting web app) — combining phone-photographed paper invoices with **Google Gemini** for AI-based field extraction, eliminating manual copy-paste work.

---

## 📌 Overview

Manually reading a hard-copy invoice and typing its details into a billing portal is slow and error-prone. This bot automates that entire flow end-to-end:

1. Hard-copy invoices are photographed on a phone and dropped into a watched local folder
2. The bot picks up each photo from that folder one by one
3. Each photo is sent to **Gemini** (via Chrome) with targeted prompts (e.g. *"Give only invoice number..."*) to extract the **invoice number**, **invoice date**, **grand total**, and **company/seller name**, reading Gemini's response from the clipboard
4. The extracted fields are logged into an **Excel register** first, for audit tracking
5. The bot then switches to Gimbooks, selects the correct seller, and fills the same extracted values into the invoice entry form
6. Saves the invoice entry in Gimbooks

---

## ⚙️ How It Works

| Step | Activity |
|------|----------|
| Iterate invoice photos from watched folder | `For Each File in Folder` |
| Open Gemini in browser, send photo + prompts | `Chrome New Tab` → Gemini |
| Extract fields via Gemini | Type prompts (e.g. *"Give only invoice number..."*) → `Get From Clipboard` |
| Log extracted fields to Excel | `Excel Process Scope` → `Write Cell` |
| Switch to Gimbooks, upload invoice | `Chrome New Tab` → Gimbooks → `Click 'Upload Bill'` |
| Select seller | `Click 'Select Seller'` |
| Fill entry form | `Type Into` → Purchase Invoice Number, Purchase Date, Amount |
| Save entry | `Click 'Save'` |
| Handle exceptions | `If / Else`, `Do` (retry/validation logic) |

---

## 🛠️ Tech Stack

- **UiPath Studio** (Workflow: `Main.xaml`)
- `UiPath.UIAutomation.Activities` — browser and UI interaction
- `UiPath.Excel.Activities` — Excel logging
- `UiPath.System.Activities` — clipboard, control flow
- `UiPath.IntegrationService.Activities`
- `UiPath.MicrosoftOffice365.Activities`
- **Target app:** Gimbooks (Chrome-based automation)

---

## 📂 Project Structure

```
Invoice-Entry-Bot/
├── Main.xaml            # Core automation workflow
├── project.json          # UiPath project configuration & dependencies
├── project.uiproj        # UiPath project file
└── entry-points.json     # Workflow entry point definition
```

---

## 🚀 Getting Started

### Prerequisites
- [UiPath Studio](https://www.uipath.com/product/studio) (Community or Enterprise)
- Google Chrome with UiPath extension enabled
- A Google account with Gemini access
- A Gimbooks account with invoice upload access
- Excel installed (for logging output)

### Setup
1. Clone this repository:
   ```bash
   git clone https://github.com/sujalbhosale/Invoice-Entry-Bot-.git
   ```
2. Open `project.json` in UiPath Studio.
3. Set up a local folder where phone-photographed invoices are saved.
4. Update the invoice source folder path and Excel log file path in `Main.xaml`.
5. Run the bot via **Main.xaml** as the entry point.

---

## 📊 Output

Every run produces an Excel log containing:

| Invoice Number | Invoice Date | Grand Total | Company Name |
|----------------|--------------|-------------|---------------|
| ...            | ...          | ...         | ...           |

---

## 🎯 Why This Project

Built to explore how **AI + RPA** can work together — instead of relying purely on rigid OCR templates, this bot uses Gemini's conversational AI prompting to pull structured data straight out of photographed, hard-copy invoices, then hands that data off to a traditional RPA flow for entry and logging.

---

## 👤 Author

**Sujal Bhosale**
- GitHub: [@sujalbhosale](https://github.com/sujalbhosale)
- LinkedIn: [sujal-bhosale](https://linkedin.com/in/sujal-bhosale-794379238)
- Email: sujalbhosale138@gmail.com

---

## 📝 License

This project is open for learning and reference purposes. Feel free to fork and adapt.
