# Quick Reference: Adding Score Tracking to Your Game

## 🎯 3-Step Score Integration

### Step 1: Add the Score Reporting Function

Copy this function into your game's `<script>` section:

```javascript
function reportScore(score) {
    // Send score to parent window (dashboard)
    if (window.parent && window.parent !== window) {
        window.parent.postMessage({
            type: 'GAME_SCORE',
            gameId: 'your-game-id',  // ⚠️ MUST match your game's ID in GAME_LIBRARY
            score: score              // Must be a NUMBER
        }, window.location.origin);
    }
}
```

### Step 2: Calculate Your Score

Design how your game scores work:

```javascript
// Example 1: Simple win counter
let wins = 0;
function onWin() {
    wins++;
    reportScore(wins * 100);  // 100 points per win
}

// Example 2: Time-based (faster is better)
let startTime = Date.now();
function onComplete() {
    let timeInSeconds = (Date.now() - startTime) / 1000;
    let score = Math.floor(10000 / timeInSeconds);  // Invert time
    reportScore(score);
}

// Example 3: Complex formula
let wins = 0;
let streak = 0;
let mistakes = 0;
function calculateScore() {
    return (wins * 100) + (streak * 50) - (mistakes * 10);
}
```

### Step 3: Call at the Right Time

Choose when to report scores:

```javascript
// Option A: On game completion
function gameComplete() {
    let finalScore = calculateScore();
    reportScore(finalScore);
    alert('Game complete! Score: ' + finalScore);
}

// Option B: After each win/achievement
function onAchievement() {
    score += 100;
    reportScore(score);  // Report immediately
}

// Option C: On new high score
function checkNewRecord() {
    if (currentScore > personalBest) {
        personalBest = currentScore;
        reportScore(currentScore);
    }
}
```

---

## ✅ Complete Example: Lucky Number Game

```html
<!DOCTYPE html>
<html>
<head>
    <title>Lucky Number Game</title>
</head>
<body>
    <h1>Lucky Number Game</h1>
    <p>Score: <span id="score">0</span></p>
    <button onclick="play()">Generate Number</button>

    <script>
        let wins = 0;
        let bestStreak = 0;
        let currentStreak = 0;

        // The score reporting function
        function reportScore(score) {
            if (window.parent && window.parent !== window) {
                window.parent.postMessage({
                    type: 'GAME_SCORE',
                    gameId: 'lucky-number-game',
                    score: score
                }, window.location.origin);
            }
        }

        function play() {
            let number = Math.floor(Math.random() * 100) + 1;
            let luckyNumbers = [7, 21, 42, 77, 99];
            
            if (luckyNumbers.includes(number)) {
                // Win!
                wins++;
                currentStreak++;
                if (currentStreak > bestStreak) {
                    bestStreak = currentStreak;
                }
                
                // Calculate and report score
                let score = (wins * 100) + (bestStreak * 50);
                document.getElementById('score').textContent = score;
                reportScore(score);  // ← Report to leaderboard
                
                alert('Lucky! Number: ' + number);
            } else {
                currentStreak = 0;
                alert('Not lucky. Number: ' + number);
            }
        }
    </script>
</body>
</html>
```

---

## 📋 Checklist

Before deploying your game:

- [ ] Added `reportScore()` function to your game
- [ ] `gameId` matches ID in GAME_LIBRARY exactly
- [ ] Score is a NUMBER (not string)
- [ ] Score is reported at logical moments (not spam)
- [ ] Tested game in iframe
- [ ] Verified score appears in leaderboard
- [ ] Higher scores show up higher on leaderboard

---

## 🎨 Score Formula Ideas

### Time-Based Games
```javascript
// Faster = better
score = Math.floor(10000 / timeInSeconds);

// Slower = worse penalty
score = Math.max(0, 1000 - timeInSeconds);
```

### Accuracy Games
```javascript
// Percentage-based
score = Math.floor((correct / total) * 1000);

// With streak bonus
score = (correct * 10) + (maxStreak * 5);
```

### Level/Stage Games
```javascript
// Progressive difficulty
score = (level * 100) + (level * level * 10);

// With time bonus
score = (level * 100) + Math.max(0, 500 - timeSpent);
```

### Collection Games
```javascript
// Items with different values
score = (commonItems * 10) + (rareItems * 50) + (legendaryItems * 200);
```

---

## ⚠️ Common Mistakes

### ❌ WRONG: String Score
```javascript
reportScore('500 points');  // Will break leaderboard sorting!
```

### ✅ CORRECT: Number Score
```javascript
reportScore(500);  // Perfect!
```

---

### ❌ WRONG: Mismatched ID
```javascript
// GAME_LIBRARY has: { id: 'lucky-number-game' }
reportScore('lucky-game', score);  // Doesn't match!
```

### ✅ CORRECT: Exact ID Match
```javascript
// GAME_LIBRARY has: { id: 'lucky-number-game' }
reportScore('lucky-number-game', score);  // Perfect match!
```

---

### ❌ WRONG: Reporting Too Often
```javascript
function onMouseMove() {
    reportScore(score);  // Spam! Called constantly!
}
```

### ✅ CORRECT: Report on Achievements
```javascript
function onLevelComplete() {
    reportScore(score);  // Good! Only on completion
}
```

---

### ❌ WRONG: Negative Scores
```javascript
reportScore(-100);  // Confusing on leaderboard
```

### ✅ CORRECT: Use Zero as Minimum
```javascript
reportScore(Math.max(0, score));  // Never negative
```

---

## 🧪 Testing Your Integration

### Test 1: Score Appears
1. Add score reporting to game
2. Play game in browser (standalone)
3. Check console: should NOT see errors

### Test 2: Dashboard Integration
1. Add game to GAME_LIBRARY
2. Open game-dashboard-enhanced.html
3. Login and play your game
4. Look for: "Score saved: [number]!" alert

### Test 3: Leaderboard Shows Score
1. After playing, click "🏆 Leaderboard"
2. Verify your score appears
3. Play again with different score
4. Verify both scores saved

### Test 4: Admin View
1. Open admin-dashboard-enhanced.html
2. Go to Leaderboards tab
3. Find your game
4. Verify scores appear correctly sorted

---

## 💡 Pro Tips

### Tip 1: Show Score Formula
Tell players how scoring works:
```html
<div class="instructions">
    <h3>Scoring:</h3>
    <p>Wins × 100 + Best Streak × 50</p>
</div>
```

### Tip 2: Display Current Score
Show score as they play:
```javascript
function updateDisplay() {
    document.getElementById('currentScore').textContent = calculateScore();
}
```

### Tip 3: Confirm on Submit
Give feedback when score is saved:
```javascript
function reportScore(score) {
    if (window.parent && window.parent !== window) {
        window.parent.postMessage({
            type: 'GAME_SCORE',
            gameId: 'your-game-id',
            score: score
        }, window.location.origin);
        
        // Visual feedback
        alert('✅ Score submitted: ' + score);
    }
}
```

### Tip 4: Personal Best Tracking
Show if player beat their previous best:
```javascript
let personalBest = localStorage.getItem('pb_gameId') || 0;

function submitScore(score) {
    if (score > personalBest) {
        personalBest = score;
        localStorage.setItem('pb_gameId', score);
        alert('🎉 New personal best!');
    }
    reportScore(score);
}
```

---

## 🎯 Score Design Best Practices

### Good Score Systems

✅ **Clear and predictable**
- Players understand how points are earned
- Consistent rules throughout game

✅ **Rewarding**
- Positive reinforcement
- Bigger achievements = bigger scores

✅ **Fair**
- Skill-based when possible
- Not purely luck-based

✅ **Scalable**
- Reasonable number ranges (0-10000)
- Room for improvement

### Score System Examples

**Memory Game:**
```
Score = (Pairs Found × 100) - (Moves × 5) + Time Bonus
```

**Reaction Game:**
```
Score = 10000 - (Average Reaction Time in ms)
```

**Quiz Game:**
```
Score = (Correct × 100) + (Perfect Streak × 50)
```

**Puzzle Game:**
```
Score = (1000 × Difficulty) - (Hints Used × 50)
```

---

## 📝 Template for New Games

Copy this template to start a new game with score tracking:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My New Game</title>
    <style>
        /* Your styles here */
    </style>
</head>
<body>
    <h1>My New Game</h1>
    <p>Score: <span id="score">0</span></p>
    
    <!-- Your game UI here -->
    
    <script>
        // Game state
        let score = 0;
        
        // Score reporting function
        function reportScore(finalScore) {
            if (window.parent && window.parent !== window) {
                window.parent.postMessage({
                    type: 'GAME_SCORE',
                    gameId: 'my-new-game',  // ⚠️ Match GAME_LIBRARY ID
                    score: finalScore
                }, window.location.origin);
            }
        }
        
        // Your game logic here
        function onGameEvent() {
            score += 100;
            document.getElementById('score').textContent = score;
            
            // Report when appropriate
            if (gameCompleted) {
                reportScore(score);
            }
        }
        
        // Initialize game
        // ...
    </script>
</body>
</html>
```

---

## 🚀 Ready to Go!

You now know:
- ✅ How to add the score reporting function
- ✅ When to call it in your game
- ✅ How to design good scoring systems
- ✅ How to test your integration
- ✅ Common mistakes to avoid

**Start adding scores to your games and see them on the leaderboard!** 🏆
