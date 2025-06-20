# Ollama Setup Guide for rename-screenshot

## Step 1: Install Ollama

```bash
# Install Ollama on macOS
brew install ollama

# Start Ollama service (runs in background)
ollama serve
```

## Step 2: Pull the Vision Model

```bash
# Download LLaVA (vision model for image analysis)
ollama pull llava

# This downloads ~4.5GB model that can understand images
# First run takes a few minutes to download
```

## Step 3: Test Ollama

```bash
# Verify it's working
ollama list
# Should show: llava model

# Test with a simple prompt
ollama run llava "Hello"
```

## Step 4: Build rename-screenshot

```bash
cd /Users/adamgoyer/Library/Mobile\ Documents/com~apple~CloudDocs/Lotus\ Oak\ Family\ of\ Companies/root/personal-projects/rename-screenshot

# Install dependencies
npm install

# Build the TypeScript code
npm run build
```

## Step 5: Run rename-screenshot

```bash
# Basic usage - watch Desktop for new screenshots
./bin/rename-screenshot.js --provider ollama --watch --watchdir ~/Desktop

# Process existing screenshots + watch for new ones
./bin/rename-screenshot.js --provider ollama --retroactive --watch --watchdir ~/Desktop

# Custom output directory
./bin/rename-screenshot.js --provider ollama --watch --watchdir ~/Desktop --outdir ~/Documents/OrganizedScreenshots
```

## Configuration (Optional)

Edit `user.config.json` to customize categories:

```json
{
  "categories": {
    "code": "the majority of the text is computer code",
    "reference": "the image is a photograph",
    "web": "the image shows a webpage",
    "documentation": "the image contains documentation or text",
    "design": "the image shows UI/UX designs or mockups",
    "other": "the image doesn't belong to other categories"
  },
  "ollama": {
    "baseURL": "http://localhost:11434/v1/",
    "model": "llava",
    "maxTokens": 30
  }
}
```

## Expected Behavior

1. **New screenshots** on Desktop are automatically:
   - Analyzed by local AI
   - Renamed with descriptive names
   - Moved to categorized folders
   - Original backed up to `Screenshots/original/`

2. **Example transformation**:
   - Before: `Screenshot 2025-06-19 at 2.15.30 PM.png`
   - After: `250619-vscode-terminal-output.png` (in `code/` folder)

## Troubleshooting

- **"Cannot connect to Ollama"**: Make sure `ollama serve` is running
- **"Model not found"**: Run `ollama pull llava` again
- **Slow processing**: First run loads model into memory (normal)
- **Wrong categorization**: Edit categories in `user.config.json`

## Privacy Note

✅ All processing happens locally
✅ No screenshots sent to cloud
✅ No API keys required
✅ Completely free to use