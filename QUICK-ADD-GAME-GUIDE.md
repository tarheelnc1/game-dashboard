# Quick Reference: Adding a Game

## 📝 Example: Adding "Lucky Number Game"

This guide shows exactly how to add the included sample game to your game browser.

---

## Step 1: The Game File is Ready ✅

The sample game is already created at:
```
/mnt/user-data/outputs/games/lucky-number-game.html
```

---

## Step 2: Add to Game Library

Open `game-dashboard-enhanced.html` and find this section (around line 290):

```javascript
const GAME_LIBRARY = [
    // ... existing games ...
    {
        id: 'true-false-quiz',
        title: 'True or False Quiz',
        description: 'Answer true or false questions and test your knowledge.',
        filename: 'true-false-quiz.html',
        category: 'quiz',
        difficulty: 'easy'
    }
];
```

Add this entry just before the closing `];`:

```javascript
const GAME_LIBRARY = [
    // ... existing games ...
    {
        id: 'true-false-quiz',
        title: 'True or False Quiz',
        description: 'Answer true or false questions and test your knowledge.',
        filename: 'true-false-quiz.html',
        category: 'quiz',
        difficulty: 'easy'
    },
    
    // NEW GAME ADDED HERE
    {
        id: 'lucky-number-game',
        title: 'Lucky Number Game',
        description: 'Generate random numbers and try to hit the lucky ones!',
        filename: 'lucky-number-game.html',
        category: 'action',
        difficulty: 'easy'
    }
];
```

**Important:** Don't forget the comma after the previous game entry!

---

## Step 3: Save and Test

1. **Save** the `game-dashboard-enhanced.html` file
2. **Open** it in your browser
3. **Login** with any username
4. **Click** "Browse All Games" button
5. **Find** "Lucky Number Game" in the list
6. **Click** "Play Now" to test it!

---

## 🎨 Understanding the Fields

```javascript
{
    id: 'lucky-number-game',        // ← Must match filename (without .html)
                                     //   Use lowercase with hyphens
    
    title: 'Lucky Number Game',      // ← Display name shown to users
                                     //   Can use capitals and spaces
    
    description: 'Generate random numbers and try to hit the lucky ones!',
                                     // ← Brief description (under 100 chars)
                                     //   Appears on game card
    
    filename: 'lucky-number-game.html',  // ← Exact filename in /games/ folder
                                         //   Must match exactly (case-sensitive!)
    
    category: 'action',              // ← Choose from:
                                     //   puzzle, action, memory, strategy, quiz
    
    difficulty: 'easy'               // ← Choose from:
                                     //   easy, medium, hard
}
```

---

## ✅ Verification Checklist

After adding the game, verify:

- [ ] No JavaScript errors in browser console (press F12)
- [ ] Game appears in "All" category
- [ ] Game appears when filtering by "Action" category
- [ ] Search finds game when typing "lucky" or "number"
- [ ] Game loads correctly when clicking "Play Now"
- [ ] "Back" button returns to game browser
- [ ] Game difficulty badge shows "EASY" in green

---

## 🔍 Finding the Right Category

**Is your game...**

| If the game is... | Use category... |
|------------------|-----------------|
| About solving problems, logic, calculations | `puzzle` |
| Fast-paced, requires quick reactions | `action` |
| Tests recall or pattern recognition | `memory` |
| Requires planning or prediction | `strategy` |
| Knowledge-based or trivia | `quiz` |

For "Lucky Number Game":
- It's about generating numbers quickly ✅
- Requires repeated attempts
- Has luck/chance element
- → **Category: `action`** ✅

---

## 📊 Complete Updated Array Example

Here's what your GAME_LIBRARY should look like after adding the game:

```javascript
const GAME_LIBRARY = [
    // Existing 10 games...
    {
        id: 'high-low-card',
        title: 'High-Low Card Game',
        description: 'Guess if the next card will be higher or lower. Build a winning streak!',
        filename: 'high-low-card.html',
        category: 'strategy',
        difficulty: 'easy'
    },
    // ... 8 more existing games ...
    {
        id: 'true-false-quiz',
        title: 'True or False Quiz',
        description: 'Answer true or false questions and test your knowledge.',
        filename: 'true-false-quiz.html',
        category: 'quiz',
        difficulty: 'easy'
    },
    
    // Your new game
    {
        id: 'lucky-number-game',
        title: 'Lucky Number Game',
        description: 'Generate random numbers and try to hit the lucky ones!',
        filename: 'lucky-number-game.html',
        category: 'action',
        difficulty: 'easy'
    }
];
```

Total games: **11** (10 original + 1 new)

---

## 🚫 Common Mistakes

❌ **WRONG - Missing comma:**
```javascript
{
    id: 'true-false-quiz',
    // ...
}                           // ← Missing comma here!
{
    id: 'lucky-number-game',
```

✅ **CORRECT - With comma:**
```javascript
{
    id: 'true-false-quiz',
    // ...
},                          // ← Comma here!
{
    id: 'lucky-number-game',
```

---

❌ **WRONG - ID doesn't match filename:**
```javascript
{
    id: 'lucky-game',              // ← Doesn't match filename!
    filename: 'lucky-number-game.html',
```

✅ **CORRECT - ID matches filename:**
```javascript
{
    id: 'lucky-number-game',       // ← Matches filename (without .html)
    filename: 'lucky-number-game.html',
```

---

❌ **WRONG - Invalid category:**
```javascript
{
    category: 'luck',              // ← Not a valid category!
```

✅ **CORRECT - Valid category:**
```javascript
{
    category: 'action',            // ← One of: puzzle, action, memory, strategy, quiz
```

---

## 🎯 Testing Your Game

### Test 1: Search Function
1. Click "Browse All Games"
2. Type "lucky" in search box
3. Game should appear ✅

### Test 2: Category Filter
1. Click "All" - game appears ✅
2. Click "Action" - game appears ✅
3. Click "Puzzle" - game disappears ✅
4. Click "All" again - game reappears ✅

### Test 3: Play Game
1. Click on game card or "Play Now" button
2. Game loads in iframe ✅
3. Click "Generate Lucky Number" - game works ✅
4. Click "Back" button - returns to browser ✅

---

## 🎉 Success!

If all tests pass, you've successfully added your first game to the browser!

Now you can add as many games as you want using the same process:
1. Create HTML file in `/games/` folder
2. Add entry to `GAME_LIBRARY` array
3. Save and test

**Happy gaming!** 🎮
