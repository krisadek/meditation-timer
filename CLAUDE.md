# Meditation Timer Project

## Default Language
**English (en-US)** - Use English as the default language for all user-facing content, code comments, and documentation unless explicitly requested otherwise by the user.

## Project Overview
An interactive meditation timer web application with guided meditation features, voice synthesis, and bilingual support.

## Key Features
- **Guided Mode**: Three meditation themes (Relaxation, Sleep, Focus) with ambient soundscapes
- **Self-Guided Mode**: Customizable timer with optional background sounds
- **Voice Guidance**: Web Speech API integration for spoken meditation instructions
- **Bilingual Support**: English and Norwegian (Norsk) languages
- **Evidence-Based Scripts**: 20 professional meditation scripts based on scientific research
- **No Dependencies**: Pure HTML/CSS/JavaScript - no build process required

## Project Structure
```
.
├── index.html                 # Main meditation timer application
├── guessing_game.py          # Simple Python number guessing game
├── meditation-scripts/       # Meditation scripts directory
│   ├── README.txt           # Documentation on techniques and research
│   ├── en/                  # English meditation scripts
│   │   ├── relaxation-*.txt
│   │   ├── sleep-*.txt
│   │   ├── focus-*.txt
│   │   └── mindfulness-*.txt
│   └── no/                  # Norwegian meditation scripts
│       └── (same structure as en/)
└── CLAUDE.md                # This file
```

## Technical Stack
- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **Audio**: Web Audio API (for sound generation)
- **Speech**: Web Speech API (for text-to-speech)
- **Deployment**: Vercel
- **Version Control**: GitHub

## Audio Implementation
### Web Audio API
- **Gong sound**: Synthesized using oscillators with inharmonic overtones
- **Ambient soundscapes**: Theme-specific (relaxation, sleep, focus) using layered sine waves
- **Background sounds**: Procedurally generated noise (rain, ocean, wind)

### Web Speech API
- **Voice settings**: Slower rate (0.85), lower pitch (0.9) for calming effect
- **Language support**: Automatic voice selection based on current language
- **Script integration**: Timed speech events synchronized with meditation timer

## Meditation Scripts
All scripts are evidence-based and include:
- Precise timing markers `[MM:SS]`
- Pause indicators `[PAUSE X]`
- Body scan techniques
- Breathing exercises (e.g., 4-7-8 technique)
- Progressive muscle relaxation
- Guided imagery
- Mindfulness practices

Scientific techniques referenced:
- Body scan meditation (reduces cortisol)
- Diaphragmatic breathing (activates parasympathetic nervous system)
- Progressive muscle relaxation (reduces anxiety and depression)
- Guided imagery (improves sleep quality)

## Development Guidelines

### Code Style
- Use clear, descriptive variable names
- Add comments for complex logic
- Keep functions focused and single-purpose
- Prefer vanilla JavaScript over libraries
- Use ES6+ features where appropriate

### Adding New Features
When adding features to index.html:
1. Maintain the existing code structure
2. Keep the self-contained nature (no external dependencies)
3. Test across browsers (especially Safari for audio)
4. Ensure mobile responsiveness
5. Update this CLAUDE.md file if architecture changes

### Adding Meditation Scripts
To add new meditation scripts:
1. Follow the existing format with timing markers
2. Include evidence-based techniques
3. Add both English and Norwegian versions
4. Update the `meditationScripts` object in index.html
5. Update the `getScriptKey()` function to map themes/durations

## Deployment
- **Platform**: Vercel
- **Production URL**: https://meditation-timer-one.vercel.app
- **GitHub**: https://github.com/krisadek/meditation-timer

### Deploy Commands
```bash
# Deploy to production
vercel --prod

# Preview deployment
vercel
```

## Git Workflow
```bash
# Make changes
git add .
git commit -m "Description of changes"
git push origin main

# Deploy to Vercel
vercel --prod
```

## Future Enhancements (Ideas)
- [ ] Add 15-min and 20-min guided meditation scripts to JavaScript
- [ ] Add visual meditation guide (breathing circle animation)
- [ ] Add session history/tracking
- [ ] Add customizable meditation themes
- [ ] Add meditation journal feature
- [ ] Add progressive meditation course/program
- [ ] Add background music options
- [ ] Mobile app version (PWA)

## Browser Compatibility
- **Chrome/Edge**: Full support
- **Firefox**: Full support
- **Safari**: Full support (note: autoplay restrictions apply)
- **Mobile**: Responsive design, touch-friendly

## Voice Availability by Browser
- **Chrome**: Good Norwegian and English voices
- **Safari**: Excellent (Samantha for English, Nora for Norwegian)
- **Firefox**: Limited voice selection
- **Edge**: Good Microsoft Neural voices

## Important Notes
- Web Speech API requires user interaction to start (browsers block autoplay)
- First speech synthesis may take 1-2 seconds to load voices
- Offline functionality: App works offline after first load (except voice synthesis may be limited)

## License
Not specified - Add license information if needed

## Contributing
This is a personal project. If you want to contribute or fork, please contact the repository owner.
