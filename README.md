# Generate Viral AI Videos with Veo 3 in N8N

This project is a no-code automation workflow built with N8Nb to generate short-form AI videos using scripts created by OpenAI and rendered through Veo 3. All inputs and outputs are managed through Google Sheets.
It’s ideal for automating video content creation at scale — useful for marketers, creators, and teams building content workflows without needing to write code.

How It Works

The workflow is triggered manually in N8N.
It reads video topic ideas from a Google Sheet.
These are passed to a prompt agent that formats them into structured prompts.
The prompt is sent to OpenAI’s ChatGPT to generate a video script.
The script is sent to the Veo 3 API to begin rendering a video.
A polling loop checks whether the video is complete.
Once ready, the video’s download URL is retrieved.
The Google Sheet is updated with the completed video link and metadata.

APIs Used

- Google Sheets API – Used to read input data and write back results.
- OpenAI API – Used to generate the video scripts based on input prompts.
- Veo 3 API (via FAL) – Used to render the videos, poll their status, and fetch the final download URLs.

Setup

- To use this workflow, you’ll need:
- An N8N instance (cloud or self-hosted)
- Access to Google Sheets and the API enabled
- An OpenAI API key
- An account with FAL and access to Veo 3

Installation

Clone or download this repository.
Import the provided n8n-workflow.json file into your N8N instance.
Set up your credentials for:
- Google Sheets
- OpenAI
- FAL/Veo 3
Update the nodes to point to your specific Google Sheet and API keys.
Run the workflow manually or on a schedule.

Example Use Case

You maintain a Google Sheet with a list of video titles or content prompts. This workflow automatically turns those prompts into full video scripts, renders the videos with Veo 3, and adds the final video URLs back into the same sheet.
