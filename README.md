**Google Chat Attendance Logger Bot**
This repository contains the documentation and core code for a Google Chat Attendance Logger Bot. This bot is designed to automate attendance and leave logging directly from Google Chat, recording entries into a Google Sheet. It serves as a practical example of integrating Google Chat with Google Sheets using Google Apps Script.

**Table of Contents**
I.   Project Overview
II.  Features
III. Technical Architecture
IV.  Project Setup (Replication Guide)
V.   Usage
VI.  Troubleshooting
VII. Maintenance
VIII.Permissions Required

**I. Project Overview**

The Google Chat Attendance Logger Bot is an innovative solution designed to significantly streamline and automate the process of tracking team attendance and various types of leave within a Google Workspace environment. Moving beyond traditional, manual spreadsheet updates, this bot empowers users to log their status effortlessly by simply sending predefined keywords directly within their Google Chat conversations. The bot then intelligently processes these inputs and automatically records the relevant information into a designated Google Sheet, providing a real-time, centralized, and easily accessible attendance log.

**This project aims to:**

- Demonstrate how to automate attendance logging for "Good Morning" messages via Google Chat, fostering a consistent daily check-in routine.

- Illustrate the efficient logging of various leave types, including Vacation Leave (VL), Emergency Leave (EL), Half Day, and Sick Leave (SL), simplifying leave requests and record-keeping.

- Provide a clear, step-by-step, and replicable example of integrating Google Chat's communication capabilities with Google Sheets' data management power using Google Apps Script.

- Showcase a basic yet effective bot interaction design, ensuring immediate user confirmations and a user-friendly experience.

- This bot is a testament to the power of Google Workspace's extensibility, offering a practical solution for common workplace needs through simple automation. It's designed to be easily understood, deployed, and adapted by anyone with basic Google Apps Script knowledge.

  **II. Features**

The bot is configured to recognize the following keywords (case-insensitive for "Good Morning", exact match for others) and log them to a Google Sheet:

- "Good Morning": Logs daily attendance.

- "VL": Logs Vacation Leave.

- "EL": Logs Emergency Leave.

- "Halfday": Logs Half Day leave.

- "SL" or "Sick Leave": Logs Sick Leave.

Upon successful logging, the bot replies with a simple "Noted." If an error occurs during logging, it provides a detailed error message. For unrecognized messages, it reminds the user of the accepted keywords.

**III. Technical Architecture**
The bot operates using Google Apps Script, which serves as the backend logic and integration layer between Google Chat and Google Sheets.

**III.I. Components**

- Google Chat: The primary user interface for bot interaction.

- Google Chat API: Manages incoming messages to the bot and sends outgoing replies.

- Google Apps Script (Web App): The core logic. It's deployed as a web app to receive webhook events from Google Chat, process messages, extract user information, and interact with Google Sheets.

- Google Sheets: The data storage where all attendance and leave entries are recorded.

**III.II Workflow**

1.) A user sends a message containing a recognized keyword in a Google Chat space (direct message or group chat) where the bot is present.

2.) Google Chat sends this message as an event payload (a "webhook") to the deployed Google Apps Script web app's URL.

3.) The Google Apps Script receives and parses the incoming JSON payload. It identifies the message content, the sender's display name, and their email.

4.) The script checks if the message text matches any of the predefined keywords.

5.) If a keyword is matched, the script opens the designated Google Sheet and appends a new row. This row includes the current timestamp, the user's name, their email, and the original message.

6.) Finally, the script constructs a JSON response (e.g., {"text": "Noted."}) and sends it back to Google Chat, which then displays the bot's confirmation message to the user.

**IIII. Project Setup (Replication Guide)**

To replicate and deploy this bot in your own Google Workspace environment, follow these steps:

**IIII.I. Prepare Your Google Sheet**
Go to Google Sheets and create a new blank spreadsheet.

Name it clearly (e.g., "Team Attendance Log").

In the first row, set up the following column headers: Timestamp, User Name, User Email, Message.

**IIII.II. Google Cloud Project Configuration**

1.) Go to the Google Cloud Console and create a new project or select an existing one.
2.) Enable the Google Chat API for your project:
- Navigate to "APIs & Services" > "Enabled APIs & Services."
- Click "+ ENABLE APIS AND SERVICES."
- Search for "Google Chat API" and enable it.

3.) Configure the Google Chat API:
- After enabling, go to "APIs & Services" > "Google Chat API" > "Configuration" tab.
- App Name: Provide a name that users will see (e.g., "Attendance Logger Bot").
- Avatar URL: (Optional) Provide an HTTPS URL to a square PNG image for your bot's avatar.
- Description: A brief description (e.g., "Logs attendance (GM, VL, EL, Halfday, SL)"). Max 40 characters.
- Functionality:
    - Check "Receive 1:1 messages."
    - Check "Join spaces and group conversations."
- Connection settings:
    -Select "HTTP endpoint URL". (You will paste the URL here after deploying your Apps Script in the next step).
-Visibility:
    -Select "Make this Chat app available to specific people and groups in your domain"
    -Enter your Google email address (and any other testers' emails) in the text box provided. This is crucial for the bot to be discoverable by your account.
    -Log errors to Logging: It's highly recommended to check this box for easier debugging.
    -Click "SAVE".
    -Log errors to Logging: It's highly recommended to check this box for easier debugging.
    -Click "SAVE".

**IIII.III. Google Apps Script Code**
The core logic resides in a Google Apps Script.

1.)Go to Google Apps Script and create a new project.

2.)Name the project (e.g., "AttendanceBotScript").

3.)Replace the default Code.gs content with the JavaScript code that can be found in code.md here: https://github.com/theremymarie/automations/blob/main/code.md

4.)Replace [Paste your Google Chat ID Here]: In the script, update the SPREADSHEET_ID constant with your actual Google Sheet ID.

5.) Deploy as Web App:
    -In the Apps Script editor, click "Deploy" > "New deployment."
    -Select "Web app."
    -Set "Execute as: Me" (your Google account) and "Who has access: Anyone."
    -Authorize the script when prompted. This grants it the necessary permissions to access your Google Sheet and receive webhooks.
    -After successful deployment, you will get a Web app URL. Copy this URL.

6.) Final Cloud Console Connection:
    -Go back to the Google Cloud Console: "APIs & Services" > "Google Chat API" > "Configuration" tab.
    -Under "Connection settings," ensure "HTTP endpoint URL" is selected. Paste the Web app URL you copied from Apps Script into the URL field.
    -Under "Triggers," select "Use a common HTTP endpoint URL for all triggers". Paste the same Web app URL into the "HTTP endpoint URL" field that appears here.
    -Click "SAVE" at the bottom of the page.

**V. Usage**

Once the bot is deployed and configured, you can interact with it in Google Chat:

-Adding the Bot:
-To a Direct Message (DM): Open Google Chat, click the "+" (New chat) button, and search for your bot's "App name". Click on its name to open a DM.
-To an Existing Space (Group Chat/Room): Open the desired Google Chat space, click the "Add people" icon (or similar option), search for your bot's "App name", and add it.
-Troubleshooting Adding: If you lack "add member" permissions in a space, try @mentioning the bot directly in the space (e.g., @Attendance Logger Bot Good Morning). If the bot is discoverable and configured correctly, this might implicitly invite it. If issues persist, contact a space manager or your Google Workspace administrator.
- Sending Commands: Simply type one of the recognized keywords in the chat:
   -Good Morning
   -VL
   -EL
   -Halfday
   -SL
   -Sick Leave
-Confirmation: The bot will reply with "Noted." upon successful logging.

**VI. Troubleshooting**
- If your bot isn't working as expected, here are common issues and their solutions:

-"Attendance Logger Bot not responding" / "Only visible to you" in Chat:
   -Cause: The bot is not properly visible or has not been successfully added to the specific chat/space.
   -Solution:
        -Verify your Google Chat email address is listed in the "Visibility" section of the Google Chat API configuration in the Cloud Console.
        -If in a group chat, confirm that the bot has been formally added to that specific space by an authorized user (a space manager or someone with "add               member" permissions). The "Only visible to you" tag explicitly indicates the bot is not a member of that group.

-Bot replies with an error message (e.g., "Sorry, I encountered an error..."):
   -Cause: The Google Apps Script encountered an error during execution.
   -Solution:
        -Go to Google Cloud Console > "Operations" > "Logging" > "Logs Explorer".
        -Select your project.
        -Filter logs for "Resource type: Audited Resource" or "Cloud Functions" and look for entries marked with "Error" related to script.google.com or your Apps Script project.
        -Common errors include "Permission denied: Spreadsheet" (meaning the script needs authorization to access your sheet), or "Sheet 'Sheet1' not found"                (indicating an incorrect sheet name or ID in the script). You might need to re-authorize the script if permissions were changed or not granted initially.

-No response, no error message in logs:
   -Cause: The webhook might not be set up correctly, or there's a propagation delay.
   -Solution:
        -Double-check that the Web app URL from your Apps Script deployment is exactly copied and pasted into both the "Connection settings" and "Triggers"                 sections of the Google Chat API configuration in the Cloud Console.
        -Wait a few minutes after making any configuration changes, as it can take time for them to propagate across Google's systems.
        -Verify that your Apps Script is indeed deployed as a "Web app" with "Execute as: Me" and "Who has access: Anyone".
**VII. Maintenance**
   - Updating the Script: If you make changes to the Apps Script code:
    1. Save the changes in the Apps Script editor.
    2. Click "Deploy" > "Manage deployments."
    3. Click the pencil icon (Edit) next to your existing Web app deployment.
    4. For "Version," select "New version".
    5. Click "Deploy." This updates the existing Web app URL without requiring you to change it again in the Cloud Console.
   - Changing Google Sheet ID or Name: If you decide to use a different Google Sheet or rename the sheet within your existing spreadsheet:
    1. Update the SPREADSHEET_ID or SHEET_NAME constant in your Apps Script code (Code.gs).
    2. Follow the steps in "7.1. Updating the Script" to deploy the new version.

**VIII. Permissions Required**
For the Google Apps Script to function correctly, it will request the following OAuth scopes during the authorization process (which occurs during the initial deployment or when permissions are updated):
   - https://www.googleapis.com/auth/script.external_request: Allows the script to receive webhook requests from Google Chat.
   - https://www.googleapis.com/auth/spreadsheets: Allows the script to read and write data to your Google Sheet.
Ensure you grant these permissions when prompted.
