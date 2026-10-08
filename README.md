# STI-OPOLY Online — 10-player real-time nursing game

A small Node.js/Socket.IO web app. One host creates a private six-character room code; up to nine other players join on separate laptops. Questions and rationales are drawn from the user-provided Chapter 44 text. Monetary events are fictional.

## Run locally

1. Install Node.js 18 or newer.
2. In this folder, run `npm install`.
3. Run `npm start`.
4. Open `http://localhost:3000`.

To test multiple players locally, use separate browser profiles or incognito windows. The host creates a room; other players join with the code.

## Deploy online (Render example)

1. Upload this project to a **private** GitHub repository (the question bank is derived from course material).
2. At https://render.com create a **New Web Service** connected to that repository.
3. Select **Node**, build command `npm install`, start command `npm start`.
4. Choose a hosting plan that supports an always-on Node web server and WebSockets. Set the instance count to **one**.
5. Open the resulting `https://...onrender.com` URL, create a room, and share the URL and room code with your classmates.

Other platforms that support persistent Node servers and WebSockets can also host this project. Do not deploy as a static-only site.

## Game rules

- 2–10 players, $1,500 each, 1 Get Out of Detention Free card per player.
- 40-space board, classic color groups, server-generated dice, pass GO +$200.
- Unowned property can be bought; rent transfers automatically. Complete color groups double base rent. Clinics and Labs have special rent formulas.
- Chance / Community Care spaces give the current player a private multiple-choice question. Incorrect answers forfeit **all** their properties to the bank (not to other players); cash remains. Correct answers trigger a random fictional in-game cash event.
- Any active player can offer properties and/or cash in a trade. Recipient must explicitly accept or decline; the server validates ownership and funds again before transfer.
- Detention: roll doubles, pay $50, or use your free card. After three unsuccessful rolls, pay $50 and move.
- Negative cash automatically liquidates properties at half purchase value; inability to pay after liquidation means bankruptcy.
- Last active player wins automatically, or host can end the game and show a 10-player leaderboard by cash plus original purchase prices of held properties.
- Simplified rules: no houses, mortgages, auctions, or double-roll bonus turns.

## Important operational limits

- Rooms and sessions are held **in server memory**. A server restart erases all games. Do not use multiple server instances without adding a shared state store.
- A player can reconnect from the same browser using the locally stored reconnect token. Clearing browser storage or switching devices prevents reclaiming that seat once the game starts.
- A disconnected player remains in the turn order. Host should wait for them to reconnect; there is no timeout or forced skip in this version.
- Room codes are invitation codes, not secure authentication. Share them only with classmates. Do not enter real patient information.
- This is an educational game, not clinical decision support. The chapter may contain older clinical guidance; use current institutional protocols for real care.

## Separate host computer / projector board

The host display is a **read-only live spectator screen**. It does not take up one of the 10 player slots, and it never shows private question answers or player trade controls.

1. Create a game from the normal website and copy the six-character room code.
2. On the classroom host computer, TV browser, or projector, open `https://YOUR-SITE/display.html?code=ROOMCODE` (substitute your actual site and room code). Alternatively, open `/display.html` and type the code.
3. The 10 players join the normal website on their own laptops. Their movements, cash, properties, dice, and the final leaderboard appear live on the host display.
4. The host may play from their own laptop and use the spectator display on another computer, or open both screens in separate tabs.

The display automatically reconnects after temporary connection interruptions, as long as the server room still exists. Game rooms are stored in server memory and disappear if the server restarts.
