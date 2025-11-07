🎬 AI Video Storage Automation (OpenAI + Google Drive + Google Sheets)

This project automates the process of storing AI-generated videos in Google Drive and logging their metadata in Google Sheets — including clickable links that let you watch each video directly from the sheet.

Built with n8n, OpenAI’s Video API, and Google APIs, this system provides a lightweight, no-code backend for managing video generation pipelines.

🚀 Features

🧠 Generates or retrieves video files via OpenAI’s API

☁️ Automatically uploads each video to Google Drive using OAuth 2.0

📊 Logs detailed metadata to Google Sheets (title, description, duration, date, etc.)

🔗 Creates clickable Google Drive links for instant playback in the spreadsheet

⚙️ Fully automated with n8n — no manual uploading or copy-pasting

🧩 Architecture Overview
OpenAI (Video Generation)
        ↓
HTTP Request Node (Fetch video)
        ↓
Google Drive Node (Upload via OAuth)
        ↓
Google Sheets Node (Log metadata)

🧠 How It Works

OpenAI Video Retrieval
The workflow starts by sending an HTTP Request to OpenAI’s video endpoint:

https://api.openai.com/v1/videos


This returns the video file and metadata (ID, duration, title, etc.).

Google Drive Upload
The video is uploaded directly to your personal Google Drive using OAuth 2.0 credentials (not a service account).

Files are stored in a designated folder (e.g., Sora)

Each upload returns a shareable link for playback

Google Sheets Logging
Once uploaded, the metadata and Google Drive link are automatically appended as a new row in your Google Sheet, making it easy to track, filter, and organize your generated videos.

Playback in Sheets
Each Drive link in the sheet opens the playable video directly in Drive — no download required.

⚙️ Setup Guide
1. Prerequisites

A Google account

n8n installed (locally or hosted)
n8n Installation Guide

An OpenAI API key

A Google Cloud Project with:

Drive API enabled

Sheets API enabled

OAuth 2.0 Client ID created

2. Connect Your Credentials

In n8n, go to:
Credentials → Create New → Google Drive (OAuth2)

Enter your OAuth Client ID and Secret from the Google Cloud Console.

Grant access to both Drive and Sheets when prompted.

(Optional) Choose a specific folder (like Sora) for video uploads.

3. Workflow Steps
Step	Node Type	Purpose
1	HTTP Request (OpenAI)	Retrieves or generates a video
2	Function Node	Formats video metadata
3	Google Drive Node	Uploads video via OAuth
4	Google Sheets Node	Logs metadata and clickable link
4. Example Output in Google Sheets
Title	Description	Duration	Drive Link	Date
“Sora Clip 01”	AI-generated scene	00:43	Watch in Drive
	2025-11-06
🧰 Tech Stack

n8n – workflow automation

OpenAI API – video generation/retrieval

Google Drive API – secure file storage

Google Sheets API – structured logging

🌱 Future Improvements

📺 Add automatic YouTube upload integration

📦 Include video tagging and categorization

🔍 Integrate analytics or video view tracking

🧠 Optionally store embeddings for content search

🧑‍💻 Author

Justin Lejeune
Cybersecurity & Software Development @ Penn State
