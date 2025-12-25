# 🎮 Game Dashboard Platform

A complete game management system with admin controls, leaderboards, and a beautiful game browser interface. Perfect for creating educational game platforms, competitions, or just hosting a collection of browser games.

![Version](https://img.shields.io/badge/version-2.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)

## ✨ Features

### 🎯 For Players
- **Game Browser** - Search and filter through all available games
- **Categories** - Puzzle, Action, Memory, Strategy, Quiz
- **Difficulty Levels** - Easy, Medium, Hard
- **Leaderboards** - Compete on per-game leaderboards
- **Personal Bests** - Track your best scores
- **User Accounts** - Simple username-based login

### ⚙️ For Admins
- **Game Control** - Enable/disable individual games or entire categories
- **Bulk Management** - Enable/disable all games with one click
- **Leaderboard Oversight** - View all scores across all games
- **User Management** - View and manage registered users
- **Real-time Stats** - Monitor total games, players, and scores

### 🛠️ For Developers
- **Easy Integration** - Add new games with 2 simple steps
- **Score Tracking** - 3-line code integration for leaderboards
- **Flexible Configuration** - Customize paths, categories, and settings
- **No Backend Required** - Runs entirely in the browser with localStorage

## 🚀 Quick Start

### 1. Clone or Download
```bash
git clone https://github.com/yourusername/game-dashboard.git
cd game-dashboard
```

### 2. Open in Browser
Simply open `game-dashboard-enhanced.html` in your web browser. No build process or server required!

### 3. Start Playing
- Login with any username
- Click "Browse All Games"
- Play and compete on leaderboards!

### 4. Admin Access
Open `admin-dashboard-enhanced.html` and login with:
- **Username:** `admin`
- **Password:** `12345bw`

⚠️ **Change default password in production!**

## 📁 Project Structure

```
game-dashboard/
├── game-dashboard-enhanced.html     # Main user interface
├── admin-dashboard-enhanced.html    # Admin control panel
├── games/                           # Game files directory
│   ├── lucky-number-game.html
│   ├── high-low-card.html
│   ├── color-memory.html
│   └── ... (add more games here)
├── docs/                            # Documentation
│   ├── ENHANCED-GAME-BROWSER-GUIDE.md
│   ├── ADMIN-LEADERBOARD-GUIDE.md
│   ├── SCORE-TRACKING-QUICK-REFERENCE.md
│   └── QUICK-ADD-GAME-GUIDE.md
└── README.md                        # This file
```

## 🎮 Included Games

The platform comes with 11 pre-configured games:

| Game | Category | Difficulty |
|------|----------|------------|
| High-Low Card Game | Strategy | Easy |
| Coin Flip Streak | Strategy | Easy |
| Dice Roll Challenge | Action | Easy |
| Color Memory Game | Memory | Medium |
| Pattern Match | Memory | Medium |
| Reaction Time Test | Action | Easy |
| Speed Clicker | Action | Easy |
| Word Scramble | Puzzle | Medium |
| Math Sprint | Puzzle | Medium |
| True or False Quiz | Quiz | Easy |
| Lucky Number Game | Action | Easy |

## 📝 Adding New Games

### Step 1: Create Your Game File
Save your game HTML file in the `games/` folder:
```
games/my-awesome-game.html
```

### Step 2: Add to Game Library
Open `game-dashboard-enhanced.html` and add to the `GAME_LIBRARY` array:

```javascript
{
    id: 'my-awesome-game',
    title: 'My Awesome Game',
    description: 'An awesome new game!',
    filename: 'my-awesome-game.html',
    category: 'puzzle',      // puzzle, action, memory, strategy, quiz
    difficulty: 'medium'     // easy, medium, hard
}
```

That's it! The game will immediately appear in the browser.

## 🏆 Adding Leaderboards to Games

Add this to your game's code:

```javascript
// Score reporting function
function reportScore(score) {
    if (window.parent && window.parent !== window) {
        window.parent.postMessage({
            type: 'GAME_SCORE',
            gameId: 'your-game-id',  // Must match GAME_LIBRARY id
            score: score             // Must be a number
        }, window.location.origin);
    }
}

// Call it when player achieves something
function onWin() {
    let finalScore = calculateScore();
    reportScore(finalScore);
}
```

Scores are automatically saved and displayed on leaderboards!

## ⚙️ Admin Features

### Game Management
- Enable/disable individual games
- Enable/disable entire categories
- Bulk enable/disable all games
- Visual game organization by category

### Leaderboards
- View all scores across all games
- Filter by specific game
- Sort by score or date
- Clear all scores (with confirmation)

### User Management
- View all registered users
- Delete users and their scores
- Track user join dates

## 🎯 Use Cases

### Educational Settings
- Create learning game platforms
- Track student progress
- Control which games are available
- Run competitions with leaderboards

### Game Jams
- Host collections of game jam entries
- Enable voting through leaderboards
- Manage which games are visible

### Personal Game Collection
- Organize your browser games
- Track high scores
- Share with friends

### Tournaments
- Enable only tournament games
- Monitor real-time leaderboards
- Manage participant access

## 🔧 Configuration

### Change Games Folder Location
In `game-dashboard-enhanced.html`, find:
```javascript
const GAME_FILES_PATH = './games/';
```

Change to:
```javascript
const GAME_FILES_PATH = './';           // Same folder
const GAME_FILES_PATH = '../games/';    // Parent folder
const GAME_FILES_PATH = '/games/';      // Root folder
```

### Customize Categories
Categories are defined in the admin dashboard. Current categories:
- 🧩 Puzzle
- ⚡ Action
- 🧠 Memory
- ♟️ Strategy
- ❓ Quiz

## 📊 Data Storage

The system uses browser localStorage to store:
- User accounts
- Game settings (enabled/disabled)
- Leaderboard scores
- Timestamps and metadata

**Note:** localStorage has a ~5-10MB limit (sufficient for thousands of scores)

## 🌐 Browser Compatibility

Works in all modern browsers:
- ✅ Chrome/Edge (recommended)
- ✅ Firefox
- ✅ Safari
- ✅ Opera

**Note:** Requires JavaScript enabled and localStorage support

## 📚 Documentation

Detailed guides are available in the `docs/` folder:

- **[Enhanced Game Browser Guide](docs/ENHANCED-GAME-BROWSER-GUIDE.md)** - Complete system overview
- **[Admin & Leaderboard Guide](docs/ADMIN-LEADERBOARD-GUIDE.md)** - Admin features and leaderboards
- **[Challenge Mode Guide](docs/CHALLENGE-MODE-GUIDE.md)** - Progressive game challenges
- **[Admin Authentication Guide](docs/ADMIN-AUTHENTICATION-GUIDE.md)** - Security and login system
- **[Score Tracking Reference](docs/SCORE-TRACKING-QUICK-REFERENCE.md)** - Add scores to your games
- **[Quick Add Game Guide](docs/QUICK-ADD-GAME-GUIDE.md)** - Step-by-step game addition

## 🔐 Admin Access

The admin dashboard is protected with authentication.

**Default Login Credentials:**
- Username: `admin`
- Password: `12345bw`

⚠️ **Important:** Change the default password before deploying!

See [Admin Authentication Guide](docs/ADMIN-AUTHENTICATION-GUIDE.md) for details on:
- How to change credentials
- Adding multiple admins
- Security best practices
- Troubleshooting

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Add New Games** - Create and submit browser games
2. **Improve Documentation** - Help make guides clearer
3. **Report Bugs** - Open issues for any problems
4. **Suggest Features** - Share ideas for improvements

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🔒 Security Note

The admin authentication provided is **basic** and suitable for local/internal use. For production deployments:

- ⚠️ Change the default admin password immediately
- ⚠️ Passwords are stored in plain text in the code (visible to anyone with file access)
- ⚠️ For public-facing sites, implement server-side authentication
- ⚠️ Use HTTPS in production
- ⚠️ Consider implementing password hashing, rate limiting, and audit logging

See [Admin Authentication Guide](docs/ADMIN-AUTHENTICATION-GUIDE.md) for more security information.

## 🙏 Credits

Created as a comprehensive game management platform for educational and entertainment purposes.

## 📞 Support

- **Issues:** Open an issue on GitHub
- **Documentation:** Check the `docs/` folder
- **Questions:** See the guides for common questions

## 🎉 Changelog

### Version 2.0 (Current)
- ✅ Admin dashboard with game controls
- ✅ Per-game leaderboards
- ✅ Category management
- ✅ Enhanced game browser
- ✅ Score tracking system
- ✅ User management

### Version 1.0
- ✅ Basic game browser
- ✅ Game filtering and search
- ✅ User login system

## 🚀 Roadmap

Future enhancements under consideration:
- [ ] Export/import leaderboard data
- [ ] Game ratings and reviews
- [ ] Advanced statistics and analytics
- [ ] Multiplayer game support
- [ ] Cloud storage integration
- [ ] Mobile app version

---

**Made with ❤️ for game enthusiasts and developers**

Happy Gaming! 🎮
