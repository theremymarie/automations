/**
 * Global variable for the Google Sheet ID.
 * REPLACE THIS WITH YOUR ACTUAL GOOGLE SHEET ID.
 * You can find this in the URL of your Google Sheet:
 * https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit
 */
const SPREADSHEET_ID = '[Paste your Google Chat ID Here]'; // IMPORTANT: Replace this with your actual Google Sheet ID!
const SHEET_NAME = 'Sheet1'; // Default sheet name, change if yours is different

/**
 * Handles POST requests from Google Chat.
 * This is the main function that Google Chat will call when a message is sent to the bot.
 *
 * @param {Object} e The event object containing information about the incoming request.
 * @returns {GoogleAppsScript.Content.TextOutput} A JSON response for Google Chat.
 */
function doPost(e) {
  const json = JSON.parse(e.postData.contents);
  console.log('Received payload:', json);

  if (json.type === 'MESSAGE') {
    return handleMessage(json);
  } else if (json.type === 'ADDED_TO_SPACE') {
    return handleAddedToSpace(json);
  } else if (json.type === 'REMOVED_FROM_SPACE') {
    return handleRemovedFromSpace(json);
  }
  return createJsonResponse("I'm not sure how to handle that event type.");
}

/**
 * Handles incoming messages from Google Chat.
 * Checks if the message is "Good Morning", "VL", "EL", "halfday", or "SL" and logs attendance/leave to Google Sheet.
 *
 * @param {Object} messageData The parsed message event data.
 * @returns {GoogleAppsScript.Content.TextOutput} A JSON response for Google Chat.
 */
function handleMessage(messageData) {
  const messageText = messageData.message.text;
  const senderName = messageData.message.sender.displayName;
  const senderEmail = messageData.message.sender.email;
  const lowerCaseMessage = messageText ? messageText.toLowerCase().trim() : '';
  let responseMessage = '';

  if (lowerCaseMessage.includes('good morning')) {
    try {
      logAttendance(senderName, senderEmail, messageText);
      responseMessage = `Noted.`;
    } catch (error) {
      console.error('Error logging attendance:', error);
      responseMessage = `Sorry, I encountered an error while logging your attendance: ${error.message}`;
    }
  } else if (lowerCaseMessage === 'vl') {
    try {
      logAttendance(senderName, senderEmail, messageText);
      responseMessage = `Noted.`;
    } catch (error) {
      console.error('Error logging VL:', error);
      responseMessage = `Sorry, I encountered an error while logging your VL: ${error.message}`;
    }
  } else if (lowerCaseMessage === 'el') {
    try {
      logAttendance(senderName, senderEmail, messageText);
      responseMessage = `Noted.`;
    } catch (error) {
      console.error('Error logging EL:', error);
      responseMessage = `Sorry, I encountered an error while logging your EL: ${error.message}`;
    }
  } else if (lowerCaseMessage === 'halfday') {
    try {
      logAttendance(senderName, senderEmail, messageText);
      responseMessage = `Noted.`;
    } catch (error) {
      console.error('Error logging Half Day:', error);
      responseMessage = `Sorry, I encountered an error while logging your Half Day: ${error.message}`;
    }
  } else if (lowerCaseMessage === 'sl' || lowerCaseMessage === 'sick leave') {
    try {
      logAttendance(senderName, senderEmail, messageText);
      responseMessage = `Noted.`;
    } catch (error) {
      console.error('Error logging Sick Leave:', error);
      responseMessage = `Sorry, I encountered an error while logging your Sick Leave: ${error.message}`;
    }
  } else {
    responseMessage = `Hello ${senderName}! I can log "Good Morning" for attendance, "VL" for Vacation Leave, "EL" for Emergency Leave, "Halfday", or "SL" (Sick Leave).`;
  }
  return createJsonResponse(responseMessage);
}

/**
 * Handles the event when the bot is added to a space.
 */
function handleAddedToSpace(eventData) {
  const spaceName = eventData.space.displayName || "this space";
  console.log(`Bot added to space: ${spaceName}`);
  return createJsonResponse(`Hello! Thanks for adding me to ${spaceName}. I'm here to log attendance when you say "Good Morning", "VL", "EL", "Halfday", or "SL" (Sick Leave).`);
}

/**
 * Handles the event when the bot is removed from a space.
 */
function handleRemovedFromSpace(eventData) {
  const spaceName = eventData.space.displayName || "this space";
  console.log(`Bot removed from space: ${spaceName}`);
}

/**
 * Logs attendance data to the specified Google Sheet.
 */
function logAttendance(userName, userEmail, message) {
  const spreadsheet = SpreadsheetApp.openById(SPREADSHEET_ID);
  const sheet = spreadsheet.getSheetByName(SHEET_NAME);
  if (!sheet) {
    throw new Error(`Sheet "${SHEET_NAME}" not found in spreadsheet ID: ${SPREADSHEET_ID}`);
  }
  const timestamp = new Date();
  sheet.appendRow([timestamp, userName, userEmail, message]);
  console.log(`Logged: ${message} for ${userName} (${userEmail}) at ${timestamp}`);
}

/**
 * Creates a JSON response object for Google Chat.
 */
function createJsonResponse(text) {
  const response = {
    "text": text
  };
  return ContentService.createTextOutput(JSON.stringify(response))
    .setMimeType(ContentService.MimeType.JSON);
}
