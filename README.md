# 🎮 HANGAR DEFENDER

**A Galaga-style Arcade Study Game for Aviation Electronics Technicians**

## 📖 Overview

Hangar Defender is an educational arcade game that combines classic Galaga-style shooter gameplay with CAET (Certified Aviation Electronics Technician) exam preparation. Defend your hangar from cascading avionics system failures while answering aviation electronics questions between stages!

## 🎯 Game Features

### Classic Galaga Gameplay
- **4 Enemy Types** with unique behaviors:
  - **Boss (Databus Corruption Bug)**: Large, blue aliens with tractor beam capture ability
  - **Butterfly (Component Failure Moth)**: Red/purple enemies with graceful swooping patterns
  - **Dragonfly (Sensor Error Fly)**: Green enemies with wide looping attacks
  - **Bee (Wire Fault Wasp)**: Fast yellow enemies with aggressive straight dives

- **Tractor Beam Capture Mechanic**: Boss enemies can capture your ship!
  - Rescue your captured ship by shooting the capturing boss
  - Activate **Dual Fighter Mode** with two ships firing simultaneously
  - Don't accidentally shoot your own captured ship!

- **Formation System**: Enemies enter in choreographed patterns and idle in formation
- **Dive Attacks**: Enemies break formation to attack with unique patterns
- **Bonus Stages**: Every 3rd stage features special challenging stages

### Educational Integration
- **100 CAET Questions** across 4 categories:
  - **Basic Electricity** (25 questions): Ohm's Law, circuits, AC/DC systems
  - **Digital Logic** (25 questions): Logic gates, binary/hex conversion, truth tables
  - **Communication/Navigation** (25 questions): VOR, ILS, ARINC 429/629, GPS, transponders
  - **Installation/Maintenance** (25 questions): Wire gauges, torque specs, FAR regulations

- **Question System**:
  - 3-5 questions between each stage
  - 20-second timer per question
  - +500 points for correct answers
  - -200 points for wrong/timeout
  - Perfect question set bonus: +2000 points

### Scoring & Progression
- **Points System**:
  - Enemies worth more points when diving
  - Dual Fighter mode: 1.5x score multiplier
  - Perfect stage clear: +5000 bonus
  - Boss rescue: +1000 bonus

- **Extra Lives**: Earned every 10,000 points (max 5 lives)

- **Top-10 Leaderboard**:
  - Arcade-style name entry (3 characters)
  - Persistent high scores with localStorage
  - Stage progression tracking

### Retro Aesthetic
- **Sega Genesis (16-bit) Visual Style**:
  - Pixelated sprites with no anti-aliasing
  - Limited color palette
  - CRT scanline effects
  - Phosphor glow on bright elements
  - Parallax starfield background

- **Aviation-Themed Sound Effects**:
  - Web Audio API generated sounds
  - Master caution alarms for enemy explosions
  - GPWS-style warnings for tractor beam
  - ATC acknowledgment tones for correct answers

## 🎮 Controls

### Movement
- **Arrow Left/Right**: Move your ship horizontally
- **Spacebar** or **Enter**: Fire weapons

### Menu Navigation
- **Arrow Up/Down**: Navigate menus and select answers
- **Arrow Left/Right**: Move cursor (in name entry)
- **Spacebar** or **Enter**: Confirm selection
- **ESC**: Pause game (during gameplay)

### Name Entry (High Score)
- **Arrow Up/Down**: Cycle through A-Z, 0-9, space
- **Arrow Left/Right**: Move between letter positions
- **Spacebar/Enter**: Confirm letter and advance

## 🚀 How to Play

### Starting the Game
1. Open `hangar-defender.html` in a modern web browser
2. Press **Spacebar** on the attract screen to start
3. Clear enemies by shooting them
4. Avoid enemy collisions and dive attacks
5. Answer CAET questions between stages

### Gameplay Tips
- **Formation Shooting**: Enemies are worth fewer points in formation
- **Diving Enemies**: Worth double points - risk vs reward!
- **Rescue Your Ship**: If captured, shoot the boss holding it (not your ship!)
- **Dual Fighter**: Doubles your firepower but one hit destroys both ships
- **Question Strategy**: Take time to think - wrong answers cost points
- **Perfect Sets**: Answer all questions correctly for massive bonuses

### Stage Progression
1. **Combat Phase**: Destroy all enemies in formation
2. **Stage Clear**: Brief victory screen
3. **CAET Questions**: 3-5 educational questions
4. **Bonus Stage**: Every 3rd stage (Stages 3, 6, 9, etc.)
5. **Next Stage**: Difficulty increases progressively

## 📊 Difficulty Scaling

| Stage | Enemies | Dive Frequency | Tractor Beam % |
|-------|---------|----------------|----------------|
| 1-3   | 32      | Every 4s       | 20%            |
| 4-6   | 36      | Every 3s       | 30%            |
| 7-9   | 40      | Every 2s       | 40%            |
| 10+   | 40      | Every 1.5s     | 50%+           |

## 🎓 Educational Value

This game is designed to help CAET exam candidates study while having fun:

- **Active Recall**: Questions appear between stages for spaced repetition
- **Immediate Feedback**: Correct/incorrect answers with point rewards/penalties
- **Category Coverage**: Equal distribution across all CAET exam domains
- **Real Scenarios**: Questions based on actual aviation electronics concepts
- **Progress Tracking**: Stage completion shows knowledge application

### CAET Exam Topics Covered
- Electrical theory and circuit analysis
- Digital systems and logic gates
- Number system conversions (binary, hex, decimal)
- Communication systems (VHF, UHF, HF)
- Navigation aids (VOR, ILS, DME, GPS)
- Data buses (ARINC 429, ARINC 629)
- Aircraft wiring and installation
- FAR Part 43 maintenance regulations
- Component identification and maintenance

## 🛠️ Technical Specifications

- **Platform**: Web-based HTML5 (single file)
- **Resolution**: 800x600px (4:3 aspect ratio)
- **Framerate**: 60 FPS (requestAnimationFrame)
- **Storage**: localStorage for leaderboards
- **Audio**: Web Audio API (no external files)
- **Compatibility**: Modern browsers (Chrome, Firefox, Safari, Edge)

## 📁 File Structure

```
hangar-defender.html    # Complete game (single file)
README.md              # This file
```

## 🎨 Design Inspiration

- **Galaga** (1981): Formation system, dive attacks, dual fighter
- **Sega Genesis** aesthetics: 16-bit color palette, CRT effects
- **Aviation Safety**: Sound design inspired by cockpit warnings
- **Arcade Classics**: High score entry, attract mode, progressive difficulty

## 🏆 High Score Strategy

**Maximize your score:**
1. **Shoot diving enemies** for double points
2. **Rescue captured ships** for dual fighter mode
3. **Answer all questions correctly** for perfect bonuses
4. **Clear stages without dying** to maintain multipliers
5. **Earn extra lives** to progress further (every 10k points)

**Top Players:**
- Stage 18+: Expert tier
- Stage 12+: Advanced tier
- Stage 6+: Intermediate tier
- Stage 3+: Beginner tier

## 📝 Development Notes

**Created using:**
- Pure HTML5 Canvas (no frameworks)
- Vanilla JavaScript (no dependencies)
- CSS for CRT effects and styling
- Web Audio API for sound synthesis

**Features implemented:**
- Object pooling for performance
- State machine for game flow
- Collision detection with hitboxes
- Particle system for explosions
- Parallax scrolling starfield
- localStorage persistence
- Question randomization

## 🎯 Future Enhancements

Potential additions:
- Mobile touch controls
- Additional enemy types
- Power-up system
- More question categories
- Difficulty settings
- Sound effects toggle
- Background music
- Demo AI mode

## 📜 License

Educational project - Free to use for CAET exam preparation

## 🙏 Credits

**Design Profile**: Complete 21-section game specification
**Development**: Claude AI (Anthropic)
**Educational Content**: Based on CAET certification exam topics
**Inspiration**: Classic arcade games and aviation training

---

**Good luck, Aviation Electronics Technician! Clear those stages and ace those questions! ✈️🎮**

*Defend • Answer • Dominate*
