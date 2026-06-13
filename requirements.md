# ALIAS GAME

## What Is This App?
The app allows users to enjoy the Alias game experience, understand the rules, and experiment with games with other users.

## Technologies Used

| Technology | Role |
|---|---|
| HTML5 | Page structure and content |
| CSS3 | Visual styling and layout |
| JavaScript (vanilla) | Client-side interactivity and data fetching |
| C# with .NET Minimal API | Web server and API endpoints |


## Graphical Design Considerations

- The game board will be an HTML table element
- The elements of the game page will be positioned using the position: attribute

---

## Pages and What They Do

### Home (`index.html`)
The landing page. It orients the visitor and explains how to use the site.

- Displays a **opening panel** with a brief description of the site's purpose.
- Contains a **download link** that allows the user to download a zip file with all the project's source files.
- Contains a **numbered how-to guide** that instructs the user to read the rules page and then enter the game page and start playing

### About (`about.html`)

Explains how the site itself was built, acting as documentation of the example.

- Describes the **purpose of the project** in plain language.
- Contains a **table breaking down each part** of the app (HTML, CSS, JavaScript, C#) and what role each plays.
- Lists every **HTML element used in the site** with a brief description of what it is used for — a practical HTML reference for students.
- Lists exactly **what the JavaScript does** across the site in plain English.
- Describes each **server-side API route** in plain English, without code — what it returns and what triggers it.

### Requirements (`requirements.html`)
| Category | Requirement | Status |
| --- | --- | --- |
| General | Multiple pages with a shared layout | Yes |
| General | About page included | Yes |
| HTML | Headings, links, lists, tables, and image | Yes |
| CSS | Single shared stylesheet | Yes |
| JavaScript | Basic interactivity and data loading | Yes |
| Server | C# endpoint returning data to the browser | Yes |

### Rules (`rules.html`)
The most important page, users should start with it first to understand what to do.
The page explains the rules of the game in plain English and uses headings and bullet points to make it easier for the user to understand
The page is divided into two parts separated by headings.
The first part, an explanation of the game itself and its rules.
The second part, an explanation of how the game is implemented, how to start, who should click on what.


### Game (`game.html`)
The most interactive and useful page.
here The players can start playing with other players.

#### There are two modes in this page
#### Mode one
When the user first opens the page, he sees a button to select the color.
He needs to click on his group's color and only then can he continue. When the color is selected 
one is added to the active teams color array on the server (POST).
The team color of the player cannot be changed after selection. After the color is selected 
the page switches to mode two.

#### Mode two
Now that the user has chosen a color, the game can begin.
The screen changes and instead of choosing colors, the game board appears.

#### The game page:
The game page combines several different graphical elements.

#### Page background:
The background of the page changes depending on the team whose turn it is to play. This will be fetched (GET) from the server each second

#### The game board:
The game board appears relatively in the middle.

- The game board is red and consists of a spiral of white circles numbered 1 to 8 and repeated up to 9 times.
Of the circles, there are 4 special circles that are red.
At the end of the spiral of circles is a large circle for victory.
The game board remains in place and does not change throughout the game.
- There are up to six pawns depending on the number of teams, each having its own color.
Each pawn is initially located on the first circle of the board and is played by one team.
The pawns move forward accross board circles during the game according to the progress of the team.
The pawns positions is fetched from the server (GET) each second.

#### The cards:
Next to the game board there are playing cards. 
Each card has a list of words numbered from 1 to 8. 
The top card is facedown for all players except the player that is currently the one saying aliases out loud.
When a card becomes face up, it is fetched from the server (GET). This happnes when either of the next section buttons is clicked.

#### Buttons elements:
Near the deck of cards are three buttons.
- **Start button:** One of the players whose team is taking turns to play should press the button.
The button starts the a timer for the player's turn.
- **+ / - Buttons:** Two buttons that are adjacent to each other
According to the rules of the game, the selected player must mark whether or not his team succeeded in the card.
The right button indicates that it succeeded and the left one indicates that it did not.

#### Score
Starts with zero at the begining of the turn. Incremented when + button is pressed. Descremented when - button is pressed. Score is only stored on the client.
It is short lived and is relevant only during the turn. When the timer (from the server) reaches 0, the Position of the current team (on the server) is advanced by score.
In the same turn the score of several teams can be positive and will increase the Position of each teams pawn (POST)

## Another elements:
- **Some shape** painted in the color of the team the user has chosen. It is not stored on the server.
- **Timer** that measures one minute from the moment the button is pressed. Located right above the button that activates it. 
The timer is started on the server (POST) and the remaining time is visible by each client (GET)
---

## Behaviour That Applies to Every Page

### Shared navigation header

Each page displays the same header at the top, containing:
- Site title and a one-line description.
- Navigation bar with five links: Home, About, Requirements, Rules, Game.
- The header is **not duplicated** in each HTML file. It is stored in a single separate file and dynamically loaded by JavaScript on each page.

### Active Navigation Highlighting

The navigation link corresponding to the **currently viewed page** is automatically highlighted (visually unique background color). This is determined by JavaScript.

### Shared Footer

Each page has an identical footer at the bottom that credits the technologies used.

---

## Data and State
The server will hold the current game state in memory:
- Turn: an integer from 1 to 6 indicating who's team turn it is. e.g. 3 == yellow
- Teams: Active teams colors: An array of integers with size 6. Each position corresponds to a fixed color. e.g. Teams[0] == 2 means there are two 
players in the red team. Position 0 is reserved for red.
- Positions of pawns: An array of length 6 where each value is a number between 1 and 71 (the number of circles on the board that the pawn on heam). Each array 
index is reserved for a specific color which matches the same color in Teams. e.g. Teams[1] and Position[1] are reserved for green
- Initially all cards: An array of strings on length 6, Can include places without value.
- Timer: an integer between 0 and 60 indicating how much time is left for the current turn. 0 indicates no one is currently playing.
## User Interactions
Users interact via clicking buttons to choose colors, starting the timer, and reporting success/failure of words.
All interactions send fetch requests to the API.

| Interaction | Where | Result |
| --- | --- | --- |
| Click a nav link | Any page | Navigates to that page; active link highlight updates |
| Click the download link | Home page | Browser downloads a zip file of the project source |
| Click a team color button | Game page - Step one | Selects the team color for the session and transitions to Step two; selection cannot be changed |
| Click the top button | Game page - Step two | Starts the 1 minute countdown timer for the teem turn and determines the speaker for the current turn|
| Click the right button (Success) | Game page - Step two | Sends a request to the server to add a positive point to the active team's current turn score |
| Click the left button (Failure) | Game page - Step two | Sends a request to the server to add a negative point (minus) to the active team's current turn score |
| Timer reaches zero (End of turn) | Game page - Step two | The server calculates the total progress (successes minus failures), updates the team's position and soldier image on the board, and switches the turn to the next team |
| Automatic state fetch | Game page - Step two | JavaScript fetches the latest game state from the server and changes the page background color according to the active team's turn |


## What the App Does NOT Do
- No user accounts or login.
- No client-side routing or single-page app behaviour.
- No error messages shown to the user when API calls fail (elements show a fallback text string).
- No admin interface.
- No search.
- Single game session only. Only one game can be active at a time
- My app is not responsive
