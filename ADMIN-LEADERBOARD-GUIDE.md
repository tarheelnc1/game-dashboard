# Admin Controls & Leaderboards - Complete Guide

## 🎯 What's New?

Your enhanced game dashboard now includes:

### ⚙️ **Admin Controls**
- Enable/disable individual games
- Enable/disable entire game categories
- Control which games users can see and play
- Bulk enable/disable all games

### 🏆 **Per-Game Leaderboards**
- Automatic score tracking for each game
- Top 10 leaderboards for every game
- User rankings and personal bests
- Admin view of all scores across all games

---

## 📂 File Overview

| File | Purpose |
|------|---------|
| `admin-dashboard-enhanced.html` | Admin panel for managing games and viewing leaderboards |
| `game-dashboard-enhanced.html` | User-facing game browser with leaderboards |
| `games/lucky-number-game.html` | Sample game with score reporting |

---

## ⚙️ Admin Dashboard

### Accessing the Admin Panel

1. **From Game Dashboard**: Click the floating "⚙️ Admin" button (bottom right)
2. **Direct URL**: Open `admin-dashboard-enhanced.html` in your browser

### Features

#### 📊 **Statistics Overview**
At the top of the admin panel, you'll see:
- **Total Games** - All games in the library
- **Active Games** - Currently enabled games
- **Total Players** - Registered users
- **Total Scores** - All recorded scores

#### 🎮 **Game Management Tab**

**View All Games Organized by Category:**
- Puzzle Games 🧩
- Action Games ⚡
- Memory Games 🧠
- Strategy Games ♟️
- Quiz Games ❓

**Category Controls:**
```
Enable/Disable entire categories at once
Toggle affects all games in that category
```

**Individual Game Controls:**
- Each game has its own enable/disable toggle
- Games show:
  - Title and description
  - Difficulty level badge
  - Game ID
  - Current status (enabled/disabled)

**Bulk Actions:**
- **✓ Enable All** - Turn on all games and categories
- **✗ Disable All** - Turn off all games (requires confirmation)

#### 🏆 **Leaderboards Tab**

**View Scores for All Games:**
- Filter by specific game or view all
- Sort by highest score or most recent
- See top 10 for each game
- Track total plays per game

**Leaderboard Features:**
- Rank badges (🥇 Gold, 🥈 Silver, 🥉 Bronze)
- Player names
- Scores
- Submission dates

**Score Management:**
- **Clear All Scores** - Remove all leaderboard data (requires double confirmation)

#### 👥 **User Management Tab**

**View All Registered Users:**
- Username
- Join date
- Actions: Delete user

**Delete User:**
- Removes user account
- Also removes all their scores from leaderboards
- Requires confirmation

---

## 🎮 How Game Visibility Works

### Default Behavior
When you first open the system:
- ✅ All games are **enabled** by default
- ✅ All categories are **enabled** by default
- ✅ Users can see and play all games

### Disabling Games

#### Option 1: Disable Individual Game
1. Go to Admin Dashboard → Game Management
2. Find the game you want to disable
3. Toggle the switch for that game OFF
4. **Result**: Game disappears from user's game browser

#### Option 2: Disable Entire Category
1. Go to Admin Dashboard → Game Management
2. Find the category section (e.g., "Action Games")
3. Toggle the category switch OFF
4. **Result**: All games in that category become disabled

#### Option 3: Disable All Games
1. Go to Admin Dashboard → Game Management
2. Click "✗ Disable All" button
3. Confirm twice
4. **Result**: Game browser shows "No games available" message

### Re-Enabling Games

**To enable individual game:**
- Toggle the game's switch back ON

**To enable category:**
- Toggle the category switch back ON
- All games in that category become enabled

**To enable all games:**
- Click "✓ Enable All" button
- All games and categories turn ON instantly

### What Users See

**When games are disabled:**
```
User searches/filters in game browser
↓
System checks admin settings
↓
Only enabled games are shown
↓
Disabled games are completely hidden
```

**User attempts to play disabled game:**
- Gets error message: "This game is currently disabled by an administrator"

---

## 🏆 Leaderboard System

### How Scores Are Tracked

#### 1. Game Reports Score
When a player achieves something in a game (e.g., wins, completes level, gets high score):

```javascript
// Inside the game file (e.g., lucky-number-game.html)
window.parent.postMessage({
    type: 'GAME_SCORE',
    gameId: 'lucky-number-game',  // Must match game ID in GAME_LIBRARY
    score: 500                      // Numeric score value
}, window.location.origin);
```

#### 2. Dashboard Receives Score
The game dashboard listens for these messages and:
- Verifies the user is logged in
- Records: username, score, timestamp, game ID
- Saves to localStorage
- Updates the leaderboard display

#### 3. Leaderboard Updates
- Scores are automatically sorted (highest first)
- Top 10 scores are displayed
- Current user's scores are highlighted
- Personal best is shown

### Score Storage Format

```javascript
{
  "lucky-number-game": [
    {
      "username": "player1",
      "score": 750,
      "timestamp": "2024-12-24T10:30:00.000Z"
    },
    {
      "username": "player2",
      "score": 500,
      "timestamp": "2024-12-24T11:00:00.000Z"
    }
  ],
  "high-low-card": [
    // More scores...
  ]
}
```

### Viewing Leaderboards

#### User View (Game Dashboard)
1. Click "Browse All Games"
2. Click on any game to play it
3. After playing, click "🏆 Leaderboard" below the game
4. See:
   - Top 10 players
   - Your personal best
   - Total number of plays
   - Your rank highlighted in blue

#### Admin View (Admin Dashboard)
1. Go to Admin Dashboard → Leaderboards tab
2. See leaderboards for ALL games at once
3. Filter by specific game
4. Sort by score or date
5. See total scores for each game

---

## 🎯 Adding Score Tracking to Your Games

### Step 1: Design Your Scoring System

Decide how scores work in your game:
- **High score is better**: Points, wins, levels completed
- **Low score is better**: Time to complete, mistakes made
- **Complex formula**: Combination of factors

**Example Formulas:**
```javascript
// Simple: Count wins
score = wins;

// Time-based: Faster is better (invert)
score = Math.floor(10000 / timeInSeconds);

// Combination
score = (wins * 100) + (streak * 50) - (mistakes * 10);
```

### Step 2: Report Score to Dashboard

Add this function to your game:

```javascript
function reportScore(myScore) {
    // Send to parent window (dashboard)
    if (window.parent && window.parent !== window) {
        window.parent.postMessage({
            type: 'GAME_SCORE',
            gameId: 'your-game-id',  // MUST match GAME_LIBRARY
            score: myScore
        }, window.location.origin);
    }
}
```

### Step 3: Call When Appropriate

**Option A: Report on Win/Completion**
```javascript
function onGameComplete() {
    const finalScore = calculateScore();
    reportScore(finalScore);
    alert('Game complete! Score: ' + finalScore);
}
```

**Option B: Report After Each Round**
```javascript
function onRoundComplete() {
    roundsWon++;
    const currentScore = roundsWon * 100;
    reportScore(currentScore);
}
```

**Option C: Report on Achievement**
```javascript
function onLuckyNumber() {
    luckyCount++;
    const score = luckyCount * 50;
    reportScore(score);
}
```

### Example: Lucky Number Game

```javascript
// Game state
let wins = 0;
let bestStreak = 0;

// When player wins
function onWin() {
    wins++;
    currentStreak++;
    
    if (currentStreak > bestStreak) {
        bestStreak = currentStreak;
    }
    
    // Calculate and report score
    const score = (wins * 100) + (bestStreak * 50);
    reportScore(score);
}

// Score reporting function
function reportScore(score) {
    if (window.parent && window.parent !== window) {
        window.parent.postMessage({
            type: 'GAME_SCORE',
            gameId: 'lucky-number-game',
            score: score
        }, window.location.origin);
    }
}
```

---

## 📋 Complete Workflow Examples

### Scenario 1: Admin Disables Action Games

**Admin Actions:**
1. Opens admin-dashboard-enhanced.html
2. Clicks Game Management tab
3. Finds "Action Games" section
4. Toggles category switch OFF

**What Happens:**
- All action games (Speed Clicker, Reaction Time, etc.) disabled
- Users can no longer see these games in browser
- Filter shows fewer games
- Search won't find disabled games

**User Experience:**
- User clicks "Browse All Games"
- Action category has no games
- Sees message: "No games available" when filtering by Action
- Can still play Puzzle, Memory, Strategy, and Quiz games

### Scenario 2: Player Sets New High Score

**User Actions:**
1. Login to game dashboard
2. Click "Browse All Games"
3. Click "Lucky Number Game"
4. Play and get lucky numbers
5. Game automatically reports score

**What Happens:**
1. Game calculates: `(5 wins × 100) + (3 streak × 50) = 650`
2. Game sends message to dashboard
3. Dashboard saves: `{ username: "player1", score: 650, timestamp: "..." }`
4. Leaderboard updates automatically
5. User sees: "Score saved: 650! Check the leaderboard below."
6. Clicks leaderboard to see ranking

**Admin Can See:**
1. Opens admin dashboard
2. Clicks Leaderboards tab
3. Sees "Lucky Number Game" with new score
4. Sees player1 ranked based on score

---

## 🚀 Advanced Use Cases

### Use Case 1: Tournament Mode

**Goal**: Enable only specific games for a competition

**Steps:**
1. Disable all games (✗ Disable All)
2. Enable only tournament games individually
3. Users can only play those games
4. Admin monitors leaderboards in real-time

### Use Case 2: Progressive Unlocking

**Goal**: Enable games gradually over time

**Week 1:**
- Enable only "easy" difficulty games

**Week 2:**
- Enable "medium" difficulty games

**Week 3:**
- Enable all games

### Use Case 3: Category Rotation

**Monday-Wednesday**: Only Puzzle games
**Thursday-Friday**: Only Action games  
**Weekend**: All games enabled

---

## ⚠️ Important Notes

### Game ID Consistency
**CRITICAL**: The `gameId` in your score report MUST exactly match the `id` in GAME_LIBRARY:

❌ **WRONG:**
```javascript
// In GAME_LIBRARY
{ id: 'lucky-number-game', ... }

// In game file
reportScore('lucky-game', score);  // IDs don't match!
```

✅ **CORRECT:**
```javascript
// In GAME_LIBRARY
{ id: 'lucky-number-game', ... }

// In game file
reportScore('lucky-number-game', score);  // Perfect match!
```

### Score Data Type
Scores should be **numbers** for proper sorting:

✅ Good: `score: 500`
✅ Good: `score: 12.5`
❌ Bad: `score: "500 points"`
❌ Bad: `score: "5:30"`

### localStorage Limits
- Browser localStorage has ~5-10MB limit
- Each score entry is small (~100 bytes)
- Can store thousands of scores safely
- Consider periodic exports/backups for long-term use

### Security Note
The postMessage system only accepts messages from same origin for security.

---

## 🔧 Troubleshooting

### Issue: Game doesn't appear in browser

**Check:**
1. Is game enabled in admin panel?
2. Is category enabled?
3. Does gameId match exactly?
4. Is file in correct folder?

**Fix:**
- Go to admin panel → enable game/category

### Issue: Scores not saving

**Check:**
1. Is user logged in?
2. Is gameId correct?
3. Is score a number?
4. Check browser console for errors

**Debug:**
```javascript
// Add to game
console.log('Reporting score:', score);
```

### Issue: Leaderboard not showing

**Check:**
1. Does game have any scores?
2. Is gameId in GAME_LIBRARY?
3. Click "🏆 Leaderboard" to expand

**Fix:**
- Play the game once to generate first score

### Issue: Admin can't disable games

**Check:**
1. Are you on admin-dashboard-enhanced.html?
2. Check browser console for errors
3. Try hard refresh (Ctrl+Shift+R)

---

## 📊 Best Practices

### For Admins

✅ **DO:**
- Test game visibility after changes
- Monitor leaderboards regularly
- Export data periodically (future feature)
- Enable games appropriate for your users

❌ **DON'T:**
- Disable all games unexpectedly
- Clear scores without warning users
- Forget to test after bulk changes

### For Game Developers

✅ **DO:**
- Use clear, meaningful game IDs
- Report scores at logical moments
- Show score formula to players
- Test score reporting thoroughly

❌ **DON'T:**
- Report score on every action (causes spam)
- Use string scores (use numbers)
- Report negative scores
- Forget to include gameId

---

## 🎉 Summary

Your enhanced system now has:

✅ **Complete admin control** over game visibility
✅ **Automatic leaderboard tracking** for all games
✅ **Category and individual game management**
✅ **Real-time score updates** 
✅ **User-friendly interfaces** for both admins and players

**Next Steps:**
1. Open admin dashboard and explore
2. Try enabling/disabling games
3. Play a game and check leaderboard
4. Add score tracking to your own games

**Happy gaming and competing!** 🎮🏆
