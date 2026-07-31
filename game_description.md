# 🌍 MapPoint — Game Design Specification

**MapPoint** is a mobile-first, daily geography game inspired by Wordle and GeoGuessr. Every day at **00:00 UTC**, players worldwide receive the exact same sequence of **5 target cities**. Players drop a pin on an interactive map (with satellite and country border views, strictly without city labels) and are scored based on proximity to the true city center. Results can be shared via spoiler-free emoji grids or a visual share card featuring a **scannable QR code**.

---

## 🎮 1. Core Gameplay Loop (Mobile-First)

### 1.1 Daily Challenge Structure
* **Global UTC Reset**: The daily sequence of 5 cities resets globally at 00:00 UTC so all players compare the exact same daily puzzle.
* **Round Flow (Rounds 1 to 5)**:
  1. **City Prompt**: Displays target location in **City, Country** format (e.g., *"Kyoto, Japan"*).
     * *Disambiguation*: State/province added where helpful (e.g., *"Springfield, Illinois, USA"*). See Section 4 for detailed dataset curation notes.
  2. **Label-Free Map Navigation**: The player navigates an interactive map with **NO place/city text labels**.
     * **View Toggles**: Players can switch between **Satellite View** (aerial imagery) and **Country Border View** (clean vector map with coastlines & borders).
  3. **Pin Placement**: Tapping anywhere drops a pin marker. Tapping a new spot updates the pin location.
  4. **Submission**: The player confirms by tapping a clean **Checkmark (✓) Icon Button** (no text button).
  5. **Result Reveal**:
     * An animated line connects the player's pin to the true city center.
     * Displays distance off (*142 km* or *88 mi*) and round score out of 5,000 points.
     * Color-coded rating badge:
       * 🟢 **Spot On / Excellent** (< 50 km)
       * 🟡 **Good** (< 500 km)
       * 🟠 **Fair** (< 2,000 km)
       * 🔴 **Miss** (≥ 2,000 km)
  6. **Next Round**: Tap "Next" to advance to the next city.

### 1.2 Summary & End Screen
* **Total Score**: Out of 25,000 points (5,000 max per round).
* **Overview Map**: Shows all 5 pins and true locations on a single global map view.
* **Stats & Streaks**: Current streak, max streak, total games played, average distance off (saved in `localStorage`).
* **UTC Countdown**: Live timer showing time remaining until the next 00:00 UTC reset.

---

## 🏆 2. Scoring System

$$\text{Points} = \max\left(0, \text{round}\left(5000 \times e^{-\frac{\text{Distance (km)}}{1500}}\right)\right)$$

* **Distance Toggle**: Interactive setting toggle between **Kilometers (km)** and **Miles (mi)**.

| Distance Error | Rating Badge | Approx. Points |
| :--- | :--- | :--- |
| **0 – 15 km** | 🟢 Spot On | 4,950 – 5,000 pts |
| **50 – 100 km** | 🟢 Excellent | 4,680 – 4,830 pts |
| **500 km** | 🟡 Good | ~3,580 pts |
| **1,500 km** | 🟠 Fair | ~1,840 pts |
| **5,000+ km** | 🔴 Miss | < 180 pts |

---

## 📱 3. Social & Sharing Features

### 3.1 Emoji Summary Text
One-tap copy button for social apps and messaging:

```text
MapPoint #42 📍 
Score: 23,450 / 25,000 (Off by: 342 km)
🟢 🟢 🟢 🟡 🟢
🔥 Streak: 7 Days
[Game URL TBD]
```

### 3.2 Visual Share Card + QR Code
* A beautifully formatted image card rendering the 5-pin overview map, daily score, and performance badges.
* Includes an **embedded QR Code** leading directly to the game's canonical URL (domain TBD) so social media viewers can scan and play immediately.
* Buttons for "Copy Image to Clipboard" and "Download Card Image".

---

## 🔮 4. Deferred Topics, TODOs & Future Features

### 4.1 Deferred Topics & Dataset Notes
- 📝 **City Uniqueness Strategy**: Revisit whether `City, State/Province, Country` is guaranteed unique, or if strict canonical GeoName IDs are needed during dataset curation to handle edge cases.
- 🌐 **Domain & Production URL**: Select final domain name to replace `[Game URL TBD]` in share texts and QR codes.

### 4.2 Phase 2 Features (Future Expansion)
- 🔐 **Social Media Authentication**: OAuth integration (Google, Apple, Twitter/X, Discord).
- 👥 **Friends Leaderboard**: Daily and weekly leaderboards among connected friends.
- ⚔️ **Custom Lobbies**: Create custom room links with specific continent/regional city pools.
