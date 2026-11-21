# 🎰 Slot Game - Unity 2D & WebGL

A classic 3-reel casino slot machine game built with Unity 2D (Universal Render Pipeline - URP) and exported to WebGL.

---

## 📖 1. Game Overview
The Slot Game simulates a retro casino slot machine experience right in your web browser. Players place bets, spin three independent reels featuring custom symbol sprites, and receive instant payouts based on winning combinations.

### 🎮 Gameplay Rules
1. **Starting Bankroll**: Every player begins with **1,000 credits**.
2. **Bet Management**:
   - Default bet is set to **10 credits**.
   - Use the **`+` (Add)** button to increase the bet in increments of 10 credits.
   - Use the **`-` (Subtract)** button to decrease the bet in increments of 10 credits (minimum bet is 10).
   - Real-time balance validation prevents betting more than your available funds.
3. **Spinning the Reels**:
   - Press the **`SPIN`** button to start. The bet is deducted immediately from your balance.
   - Controls are locked during the spin to ensure state integrity.
   - The reels cycle through random symbols with staggered stop timers (Reel 1 stops first, followed by Reel 2 and Reel 3) to build classic casino suspense.
4. **Payout Table & Win Logic**:
   - 🌟 **JACKPOT (3 Matching Symbols)**: Payout is **10x current bet** (`CurrentBet * 10`).
   - 🥈 **PAIR (2 Matching Symbols)**: Payout is **2x current bet** (`CurrentBet * 2`).
   - ❌ **NO MATCH**: Bet is lost, status reads *"You Lose! Try Again"*.
   - If balance drops below the bet amount, the machine warns *"Insufficient Balance!"*.

---

## 🚀 2. Instructions to Run WebGL Build

The repository includes a complete, pre-compiled WebGL production build inside `/Build/WebGL/`.

Because WebAssembly (`.wasm`) and WebGL require HTTP/HTTPS streaming (browsers restrict them when opening directly from local `file:///` paths due to CORS), run the game using any simple local web server:

### Option A: Using Python (Recommended - 1 Command)
```bash
# Navigate to the WebGL build folder
cd Build/WebGL

# Launch local HTTP server
python -m http.server 8080
```
Open your browser and visit:
👉 **`http://localhost:8080`**

---

### Option B: Using Node.js
If you have Node.js installed:
```bash
# Using npx serve
npx serve Build/WebGL

# OR using http-server
npx http-server Build/WebGL -p 8080
```
Open the URL shown in your terminal (e.g. `http://localhost:3000` or `http://localhost:8080`).

---

### Option C: Using VS Code Live Server
1. Open the project folder in Visual Studio Code.
2. Install the **Live Server** extension.
3. Right-click on `Build/WebGL/index.html` and select **"Open with Live Server"**.

---

### Option D: Play Online via GitHub Pages
The game is deployed live on GitHub Pages:
🔗 **[Play Live WebGL Demo](https://shashank981.github.io/slotgame/)**

---

## ✨ 3. Bonus Features

1. **Staggered Reel Deceleration (Anticipation Timing)**:
   - Rather than stopping all reels simultaneously, Reel 1 stops at `1.0s`, Reel 2 at `1.5s`, and Reel 3 at `2.0s`. This creates the classic physical slot machine feeling of anticipation on the final reel.
2. **Multi-Tier Win Structure**:
   - Payout logic rewards both full Jackpots (10x) and near-miss Pairs (2x), sustaining player engagement and balancing bankroll longevity.
3. **Dynamic Bet Guardrails**:
   - Interactive balance checking automatically clamps bet values to prevent negative betting or betting more than the current bankroll.
4. **State-Preserving UI Locking**:
   - During spinning coroutines, all button interactions (`Spin`, `+`, `-`) are cleanly disabled (`interactable = false`) to prevent race conditions or duplicate transactions.
5. **Universal Render Pipeline (URP) 2D**:
   - Built on Unity's modern 2D Universal Render Pipeline with global 2D lighting and clean canvas scaling.
6. **Responsive WebGL Canvas**:
   - Automatically adapts canvas scaling to desktop monitors, laptop screens, and mobile browser viewports.

---

## 🧠 4. Thought Process or Approach

### Architecture & Design Decisions
- **Decoupled Reel & Game Manager Responsibilities**:
  - `Reel.cs`: Dedicated to local reel state. Handles sprite cycling via rapid interval updates (`0.05s`), captures the final stop index (`ResultIndex`), and manages the boolean spin flag (`IsSpinning`).
  - `SlotGameManager.cs`: Acts as the central game coordinator. Orchestrates bankroll math, bet modification, initiates staggered coroutines, checks combinations across all reels, and updates TextMeshPro UI elements.
- **Asynchronous Coroutine Workflow**:
  - Leveraged Unity `IEnumerator` coroutines with `WaitUntil(() => !reel.IsSpinning)`. This keeps reel animations non-blocking, completely smooth, and easy to sequence without complex callback trees.
- **Payline Evaluation**:
  - Compares reel results (`r1 == r2 && r2 == r3` for Jackpot, and `r1 == r2 || r2 == r3 || r1 == r3` for Pairs).
- **Clean Repository Hygiene**:
  - Standard Unity `.gitignore` tuned to exclude transient cache files (`Library/`, `Temp/`, `Logs/`, `UserSettings/`, `.vs/`) while explicitly preserving `/Build/WebGL/` as mandated by submission guidelines.

---

## 📁 Project Structure
```
SLOTGAME/
├── Assets/
│   ├── Animations/          # Reel and UI animation assets
│   ├── Prefabs/             # Reusable UI & Reel prefabs
│   ├── Scenes/
│   │   └── SampleScene.unity# Main 2D slot machine scene
│   ├── Scripts/
│   │   ├── Reel.cs          # Reel spinning animation & outcome selection
│   │   └── SlotGameManager.cs# Game loop, balance, bets, & payout logic
│   ├── Settings/            # Universal Render Pipeline (URP) profiles
│   ├── Sounds/              # Sound effects & audio clips
│   ├── Sprites/             # Reel icons, frames, and background graphics
│   ├── TextMesh Pro/        # High-definition fonts & styling
│   └── UI/                  # UI components and layout structures
├── Build/
│   └── WebGL/               # Production WebGL build
│       ├── Build/           # WASM binary, data, loader, & framework
│       ├── TemplateData/    # Favicons, style.css, & progress bars
│       └── index.html       # WebGL entry page
├── Packages/                # Unity Package Manager manifests
├── ProjectSettings/         # Unity engine, physics, and input configurations
├── .gitattributes           # Git line normalization
├── .gitignore               # Unity-optimized ignore rules
└── README.md                # Comprehensive documentation
```

---

## 🔗 Repository Link
- **GitHub Repository**: [https://github.com/shashank981/slotgame](https://github.com/shashank981/slotgame)
