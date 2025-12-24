# Fix for n8n Binary Data "filesystem-v2" Error

## Problem
When n8n stores binary data in "filesystem" mode, the Code node receives references like "filesystem-v2" instead of actual base64 data, causing the error:
```
Base64 decoding failed for "filesystem-v2"
```

## Solution Options

### Option 1: Change n8n Binary Data Mode (Recommended)

1. In n8n, go to **Settings** → **Workflows**
2. Find the setting for **"Binary Data Mode"** or **"Binary Data Storage"**
3. Change it from **"filesystem"** to **"inline"** or **"database"**
4. This will store binary data directly in the workflow execution instead of filesystem references

### Option 2: Use HTTP Request Node to Read Binary Data

Instead of using a Code node, you can:
1. Use the HTTP Request node's ability to handle binary data directly
2. Or use a "Read Binary File" node if available

### Option 3: Update the Code Node (Current Implementation)

The current workflow tries to handle both modes, but if you're still getting the error:

1. **Check n8n Settings:**
   - Go to your n8n instance settings
   - Look for "Binary Data" or "File Storage" settings
   - Ensure binary data is stored inline or in database, not filesystem

2. **Alternative: Use Base64 Encoding Node**
   - Add a node that converts binary to base64 before the Code node
   - Some n8n versions have a "Convert to Base64" node

### Option 4: Manual Workaround

If you can't change the binary mode, you might need to:
1. Use n8n's built-in binary handling in HTTP Request nodes
2. Or configure the webhook to accept base64-encoded images directly from the frontend

## Quick Fix

The easiest solution is to change n8n's binary data storage mode to "inline" in your n8n instance settings. This will make binary data available directly in Code nodes as base64 strings.

