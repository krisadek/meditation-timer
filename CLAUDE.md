# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Default Language
**English (en-US)** - Use English as the default language for all user-facing content, code comments, and documentation unless explicitly requested otherwise by the user.

## Project Overview
Single-page meditation timer application (`index.html`) with guided meditation, voice synthesis, and ambient soundscapes. No build process, no dependencies.

## Architecture

### Core Structure
The app is a **single self-contained HTML file** with three main systems:

1. **Audio Engine** (Web Audio API) - Lines ~500-700
   - `playGong()` - Synthesized gong with inharmonic overtones
   - `createNoise()` - Procedural noise generation (rain/ocean/wind)
   - `startGuidedAudio()` - Theme-specific ambient soundscapes
   - `stopGuidedAudio()` - Fade out and cleanup

2. **Speech Synthesis** (Web Speech API) - Lines ~566-630
   - **Queue-based system** to prevent overlapping speech
   - `speakNow()` - Core speech function (synchronous, no cancel)
   - `queueSpeech()` - Adds to queue
   - `processQueue()` - Processes queue one utterance at a time
   - `stopAllSpeech()` - Clears queue and cancels speech
   - **CRITICAL**: Never call `speechSynthesis.cancel()` in `speakNow()` - it causes "canceled" errors

3. **Meditation Script Scheduler** - Lines ~1125-1170
   - `parseScript()` - Parses `[MM:SS]` timestamps and `[PAUSE X]` markers
   - `scheduleGuidedMeditation()` - Creates setTimeout events for timed speech
   - `meditationScripts` object - Embedded scripts for nb-NO and en-US
   - Currently embedded: relaxation-5, relaxation-10, sleep-10, focus-10

### Data Flow (Guided Meditation)
```
User clicks Start
  → gStartBtn handler checks if wasReset
  → Calls scheduleGuidedMeditation(scriptKey, duration)
    → parseScript() extracts timing events from script text
    → Creates setTimeout() for each event
    → Each timeout calls speak(text)
      → queueSpeech() adds to queue
      → processQueue() speaks one at a time
```

### Key State Variables
- `guidedMeditationEnabled` - Master toggle for voice guidance (default: true)
- `voiceEnabled` - Global voice on/off (controlled by top toggle)
- `currentLang` - 'en-US' or 'nb-NO' (default: 'en-US')
- `speechQueue` - Array of pending speech utterances
- `isSpeaking` - Boolean flag to prevent overlap
- `scheduledEvents` - Array of setTimeout IDs for meditation scripts

## Development

### Testing Locally
```bash
# Open in browser
open index.html

# Or use local server if needed
python3 -m http.server 8000
```

### Deployment
Automatic via Vercel + GitHub integration:
```bash
git add .
git commit -m "Description"
git push origin main
# Vercel deploys automatically
```

Manual deploy:
```bash
vercel --prod
```

### Adding Guided Meditation Scripts

**In `meditation-scripts/` folder** (for reference):
- Text files with `[MM:SS]` markers and `[PAUSE X]` indicators
- Not loaded by app, just documentation

**In `index.html`** (actually used by app):
1. Find `meditationScripts` object (~line 664)
2. Add script for both `'nb-NO'` and `'en-US'`
3. Update `available` object in `getScriptKey()` function (~line 1502)
4. Script key format: `'theme-duration'` (e.g., `'relaxation-10'`)

Example:
```javascript
const meditationScripts = {
    'en-US': {
        'relaxation-15': `[0:00] Welcome...
[0:30] First instruction...
[PAUSE 10]
[1:00] Next instruction...`
    }
};

// Then update getScriptKey:
const available = {
    'relaxation': [5, 10, 15],  // Added 15
    'sleep': [10],
    'focus': [10]
};
```

### Speech Synthesis Debugging

**Common Issues:**
- "Speech error: canceled" → Something is calling `cancel()` inappropriately
- No sound but `speak()` called → Check browser autoplay settings or site permissions
- Queue not processing → Check `isSpeaking` flag and `onend` handler

**Debug in Console:**
```javascript
// Check queue state
speechQueue
isSpeaking

// Manual test
queueSpeech("Test message")

// Check voices
window.speechSynthesis.getVoices()
```

### Adding Philosophical Quotes
Quotes are loaded from `philosophical-quotes.json` on page load. To add/modify:
1. Edit `philosophical-quotes.json`
2. Follow existing format: `{ "text": "...", "author": "...", "era": "..." }`
3. New quote displays on page refresh (random selection)

## Browser Compatibility Notes

### Chrome
- **Autoplay**: Strict - requires user gesture
- **Voices**: ~200 available, good quality
- **Issue**: May block `speechSynthesis.speak()` if called from `setTimeout` without prior user interaction

### Safari
- **Voices**: Best quality (Samantha, Nora)
- **Issue**: May require explicit autoplay permission

### Firefox
- **Voices**: Limited selection
- **Audio**: Full Web Audio API support

## Critical Implementation Details

### Why Queue System for Speech
Browsers cancel pending utterances if `speak()` is called while another is playing. Queue ensures sequential playback.

### Why No `cancel()` in `speak()`
Calling `cancel()` before `speak()` in a scheduled context causes immediate cancellation. Only call `cancel()` when actually stopping meditation (pause/reset).

### Meditation Script Timing
Scripts use `setTimeout(callback, event.time * 1000)` where `event.time` is seconds from start. NOT relative delays between events.
