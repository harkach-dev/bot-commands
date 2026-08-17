# Connections User & Player Guide

Welcome to the **Connections** guide! This document explains how the musical word-matching game works in WavezBOT, how your streaks are tracked, and how to use all the commands available to DJs and players.

---

## 1. What is Connections?

**Connections** is a fun, continuous musical chain-reaction game played right in the music room.

When you step up to DJ, your goal is to "connect" your song to the song played immediately before yours. A connection is made whenever your song title shares a sequence of **4 or more letters** with the previous song title.

### Key Rules & Highlights

- **4-Letter Minimum**: To make a connection, the matching word or letter sequence must be at least **4 letters long** (e.g. `love`, `rock`, `star`, `dance`, `california`).
- **Opt-In Game**: Connections is completely optional. You choose when to join or leave.
- **Personal Streaks**: Your streak measures how many times _you_ connect on your own DJ turns. It does not matter how many other DJs play between your turns!
- **Play With Everyone**: You can connect to the previous song even if the DJ who played it has not joined the game.
- **Separate Per Room**: Your stats and streaks are tracked separately for each room you visit.
- **Saved Across Bot Restarts**: Your records and stats are safely saved in the bot's database, so they will never disappear when the bot restarts.

---

## 2. How Song Titles are Matched (The Cleaning Magic)

You do not need to worry about strange formatting, punctuation, or YouTube video titles ruining your match. Before checking for a connection, WavezBOT automatically cleans and "squashes" both song titles to make matching fair and easy.

### What the Bot Automatically Does:

1. **Ignores Capitalization**: Uppercase and lowercase letters are treated the same (`Rock` = `rock`).
2. **Removes Punctuation & Symbols**: Spaces, dashes, quotes, apostrophes, and symbols are ignored (`I'll Fly` becomes `illfly`).
3. **Cleans Accents & Special Characters**: Letters like `é`, `ñ`, and `ü` are turned into normal letters like `e`, `n`, and `u` (`Café` becomes `cafe`).
4. **Strips Video & Audio Tags**: Text inside brackets or parentheses like `[Official Music Video]`, `(Live at Wembley)`, or `{4K}` is removed.
5. **Removes Collaboration / Featured Artists**: Tags like `feat. Drake`, `ft. Ariana`, or `w/ Snoop Dogg` are removed so only the actual song title is compared.
6. **Removes Mix & Version Labels**: Terms like `Radio Edit`, `Extended Mix`, `Club Mix`, or `Remix` are automatically cleaned out.
7. **Removes Trailing Descriptors**: Ending tags like `// Metal Cover` or `-- Acoustic` are ignored.

### Examples of Title Cleaning & Matching

| Previous Song Title                     | Your Song Title                       | Cleaned Text Checked by Bot            | Match Result                                              |
| :-------------------------------------- | :------------------------------------ | :------------------------------------- | :-------------------------------------------------------- |
| **Hotel California (Live)**             | **California Gurls feat. Snoop Dogg** | `hotelcalifornia` vs `californiagurls` | ✅ **Connected!** (Match: `"california"`)                 |
| **Star Spangled Banner // Metal cover** | **Dark Star**                         | `starspangledbanner` vs `darkstar`     | ✅ **Connected!** (Match: `"star"`)                       |
| **I'll Fly With You**                   | **Fly With Me (Radio Edit)**          | `illflywithyou` vs `flywithme`         | ✅ **Connected!** (Match: `"flywith"`)                    |
| **Fly Away**                            | **Fly Me to the Moon**                | `flyaway` vs `flymetothemoon`          | ❌ **No Match** (`"fly"` is only 3 letters; minimum is 4) |

---

## 3. How Streaks & Scoring Work

The bot tracks three main stats for every participating player:

1. **Total Connections**: The all-time number of successful connections you have made in the room.
2. **Current Streak**: How many successful connections you have made in a row during your recent turns.
3. **Record Streak**: The highest streak length you have ever achieved in this room.

### Important Gameplay Situations

- **What Happens If You Miss a Connection?**
  If you play a song that does not connect to the previous song, your **Current Streak** resets to `0`. However, your **Total Connections** and your **Record Streak** are never lost.
- **DJs Playing Between Your Turns**:
  Streaks are 100% individual. If DJ Alice plays, and then three other DJs play their songs before Alice's next turn, Alice's streak is still alive and waiting for her next turn.
- **Consecutive Plays by the Same DJ**:
  If you play two or more songs in a row (for example, if you are the only DJ on the decks), your second song will _not_ count as a connection and will _not_ break your streak. The bot simply pauses and waits for your next turn after another DJ plays.
- **What If a Song Has a Very Short or Missing Title?**
  If a DJ plays a song with a title that has fewer than 4 letters (or is unreadable), your streak will **not** be broken. The bot keeps the previous valid song as the active target for you to connect to! In chat, the bot will post a friendly reminder showing the active song title to aim for.

---

## 4. Summary Cheat Sheet for Players

| Goal                         | Command to Type       | Notes                                                    |
| :--------------------------- | :-------------------- | :------------------------------------------------------- |
| **Join Connections**         | `!connections optin`  | Starts tracking your stats in the room.                  |
| **Check Your Stats**         | `!connections`        | Shows your total, current streak, and record.            |
| **Check Another DJ's Stats** | `!connections @Alice` | Replace `@Alice` with any user's name.                   |
| **Leave Connections**        | `!connections optout` | **Warning**: Resets and deletes your stats in this room. |

> [!TIP]
> **Command Shortcuts**: You can use `!conn` or `!connection` instead of typing out `!connections`!

---

## 5. Player Commands Guide

### Command 1: `!connections optin` (Join the Game)

Use this command to join the Connections game in your current room.

#### How to Use It

- Type `!connections optin` (or `!connections join`).

#### Examples of Allowed Usage

```text
!connections optin
!connections join
!conn optin
```

#### What You Will See in Chat

- **First-time opt in**:
  ```text
  @Alice, you have successfully opted into Connections! Your streak starts now.
  ```
- **If you are already opted in**:
  ```text
  @Alice, you are already participating in Connections! (Streak: 5 | Record: 8 | Total: 14)
  ```

_(Your existing stats and streaks are safely preserved without resetting anything)._

---

### Command 2: `!connections` (Check Your Stats)

Use this command to see how well you are doing in the current room.

#### How to Use It

- Type `!connections` (or `!connections stats`).

#### Examples of Allowed Usage

```text
!connections
!conn
!connections stats
```

#### What You Will See in Chat

```text
🔗 @Alice's Connections stats: Total: 14 | Streak: 5 | Record: 8
```

#### What Happens If You Haven't Joined Yet

```text
@Alice, you are not opted into Connections. Type !connections optin to join!
```

---

### Command 3: `!connections @username` (Look Up Another DJ)

Use this command to check the scores and streaks of any other DJ in the room.

#### How to Use It

- Type `!connections @username` or `!connections stats @username` (replace `@username` with their name).

#### Examples of Allowed Usage

```text
!connections @Bob
!connections stats @Bob
!conn @Charlie
```

#### What You Will See in Chat

```text
🔗 @Bob's Connections stats: Total: 27 | Streak: 12 | Record: 12
```

#### What Does NOT Work / Errors to Avoid

- **Looking up a player who hasn't opted in**:
  - `!connections @Dave`
  - _Result_: `@Dave is not opted into Connections in this room.`
- **Looking up a user who doesn't exist**:
  - `!connections @GhostUser`
  - _Result_: `Could not find user '@GhostUser'.`

---

### Command 4: `!connections optout` (Leave the Game)

Use this command if you no longer wish to participate in Connections.

> [!WARNING]
> **Data Reset**: Opting out will permanently delete your streak, record, and total score in this room. If you decide to join again later, you will start fresh from 0.

#### How to Use It

- Type `!connections optout` (or `!connections leave`).

#### Examples of Allowed Usage

```text
!connections optout
!connections leave
!conn optout
```

#### What You Will See in Chat

- **When opted out successfully**:
  ```text
  @Alice, you have opted out of Connections. Your connection history and streaks in this room have been purged.
  ```
- **If you were not opted in**:
  ```text
  @Alice, you are not currently participating in Connections.
  ```

---

## 7. Frequently Asked Questions (FAQ)

### Do I have to play the exact same song to connect?

No! You only need to share a sequence of 4 or more letters in the song titles. For example, _"Summer Nights"_ connects with _"Night Moves"_ through the word **"night"** (5 letters).

### Can I connect words that are inside larger words?

Yes! As long as a matching 4-letter sequence appears in both cleaned titles, it counts as a connection. For example, _"Rain"_ (4 letters) connects with _"Rainbow"_ or _"Brainstorm"_.

### Does punctuation or capitalization matter?

Not at all. The bot removes all punctuation (like `!`, `?`, `'`, `-`) and converts everything to lowercase before comparing.

### Will my streak break if someone else plays between my songs?

No. Your streak is personal to you. You can take a break, let five other DJs play, and your streak will still be waiting for your next turn.

### What happens if I play two songs in a row?

Playing consecutive songs does not count as a connection and does not break your streak. Your streak stays exactly the same until your next turn after someone else DJs.

### Are my stats saved if the bot goes offline?

Yes. All player stats and room settings are stored safely in a persistent database and will resume right where you left off.

---

Have fun, get creative with your song selections, and see how long of a streak you can build! 🎧🔗🎶
