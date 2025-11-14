# CLAUDE.md - AI Assistant Guide for CAET Simulators Repository

## 🎯 Repository Overview

**Hangar Defender** is a Galaga-style educational arcade game combining classic shoot-'em-up gameplay with CAET (Certified Aviation Electronics Technician) exam preparation. This is a single-file HTML5 application with no external dependencies.

### Quick Facts
- **Primary File**: `index.html` (3,017 lines)
- **Technology**: Vanilla JavaScript, HTML5 Canvas, Web Audio API
- **Architecture**: Single-file monolithic application (HTML + CSS + JS)
- **Educational Content**: 100 CAET exam questions across 4 categories
- **Visual Style**: 16-bit Sega Genesis aesthetic with CRT effects
- **No Dependencies**: Pure vanilla JavaScript, no frameworks or libraries

## 📁 Repository Structure

```
/caet-simulators/
├── index.html          # Complete single-file game (3,017 lines)
└── README.md          # User-facing documentation (223 lines)
```

### File Breakdown: index.html

| Lines | Section | Description |
|-------|---------|-------------|
| 1-121 | HTML/CSS Setup | Container, canvas, CRT effects styling |
| 122-200 | Game Constants | Configuration, colors, state variables |
| 201-424 | Arcade Effects | Screen shake, flash, slow-mo, combos |
| 425-548 | Player Class | Player ship with shooting and movement |
| 550-598 | Bullet Class | Projectile system for player/enemies |
| 600-999 | Enemy Class | 4 enemy types with AI and animations |
| 1001-1034 | Particle System | Explosions and visual effects |
| 1036-1063 | Star Field | Parallax scrolling background |
| 1065-1575 | Question Bank | 100 CAET questions (25 per category) |
| 1577-1664 | Audio System | Web Audio API sound synthesis |
| 1666-1704 | Explosion Effects | Particle-based explosions |
| 1706-1760 | Formation AI | Enemy grid formation and movement |
| 1762-1919 | Collision/Scoring | Hit detection and point calculations |
| 1921-1956 | Floating Text | Score popups and messages |
| 1958-2041 | Leaderboard | localStorage-based high scores |
| 2043-2118 | HUD Rendering | UI display system |
| 2120-2304 | Question System | Educational quiz implementation |
| 2306-2360 | Bonus Stages | Every 3rd stage special mode |
| 2362-2487 | Stage Management | Stage transitions and progression |
| 2489-2536 | Attract Mode | Title screen and demo |
| 2538-2678 | Game Over/Name Entry | End game and leaderboard entry |
| 2680-2693 | Pause System | Game pause functionality |
| 2696-2852 | Update Loop | Main game logic update |
| 2854-2980 | Draw Loop | Main rendering pipeline |
| 2982-3013 | Game Loop/Input | requestAnimationFrame and events |

## 🏗️ Architecture Patterns

### State Machine

The game uses a centralized state machine with 9 distinct states:

```javascript
gameState = 'ATTRACT'  // Possible values:
// - ATTRACT: Title screen
// - STAGE_INTRO: "Stage X Ready" screen
// - READY: "Ready!" countdown
// - PLAYING: Active gameplay
// - PAUSED: Game paused (ESC key)
// - QUESTIONS: Between-stage quiz
// - BONUS: Bonus stage (every 3rd stage)
// - GAME_OVER: Death screen
// - NAME_ENTRY: High score name input
```

**State Flow:**
```
ATTRACT → STAGE_INTRO → READY → PLAYING → (stage clear) → QUESTIONS
                                    ↓                          ↓
                                  PAUSED              (every 3rd: BONUS)
                                    ↓                          ↓
                              (resume: PLAYING)           STAGE_INTRO
                                    ↓                          ↓
                            (death) GAME_OVER → NAME_ENTRY → ATTRACT
```

### Object-Oriented Design

**Core Classes:**

1. **Player** (lines 425-548)
   - Single instance stored in `player` variable
   - Handles movement, shooting, dual-fighter mode
   - Properties: `x, y, width, height, speed, shootCooldown, isDual`

2. **Enemy** (lines 600-999)
   - Array of instances in `enemies[]`
   - Four types: `boss`, `butterfly`, `dragonfly`, `bee`
   - State machine: `ENTERING → FORMATION → DIVING → RETURNING`
   - Unique dive patterns per type

3. **Bullet** (lines 550-598)
   - Array of instances in `bullets[]`
   - Both player and enemy bullets
   - Property `isEnemy` distinguishes direction

4. **Particle** (lines 1001-1034)
   - Array of instances in `particles[]`
   - Used for explosions and engine trails
   - Simple physics: velocity, gravity, fade

5. **Star** (lines 1036-1063)
   - Array of instances in `stars[]`
   - Parallax background with depth
   - Twinkling animation

6. **BulletTrail** (lines 395-418)
   - Array of instances in `bulletTrails[]`
   - Motion blur effect for bullets
   - Short-lived visual enhancement

## 🎨 Coding Conventions

### Naming Standards

**Variables:**
- `camelCase` for all variables: `currentStage`, `gameState`, `extraLifeThreshold`
- `SCREAMING_SNAKE_CASE` for constants: `GAME_WIDTH`, `GAME_HEIGHT`, `FPS`
- Descriptive names preferred: `tractorBeamActive`, `bonusEnemiesDestroyed`

**Functions:**
- `camelCase` verbs: `updateDiveAttacks()`, `checkCollisions()`, `startNewGame()`
- Draw prefix: `drawHUD()`, `drawGameOver()`, `drawBoss()`
- Update prefix: `updateQuestions()`, `updateBonusStage()`

**Classes:**
- `PascalCase`: `Player`, `Enemy`, `Bullet`, `Particle`, `Star`

### Code Organization

**Section Headers:**
```javascript
// ===== SECTION NAME =====
```
Used to mark major system boundaries (23 sections total)

**Comment Style:**
- Single-line comments: `// Explanation`
- Used sparingly, mainly for complex logic
- State transitions are commented
- Physics calculations explained

**Function Grouping:**
- Related functions placed together
- Helper functions near their primary functions
- Class methods grouped by purpose (update, draw, utility)

### Key Patterns

1. **Object Pooling**: Entities marked `active = false` instead of array deletion
2. **Array Filtering**: Periodic cleanup with `array.filter(e => e.active)`
3. **Frame-Based Logic**: Use `frameCount % n` for periodic actions
4. **Guard Clauses**: Early returns for invalid states
5. **Factory Functions**: `createFormation()`, `createExplosion()`, `generateQuestions()`
6. **Separation of Concerns**: Update logic separate from draw logic

## 🎮 Game Systems Deep Dive

### Enemy AI System

**Formation Grid** (lines 1706-1744):
```javascript
// Layout (32-40 enemies total):
// Row 1: Boss (4-6 units)
// Row 2: Butterfly (6 units)
// Row 3: Dragonfly (6 units)
// Rows 4-5: Bee (16 units)
```

**Dive Attack Patterns** (lines 735-775):
- **Boss**: Gentle arc, potential tractor beam (25% chance)
- **Butterfly**: Figure-8 swooping pattern
- **Bee**: Fast straight dive with player tracking
- **Dragonfly**: Wide looping pattern

**Dive Frequency Calculation**:
```javascript
diveFrequency = Math.max(1500, 4000 - (currentStage * 200))
// Stage 1-3: Every 4s
// Stage 4-6: Every 3s
// Stage 7-9: Every 2s
// Stage 10+: Every 1.5s
```

### Scoring System

**Points Table** (lines 1900-1919):
```javascript
// Enemy Points:
Boss:       150 (formation) / 400 (diving)
Butterfly:   80 (formation) / 160 (diving)
Dragonfly:  100 (formation) / 160 (diving)
Bee:         50 (formation) / 100 (diving)

// Bonuses:
Correct Answer:           +500
Wrong Answer:             -200
Perfect Question Set:    +2000
Perfect Stage Clear:     +5000
Ship Rescue:             +1000
Bonus Stage Perfect:    +10000

// Multipliers:
Dual Fighter Mode:        1.5x
Combo (5+):               1.2x
Combo (10+):              1.5x

// Extra Lives:
Every 10,000 points (max 5 lives)
```

### Question System

**Categories** (lines 1065-1575):
1. **basicElectricity**: Ohm's Law, circuits, AC/DC (25 questions)
2. **digitalLogic**: Logic gates, binary/hex conversion (25 questions)
3. **communication**: VOR, ILS, ARINC, GPS (25 questions)
4. **maintenance**: Wire gauges, torque, FAR Part 43 (25 questions)

**Structure**:
```javascript
{
  question: "string",
  answers: ["A", "B", "C", "D"],  // Always 4 options
  correct: 0  // Index (0-3) of correct answer
}
```

**Selection Algorithm** (lines 2138-2153):
- Rotates through categories evenly
- 5 questions per set (all 4 categories represented)
- Random selection within each category
- No duplicate tracking (questions can repeat across stages)

### Audio System (Web Audio API)

**Sound Types** (lines 1589-1664):
All sounds are synthesized in real-time using oscillators:

```javascript
shoot:        Square wave, 800Hz, 0.1s
explosion:    Sawtooth wave, 200Hz, 0.3s
tractorBeam:  Sine wave, 150→100Hz sweep, 1s
extraLife:    Sine wave, 1200Hz, 0.2s
correct:      Sine wave, 1000Hz, 0.15s (ATC tone)
wrong:        Sawtooth wave, 300Hz, 0.4s (buzzer)
warning:      Square wave, 800↔400Hz alternating, 0.6s
```

**Implementation**:
```javascript
function playSound(type) {
  if (!settings.soundEnabled) return;

  const audioContext = new (window.AudioContext || window.webkitAudioContext)();
  const oscillator = audioContext.createOscillator();
  const gainNode = audioContext.createGain();

  // Configure based on type
  // Connect: oscillator → gainNode → destination
  // Start and stop with timing
}
```

### Persistence (localStorage)

**Keys Used**:
- `'hangarDefender_leaderboard'`: JSON array of top 10 scores

**Leaderboard Entry Structure**:
```javascript
{
  name: "ACE",    // 3-character string
  score: 125400,  // Integer
  stage: 18       // Integer (stage reached)
}
```

**Functions** (lines 1966-2031):
- `loadLeaderboard()`: Load from localStorage or use defaults
- `saveLeaderboard()`: Persist to localStorage
- `checkHighScore()`: Determine if score qualifies (top 10)
- `addToLeaderboard()`: Insert new entry and trim to 10

## 🛠️ Common Modification Patterns

### Adding New Enemy Types

1. **Add to setupType()** (lines 628-658):
```javascript
case 'newType':
  this.width = 35;
  this.height = 35;
  this.color = '#ff9900';
  this.pointsFormation = 120;
  this.pointsDiving = 240;
  break;
```

2. **Create draw method** (after line 951):
```javascript
drawNewType() {
  // Custom drawing code
  ctx.fillStyle = this.color;
  // ... sprite drawing
}
```

3. **Add to draw() switch** (lines 795-808):
```javascript
case 'newType':
  this.drawNewType();
  break;
```

4. **Add dive pattern** in executeDive() (lines 735-775):
```javascript
case 'newType':
  // Custom dive pattern
  this.x = this.formationX + Math.sin(this.diveProgress * Math.PI) * 100;
  this.y = this.formationY + this.diveProgress * 400;
  break;
```

5. **Update createFormation()** (lines 1706-1744):
```javascript
// Add to grid layout
for (let i = 0; i < 6; i++) {
  enemies.push(new Enemy('newType', formationX, formationY, gridX, gridY));
}
```

### Adding CAET Questions

Locate the appropriate category in `caetQuestions` object (lines 1066-1575):

```javascript
communication: [
  {
    question: "What is the primary frequency range for VHF communication?",
    answers: [
      "118.0 to 136.975 MHz",
      "108.0 to 117.95 MHz",
      "225.0 to 399.95 MHz",
      "960 to 1215 MHz"
    ],
    correct: 0
  },
  // Add new question here
  {
    question: "Your new question here?",
    answers: ["Option A", "Option B", "Option C", "Option D"],
    correct: 1  // Index of correct answer (0-3)
  }
]
```

**Guidelines**:
- Always provide exactly 4 answer options
- `correct` is zero-indexed (0, 1, 2, or 3)
- Keep questions concise (fits on screen)
- Ensure answers are distinct and clear

### Adjusting Difficulty

**Enemy Count** (lines 1716-1742):
```javascript
// Increase boss count
for (let i = 0; i < 8; i++) {  // Was 4-6
  enemies.push(new Enemy('boss', ...));
}
```

**Dive Frequency** (line 1750):
```javascript
const diveFrequency = Math.max(1000, 4000 - (currentStage * 300));
// Lower minimum = faster dives at high stages
// Larger multiplier = quicker ramp-up
```

**Tractor Beam Chance** (line 728):
```javascript
if (this.type === 'boss' && Math.random() < 0.5) {  // Was 0.25
  this.tractorBeamActive = true;
}
```

**Scoring** (lines 628-658):
```javascript
this.pointsFormation = 200;  // Increase from 150
this.pointsDiving = 500;     // Increase from 400
```

### Visual Customization

**Color Palette** (lines 140-156):
```javascript
const COLORS = {
  background: '#0a0a2e',
  player: '#00ffff',      // Cyan player ship
  boss: '#3366ff',        // Blue boss
  // Modify any color here
  newColor: '#custom'
};
```

**Canvas Size** (lines 134-135):
```javascript
const GAME_WIDTH = 800;   // Change resolution
const GAME_HEIGHT = 600;
// Note: Also update <canvas> element width/height attributes
```

**Visual Effects Intensity**:
```javascript
// Screen shake (line 203)
addScreenShake(8, 20);  // (intensity, duration)

// Screen flash (line 220)
addScreenFlash('#ffffff', 0.5, 15);  // (color, alpha, duration)

// Slow motion (line 279)
slowMotion.factor = 0.3;  // 0.0-1.0 (lower = slower)
```

### Adding New Sounds

Add to `playSound()` function (lines 1589-1664):

```javascript
function playSound(type) {
  if (!settings.soundEnabled) return;

  const audioContext = new (window.AudioContext || window.webkitAudioContext)();
  const oscillator = audioContext.createOscillator();
  const gainNode = audioContext.createGain();

  // Add new case
  if (type === 'newSound') {
    oscillator.type = 'sine';
    oscillator.frequency.value = 600;
    gainNode.gain.setValueAtTime(0.3, audioContext.currentTime);
    gainNode.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.2);
    oscillator.stop(audioContext.currentTime + 0.2);
  }

  // Connect and start
  oscillator.connect(gainNode);
  gainNode.connect(audioContext.destination);
  oscillator.start();
}
```

## 🔍 Key Functions Reference

### Game Flow

| Function | Line | Purpose |
|----------|------|---------|
| `startNewGame()` | ~2400 | Initialize new game session |
| `startStage()` | ~2420 | Begin a new stage |
| `startQuestions()` | ~2200 | Transition to question phase |
| `startBonusStage()` | ~2310 | Trigger bonus stage |
| `gameOver()` | ~2550 | Handle player death |

### Entity Management

| Function | Line | Purpose |
|----------|------|---------|
| `createFormation()` | 1706 | Generate enemy formation grid |
| `updateDiveAttacks()` | 1746 | Trigger enemy dive attacks |
| `checkCollisions()` | 1778 | Detect all collision types |
| `createExplosion()` | 1666 | Generate explosion particles |

### Rendering

| Function | Line | Purpose |
|----------|------|---------|
| `drawHUD()` | 2043 | Render score, lives, stage info |
| `drawQuestions()` | 2240 | Render quiz interface |
| `drawLeaderboard()` | 2100 | Render high score table |
| `drawStageIntro()` | ~2450 | "Stage X Ready" screen |

### UI Utilities

| Function | Line | Purpose |
|----------|------|---------|
| `showFloatingText()` | 1921 | Create score popup |
| `wrapText()` | 2260 | Word wrap for questions |
| `showChallengeText()` | 297 | Display combo/achievement text |

## ⚡ Performance Considerations

### Optimization Techniques Used

1. **Object Pooling**: Entities marked `active = false` instead of deleted
2. **Periodic Cleanup**: Arrays filtered every N frames
3. **requestAnimationFrame**: Browser-optimized 60 FPS loop
4. **Delta Time**: Frame-independent updates
5. **Conditional Rendering**: Only draw visible/active entities

### Performance Characteristics

- **Typical Load**: 40 enemies + 50 bullets + 100 particles = ~60 FPS
- **Bottlenecks**: Canvas text rendering (questions screen)
- **Optimized For**: 800x600 resolution at 60 FPS
- **Not Optimized For**: Mobile devices, high-DPI displays

### Known Limitations

- Single-threaded (JavaScript main thread)
- No Web Workers (simple enough not to need)
- localStorage only (no cloud sync)
- No texture atlases (all vector graphics)
- Arrays filtered every frame (acceptable for ~200 entities)

## 🐛 Common Issues and Solutions

### Issue: Game runs too fast/slow
**Cause**: High refresh rate monitors or delta time issues
**Solution**: Check delta time calculation in update loop (line 2740)

### Issue: Sounds don't play
**Cause**: Browser autoplay policies
**Solution**: User interaction required before audio; handled by attract mode

### Issue: localStorage quota exceeded
**Cause**: Large leaderboard (unlikely with just 10 entries)
**Solution**: Clear localStorage or reduce entry count

### Issue: Questions repeating too often
**Cause**: Random selection from 25-question pools
**Solution**: Implement question tracking array to avoid recent repeats

### Issue: Collision detection inaccurate
**Cause**: Hitboxes may need tuning per enemy type
**Solution**: Adjust width/height in setupType() (lines 628-658)

## 📋 Development Workflows

### Testing Changes

**Quick Test Cycle**:
1. Edit `index.html` in your editor
2. Refresh browser (F5)
3. Press Space to start game
4. Test specific feature

**Testing Specific States**:
```javascript
// Add temporary code to skip to state
gameState = 'QUESTIONS';  // Test questions
currentStage = 3;         // Test stage 3
lives = 1;                // Test low lives scenario
```

**Testing Enemy Types**:
```javascript
// In createFormation(), create only one type:
for (let i = 0; i < 40; i++) {
  enemies.push(new Enemy('boss', ...));  // All bosses
}
```

### Adding Features Checklist

- [ ] Identify relevant section in code (use line number table)
- [ ] Check existing patterns for similar features
- [ ] Follow naming conventions (camelCase functions, PascalCase classes)
- [ ] Add section header comment if new major system
- [ ] Update state machine if new game state needed
- [ ] Test in multiple game states (PLAYING, QUESTIONS, etc.)
- [ ] Verify no console errors
- [ ] Check performance (should maintain 60 FPS)
- [ ] Update README.md if user-facing feature

### Git Workflow

**Branch Strategy**:
- Main branch: `main` (or master)
- Feature branches: `claude/claude-md-*` (for AI-driven development)

**Commit Message Style** (observed in git log):
```
verb: Short description

Examples:
- Add: comprehensive README documentation
- Fix: game start - now properly uses stage intro system
- Rename: hangar-defender.html to index.html
- Enhance: Hangar Defender with complete features
```

**Important Git Notes**:
- Always use `git push -u origin <branch-name>`
- Branch must start with `claude/` and match session ID
- Retry network failures up to 4 times with exponential backoff

## 🎯 Best Practices for AI Assistants

### DO:
- ✅ Read relevant sections before making changes
- ✅ Follow existing naming conventions exactly
- ✅ Test changes by refreshing browser
- ✅ Use section headers for new major systems
- ✅ Preserve the single-file architecture
- ✅ Maintain 16-bit aesthetic (pixelated, limited colors)
- ✅ Keep questions CAET-relevant and accurate
- ✅ Comment complex logic (physics, AI patterns)
- ✅ Use object pooling pattern for new entity types

### DON'T:
- ❌ Split into multiple files (defeats portability)
- ❌ Add external dependencies (keep vanilla JS)
- ❌ Use var (use const/let only)
- ❌ Add images/audio files (synthesize everything)
- ❌ Break localStorage compatibility
- ❌ Remove CRT effects (core to aesthetic)
- ❌ Change canvas size without updating constants
- ❌ Add server-side features (client-only design)
- ❌ Modify question data without verification

### Code Quality Checks

Before committing changes:
1. **No console errors**: Check browser DevTools
2. **60 FPS maintained**: Monitor frame rate during gameplay
3. **All states functional**: Test ATTRACT → PLAYING → QUESTIONS → GAME_OVER flow
4. **localStorage works**: Verify leaderboard persists across refreshes
5. **Sounds play**: Test all audio triggers
6. **Questions display correctly**: Check text wrapping and timer
7. **Enemies behave correctly**: Verify formation and dive patterns

## 📚 Additional Resources

### External Documentation
- **HTML5 Canvas API**: https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API
- **Web Audio API**: https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API
- **localStorage**: https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage
- **requestAnimationFrame**: https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame

### CAET Exam Resources
- Questions based on real CAET certification topics
- Categories align with official exam domains
- Content suitable for study and review

### Game Design Inspiration
- **Galaga** (1981): Formation system, dive attacks, dual fighter
- **Sega Genesis** aesthetics: 16-bit palette, CRT effects
- Classic arcade conventions: Attract mode, high scores, progressive difficulty

## 🔄 Version History

Tracked via git commits:
- Initial creation: Hangar Defender base game
- Feature additions: Complete arcade features
- Professional polish: Visual effects and sound
- Arcade transformation: Enhanced effects system
- Stage intro fix: Proper game start flow
- File rename: hangar-defender.html → index.html

---

## 📝 Quick Reference Card

### File Structure
```
index.html (3,017 lines)
├── HTML/CSS (1-121)
├── Constants (122-200)
├── Classes (201-999)
├── Questions (1065-1575)
├── Systems (1577-2693)
└── Game Loop (2696-3013)
```

### Key Variables
```javascript
gameState      // Current state (9 possible values)
currentStage   // Stage number (1+)
score          // Current score
lives          // Remaining lives (max 5)
player         // Player instance or null
enemies[]      // Enemy array
bullets[]      // Bullet array
particles[]    // Particle array
```

### Quick Commands
```javascript
// Start new game
startNewGame()

// Skip to stage
currentStage = 5; startStage()

// Give lives
lives = 5

// Add score
score += 10000

// Test question system
gameState = 'QUESTIONS'; startQuestions()
```

---

**Last Updated**: 2025-11-14
**For**: Claude AI Assistant
**Repository**: caet-simulators
**Primary File**: index.html (Hangar Defender)
