# GitHub Setup Instructions

## Quick Setup

1. **Create a new repository on GitHub:**
   - Go to https://github.com/new
   - Name it (e.g., `professional-photo-generator`)
   - Choose Public or Private
   - Don't initialize with README (we already have one)
   - Click "Create repository"

2. **Push your code to GitHub:**

   Open PowerShell/Terminal in this directory and run:

   ```powershell
   # Initialize git (if not already done)
   git init

   # Add all files
   git add .

   # Make initial commit
   git commit -m "Initial commit: Professional Photo Generator with OpenRouter Gemini"

   # Add your GitHub repository as remote (replace with your actual repo URL)
   git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

   # Push to GitHub
   git branch -M main
   git push -u origin main
   ```

3. **After pushing, update the API key:**
   - In n8n, open the "OpenRouter Gemini API" node
   - Replace `YOUR_OPENROUTER_API_KEY_HERE` with your actual API key: `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`

## Deploying Online

### Option 1: GitHub Pages (Free, Static Hosting)

1. Go to your GitHub repository
2. Click **Settings** → **Pages**
3. Under "Source", select **main** branch and **/ (root)** folder
4. Click **Save**
5. Your site will be available at: `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME`

**Note:** Update the `webhookUrl` in `index.html` to point to your n8n webhook URL.

### Option 2: Netlify (Free, Easy Deployment)

1. Go to https://www.netlify.com
2. Sign up/login with GitHub
3. Click "Add new site" → "Import an existing project"
4. Connect your GitHub repository
5. Deploy settings:
   - Build command: (leave empty)
   - Publish directory: `/` (root)
6. Click "Deploy site"
7. Update the `webhookUrl` in `index.html` with your n8n webhook URL

### Option 3: Vercel (Free, Fast)

1. Go to https://vercel.com
2. Sign up/login with GitHub
3. Click "Add New Project"
4. Import your GitHub repository
5. Deploy (no build settings needed for static HTML)
6. Update the `webhookUrl` in `index.html` with your n8n webhook URL

## Important Security Note

⚠️ **Your API key is NOT in the GitHub repository** - it's been replaced with a placeholder. Make sure to:
- Add your API key in n8n after importing the workflow
- Never commit your actual API key to GitHub
- Consider using n8n environment variables for production

## Testing

After deploying:
1. Make sure your n8n workflow is active
2. Update the `webhookUrl` in your deployed `index.html` to point to your n8n webhook
3. Test the upload and generation functionality

