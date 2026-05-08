---
description: "Speak the last response aloud via Kokoro TTS"
---

# Speak Last Response

Read your most recent response in this conversation (the message you wrote just before the user invoked /speak). Follow these rules:

1. If the response contains a `<!-- TTS_SUMMARY ... TTS_SUMMARY -->` block, extract **only** that inner text.
2. Otherwise, take the full response text but strip all:
   - Code blocks (fenced and inline)
   - URLs (replace with "a link")
   - File paths
   - Markdown formatting (headers, bold, italic, list markers)
   - Tool call descriptions
3. Keep the result under 5000 characters.
4. Pipe the cleaned text to `kokoro-speak` via Bash. Example:

```bash
echo "the cleaned text here" | kokoro-speak
```

5. Do **not** add any commentary before or after — just run the Bash command silently.
6. If `kokoro-speak` reports the daemon is not running, tell the user to check `launchctl list | grep kokoro`.
