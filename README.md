# Professional Photo Generator

A simple web application that allows users to upload up to 5 photos and generate professional images using OpenRouter's Gemini 2.5 Flash Image (Nano Banana) model through n8n.

## Features

- Upload up to 5 photos via drag & drop or file browser
- Custom prompt input for image generation
- Beautiful, modern UI
- Integration with n8n workflow
- Real-time preview of uploaded images

## Setup Instructions

### 1. Frontend (HTML Page)

1. Open `index.html` in a web browser or serve it using a local web server
2. Update the webhook URL in the JavaScript code:
   ```javascript
   const webhookUrl = 'YOUR_N8N_WEBHOOK_URL_HERE';
   ```
   Replace `YOUR_N8N_WEBHOOK_URL_HERE` with your actual n8n webhook URL

### 2. n8n Workflow Setup

1. **Import the workflow:**
   - Open n8n
   - Go to Workflows
   - Click "Import from File" or "Import from URL"
   - Select `n8n-workflow.json`

2. **Configure OpenRouter API:**
   - In the "OpenRouter Gemini API" node, set up your OpenRouter API credentials
   - You'll need an OpenRouter API key (get one from https://openrouter.ai/keys)
   - Create an HTTP Header Auth credential in n8n with:
     - Name: `Authorization`
     - Value: `Bearer YOUR_OPENROUTER_API_KEY`
   - Optionally set HTTP-Referer header with your website URL

3. **Update the Webhook URL:**
   - After deploying the workflow, copy the webhook URL
   - Update the `webhookUrl` variable in `index.html`

4. **Configure the Webhook Node:**
   - The webhook is set to accept POST requests at path `/generate-photo`
   - Make sure it's configured to accept file uploads (multipart/form-data)

### 3. How It Works

The workflow:
- Accepts multipart/form-data with up to 5 image files
- Converts uploaded images to base64 format
- Sends images and prompt to OpenRouter's Gemini 2.5 Flash Image model
- Uses either the `/api/v1/images/generations` endpoint (if available) or `/api/v1/chat/completions` with vision
- Returns the generated image URL to display on the website

## Usage

1. Open `index.html` in your browser
2. Upload up to 5 photos by dragging and dropping or clicking to browse
3. (Optional) Enter a custom prompt describing how you want your professional photo to look
4. Click "Generate Professional Photo"
5. Wait for the AI to generate your image
6. View the result on the page

## API Endpoints

### Webhook Endpoint
- **Method:** POST
- **Path:** `/generate-photo`
- **Content-Type:** multipart/form-data
- **Body:**
  - `image1`, `image2`, ..., `image5`: Image files (optional)
  - `prompt`: Text prompt for image generation (optional, has default)

### Response Format
```json
{
  "success": true,
  "imageUrl": "https://...",
  "revisedPrompt": "..."
}
```

## Notes

- The current workflow uses OpenRouter's Gemini 2.5 Flash Image (Nano Banana) model for image generation
- The workflow automatically converts uploaded images to base64 format for API submission
- The workflow tries to use the images/generations endpoint first, falling back to chat/completions if needed
- Make sure your OpenRouter API key has sufficient credits
- Check OpenRouter's documentation for the latest model names and API endpoints

## Troubleshooting

- **Images not uploading:** Check that the webhook URL is correct and the n8n workflow is active
- **API errors:** Verify your OpenRouter API key is valid and has sufficient credits. Check the workflow execution logs in n8n for detailed error messages
- **CORS issues:** If hosting the HTML file, you may need to configure CORS headers in n8n or use a proxy
- **Model not found:** Verify the model name `google/gemini-2.5-flash-image` is correct. Check OpenRouter's model list for the latest available models

