# CompareDocs — Setup Guide
### Outlook Sidebar Add-in · Compare Quote, Binder, Invoice + Application

---

## What you'll end up with

A **Compare Documents** button in your Outlook ribbon that opens a sidebar panel.
Pin the panel once — it stays open all day as you move between emails.

**Your daily workflow:**
1. Open the email with the **Quote** → click "Add from email"
2. Open the email with the **Binder + Invoice** → click "Add from email"
3. Optionally open the email with the **ACORD Application** → click "Add from email"
4. Click **Compare Documents** — results appear in the sidebar instantly

No saving to desktop. No copy/paste. No switching apps.

---

## Before you start

You need:
- A free **GitHub account** — github.com (2 min to create)
- Your **Anthropic API key** — console.anthropic.com → API Keys → Create key
- **Microsoft 365 Outlook** — desktop app or Outlook on the web (both work)
- Classic Outlook on Windows (build 7668.2000 or later) for the pin feature

---

## Step 1 — Add your API key

Open **taskpane.html** in Notepad (right-click → Open with → Notepad).

Find this line:

    const API_KEY = "YOUR_ANTHROPIC_API_KEY_HERE";

Replace the placeholder with your actual key:

    const API_KEY = "sk-ant-api03-...your key here...";

Save and close.

---

## Step 2 — Create your GitHub repository

1. Sign in to github.com
2. Click **+** (top right) → **New repository**
3. Name it exactly: `comparedocs`
4. Set to **Public**
5. Check **Add a README file**
6. Click **Create repository**

---

## Step 3 — Upload files

In your new repository:
1. Click **Add file** → **Upload files**
2. Upload these files from the comparedocs folder:
   - `taskpane.html`
   - `manifest.xml`
   - Any PNG image file renamed to `icon-16.png`
   - Any PNG image file renamed to `icon-32.png`
   - Any PNG image file renamed to `icon-80.png`

   (For icons, any square PNG works for now — the button will still function.)

3. Click **Commit changes**

---

## Step 4 — Enable GitHub Pages (free hosting)

1. In your repository, click **Settings** (top menu)
2. Click **Pages** (left sidebar, under "Code and automation")
3. Under **Source**, select: `Deploy from a branch`
4. Under **Branch**, select: `main` and `/ (root)`
5. Click **Save**

Wait 2–3 minutes. Your app is now live at:

    https://YOUR-GITHUB-USERNAME.github.io/comparedocs/taskpane.html

Visit that URL to confirm — you should see the CompareDocs interface.

---

## Step 5 — Update manifest.xml with your URL

Open **manifest.xml** in Notepad.

Replace every occurrence of:

    YOUR-GITHUB-USERNAME

...with your actual GitHub username. It appears about 8 times.

Also replace:

    REPLACE-WITH-YOUR-GUID-HERE

...with a unique ID. Go to **https://www.uuidgenerator.net/** — copy the GUID
shown and paste it there. Looks like: `a1b2c3d4-e5f6-7890-abcd-ef1234567890`

Also replace:

    YOUR BROKERAGE NAME

...with your brokerage name.

Save the file, then go back to GitHub and re-upload the updated `manifest.xml`
(drag it onto the same upload page — it will replace the old one).

---

## Step 6 — Install the add-in in Outlook

### Outlook on the web / New Outlook

1. Open any email that has an attachment
2. Click the **...** (More actions) button at the top of the email
3. Click **Get Add-ins**
4. Click **My add-ins** in the left sidebar
5. Scroll to **Custom add-ins** at the bottom
6. Click **+ Add a custom add-in** → **Add from URL**
7. Paste your manifest URL:
   `https://YOUR-GITHUB-USERNAME.github.io/comparedocs/manifest.xml`
8. Click **Install** — dismiss the warning (it's just because it's not from the Marketplace)

### Classic Outlook on Windows

1. Open Outlook
2. Click **File** → **Manage Add-ins**
   (This opens Outlook on the web — follow the steps above from there)

---

## Step 7 — Pin the sidebar and start using it

1. Open an email that has attachments
2. In the Outlook ribbon, find the **Insurance Tools** group
3. Click **Compare Documents** — the sidebar opens on the right
4. Click the **📌 pin icon** at the top of the sidebar panel to keep it pinned
5. Now browse to your emails:
   - Open the Quote email → click **Add from email**
   - Open the Binder/Invoice email → click **Add from email**
   - Optionally open the Application email → click **Add from email**
6. Verify the document assignments in the slots panel
7. Click **Compare Documents**

---

## How the sidebar works

**Green dot** = slot loaded  
**Amber dot** = required, not yet loaded  
**Gray dot** = optional, not loaded

When you click **Add from email**, the app:
1. Downloads each attachment from that email
2. Extracts the text (PDF, DOCX, or TXT)
3. Uses Claude to identify which document is which
4. Fills the correct slots automatically

If Claude gets one wrong, tap the attachment in the list and a sheet slides up
letting you assign it to the correct slot manually.

---

## Troubleshooting

**Button doesn't show in ribbon**  
The add-in only activates on emails with attachments. Open an email that has
a PDF or document attached.

**"Could not authenticate"**  
You need to be signed in to Microsoft 365. Close the sidebar, sign out and
back in to Outlook, then try again.

**"Download failed (HTTP 401)"**  
Your Microsoft 365 tenant may have REST API access restricted. Ask your IT
admin to confirm add-ins can access message attachments via the Outlook REST API.

**"Scanned PDF — no text layer"**  
The underwriter sent an image-only PDF (photographed or printed-to-PDF). Ask
them to send a text-based PDF (exported directly from Word/their system). You
can't extract text from a scanned image automatically.

**Sidebar closes when I switch emails**  
Make sure you clicked the 📌 pin icon after opening the sidebar. Without pinning,
it closes when you navigate away.

**The sidebar doesn't update when I open a new email**  
This is the ItemChanged event. If it stops working, close and reopen the sidebar
by clicking Compare Documents in the ribbon again.

---

## Security note

Your Anthropic API key is in `taskpane.html`. This is fine for personal use.
The file is public on GitHub Pages, so don't use a key that has access to
billing-sensitive operations beyond the API. For team deployment, set up a
small backend proxy (a Vercel serverless function works well) that holds the
key server-side and forwards requests — ask for help with that when you're ready.

---

## Files in this package

    taskpane.html   The sidebar app
    manifest.xml    Tells Outlook where the app is and what button to show
    SETUP.md        This guide
