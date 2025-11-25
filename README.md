# **Sora API YouTube Shorts Uploader**

### *Automated TikTok → YouTube Shorts Upload System Using n8n, Google APIs, and a Custom Frontend*

This project is an end-to-end automation pipeline that:

1. **Generates an AI video using OpenAI’s Sora API** (or any video generation endpoint you connect)
2. **Uploads the video to your Google Drive**
3. **Logs metadata to Google Sheets**
4. **Downloads the video for re-upload**
5. **Automatically uploads the video to your YouTube Shorts channel**
6. **Provides a custom frontend with buttons to trigger workflows & navigate to essential pages**

---

# 🚀 **How the Backend Works (n8n Workflow)**

Below is a breakdown of every node **exactly as they appear in the screenshot** and what each one does.

## **🔴 1. Webhook**

* **Purpose:** Starts the entire workflow when your frontend sends the prompt.
* **Triggered by:** Clicking the *lightbulb → “Run Workflow”* button on the frontend.
* **Output:** Receives the prompt text from your website.

---

## **🌐 2. HTTP Request (Sora Prompt)**

* **Purpose:** Sends the user’s prompt to the **OpenAI Sora API** to start generating the video.
* **Input:** The prompt from the Webhook.
* **Output:** Returns a `run_id` from Sora.

---

## **🌐 3. HTTP Request1 (Polling Sora for Completion)**

* **Purpose:** Every ~15 seconds, this node checks if Sora finished generating the video.
* **Logic:**

  * If `status != "completed"`, loop back into itself (via the IF node).
  * When `status == "completed"`, continue forward.

---

## **⏱️ 4. Wait**

* **Purpose:** Creates a 15-second delay between each poll.
* **Connected between:** `HTTP Request1` → `Wait` → back to `HTTP Request1`.

---

## **📥 5. HTTP Request2 (Download Sora Video URL)**

* **Purpose:** After Sora finishes, this node fetches the **actual file URL**.

---

## **📤 6. Upload File (Google Drive)**

* **Requires:**

  * **Google OAuth2 credentials**
  * **Google Drive API enabled** in Google Cloud Console
* **Purpose:** Uploads the downloaded Sora video to your Google Drive folder.
* **Output:** Produces a `webViewLink` (used later for Sheets + YouTube).

---

## **🔧 7. Code Node (Formatting Metadata)**

Your current code (unchanged):

```js
const soraData = $items("HTTP Request1")[0].json;

return [
  {
    json: {
      Title: soraData.prompt || soraData.input?.prompt || "No prompt found",
      Date: new Date().toLocaleString(),
      Video_Link: $json.webViewLink || $json.data?.webViewLink || "No link found"
    }
  }
];
```

**Purpose:**

* Pull prompt + timestamp
* Grab the Google Drive video link
* Format everything into a single object for Google Sheets

---

## **📄 8. Append Row in Sheet (Google Sheets)**

* **Requires:**

  * **Google Sheets API credentials**
* **Purpose:** Writes one row to your log spreadsheet:

  * Prompt
  * Date
  * Video Drive link

---

## **⬇️ 9. Download File (Google Drive)**

* **Purpose:** Downloads the uploaded video from Drive so it can be sent to YouTube.
* **Input:** The `fileId` from the previous Drive Upload node.

---

## **📺 10. Upload a video (YouTube)**

* **Requires:**

  * **YouTube Data API OAuth2 credentials**
  * Your Google Cloud project must have:
    ✔ YouTube Data API v3 enabled
    ✔ OAuth client with “YouTube Data API” scopes

* **Purpose:** Uploads the downloaded video as a **YouTube Short**.

* **Important:**

  * You must use a **valid YouTube category ID**.
  * Shorts use **categoryId = 22 (People & Blogs)**.
  * Title + description must come from earlier nodes.

* **Example YouTube fields:**

  * **Title:** `Expression → {{$node["Code"].json["Title"]}}`
  * **Description:** `Check this out! #Shorts #AI #Viral #Short #Funny`
  * **Tags:** `Shorts,AI,Viral,Short,Funny`
  * **File:** Binary → `{{$binary.data}}`

---

# ⚙️ **Required API Integrations**

## **1. Google OAuth2**

Enable in Google Cloud:

* Google Drive API
* Google Sheets API
* YouTube Data API v3

Then create:

* **OAuth Client ID (Desktop App)**
* Paste credentials into **n8n → Credentials → Google OAuth2**

---

## **2. Google Sheets API**

* Create a spreadsheet
* Share with your service account email
* Connect in n8n via Google Sheets node.

---

## **3. YouTube OAuth (YouTube Data API v3)**

* Must enable **YouTube Data API v3**
* Scopes needed:

  * `youtube.upload`
  * `youtube`
* Select **External** or **Internal** OAuth consent screen
* Add your Google account as test user

---

# 🎨 **Frontend (index.html UI)**

The frontend is a lightweight static HTML page with a modern full-screen hero design.

**The UI was designed from scratch** (HTML + CSS + tiny bit of JS).
It includes **six circular icon buttons**, each with a dedicated function.

---

## **🖥️ Buttons & What They Do**

### **💡 Lightbulb Icon — Generate Prompt Ideas**

* Calls a local JS prompt generator using ChatGPT
* Pops up a list of idea suggestions
  *(You removed the OpenAI node from the backend to avoid costs — this stays front-end only.)*

---

### **🏀 Basketball Icon — Trigger Workflow**

* Sends the user’s prompt to your n8n webhook
* Starts the entire Sora → Drive → Sheets → YouTube pipeline

---

### **🐙 GitHub Icon — Open the Project Repo**

* Links to:
  `https://github.com/justinlej12/Sora-2-API-n8n-Automation`

---

### **📷 Camera Icon — Open YouTube Channel**

* Opens your YouTube channel where the Shorts are uploaded

---

### **📄 Sheets Icon — Open Organizer Sheet**

* Links directly to your Google Sheets log
* Shows: Prompt, Date, Drive link, etc.

---

### **📁 Google Drive Icon — Open Video Folder**

* Opens the Drive folder where n8n uploads each generated Sora video

---

# 🧩 **How It All Fits Together**

1. You open the **frontend** → click the **Basketball button**
2. It sends your prompt → **n8n Webhook**
3. n8n calls **OpenAI Sora** → gets `run_id`
4. n8n **polls** until the video is done
5. n8n downloads the video → uploads it to **Google Drive**
6. n8n logs metadata to **Google Sheets**
7. n8n uploads the video to **YouTube Shorts**
8. You can view results via the **YouTube**, **Sheets**, and **Drive** buttons

---

# 🎉 **Result**

Your workflow now:

* Requires **zero manual uploading**
* Posts **Shorts automatically**
* Tracks everything
* Gives you a clean frontend dashboard
* Has already gotten **2,400+ views and new subs** on YouTube (W 💪)


