# airtable_-automation-script_-easypost-shipping-Update
This is a script that is used to make a custom automation in Airtable in order to automatically update the status of shipments when given the carrier and tracking number. 
# **User Guide: EasyPost Tracking Script for Airtable Automations**

This guide explains how to configure and use the EasyPost tracking script within Airtable automations. The script enables tracking of shipments from carriers such as FedEx, UPS, USPS, and DHL Express through EasyPost's API. It also ensures that tracking information is correctly formatted in Eastern Standard Time (EST) and updates specific fields in your Airtable records.

---

## **1. Airtable Table Structure (Required Fields)**

To ensure the script functions correctly, your Airtable table must be structured with the following fields:

| **Field Name**          | **Field Type**       | **Description**                                         |
|-------------------------|---------------------|---------------------------------------------------------|
| `Tracking Number`       | Single Line Text    | The shipment’s tracking number.                         |
| `Carrier`              | Single Select       | The shipping carrier (FedEx, UPS, USPS, DHL Express).   |
| `Tracking Status`       | Single Line Text    | Displays the latest tracking status.                    |
| `Tracking ETA`          | Single Line Text    | Displays the estimated delivery date/time (formatted in EST). |
| `Tracking Details`      | Single Line Text    | Provides the most recent tracking event message.        |
| `Tracking Location`     | Single Line Text    | Displays the last known location of the package.        |
| `Last Tracking Update`  | Date/Time Field     | Stores the last **automated** tracking update timestamp. |
| `Last Manual Update`    | Date/Time Field     | Stores the last **manual** update timestamp when triggered by the button. |

> **⚠️ Important Notes:**  
> - `Carrier` must match EasyPost’s carrier names (`fedex`, `ups`, `usps`, `dhl_express`).
> - `Last Tracking Update` is automatically updated when tracking updates occur.
> - `Last Manual Update` is only updated when the script is triggered via the button.
> - `Tracking Location` should be a **single line text** field to store the most recent location.

---

## **2. Configuring the Airtable Automation**

### **Step 1: Set Up the Automation in Airtable**
1. Open your **Airtable Base**.
2. Go to the **Automations** tab.
3. Click **Create an Automation** or open an existing automation.
4. Set up a **Trigger**:
   - Choose **"When a record is updated"** if you want the script to run automatically when the tracking number or carrier field is modified.
   - Alternatively, choose **"When a button is clicked"** if you want users to manually trigger the script.

### **Step 2: Add a "Run a script" Action**
1. Click **Add Action**.
2. Select **Run a script**.
3. Click **Edit script** to open the scripting editor.

---

## **3. Adding Variables to the Script**
Before pasting the script, you need to define the input variables in Airtable.

### **Step 1: Add the Following Variables**
Each of these must be mapped to the corresponding Airtable fields in your table.

| Variable Name           | Description                                      | Field Type in Airtable |
|-------------------------|--------------------------------------------------|------------------------|
| `trackingNumber`        | The shipment's tracking number.                 | Single Line Text       |
| `carrier`              | The shipping carrier (e.g., FedEx, UPS, DHL).   | Single Select          |
| `recordId`             | The unique Airtable record ID.                   | Airtable Record ID     |
| `trackingStatusDetails` | Stores tracking status updates.                  | Single Line Text       |
| `trackingLocation`      | Stores the last known shipment location.         | Single Line Text       |
| `isManualUpdate`       | Stores the last manual update timestamp.         | Date/Time Field        |

1. Click **+ Add input variable** and enter each of the variable names as listed above.
2. Assign each variable to its corresponding field in the Airtable table.

---

## **4. Understanding Timezone Handling**
This script includes built-in **timezone formatting** to ensure timestamps are displayed in Eastern Standard Time (EST).

### **How It Works:**
- **`getLocalESTTime()`**: Converts the system’s local time to EST and formats it as:
### **How It Works:**
- **`getLocalESTTime()`**: Converts the system’s local time to EST and formats it as:
- - **`convertToEST(isoDate)`**: Converts EasyPost’s UTC timestamps to EST automatically.

This ensures that all tracking timestamps, including estimated delivery and last update times, are correctly formatted and standardized for clarity.

---

## **4. How the Script Works**
Once triggered, the script follows these steps:

1. **Validates the input:**
 - Ensures that a **tracking number** is provided.
 - Checks if the carrier is supported.
 - Prevents execution if the package is already **delivered**.
 - Prevents execution if the **last manual update** was within the last **30 minutes**.

2. **Sends a tracking request to EasyPost API**:
 - Uses the **EasyPost API** to fetch real-time shipment data.
 - Extracts **status, estimated delivery date, tracking details, and last known location**.

3. **Updates the Airtable Record**:
 - If the script is triggered **automatically**, it updates the `Last Tracking Update` field.
 - If the script is triggered **manually**, it updates the `Last Manual Update` field.

---

## **5. How to Use the Automation**
### **Automatic Tracking Updates**
- The automation will run whenever a **tracking number or carrier field is updated**.
- The script will check the last tracking status and update Airtable accordingly.

### **Manual Tracking Updates**
- Click the **button in the Airtable interface** to manually trigger the script.
- If the last manual update was **less than 30 minutes ago**, the script **will not run** to avoid excessive API calls.
- If the shipment is **delivered**, the script will **skip updating** to prevent unnecessary requests.

---

## **6. Summary**
By following this guide, you can set up an automated tracking system in Airtable that:
- **Fetches live shipment tracking updates** using EasyPost.
- **Formats timestamps in EST** for consistency.
- **Prevents redundant API calls** by limiting manual updates to every **30 minutes**.
- **Skips delivered shipments** to avoid unnecessary updates.

This script streamlines tracking management for your shipments while ensuring accurate and up-to-date tracking information in your Airtable records.

---

**🚀 You're now ready to integrate and use this tracking system in your Airtable automations!**
