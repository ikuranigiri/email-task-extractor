# Email Task Extractor

A standalone web app that scans your Gmail inbox, uses Claude AI to identify important emails and extract actionable tasks, then sends them to ClickUp — with a direct link back to the original email.

## What it does

- Scans your Gmail inbox with a configurable time window and email count
- Uses Claude Haiku to identify important emails and skip newsletters/promos
- Extracts actionable tasks with priority levels (urgent / high / normal / low / none)
- Shows a review screen — edit priorities, deselect tasks you don't want
- Sends selected tasks to ClickUp with a description and direct Gmail link
- Remembers the last scan time so you never re-scan the same emails
- Shows FYI emails (important but no tasks) and skipped emails (newsletters etc)

## Prerequisites

You need three API credentials before the app will work:

1. **Anthropic API key** — for Claude Haiku email analysis
2. **Gmail OAuth Client ID** — to read your Gmail inbox
3. **ClickUp API key** — to create tasks

---

## Setup Guide

### 1. Anthropic API Key

1. Go to [console.anthropic.com](https://console.anthropic.com) and sign in (separate from Claude.ai)
2. Click **API Keys** in the left sidebar → **Create Key**
3. Name it anything (e.g. `Email Task Extractor`)
4. **Copy the key immediately** — it's only shown once. It starts with `sk-ant-...`
5. Go to **Billing** and add a small credit — $5 lasts months at Haiku pricing (~$0.002 per 20-email scan)

### 2. Gmail OAuth Client ID

This is the most involved step but only needs to be done once.

**Create a Google Cloud project:**
1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Click the project selector at the top left → **New project**
3. Give it any name → **Create**
4. Make sure your new project is selected in the top left dropdown

**Enable the Gmail API:**
1. Go to **APIs & Services** → **Library**
2. Search for **Gmail API** → click it → **Enable**

**Configure the OAuth consent screen:**
1. Go to **APIs & Services** → **OAuth consent screen**
2. User type: **External** → **Create**
3. Fill in:
   - App name: `Email Task Extractor` (or anything)
   - User support email: your email
   - Developer contact email: your email
4. Click **Save and Continue**
5. On the Scopes page: click **Save and Continue** (no changes needed)
6. On the Test users page: click **+ Add users** → add your Gmail address → **Save and Continue**
7. Click **Back to Dashboard**

> ⚠️ **Important:** You must add your Gmail address as a test user or you'll get an "Access blocked" error when signing in.

**Create the OAuth Client ID:**
1. Go to **APIs & Services** → **Credentials**
2. Click **+ Create Credentials** → **OAuth client ID**
3. Application type: **Web application**
4. Name: anything (e.g. `Email Task Extractor Web`)
5. Under **Authorised JavaScript origins** click **+ Add URI** and enter:
   ```
   https://yourusername.github.io
   ```
   > ⚠️ Use only the root domain — no path, no trailing slash. Google will reject anything like `https://yourusername.github.io/email-task-extractor`.
6. Under **Authorised redirect URIs** click **+ Add URI** and enter the same:
   ```
   https://yourusername.github.io
   ```
7. Click **Create**
8. Copy the **Client ID** (`xxxxxx.apps.googleusercontent.com`)

### 3. ClickUp API Key

1. Go to [app.clickup.com/settings/apps](https://app.clickup.com/settings/apps)
2. Under **API Token** click **Generate** (or copy if one already exists)
3. Copy the key (starts with `pk_...`)

**Finding your ClickUp List ID:**
Navigate to the list you want tasks sent to in ClickUp. Look at the URL:
`https://app.clickup.com/WORKSPACE/v/l/XXXXXXX` — the last number is your List ID.

---

## Deploying to GitHub Pages

1. Create a new GitHub repo named `email-task-extractor` (must be **Public**)
2. Upload `index.html` to the repo (**Add file** → **Upload files**)
3. Go to **Settings** → **Pages**
4. Source: **Deploy from a branch** → Branch: **main** → Folder: **/ (root)** → **Save**
5. Wait ~60 seconds, then your app is live at:
   ```
   https://yourusername.github.io/email-task-extractor
   ```

---

## First launch

1. Open your app URL
2. Click the **⚙** settings icon in the top right
3. Fill in all fields:
   - Anthropic API Key
   - Gmail OAuth Client ID
   - ClickUp API Key
   - ClickUp List ID
   - ClickUp List Name
4. Click **Save settings**
5. Click **Scan inbox** — a Gmail login popup will appear
6. Sign in and click **Continue** when Google warns the app isn't verified (this is expected for personal apps in test mode)

> All credentials are stored in your browser's `localStorage` — they never touch GitHub or any server.

---

## Updating the app

When a new version of `index.html` is available:

1. Download the new file
2. Go to your GitHub repo → click `index.html` → **Edit** (pencil icon)
3. Select all → delete → paste in new content → **Commit changes**

After uploading, your browser may show the old cached version. To force it to load the new one:
- Try **incognito/private mode** first — always works
- Or clear your cache: **Chrome Settings** → **Privacy and Security** → **Clear browsing data** → check **Cached images and files** → **Clear**

---

## Sharing with others

Anyone can visit your GitHub Pages URL and configure the app with their own credentials — your keys are never visible to them.

**To let a friend use your Gmail OAuth Client ID** (easiest):
1. Go to Google Cloud Console → **OAuth consent screen** → **Test users**
2. Add their Gmail address
3. Share your Client ID with them — they enter it in their settings

**For fully independent setup:**
They go through the Gmail OAuth setup themselves and use their own Client ID. They just need to add `https://yourusername.github.io` as an authorised origin (since they're using your hosted app URL).

---

## Cost estimate

| Usage | Cost |
|---|---|
| 1 scan of 20 emails | ~$0.002 |
| Daily scans for a month | ~$0.06 |
| Daily scans for a year | ~$0.73 |

Set a spending limit in [Anthropic Console](https://console.anthropic.com) as a safety net.

---

## Limitations

- Analyses up to 50 emails per scan reliably (token limit)
- Gmail OAuth is in "test mode" — you must add users manually in Google Cloud Console
- App must be hosted on an origin registered in your OAuth client (GitHub Pages URL)
- No email modification — read only

## Privacy & Security

- API keys stored in browser `localStorage` only — never in the repo
- Email content (subject, sender, snippet) is sent to Anthropic API for analysis
- Enable 2FA on your GitHub account to protect the app code
- Set a spend limit on your Anthropic account
- The app requests `gmail.readonly` scope — it cannot send, delete, or modify emails
