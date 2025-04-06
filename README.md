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

## Pseudocode Implementation (High-Level)

Below is a high-level description of the main events in the game loop. Note that this is not functional code, but instead a conceptual outline meant to illustrate the game flow.

```plaintext
[HIGH-LEVEL EXPANDED PSEUDOCODE]

// Initialize game resources:
  - Create a 52-card play deck (with red backs).
  - Create a 10-card Fixit deck (with blue backs).
  - Shuffle both decks.
  - From the Fixit deck, deal 2 cards face down (pre-dealt Fixit hand).
  - From the play deck, deal 5 cards to the player.
  - Initialize game state variables (e.g., currentTurn, score, target number, etc.).

// Main Game Loop:
WHILE gameIsRunning:
  - Render the current game state:
      - Display player's hand, locked sequences, score, and remaining deck count.
  - Prompt for player's input:
      - userInput = getUserInput()  // e.g., "playCard" or "playFixit"
  - Process the input:
      IF userInput is "playCard":
          - card = selectCardFromHand()        // Let the player choose a card
          - IF isValidMove(card):
                - updateGameStateWithCard(card)  // Integrate card into a sequence
                - log the event (card played)
          - ELSE:
                - notify the player of an invalid move (no state change)
      ELSE IF userInput is "playFixit":
          - IF fixitHand is not empty:
                - fixitCard = removeCardFromFixitHand()  // Pre-dealt Fixit card selected
                - reveal fixitCard and determine its action
                - applyFixitMove(fixitCard, targetSequence)  // Insertion, reorder, remove, retrieve
                - mark fixitCard as used
                - log the event and immediately terminate the current action phase
                - (Skip any remaining actions in this turn)
          - ELSE:
                - notify the player that no Fixit cards are available
  - Check for game end conditions:
      - IF (play deck is empty AND player's hand is empty) OR win/loss criteria met:
              - set gameIsRunning to false
      - ELSE:
              - Prepare for the next turn:
                    - Optionally, draw additional cards if the hand is below a threshold
                    - Increment currentTurn and reset temporary phase values

// End loop when gameIsRunning is false.
```

This description is intended to serve as a conceptual guide and should not be treated as production code.

## Gameplay Notes

- **Game Flow:** Players begin by shuffling the decks and dealing cards. During each turn, they choose to play a normal card or a Fixit card. Playing a Fixit card immediately ends the turn.
- **Winning Conditions:** The game continues until the play deck and hand are empty, or a specific target number is reached as per the game rules.
- **Persistence:** Game state is saved and restored using localStorage, allowing players to resume their game between sessions.

## Next Steps

- Refine detailed game rules and scoring mechanisms.
- Implement unit tests to ensure randomness during shuffle and proper state updates.
- Finalize UI design using Tailwind CSS.

Happy coding and game developing!
