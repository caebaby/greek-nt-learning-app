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

## Current Curriculum

Based on Biblingo lessons 1-8:
- 30+ vocabulary words with proper declensions
- Present Active & Medio-Passive Indicative
- Articles, pronouns, εἰμί conjugations
- Deponent verbs and paradigm practice

## Usage

1. **Vocabulary Mode**: See Greek word → recall meaning → reveal answer → mark known/unknown
2. **Paradigm Mode**: Identify cases, persons, numbers with multiple choice
3. **Add Words**: Smart auto-detection or manual entry with lesson document upload

## Technology

- **Frontend**: Vanilla JavaScript, CSS3, HTML5
- **Storage**: Browser localStorage with export/import capability
- **Deployment**: Railway with GitHub integration
- **Mobile**: Progressive Web App with offline support

## Development

```bash
# Local development
npm run dev

# Deploy to Railway
git push origin main
