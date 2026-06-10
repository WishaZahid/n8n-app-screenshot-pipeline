# n8n-app-screenshot-pipeline
Automated App Store screenshot generation pipeline built in n8n — drop a folder in Google Drive, get a full branded screenshot set back in minutes.

## About the Project

App Store screenshots are the single highest-impact conversion asset a mobile app can have. For a Dubai-based app development agency shipping apps across multiple clients, producing a professional screenshot set for every app was a recurring cost that never went away. Every new app meant a new designer brief, rounds of revisions, and days of waiting. Every app update meant doing it all over again. On top of the time and money, screenshots made by different designers at different times looked inconsistent, and some felt disconnected from the actual app UI entirely.

This system solves that completely. The agency drops a project folder into Google Drive containing their app screenshots and a set of competitor reference images. The pipeline analyzes the app's visual identity, studies the competitor screenshots, synthesizes a coherent theme that puts the app's own brand first, resolves it into six concrete screenshot slot definitions, generates the full set, and delivers the output back to the same Drive folder. No designer, no brief, no revision cycle.

---

## Core Features

- **Folder-Triggered Automation:** The entire pipeline fires the moment a correctly structured project folder is dropped into Google Drive. No manual trigger needed.
- **Parallel Visual Analysis:** App screenshots and competitor screenshots are analyzed simultaneously through separate Gemini pipelines, cutting processing time in half.
- **Brand-First Theme Synthesis:** Competitor data informs layout and composition only. The app's own color palette, typography, and mood are always the primary source for the visual theme, enforced directly in every AI prompt.
- **Six-Slot Screenshot Resolution:** The theme is resolved into six concrete screenshot definitions with deliberate variation in gradient direction, typography weight, phone tilt, and subtitle opacity across slots.
- **Truncation Recovery Parser:** A two-stage JSON recovery system salvages partial Gemini responses when the model hits its output limit mid-response, keeping the pipeline running rather than crashing on an incomplete payload.

---

## Built With

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Automation** | n8n | Workflow orchestration and logic |
| **Competitor Analysis** | Google Gemini Pro Preview | Competitor screenshot analysis and design brief extraction |
| **Theme Synthesis** | Google Gemini Pro Preview | Creative director theme decision and slot resolution |
| **App UI Analysis** | Google Gemini Flash Lite | App screenshot visual identity extraction |
| **Image Generation** | Google Gemini Flash Image Preview | Final screenshot generation |
| **Trigger and Storage** | Google Drive | Folder watching, image input, and output delivery |
| **Logic Layer** | JavaScript (n8n Code Nodes) | Custom aggregation, parsing, and truncation recovery |

---

## Getting Started

### Prerequisites
- n8n instance (self-hosted or cloud)
- Google Cloud project with Gemini API access (Pro Preview and Flash models)
- Google account with Drive API access
- Project folders structured with the required subfolder format (see below)

### Installation

1. **Import the workflow**
   - Open your n8n instance
   - Go to Workflows and click Import
   - Upload `Screenshot_Pipeline_NanoBanana.json`

2. **Set up credentials in n8n**
   - Google Drive: connect via OAuth2
   - Gemini: add your Google API key as an HTTP Header credential for all Gemini HTTP Request nodes

3. **Configure environment variables**
   Set the following directly in the relevant n8n nodes:

   | Variable | Where | Description |
   |----------|-------|-------------|
   | `WATCH_FOLDER_ID` | Drive Watch node | Google Drive folder ID to monitor for new project folders |
   | `GEMINI_API_KEY` | All HTTP Request nodes | Your Google Gemini API key |
   | `IMAGE_RESIZE_WIDTH` | Resize nodes | Default is 720px, adjust based on API cost preference |

4. **Set up the Drive folder structure**
   Every project folder dropped into the watched Drive folder must contain exactly two subfolders:

   ```
   ProjectFolderName/
   ├── app-screenshots/     # UI screenshots from the actual app
   └── competitors/         # Screenshot images from competing apps
   ```

   If either subfolder is missing or duplicated, the workflow stops and returns a precise error message.

5. **Activate the workflow**
   - Turn on the Screenshot Pipeline workflow in n8n
   - The Drive folder is checked every minute for new project folders

---

## Usage Guide

### How to Generate a Screenshot Set

1. **Create a project folder** in your local Drive sync or directly in Google Drive with any name that identifies the app.
2. **Add the two required subfolders** inside it: one named `app-screenshots` and one named `competitors`.
3. **Drop app UI screenshots** into the `app-screenshots` folder. These are raw screens from the actual app.
4. **Drop competitor screenshots** into the `competitors` folder. These are App Store screenshots from competing apps in the same category.
5. **Wait for the pipeline to fire.** The workflow checks the watched folder every minute and triggers automatically when a new project folder is detected.
6. **Collect the output.** A timestamped output folder is created inside your project folder in Google Drive containing the full set of generated App Store screenshots.

### Folder Naming Tips

- Use clear folder names that describe the app for easy tracking
- Competitor images can be organized into subfolders by competitor name inside the `competitors` folder
- The output folder is timestamped automatically so multiple runs on the same project do not overwrite previous results

### What the Pipeline Produces

- Six App Store screenshots per project
- Each slot varies in gradient direction, phone tilt, typography weight, and subtitle opacity for visual variety
- Each screenshot is matched to the most contextually appropriate app screen based on filename

---

## Roadmap

- [x] Google Drive folder-triggered pipeline
- [x] Parallel app and competitor image analysis
- [x] Brand-first theme synthesis with competitor layout reference
- [x] Six-slot screenshot resolution with controlled visual variation
- [x] Truncation recovery parser for partial Gemini responses
- [x] Timestamped output delivery back to Google Drive

---

## Contact & Team

Built by **Codoro** - AI Automation Agency

| | |
|--|--|
| Website | [codoroai.com](https://codoroai.com) |
| Email | services@codoroai.com |
| LinkedIn | [Codoro on LinkedIn](https://www.linkedin.com/company/codoro) |
| Upwork | [Hire us on Upwork](https://www.upwork.com/freelancers/~01f0abdab02d0a6daa) |
