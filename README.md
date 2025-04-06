# Nexus Game

## Overview and Objective

Nexus Game is a web-based solitaire card game designed to be played either via a web app or with physical decks.  
In this game, you build sequences of cards to achieve a target number, lock them for points, and use special Fixit cards to manipulate locked sequences or correct misplays.  
The game emphasizes strategy, suspense (via face-down Fixit cards), and careful state management.

## Game Mechanics and Key Rules

- **Sequence Building:**  
  Create sequences by arranging cards in ascending order. Clarify how duplicates or near-complete sequences are handled.

- **Locking Mechanism:**  
  Lock sequences to secure points. Locked sequences may provide bonus points.

- **Fixit Cards:**  
  A deck of 10 special Fixit cards is included. These cards are single-use and played face down. When a Fixit card is played:
  - Place the card face down then reveal it immediately.
  - Its action (e.g., inserting, reordering, removing, or retrieving a card) is applied.
  - The turn ends immediately after the Fixit move.
- **Scoring and Turn Mechanics:**  
  Define the target number (static or dynamic) and how bonus points or penalties (e.g., for unused Fixit cards) are applied. Further clarify when and how the action phase ends.

## Modular Architecture and File Structure

This project is built with ES6 modules for maintainability and testing. The key aspects are:

- **Entry Point:**  
  The main game logic is initialized in `/src/main.js`.  
  The `index.html` file (located at the project root) loads this entry point via:

  ```html
  <script type="module" src="./src/main.js"></script>
  ```

- **Modular Organization:**  
  Logic is separated into modules located in `/src/modules`:

  - **deckManager.js:** Handles deck operations like shuffling (Fisher-Yates algorithm) and dealing cards.
  - **gameState.js:** Manages the game state, including current turn, score, and locked sequences.
  - **uiRenderer.js:** Renders UI components using Tailwind CSS.
  - **fixitLogic.js:** Contains the logic for executing Fixit card moves and enforcing immediate turn termination.

- **Other Key Files:**
  - `package.json`: Manages dependencies such as Vite, Tailwind CSS, PostCSS, etc.
  - `vite.config.js`: Configures development and build settings.
  - `/public`: Contains static assets like images and icons for gameplay.

## Pseudocode Implementation

Below is an excerpt of the pseudocode that guides the game’s core loop and logic:

```JavaScript
// [PSEUDOCODE]
// Initialize decks and game state:
playDeck = createPlayDeck();          // Create 52 cards with red back.
fixitDeck = createFixitDeck();        // Create 10 Fixit cards with blue back.
playDeck = shuffleDeck(playDeck);
fixitDeck = shuffleDeck(fixitDeck);
fixitHand = dealCards(fixitDeck, 2);   // Pre-dealt Fixit cards.
hand = dealCards(playDeck, 5);         // Initial player hand.

// Main Game Loop:
while (gameIsRunning) {
    renderGameState(hand);
    let userInput = getUserInput();
    if (userInput === "playCard") {
        let playedCard = hand.shift();
        updateGameState(playedCard);
    } else if (userInput === "playFixit") {
        if (fixitHand.length > 0) {
            let fixitCard = fixitHand.shift();
            applyFixitMove(fixitCard, targetSequence);
            fixitCard.used = true;
            continue; // End turn after a Fixit move.
        }
    }
    if (isGameOver() || (playDeck is empty && hand is empty)) {
        gameIsRunning = false;
    } else {
        // Prepare new turn (e.g., draw additional cards)
    }
}
```

## Gameplay Notes

- **Game Flow:** Players begin by shuffling the decks and dealing cards. During each turn, they choose to play a normal card or a Fixit card. Playing a Fixit card immediately ends the turn.
- **Winning Conditions:** The game continues until the play deck and hand are empty, or a specific target number is reached as per the game rules.
- **Persistence:** Game state is saved and restored using localStorage, allowing players to resume their game between sessions.

## Next Steps

- Refine detailed game rules and scoring mechanisms.
- Implement unit tests to ensure randomness during shuffle and proper state updates.
- Finalize UI design using Tailwind CSS.

Happy coding and game developing!
