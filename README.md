# Tic-Tac-Toe Game 🎮

A beautiful, interactive Tic-Tac-Toe game built with HTML, CSS, and JavaScript. Two players can take turns marking X and O on a 3x3 grid, with the game automatically detecting wins and draws.

![Tic-Tac-Toe Game](https://pantane1.github.io/xo/)

## 📋 Table of Contents
- [Features](#features-)
- [How to Play](#how-to-play-)
- [Demo](#demo-)
- [Technologies Used](#technologies-used-)
- [Installation](#installation-)
- [Code Structure](#code-structure-)
- [Game Logic](#game-logic-)
- [Customization](#customization-)
- [Browser Support](#browser-support-)
- [Future Improvements](#future-improvements-)
- [Contributing](#contributing-)
- [License](#license-)
- [Credits](#credits-)
- [FAQ](#faq-)
- [Troubleshooting](#troubleshooting-)

## Features ✨

- **Two-player gameplay** - Play with a friend on the same device
- **Beautiful UI** - Gradient design with smooth animations and hover effects
- **Win detection** - Automatically detects when a player wins and highlights the winning line
- **Draw detection** - Recognizes when the game ends in a draw
- **Turn indicator** - Clearly shows whose turn it is
- **Game status** - Real-time updates on game progress
- **Reset functionality** - Start a new game anytime with one click
- **Responsive design** - Works on different screen sizes
- **Visual feedback** - Hover effects and winning line highlighting
- **Accessibility** - Keyboard and screen reader friendly

## How to Play 🎯

### Basic Rules
1. **Start the game** - Open the HTML file in your browser
2. **Take turns** - Players alternate clicking on empty cells
3. **Mark your spot** - First player uses **X**, second player uses **O**
4. **Win the game** - Get three of your marks in a row:
   - Horizontally (top, middle, bottom row)
   - Vertically (left, middle, right column)
   - Diagonally (top-left to bottom-right, or top-right to bottom-left)
5. **Game end** - The game announces the winner or if it's a draw
6. **Play again** - Click "New Game" button to reset and play another round

### Winning Combinations
```
Row 1: [0,1,2]     Row 2: [3,4,5]     Row 3: [6,7,8]
Col 1: [0,3,6]     Col 2: [1,4,7]     Col 3: [2,5,8]
Diag 1: [0,4,8]    Diag 2: [2,4,6]
```

## Demo 🎪

You can play the game instantly by:
1. Copying the code into a file named `index.html`
2. Opening it in any web browser
3. Starting to play!

**Live Demo:** [Coming Soon]

## Technologies Used 💻

- **HTML5** - Game structure and layout
- **CSS3** - Styling, gradients, animations, and responsive design
- **JavaScript (ES6+)** - Game logic, event handling, and DOM manipulation
- **Flexbox/Grid** - Modern CSS layouts
- **CSS Animations** - Smooth transitions and hover effects

## Installation 🚀

### Method 1: Direct Download (Easiest)
1. Create a new file called `index.html` on your computer
2. Copy the entire game code into this file
3. Save the file (Ctrl+S or Cmd+S)
4. Double-click to open in your browser

### Method 2: Clone from Repository
```bash
git clone https://github.com/yourusername/tictactoe.git
cd tictactoe
open index.html  # On Mac
# OR
start index.html # On Windows
# OR
xdg-open index.html # On Linux
```

### Method 3: VS Code with Live Server (Best for Development)
1. Install VS Code
2. Create `index.html` with the game code
3. Install "Live Server" extension (by Ritwick Dey)
4. Right-click on `index.html` and select "Open with Live Server"
5. Game auto-reloads when you make changes

### Method 4: Python HTTP Server
```bash
# Python 3
python -m http.server 8000

# Python 2
python -m SimpleHTTPServer 8000

# Then open http://localhost:8000 in your browser
```

### Method 5: Using Node.js
```bash
npm install -g http-server
http-server
# Then open http://localhost:8080
```

## Code Structure 📁

```
tictactoe/
│
├── index.html          # Main game file (HTML, CSS, and JavaScript)
├── README.md           # This documentation file
├── screenshot.png      # Game screenshot (optional)
├── .gitignore         # Git ignore file
└── assets/            # Optional assets folder
    ├── css/           # Separate CSS file (optional)
    ├── js/            # Separate JavaScript file (optional)
    └── sounds/        # Sound effects (optional)
```

### File Breakdown
```html
<!-- index.html Structure -->
<!DOCTYPE html>
<html>
<head>
    <!-- Meta tags and CSS -->
</head>
<body>
    <!-- Game container with board and UI -->
    <div class="game-container">
        <h1>🎮 Tic-Tac-Toe</h1>
        <div class="player-turn">X's Turn</div>
        <div class="board" id="board"></div>
        <div class="status">Player X's turn</div>
        <button onclick="resetGame()">New Game 🔄</button>
    </div>
    
    <!-- JavaScript game logic -->
    <script>
        // Game code here
    </script>
</body>
</html>
```

## Game Logic 🔧

### Board Representation
```javascript
// Empty board
board = ['', '', '', '', '', '', '', '', ''];

// After some moves
board = ['X', 'O', 'X', 
         'O', 'X', '', 
         '', '', 'O'];

// Index mapping:
// 0 (top-left) | 1 (top-middle) | 2 (top-right)
// 3 (middle-left) | 4 (center) | 5 (middle-right)
// 6 (bottom-left) | 7 (bottom-middle) | 8 (bottom-right)
```

### Win Conditions
```javascript
winningConditions = [
    // Rows
    [0, 1, 2], // Top row
    [3, 4, 5], // Middle row
    [6, 7, 8], // Bottom row
    
    // Columns
    [0, 3, 6], // Left column
    [1, 4, 7], // Middle column
    [2, 5, 8], // Right column
    
    // Diagonals
    [0, 4, 8], // Top-left to bottom-right
    [2, 4, 6]  // Top-right to bottom-left
];
```

### Game States
```javascript
const GameStates = {
    ACTIVE: 'active',
    WIN: 'win',
    DRAW: 'draw'
};

// Current state tracking
let gameActive = true;
let moveCount = 0;
let currentPlayer = 'X';
```

### Core Functions

#### 1. Create Board
```javascript
function createBoard() {
    const boardElement = document.getElementById('board');
    boardElement.innerHTML = '';
    
    for (let i = 0; i < 9; i++) {
        const cell = document.createElement('div');
        cell.className = 'cell';
        cell.setAttribute('data-index', i);
        cell.addEventListener('click', handleCellClick);
        boardElement.appendChild(cell);
    }
}
```

#### 2. Handle Move
```javascript
function handleCellClick(event) {
    const cell = event.target;
    const index = cell.getAttribute('data-index');

    if (board[index] !== '' || !gameActive) {
        return;
    }

    makeMove(index, cell);
}
```

#### 3. Check Win Condition
```javascript
function checkResult() {
    let roundWon = false;

    for (let i = 0; i < winningConditions.length; i++) {
        const [a, b, c] = winningConditions[i];
        
        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
            roundWon = true;
            highlightWinningCells([a, b, c]);
            break;
        }
    }

    if (roundWon) {
        endGame('win');
        return;
    }

    if (moveCount === 9) {
        endGame('draw');
        return;
    }

    switchPlayer();
}
```

#### 4. Reset Game
```javascript
function resetGame() {
    board = ['', '', '', '', '', '', '', '', ''];
    currentPlayer = 'X';
    gameActive = true;
    moveCount = 0;
    
    updateUI();
    clearHighlights();
}
```

## Customization 🎨

### Theme Customization

#### Change Colors
```css
/* Modify gradient colors */
body {
    background: linear-gradient(135deg, #ff6b6b 0%, #4ecdc4 100%);
    /* Change to your preferred colors */
}

/* Change cell colors */
.cell {
    border: 3px solid #ff6b6b;
}

.cell.x {
    color: #ff6b6b; /* X color */
}

.cell.o {
    color: #4ecdc4; /* O color */
}

/* Customize winning highlight */
.highlight {
    background: linear-gradient(135deg, #ffeaa7 0%, #fdcb6e 100%);
}
```

#### Adjust Board Size
```css
.board {
    grid-template-columns: repeat(3, 120px); /* Increase from 100px */
    gap: 15px; /* Adjust spacing */
}

.cell {
    width: 120px;
    height: 120px;
    font-size: 4em; /* Larger text */
}
```

#### Add Animations
```css
@keyframes popIn {
    0% {
        transform: scale(0);
        opacity: 0;
    }
    80% {
        transform: scale(1.1);
    }
    100% {
        transform: scale(1);
        opacity: 1;
    }
}

.cell {
    animation: popIn 0.3s ease-out;
}
```

### Advanced Customizations

#### Add Sound Effects
```javascript
// Add this function
function playSound(soundType) {
    const sounds = {
        move: new Audio('move.mp3'),
        win: new Audio('win.mp3'),
        draw: new Audio('draw.mp3')
    };
    
    if (sounds[soundType]) {
        sounds[soundType].play().catch(e => console.log('Audio play failed:', e));
    }
}

// Call in appropriate places
function makeMove() {
    playSound('move');
    // ... rest of move logic
}
```

#### Add Score Tracking
```javascript
// Add to your JavaScript
let scores = { X: 0, O: 0 };

function updateScore(winner) {
    scores[winner]++;
    document.getElementById('score-x').textContent = `X: ${scores.X}`;
    document.getElementById('score-o').textContent = `O: ${scores.O}`;
}

// Add to HTML
<div class="scores">
    <span id="score-x">X: 0</span>
    <span id="score-o">O: 0</span>
</div>
```

#### Add Difficulty Levels (for AI)
```javascript
const difficulty = {
    EASY: 'easy',
    MEDIUM: 'medium',
    HARD: 'hard'
};

function setDifficulty(level) {
    currentDifficulty = level;
}

function getAIMove() {
    switch(currentDifficulty) {
        case 'easy':
            return getRandomMove();
        case 'medium':
            return getMediumMove();
        case 'hard':
            return getBestMove();
    }
}
```

## Browser Support 🌐

| Browser | Minimum Version | Support | Notes |
|---------|----------------|---------|-------|
| Chrome  | 60+ | ✅ Full | Best performance |
| Firefox | 60+ | ✅ Full | Full support |
| Safari  | 12+ | ✅ Full | Minor gradient differences |
| Edge    | 79+ | ✅ Full | Full support |
| Opera   | 50+ | ✅ Full | Full support |
| Brave   | All | ✅ Full | Based on Chrome |
| IE      | - | ❌ Not supported | Use modern browser |

## Future Improvements 🚧

### Short Term (Next Release)
- [x] Basic game functionality
- [x] Win/draw detection
- [x] Reset feature
- [ ] **AI Opponent** - Play against computer with difficulty levels
- [ ] **Score Tracking** - Keep track of wins across multiple rounds
- [ ] **Sound Effects** - Add audio feedback for moves and wins

### Medium Term
- [ ] **Dark/Light Theme** - Toggle between color schemes
- [ ] **Player Names** - Custom player name input
- [ ] **Animations** - Smooth transitions for X and O placements
- [ ] **Game History** - View previous moves with undo/redo
- [ ] **Timer** - Add time limit for moves

### Long Term
- [ ] **Mobile Touch** - Optimized touch controls for mobile devices
- [ ] **Multiplayer Online** - Play with friends over the internet
- [ ] **Tournament Mode** - Multiple games with brackets
- [ ] **Replay System** - Watch recorded games
- [ ] **Statistics** - Win rates, favorite moves, etc.
- [ ] **Custom Boards** - 4x4, 5x5 grid options

## Contributing 🤝

We welcome contributions! Here's how you can help:

### Development Process

1. **Fork the repository**
   ```bash
   # Click 'Fork' on GitHub
   ```

2. **Clone your fork**
   ```bash
   git clone https://github.com/your-username/tictactoe.git
   cd tictactoe
   ```

3. **Create a feature branch**
   ```bash
   git checkout -b feature/AmazingFeature
   ```

4. **Make your changes**
   - Follow existing code style
   - Add comments for complex logic
   - Test thoroughly

5. **Commit your changes**
   ```bash
   git add .
   git commit -m 'Add some AmazingFeature'
   ```

6. **Push to the branch**
   ```bash
   git push origin feature/AmazingFeature
   ```

7. **Open a Pull Request**
   - Describe your changes
   - Reference any related issues
   - Wait for review

### Contribution Ideas

#### Beginner Friendly
- Fix typos in comments
- Improve documentation
- Add more comments
- Test on different browsers

#### Intermediate
- Add AI opponent
- Improve mobile responsiveness
- Add more animations
- Create themes/skins
- Add keyboard shortcuts

#### Advanced
- Implement online multiplayer
- Add database for statistics
- Create React/Vue version
- Add voice commands
- Implement machine learning AI

### Code Style Guide

```javascript
// Use consistent naming
let board = []; // Arrays: camelCase
let currentPlayer = 'X'; // Variables: camelCase
const WINNING_COMBINATIONS = []; // Constants: UPPER_SNAKE_CASE

// Functions: camelCase, descriptive names
function checkWinner() {}
function resetGame() {}
function handleCellClick() {}

// Add comments for complex logic
// Check all 8 winning combinations
for (let i = 0; i < winningConditions.length; i++) {
    // Logic here
}
```

## License 📝

This project is licensed under the MIT License - see below:

```
MIT License

Copyright (c) 2024 Tic-Tac-Toe Game

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Credits 👏

### Original Creator
- **Your Name** - Initial work and design

### Contributors
- [Name] - AI opponent feature
- [Name] - Mobile optimization
- [Name] - Bug fixes and improvements

### Acknowledgments
- Font Awesome for icons
- Google Fonts for typography
- MDN Web Docs for references
- Stack Overflow community
- All players and testers

### Inspiration
- Classic Tic-Tac-Toe games
- Modern minimalist design trends
- Google's Material Design
- Apple's Human Interface Guidelines

## FAQ ❓

### General Questions

**Q: Can I play against the computer?**
A: Currently, it's two-player only. AI opponent is planned for future releases.

**Q: Is this game free?**
A: Yes, completely free and open source!

**Q: Can I use this in my project?**
A: Yes, under the MIT license. Just give credit!

**Q: Does it work on mobile phones?**
A: Yes, it's responsive but optimized for desktop. Mobile improvements coming soon.

### Technical Questions

**Q: Why won't the game load?**
A: Try these steps:
   1. Make sure you saved the file as .html
   2. Check if JavaScript is enabled
   3. Try a different browser
   4. Clear your browser cache

**Q: Can I add more features?**
A: Absolutely! Fork the project and customize as you like.

**Q: How do I change the board size?**
A: Modify the CSS grid properties in the style section.

**Q: Can I save game progress?**
A: Not yet, but localStorage support is planned.

## Troubleshooting 🔧

### Common Issues and Solutions

#### Issue 1: Game doesn't load
```html
<!-- Make sure your HTML structure is correct -->
<!DOCTYPE html>
<html>
<head>...</head>
<body>...</body>
</html>
```

#### Issue 2: Clicking doesn't work
```javascript
// Check if event listeners are attached
console.log('Cell clicked:', index); // Add debug line
```

#### Issue 3: Win detection not working
```javascript
// Verify board array is updating
console.log('Current board:', board);
console.log('Checking win conditions...');
```

#### Issue 4: Styles not applying
```css
/* Check for CSS syntax errors */
.cell {  /* Correct */
    property: value;
}

cell {  /* Wrong - missing dot */
    property: value;
}
```

### Debugging Tips

1. **Use Browser DevTools**
   - F12 to open
   - Check Console for errors
   - Inspect Elements for HTML structure
   - Debug JavaScript with breakpoints

2. **Add Logging**
   ```javascript
   console.log('Move made:', index, currentPlayer);
   console.log('Board state:', board);
   console.log('Game active:', gameActive);
   ```

3. **Test Incrementally**
   - Start with empty board
   - Test one move
   - Test winning combinations
   - Test draw condition
   - Test reset

## Support 📧

### Getting Help

1. **Check Documentation**
   - Read this README thoroughly
   - Check comments in code

2. **Search Issues**
   - Visit [GitHub Issues](https://github.com/yourusername/tictactoe/issues)
   - Search for similar problems

3. **Create New Issue**
   ```markdown
   ## Description
   [Describe the problem]

   ## Steps to Reproduce
   1. [Step 1]
   2. [Step 2]
   3. [Step 3]

   ## Expected Behavior
   [What should happen]

   ## Actual Behavior
   [What actually happens]

   ## Environment
   - Browser: [e.g., Chrome 120]
   - OS: [e.g., Windows 11]
   - Version: [e.g., v1.0.0]
   ```

4. **Contact**
   - Email: [your-email@example.com]
   - Twitter: [@yourhandle]
   - Discord: [Server link]

## Version History 📅

### Version 1.0.0 (Current)
- ✅ Basic game functionality
- ✅ Two-player support
- ✅ Win/draw detection
- ✅ Reset game feature
- ✅ Responsive design
- ✅ Gradient UI
- ✅ Turn indicator
- ✅ Winning highlight

### Version 0.9.0 (Beta)
- ✅ Core game logic
- ✅ Basic styling
- ✅ Event handling
- ⬜ Testing completed

### Version 0.1.0 (Alpha)
- ✅ Initial concept
- ✅ Basic board
- ✅ Move validation
- ⬜ UI polish needed

## Roadmap 🗺️

### Q1 2024
- Release v1.0.0
- Bug fixes and polish
- Mobile optimization

### Q2 2024
- AI opponent (Easy mode)
- Score tracking
- Sound effects

### Q3 2024
- AI opponent (Medium/Hard)
- Dark mode
- Player profiles

### Q4 2024
- Online multiplayer
- Tournament mode
- Statistics dashboard

---

## ⚡ Quick Start

If you just want to play right now:

1. Copy this minimal code:

``html
<!DOCTYPE html>
<html>
<head>
    <title>Tic-Tac-Toe</title>
    <style>
        /* Add the CSS from the main game here */
    </style>
</head>
<body>
    <!-- Add the HTML from the main game here -->
    <script>
        // Add the JavaScript from the main game here
    </script>
</body>
</html>
```

2. Paste into a text editor
3. Save as `game.html`
4. Double-click to play!

## 🌟 Star History

If you like this project, please give it a star on GitHub! It helps others discover it.

## 🤝 Code of Conduct

Please note that this project is released with a Contributor Code of Conduct. By participating in this project you agree to abide by its terms.

### Our Pledge
We pledge to make participation in our project a harassment-free experience for everyone.

### Our Standards
- Using welcoming language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what's best for the community

---

## 🎉 Thank You!

Thank you for checking out this Tic-Tac-Toe game! Whether you're playing, learning, or contributing, we appreciate your interest.

**Happy coding and have fun playing!** 🎮✨

---

*Made with ❤️ for game lovers everywhere*

<!-- FOOTER -->
<footer style="margin-top: 40px; text-align: center;">
    <p>
        <a href="#">
            <img src="https://github.com/Pantane1/nf/blob/main/public/ph.png" alt="ph-logo" style="width:100px; height:auto;">
        </a>
    </p>
    <p>
        <a href="#">
            <img src="http://readme-typing-svg.herokuapp.com?color=ACAF50&center=true&vCenter=true&multiline=false&lines=LONG+LIVE+THE+NJAGI'S" alt="typing-effect">
        </a>
    </p>
    <p style="margin-top:10px; font-size:14px; color:#888;">
        &copy; 2026 CipherGrid. All rights reserved.
    </p>
</footer>
