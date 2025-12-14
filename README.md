#  Raja Mantri Chor Sipahi (King Minister Thief Police) Backend

This is the backend server for the classic Indian multiplayer party game, Raja Mantri Chor Sipahi. It is built using Node.js and Express to manage game state, player roles, scoring, and RESTful API endpoints for game management.

---

## Key Features

* **RESTful API:** Manage room creation, joining, role assignment, and result calculation.
* **In-Memory State:** Uses a global `state` object to manage rooms and player data for fast, session-based gameplay.
* **Correct Scoring Logic:** Implements the traditional scoring rules, including the critical Mantri-Chor point exchange on an incorrect guess.
* **UUID Generation:** Uses `uuid` to ensure unique IDs for rooms and players.

---

## Setup and Installation

### Prerequisites

You need to have Node.js and npm installed on your system.

### Steps

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/YOUR-USERNAME/raja-mantri-chor-sipahi-backend.git](https://github.com/YOUR-USERNAME/raja-mantri-chor-sipahi-backend.git)
    cd raja-mantri-chor-sipahi-backend
    ```

2.  **Install Dependencies:**
    ```bash
    npm install express body-parser uuid
    ```

3.  **Run the Server:**
    ```bash
    node server.js
    ```
    The server will start running on `http://localhost:3000`.

---

## API Endpoints

The server manages the game state using the following REST endpoints. All endpoints use `http://localhost:3000` as the base URL.

| Endpoint | Method | Description | Body Example | Response Example |
| :--- | :--- | :--- | :--- | :--- |
| `/room/create` | `POST` | Creates a new room. | `{"playerName": "Host Name"}` | `{roomId, playerId, playerName}` |
| `/room/join` | `POST` | Allows a player to join by ID. | `{"roomId": "...", "playerName": "Guest Name"}` | `{roomId, playerId, playerName}` |
| `/room/assign/:roomId` | `POST` | **Starts the round.** Assigns the 4 roles randomly. Requires exactly 4 players. | (None) | `{"success": true, "status": "PLAYING"}` |
| `/role/me/:rId/:pId` | `GET` | **PRIVATE.** Retrieves a player's assigned role. | (None) | `{"role": "Mantri"}` |
| `/guess/:roomId` | `POST` | Mantri submits their guess for the Chor. | `{"playerId": "MantriID", "guessedPlayerId": "SuspectID"}` | `{"success": true, "message": "..."}` |
| `/result/:roomId` | `GET` | Reveals roles, calculates scores for the round, and updates cumulative scores. | (None) | `{success: true, results: {...}}` |
| `/leaderboard/:roomId`| `GET` | Returns the current cumulative scores for all players in the room, sorted. | (None) | `{success: true, leaderboard: [...]}` |
| `/room/reset/:roomId` | `POST` | Resets the roles and status for a new round (keeps players and scores). | (None) | `{"success": true, "status": "WAITING"}` |

---

## Testing the API with Postman

A Postman Collection is provided in this repository (`RajaMantri.postman_collection.json`) to quickly test the entire game flow. 

### 1. Import the Collection

1.  Open Postman.
2.  Click **Import** and select the `RajaMantri.postman_collection.json` file from the cloned repository.

### 2. Follow the Game Flow

Run the requests in the following order, ensuring you save the `roomId` and `playerId`s as environment variables or manually update the request URLs/bodies as you go:

1.  **POST Create Room:** Get `ROOM_ID` and `P1_ID`.
2.  **POST Join Room:** Run 3 times to get `P2_ID`, `P3_ID`, `P4_ID`.
3.  **POST Assign Roles:** Starts the round.
4.  **GET Find Role:** Run 4 times to identify the **Mantri** and **Chor** ID.
5.  **POST Submit Guess:** Use the **Mantri ID** and the chosen suspect ID (Chor or another player).
6.  **GET Get Results & Score:** Check the round scores based on the guess outcome.
7.  **GET Get Leaderboard:** View the cumulative scores.
8.  **POST Reset for New Round:** Prepare for the next round.
