# Greek NT Learning App

A modern Greek New Testament learning application with advanced spaced repetition, active recall, and smart analytics. Built to complement Biblingo curriculum.

## Features

- **Vocabulary Practice** with active recall and hidden answers
- **Paradigm Practice** for verb forms, noun cases, articles, and pronouns  
- **Advanced Spaced Repetition** using SuperMemo 2 algorithm
- **Smart Analytics** with personalized recommendations
- **Progressive Web App** optimized for mobile learning
- **Auto-Detection** for word types and declensions
- **Lesson Document Upload** for automatic vocabulary import
- **Persistent Memory** - never lose your progress!
- **Auto-Save System** - saves progress every 30 seconds
- **Backup & Restore** - export/import your complete progress

## Current Curriculum

Based on Biblingo lessons 1-8:
- 30+ vocabulary words with proper declensions
- Present Active & Medio-Passive Indicative
- Articles, pronouns, εἰμί conjugations
- Deponent verbs and paradigm practice

## Memory & Progress System

### Automatic Saving
- **Auto-save every 30 seconds** while you're using the app
- **Saves when you switch tabs** or minimize the browser
- **Saves before closing** the browser window
- **Remembers your exact position** in vocabulary and paradigms

### What Gets Saved
- All vocabulary words and your progress on each one
- All paradigm forms and your accuracy rates
- Your session statistics (correct/incorrect answers)
- Category performance (articles, verbs, etc.)
- Current study mode and settings
- Daily progress tracking

### Manual Backup Options
- **Export Progress**: Download a complete backup file
- **Import Progress**: Restore from a backup file
- **Storage Status**: See when data was last saved
- **Manual Save**: Force save your current progress

### If You Lose Progress
1. Check the "Last Saved" time in the Memory Management section
2. Try refreshing the page - data should restore automatically
3. If problems persist, import from your last backup file
4. Contact support with details about when the issue occurred

## Usage

1. **Vocabulary Mode**: See Greek word → recall meaning → reveal answer → mark known/unknown
2. **Paradigm Mode**: Identify cases, persons, numbers with multiple choice
3. **Add Words**: Smart auto-detection or manual entry with lesson document upload
4. **Memory Management**: Monitor saves, backup progress, clear data if needed

## Technology

- **Frontend**: Vanilla JavaScript, CSS3, HTML5
- **Storage**: Browser localStorage with automatic backup/restore
- **Database**: Optional cloud sync (Supabase integration planned)
- **Deployment**: Railway with GitHub integration
- **Mobile**: Progressive Web App with offline support

## Data Storage Options

### Current: Browser Storage (localStorage)
- ✅ **Works offline**
- ✅ **Fast and responsive**
- ✅ **No account required**
- ⚠️ **Limited to one device**
- ⚠️ **Can be lost if browser data is cleared**

### Planned: Cloud Database (Supabase)
- ✅ **Sync across multiple devices**
- ✅ **Never lose progress**
- ✅ **Backup in the cloud**
- ✅ **Share progress with teachers**
- ⚠️ **Requires internet connection**
- ⚠️ **Account creation needed**

## Development

```bash
# Local development
npm run dev

# Deploy to Railway
git push origin main
```

## Future Enhancements

### Version 1.1 (Next Release)
- [ ] Enhanced memory system with auto-save
- [ ] Backup/restore functionality
- [ ] Storage status monitoring
- [ ] Data validation and repair

### Version 1.2 (Cloud Integration)
- [ ] Supabase database integration
- [ ] User accounts and authentication
- [ ] Cross-device synchronization
- [ ] Teacher/student progress sharing

### Version 1.3 (Advanced Features)
- [ ] Offline-first with cloud sync
- [ ] Lesson plan integration
- [ ] Advanced analytics dashboard
- [ ] Community vocabulary sharing

## Database Architecture (Planned)

### Tables Structure
```sql
-- Users table
users (
  id, email, created_at, last_login,
  settings (json), daily_goal
)

-- Vocabulary progress
vocabulary_progress (
  user_id, word_id, correct_count,
  attempt_count, last_reviewed,
  ease_factor, interval_days
)

-- Paradigm progress  
paradigm_progress (
  user_id, paradigm_id, correct_count,
  attempt_count, last_reviewed,
  ease_factor, interval_days
)

-- Session tracking
study_sessions (
  user_id, session_date, duration,
  words_studied, accuracy_rate,
  mode_used
)
```

## Support

- **Technical Issues**: Check browser console for error messages
- **Data Loss**: Use export/import backup system
- **Feature Requests**: Submit via GitHub issues
- **General Questions**: Contact through app feedback system
