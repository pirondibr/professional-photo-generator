# n8n Workflow Troubleshooting Guide

## Common Issues and Solutions

### Error 500: Internal Server Error

This usually means something is failing in the n8n workflow. Here's how to debug:

#### 1. Check the API Key
- Open the "OpenRouter Gemini API" node in n8n
- Make sure the Authorization header has your actual API key:
  - Replace `YOUR_OPENROUTER_API_KEY_HERE` with: `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`
- The format should be: `Bearer sk-or-v1-...`

#### 2. Check the Model Name
- The model name might be incorrect
- Try changing `google/gemini-2.5-flash-image` to one of these:
  - `google/gemini-2.0-flash-exp:free` (free tier)
  - `google/gemini-flash-1.5`
  - Check https://openrouter.ai/models for the latest model names

#### 3. Check n8n Execution Logs
1. In n8n, go to "Executions" in the left sidebar
2. Find the failed execution
3. Click on it to see detailed error messages
4. Check each node to see where it's failing:
   - **Webhook node**: Should show the incoming request
   - **Process Upload node**: Should show if files are being received
   - **OpenRouter API node**: Will show the API error if there is one

#### 4. Verify File Upload Handling
- The webhook should automatically handle multipart/form-data
- Check the "Process Upload & Convert Images" node output
- It should show:
  - `imageCount`: number of images processed
  - `prompt`: the prompt text
  - `images`: array of base64 image data

#### 5. Test the OpenRouter API Directly
You can test if your API key works by making a direct request:

```bash
curl https://openrouter.ai/api/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer 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" \
  -d '{
    "model": "google/gemini-2.0-flash-exp:free",
    "messages": [{
      "role": "user",
      "content": "Generate a professional headshot photo"
    }]
  }'
```

### Common Error Messages

#### "Model not found" or "Invalid model"
- The model name is incorrect
- Check OpenRouter's model list: https://openrouter.ai/models
- Update the model name in the "Prepare OpenRouter Request" node

#### "Unauthorized" or "401"
- API key is missing or incorrect
- Make sure it's in the format: `Bearer sk-or-v1-...`
- Check that the API key has credits

#### "No image data found in response"
- The API might not be returning images in the expected format
- Check the "rawResponse" in the error to see what OpenRouter actually returned
- Gemini models might return text descriptions instead of images
- You may need to use a different model that actually generates images

### Debugging Steps

1. **Enable Debug Mode in n8n:**
   - In the workflow, add a "Set" node after each step
   - Log the output to see what data is being passed

2. **Check Console Logs:**
   - The updated workflow includes console.log statements
   - Check n8n's execution logs to see these messages

3. **Test Without Images:**
   - Try sending a request without images first
   - This will help isolate if the issue is with file handling or the API call

4. **Verify Webhook URL:**
   - Make sure the webhook URL in index.html matches your n8n webhook
   - The webhook should be active (toggle should be ON)

### Quick Fixes

1. **Update API Key:**
   - In n8n, edit the "OpenRouter Gemini API" node
   - Update the Authorization header value

2. **Try a Different Model:**
   - In the "Prepare OpenRouter Request" node
   - Change the model to: `google/gemini-2.0-flash-exp:free`

3. **Check Credits:**
   - Go to https://openrouter.ai/keys
   - Make sure your account has credits

4. **Re-import Workflow:**
   - Delete the current workflow
   - Re-import `n8n-workflow.json`
   - Make sure to update the API key after importing

