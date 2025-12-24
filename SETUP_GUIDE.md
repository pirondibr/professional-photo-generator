# Setup Guide for Professional Photo Generator

## Quick Start

### Step 1: Set up n8n Workflow

1. **Import the workflow:**
   - Open your n8n instance
   - Click on "Workflows" in the left sidebar
   - Click the "+" button or "Add workflow"
   - Click on the three dots menu (⋮) and select "Import from File"
   - Select the `n8n-workflow.json` file

2. **Configure OpenRouter API Credentials:**
   - In the workflow, click on the "OpenRouter Gemini API" node
   - Under "Authentication", you need to set up HTTP Header Auth
   - Create a new credential or use an existing one:
     - **Name:** Authorization
     - **Value:** `Bearer YOUR_OPENROUTER_API_KEY`
   - Replace `YOUR_OPENROUTER_API_KEY` with your actual OpenRouter API key
   - You can get an API key from: https://openrouter.ai/keys
   - Also update the HTTP-Referer header with your website URL (optional but recommended)

3. **Activate the workflow:**
   - Click the "Active" toggle at the top right to activate the workflow
   - Copy the webhook URL that appears (it will look like: `https://your-n8n-instance.com/webhook/generate-photo`)

### Step 2: Configure the HTML Page

1. **Open `index.html` in a text editor**

2. **Find this line (around line 280):**
   ```javascript
   const webhookUrl = 'YOUR_N8N_WEBHOOK_URL_HERE';
   ```

3. **Replace it with your actual n8n webhook URL:**
   ```javascript
   const webhookUrl = 'https://your-n8n-instance.com/webhook/generate-photo';
   ```

4. **Save the file**

### Step 3: Test the Application

1. **Open `index.html` in a web browser:**
   - You can simply double-click the file, or
   - Use a local web server (recommended for production):
     ```bash
     # Using Python
     python -m http.server 8000
     
     # Using Node.js (if you have http-server installed)
     npx http-server
     ```

2. **Test the upload:**
   - Upload 1-5 photos
   - Optionally enter a custom prompt
   - Click "Generate Professional Photo"
   - Wait for the result

## Troubleshooting

### Issue: "CORS error" or "Network error"

**Solution:** If you're opening the HTML file directly (file://), you may encounter CORS issues. Use a local web server instead:
- Python: `python -m http.server 8000` then visit `http://localhost:8000`
- Node.js: `npx http-server` then visit the URL shown

### Issue: "401 Unauthorized" from OpenRouter

**Solution:** 
- Check that your OpenRouter API key is correct
- Make sure you have credits in your OpenRouter account
- Verify the API key format: `Bearer sk-or-v1-...`
- Check that the API key has the correct permissions

### Issue: "No image received"

**Solution:**
- Check the n8n workflow execution logs
- Verify the webhook URL is correct
- Make sure the workflow is activated (toggle should be ON)

### Issue: Files not uploading

**Solution:**
- Check browser console for errors (F12 → Console)
- Verify the webhook URL is accessible
- Make sure n8n workflow is active and the webhook path matches

## Environment Variables (Optional)

If you prefer using environment variables in n8n:

1. In n8n, go to Settings → Environment Variables
2. Add: `OPENROUTER_API_KEY` = `your-api-key-here`
3. The workflow will automatically use `{{ $env.OPENROUTER_API_KEY }}`

## Customization

### Change the default prompt:
Edit the workflow node "Process Upload & Convert Images" and modify the default prompt string.

### Change the model:
In the "Prepare OpenRouter Request" node, you can modify the model name in the JavaScript code:
- Current: `google/gemini-2.5-flash-image` (Gemini 2.5 Flash Image Nano Banana)
- Alternative: `google/gemini-2.0-flash-exp:free` (free tier option)

**Important:** The exact model name may vary. To find the correct model name:
1. Visit https://openrouter.ai/models
2. Search for "Gemini" or "Nano Banana"
3. Copy the exact model identifier
4. Update the model name in the "Prepare OpenRouter Request" node's JavaScript code

If you get a "model not found" error, try:
- `google/gemini-2.0-flash-exp:free`
- `google/gemini-flash-1.5`
- Or check OpenRouter's latest model list

## API Costs

OpenRouter pricing varies by model. Check OpenRouter's pricing page for current rates:
- Visit: https://openrouter.ai/models
- Look for Gemini 2.5 Flash Image (Nano Banana) pricing

Make sure you have sufficient credits in your OpenRouter account.

