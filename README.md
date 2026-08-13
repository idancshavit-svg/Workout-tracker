# Workout Tracker

A personal workout tracking web application with a premium, futuristic interface. Track your workouts, record actual performance, and view historical data—all locally in your browser.

## Features

### Workout Management
- **Create multiple workout plans** (Full Body, Yoga, Calisthenics, etc.)
- **Organize workouts within plans** (e.g., Workout A, Workout B)
- **Add custom exercises** with your own names
- **Three exercise types**:
  - **Weighted**: Sets × Reps @ Weight (e.g., 3 × 8 @ 60kg)
  - **Bodyweight**: Sets × Reps (e.g., 3 × 10 pull-ups)
  - **Time-based**: Sets × Duration (e.g., 3 × 30s handstand hold)

### Workout Sessions
- **Start workout from any plan** — creates a snapshot of the plan
- **Fast set recording** — optimized for one-handed mobile input
- **Per-set tracking** — record weight/reps/duration for each set individually
- **Auto-save** — data persists if you close the browser
- **Resume workouts** — pick up where you left off

### Historical Data
- **Immutable workout history** — editing plans never affects past workouts
- **View completed workouts** — chronological list with details
- **See exactly what you did** — each set's actual performance preserved

### Design
- **Premium dark theme** with subtle glass effects
- **Apple-inspired aesthetic** with futuristic accents
- **Mobile-first** — optimized for iPhone
- **Responsive** — works great on desktop too
- **Smooth animations** and micro-interactions

### Progressive Web App (PWA)
- **Add to home screen** on iOS/Android
- **Works offline** after installation
- **Fast loading** with service worker caching
- **App-like experience** without an app store

## Architecture

### Data Model

**Plans** → **Workouts** → **Exercises**
```
WorkoutPlan (mutable)
├── name: "Full Body"
├── workouts:
│   ├── PlannedWorkout "Workout A"
│   │   └── exercises:
│   │       ├── Bench Press (3 sets, 8 reps, 60kg)
│   │       └── Squat (4 sets, 6 reps, 80kg)
│   └── PlannedWorkout "Workout B"
│       └── exercises: [...]
```

**Sessions** (immutable snapshots)
```
WorkoutSession (created when you start a workout)
├── id: unique session ID
├── planName: "Full Body"
├── workoutName: "Workout A"
├── startedAt: timestamp
├── exercises: [
│   ├── SessionExercise (snapshot of Bench Press at workout time)
│   │   └── completedSets: [
│   │       ├── {weight: 60, reps: 8, completed: true}
│   │       ├── {weight: 62.5, reps: 8, completed: true}
│   │       └── {weight: 62.5, reps: 7, completed: true}
│   └── ...
├── completedAt: timestamp
├── inProgress: false
```

### Key Architectural Principle: Immutable History

When you start a workout, the app **snapshots the current plan** into a `WorkoutSession`. This snapshot is completely independent of the plan.

**Example:**
1. Create plan: "Bench Press 3 × 8 @ 60kg"
2. Start workout → creates session with snapshot
3. Complete workout, save session
4. Edit plan: "Bench Press 4 × 6 @ 70kg"
5. View historical session → **still shows 3 × 8 @ 60kg**

This ensures perfect historical accuracy.

### Storage

- **Browser localStorage** for persistent storage
- **Automatic save** on every change
- **No account needed** — purely local
- **Data stays on your device**

## How to Use

### Getting Started

1. **Open the app** on your phone or computer
2. **Create a plan** by clicking "Create Plan"
3. **Add a workout** to the plan (e.g., "Workout A")
4. **Add exercises** to the workout
5. **Save the plan**

### Creating an Exercise

1. Click "Add Exercise"
2. Enter the exercise name (e.g., "Bench Press")
3. Choose the type:
   - **Weighted**: specify weight in kg
   - **Bodyweight**: just sets and reps
   - **Time-based**: specify duration in seconds
4. Set target values (sets, reps/duration, weight)

### Starting a Workout

1. Go to Home
2. Tap a plan card to view its workouts
3. Tap "Start Workout" on any workout
4. You'll see all exercises with their targets
5. For each set, enter your actual performance:
   - Weight lifted (if applicable)
   - Reps achieved (if applicable)
   - Duration (if applicable)
6. Tap "Complete Set" to mark it done
7. When all sets are done, tap "Finish Workout"

### Recording Sets

The set entry interface is optimized for speed:
- Large, easy-to-tap input fields
- Clear visual feedback when sets are completed
- One-handed iPhone operation
- Haptic feedback (if supported)

**For weighted exercises:**
- Enter weight per set (can vary from your target)
- Enter reps achieved
- Complete

**For bodyweight exercises:**
- Just enter reps

**For time-based:**
- Just enter duration in seconds

### Viewing History

1. Go to History tab (bottom navigation)
2. See all completed workouts in reverse chronological order
3. Tap any workout to see details
4. View each exercise with all sets you actually completed

### Editing Plans

Edit your plans without affecting historical data:
- Rename workouts
- Add/remove exercises
- Change target weight, reps, or duration
- Reorder exercises

Past workouts remember exactly what you did at the time.

## Deployment & Installation

### Local Testing

1. Save all three files to a folder:
   - `index.html`
   - `manifest.json`
   - `sw.js`

2. Open `index.html` in a browser
   - For full PWA features, serve over HTTPS (or localhost)

### Production Deployment

**GitHub Pages:**
1. Create a GitHub repo
2. Enable GitHub Pages
3. Upload the three files
4. Access at `https://yourusername.github.io/workout-tracker`

**Netlify:**
1. Drag and drop the folder to Netlify
2. Configure as a SPA (single page app)
3. Get a live URL instantly

**Your Own Server:**
1. Upload files to any web server
2. Ensure HTTPS (required for PWA)
3. Add proper CORS headers if needed

### iOS Installation (PWA)

1. Open the app in Safari
2. Tap the share button
3. Tap "Add to Home Screen"
4. Give it a name (or keep "Workout")
5. Tap "Add"

The app will now appear as an icon on your home screen and work offline.

### Android Installation (PWA)

1. Open the app in Chrome
2. Tap the menu button (three dots)
3. Tap "Install app" or "Add to Home Screen"
4. Confirm

## Technical Stack

- **Framework**: React 18 (CDN, no build step needed)
- **Storage**: Browser localStorage
- **PWA**: Web App Manifest + Service Worker
- **Styling**: CSS with CSS variables
- **Bundle**: Single HTML file (index.html)

## Browser Compatibility

- ✅ iOS 14.7+ (Safari)
- ✅ Android 6+ (Chrome)
- ✅ macOS (Safari, Chrome, Firefox)
- ✅ Windows (Chrome, Edge, Firefox)

## Data Privacy

- **100% local storage** — nothing sent to servers
- **No tracking** — no analytics
- **No accounts** — no authentication
- **Completely private** — your data stays on your device

## Future Enhancements (Not in v1)

- Progression suggestions (e.g., "next time try 65kg")
- Advanced analytics (charts, trends)
- Exercise history filtering
- Backup/export data as JSON
- Rest timers between sets
- Multiple profiles (if desired)

## File Structure

```
├── index.html      # Main app (React + UI)
├── manifest.json   # PWA configuration
├── sw.js          # Service worker (offline support)
└── README.md      # This file
```

## Troubleshooting

### Data not saving?
- Check that localStorage is enabled in your browser
- Try a different browser
- Clear browser cache if corrupted

### Not working offline?
- Make sure you've visited the app once (to cache files)
- Service worker may take 30 seconds to activate
- Check browser console for errors

### PWA not installing on iOS?
- Open in Safari (not Chrome)
- Make sure site is served over HTTPS
- Try "Add to Home Screen" from share menu

### Performance issues?
- App should be very fast (single HTML file)
- If slow, clear browser cache
- Check for browser extensions that might interfere

## Support

This is a personal project for your own use. No external support needed—all code is self-contained.

For issues or ideas:
1. Check the troubleshooting section
2. Open browser developer console (F12) for error messages
3. Review the architecture section to understand how data flows

---

**Enjoy tracking your workouts! 💪**

Made with ❤️ for beautiful, functional fitness tracking.
