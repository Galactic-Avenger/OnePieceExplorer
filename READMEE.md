# One Piece Explorer — iOS App

A One Piece–themed iOS app built with **UIKit** and **Swift** that lets users browse characters, crews, and Devil Fruits pulled live from a GraphQL API. Includes a trivia quiz, a favorites system, a custom wanted poster leaderboard, and a card-expand navigation transition.

---

## Demo

[![One Piece Explorer Demo](https://img.youtube.com/vi/YBJABlX2ddI/hqdefault.jpg)](https://youtube.com/shorts/YBJABlX2ddI)

---

## Features

### Live GraphQL API Integration
- Built a custom `APIService` class using `URLSession` that queries a GraphQL endpoint via HTTP POST
- Supports paginated character fetching, name-based search, single-character lookup by ID, random character fetch, and concurrent multi-page loading with `DispatchGroup` and `NSLock` for thread safety
- Structured error handling via a custom `APIError` enum covering network failures, decoding errors, and GraphQL-level errors

### Multi-Mode Browser (Characters / Crews / Devil Fruits)
- Horizontal pill-tab switcher that dynamically swaps child view controllers into a shared container view
- Follows the **child view controller pattern** — each mode (`CharactersViewController`, `CrewsViewController`, `DevilFruitsViewController`) is embedded and managed via `addChild` / `removeFromParent`

### Characters Grid with Infinite Scroll & Search
- Paginated `UICollectionView` grid that loads the next page as the user scrolls toward the bottom
- Pull-to-refresh via `UIRefreshControl`
- Real-time search: instant local filter for immediate feedback + debounced API call (0.5s) for server-side results
- Search history persisted via `PlistService`

### Character Detail Screen
- Full-bleed hero image with a `CAGradientLayer` fade for badge legibility
- Overlay badges for Devil Fruit type and character origin placed directly on the image
- Per-crew color theming: accent color, card gradient, and glow effect change based on which crew the character belongs to (Straw Hats, Marines, Beast Pirates, Big Mom Pirates, and more)
- Rarity chip (UR / SSR / SR) with matching glow shadow via `layer.shadowColor`
- Initials avatar fallback when image fails to load
- Info card: Bounty, Devil Fruit, Age, Birthday, Blood Type, Origin, and Debut — all pulled from the API
- Share sheet via `UIActivityViewController`

### Custom Card-Expand Navigation Transition
- `UIViewControllerAnimatedTransitioning` implementation: when a character card is tapped, a snapshot of the card animates into the hero position of the detail screen using spring physics (`usingSpringWithDamping`), replacing the default push slide

### Trivia Quiz
- Dynamically generates three question types from live API data: "Who has this bounty?", "Which crew is this character in?", and "Who ate the X Devil Fruit?"
- Tracks score and streak across questions
- Pulls characters and Devil Fruits concurrently to build the question pool

### Favorites System
- Long-press context menu (`UIContextMenuConfiguration`) on any character card to add or remove from favorites
- Favorites persisted locally via `PlistService` (property list files)
- Empty state view when no favorites are saved
- Favorites tab reloads on `viewWillAppear` to stay in sync with changes made elsewhere

### Wanted Poster Leaderboard
- Custom `UICollectionViewLayout` (`WantedPosterWallLayout`) that arranges cells like a marine bulletin board — slight random rotations, staggered positions, top-3 characters displayed larger than the rest, with all layout math cached in `prepare()` for smooth scrolling
- Custom `WantedPosterCell` styled as an in-universe wanted poster: parchment background, "WANTED — DEAD OR ALIVE" header, character photo, red bounty amount, and rank label
- Fetches multiple API pages concurrently to build the full bounty leaderboard

### Animated Splash Screen
- Seamless handoff from the static launch screen using the same Luffy image (no visible jump)
- Plays *Bink's Sake* via `AVAudioPlayer` (native AVFoundation — no third-party libraries)
- Animated gold title fade-in, tap-to-skip, 10-second auto-transition to main app

### Network Monitoring & Offline Banner
- Monitors connectivity in real time via `NotificationCenter`
- Animated banner slides in when offline and slides out when connection is restored

### Image Caching
- Custom in-memory `ImageCache` backed by `NSCache` to avoid redundant network requests for character images

### Theme System
- Centralized `Theme` struct providing consistent colors, gradients, glow effects, and navigation bar styling across all screens
- Per-crew accent colors and card gradients keyed to `CrewTheme` enum

---

## Tech Stack

| | |
|---|---|
| **Language** | Swift 5.0 |
| **UI Framework** | UIKit (programmatic layout — no Storyboards for main UI) |
| **Networking** | URLSession + GraphQL (POST) |
| **Data Persistence** | Property List (PlistService) |
| **Audio** | AVFoundation (AVAudioPlayer) |
| **Architecture** | MVC with child view controller composition |
| **Layout** | NSLayoutConstraint-based Auto Layout |
| **Platform** | iPhone & iPad (iOS 26.2+) |
| **Xcode** | 26.3 |

---

## Getting Started

### Requirements
- Xcode 26.3 or later
- iOS 26.2+ device or simulator

### Running the App
1. Clone the repo
   ```bash
   git clone https://github.com/your-username/OnePiece.git
   ```
2. Open `OnePiece.xcodeproj` in Xcode
3. Select a simulator or connected device
4. Press **⌘ R** to build and run

> No third-party dependencies — no CocoaPods, Carthage, or Swift Package Manager required.

---

## Project Structure

```
OnePiece/
├── OnePiece.xcodeproj/
└── OnePiece/
    └── OnePieceExplorer/
        ├── Controllers/      # View controllers (Characters, Detail, Quiz, Favorites, Leaderboard, Splash)
        ├── Cells/            # Custom collection view cells and layouts
        ├── Models/           # Data models (Character, DevilFruit, Crew, Canon)
        ├── Services/         # APIService (GraphQL), PlistService (persistence)
        ├── Extensions/       # BountyLabel and other UI extensions
        └── Support/          # AppDelegate, Theme, audio assets
```

---

## About

Built by Ali Saleh as a CSCI course project at CUNY Hunter College.
