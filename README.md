# Email Task Extractor

A standalone web app that scans your Gmail inbox, uses Claude AI to identify important emails and extract actionable tasks, then sends them to ClickUp.

## Features

- Scans Gmail inbox with configurable time windows
- AI-powered email analysis via Claude Haiku (fast + cheap)
- Editable priority per task (urgent / high / normal / low / none)
- One-click send to ClickUp with correct priority mapping
- Remembers last scan time to avoid rescanning processed emails
- All credentials stored locally in your browser — nothing in the repo

## Setup

### 1. Anthropic API Key
1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Click **API Keys** → **Create Key**
3. Add a small billing credit ($5 lasts months at Haiku pricing)
4. Copy your key (`sk-ant-...`)

### 2. Gmail OAuth Client ID
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project (or use an existing one)
3. Go to **APIs & Services** → **Enable APIs** → search for and enable **Gmail API**
4. Go to **APIs & Services** → **Credentials** → **Create Credentials** → **OAuth Client ID**
5. Application type: **Web application**
6. Add to **Authorised JavaScript origins**: your GitHub Pages URL (e.g. `https://yourusername.github.io`)
   - Also add `http://localhost` if you want to test locally
7. Copy the **Client ID** (`xxxxxx.apps.googleusercontent.com`)

### 3. ClickUp API Key
1. Go to [ClickUp Settings → Apps](https://app.clickup.com/settings/apps)
2. Click **Generate** under API Token
3. Copy the key (`pk_...`)

### 4. First launch
1. Open the app
2. Click the ⚙ settings icon
3. Paste in all three keys + your ClickUp list ID and list name
4. Click **Save settings**

### Finding your ClickUp List ID
Open ClickUp, navigate to the list you want tasks sent to, and look at the URL:
`https://app.clickup.com/WORKSPACE_ID/v/l/LIST_ID` — the last number is your list ID.

## Deploying to GitHub Pages

1. Create a new GitHub repo named `email-task-extractor`
2. Upload `index.html` to the repo
3. Go to **Settings** → **Pages**
4. Source: **Deploy from a branch** → branch: `main`, folder: `/ (root)`
5. Your app will be live at `https://yourusername.github.io/email-task-extractor`

**Important:** Make sure your GitHub Pages URL is added to the Gmail OAuth Client ID's authorised origins (step 2.6 above) — otherwise the Gmail login popup will be blocked.

## Cost estimate

| Usage | Cost |
|---|---|
| 1 scan of 20 emails | ~$0.002 |
| Daily scans for a month | ~$0.06 |
| Daily scans for a year | ~$0.73 |

Claude Haiku is extremely cost-effective for this use case.

## Privacy

- Your API keys are stored in your browser's `localStorage` — they never touch GitHub or any server
- Email content is sent to the Anthropic API for analysis (subject to [Anthropic's privacy policy](https://www.anthropic.com/privacy))
- No data is stored anywhere except your own browser and ClickUp
