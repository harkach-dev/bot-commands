# DJ Battle User & Player Guide

Welcome to the **DJ Battle** guide! This document explains how DJ battles work in WavezBOT and how to use all the commands available to Resident DJs and room moderators (`minRole: "resident_dj"` or higher).

---

## 1. What is a DJ Battle?

A **DJ Battle** is a fun, friendly competition in the music room where two or more DJs go head-to-head. As each DJ plays their songs, the bot automatically tracks listener reactions (**Woots** and **Mehs**) to calculate live scores.

### Key Rules & Features

- **Participant Cap**: A battle can have anywhere from **2 to 10 DJs**.
- **Song Cap (Max Tracks)**: Each battle has a limit on how many songs each DJ can play (usually **3 songs**, but moderators can set it anywhere between **1 and 10 songs**).
- **Automatic Battle Finish**: As soon as every DJ in the battle finishes their allowed number of songs, the battle ends automatically and announces the winner in chat!
- **Score Formula**:
  $$\text{Net Score} = \text{Total Woots} - \text{Total Mehs} + \text{Moderator Score Adjustments}$$
- **Offline Indicator (`📵`)**: If a competing DJ leaves the room or disconnects during a battle, a small phone icon (`📵`) will appear next to their name on the scoreboard so everyone knows they are currently away.
- **Multiple Battles**: DJs can participate in more than one active battle round at the same time. Each song played counts toward all active battles they are enrolled in.

---

## 2. How Winners & Ties Are Decided

When a battle finishes, WavezBOT calculates the final standings using a strict tie-breaking system:

1. **Highest Total Score**: The DJ with the most net points ($\text{Woots} - \text{Mehs} + \text{Adjustments}$) wins **1st Place**.
2. **First Tie-Breaker (Average Song Score)**: If total scores are tied, the DJ with the higher average score per song (from pure Woots minus Mehs) wins.
3. **Second Tie-Breaker (Fewest Mehs)**: If still tied, the DJ with the fewest total Mehs wins.
4. **Shared Rank (Tie)**: If all tie-breakers are identical, the DJs share the rank (for example, tied for 1st Place).

---

## 3. Summary Cheat Sheet for Resident DJs

| Goal                          | Command Example           | Requirements / Notes                     |
| :---------------------------- | :------------------------ | :--------------------------------------- |
| **Check running battles**     | `!djbattles status`       | Shows live scores & remaining songs.     |
| **Check a specific battle**   | `!djbattles status 1`     | Replace `1` with the battle number.      |
| **List all active battles**   | `!djbattles list`         | Shows all active battles in room.        |
| **View room winner history**  | `!djbattles history`      | Lists past winners & runner-up scores.   |
| **Look up a DJ's stats**      | `!djbattles stats @Alice` | **Must include `@`** before the DJ name. |
| **Look up past room summary** | `!djbattles stats past`   | Summary of past completed battles.       |

---

## 4. Public Commands (`!djbattles`)

Resident DJs and higher role holders can use the `!djbattles` command to check live scoreboards, see running battles, view room history, and look up DJ statistics.

> [!NOTE]
> **Role Requirement**: Requires `resident_dj` role or above.
> **Command Prefix**: Type `!djbattles` followed by a sub-command name like `status`, `list`, `history`, or `stats`.

---

### Command 1: `!djbattles status` (Check Live Scoreboards)

Use this command to view the live standings, current scores, and remaining songs for running battles.

#### How to Use It

- **View all running battles**: Type `!djbattles status`
- **View a specific battle by ID**: Type `!djbattles status 1` (replace `1` with the battle number)

#### Examples of Allowed Usage

```text
!djbattles status
!djbattles status 1
!djbattles status 12
```

#### What You Will See in Chat

```text
📊 Battle #1 Standing (Track Cap: 3 songs/DJ | Notes: Rock Night Showdown): 1. @Alice (24.0 pts | 1/3 songs) ・ 2. @Bob 📵 (18.0 pts | 1/3 songs)
```

_(Notice the `📵` next to `@Bob` showing he is currently offline)._

#### What Does NOT Work / Errors to Avoid

- **Checking a non-existent or finished battle**:
  - `!djbattles status 999`
  - _Result_: `Battle #999 not found or is not active.`
- **Using text instead of a battle number**:
  - `!djbattles status rock`
  - _Result_: `Battle #rock not found or is not active.`

---

### Command 2: `!djbattles list` (List Active Battles)

Use this command to get a quick summary of all battles currently running in the room.

#### How to Use It

- Type `!djbattles list`

#### Examples of Allowed Usage

```text
!djbattles list
```

#### What You Will See in Chat

```text
📋 Active DJ Battles in room:
- Battle #1: @Alice and @Bob (Track Cap: 3 songs/DJ | Notes: Rock Night Showdown)
- Battle #2: @Charlie and @Dave 📵 (Track Cap: 5 songs/DJ)
```

#### What Does NOT Work / Errors to Avoid

- **Adding extra arguments**:
  - `!djbattles list @Alice`
  - _Result_: `!djbattles list` does not take extra parameters; it simply lists all active battles in the current room.

---

### Command 3: `!djbattles history` (View Past Winners)

Use this command to see the winners and scores of completed past battles in the room.

#### How to Use It

- Type `!djbattles history`

#### Examples of Allowed Usage

```text
!djbattles history
```

#### What You Will See in Chat

```text
📜 Completed DJ Battles:
- Battle #1: Winner @Alice (52.0 pts vs @Bob 45.0 pts)
- Battle #2: Co-Winners @Charlie and @Dave 📵 (30.0 pts)
```

---

### Command 4: `!djbattles stats` (View Player & Battle Statistics)

Use this command to look up detailed statistics for yourself, another DJ, past battles, or specific battle IDs.

#### How to Use It

1. **Quick Status Check**: Type `!djbattles stats` (Shows current active battle standings, same as `!djbattles status`).
2. **View Room Past Summary**: Type `!djbattles stats past`
3. **View a Specific DJ's Stats**: Type `!djbattles stats @username` (Must start with `@`).
4. **View a DJ's Stats for Specific Battles**: Type `!djbattles stats @username 1` or `!djbattles stats @username 1 2` (up to 2 battle numbers).
5. **View Specific Battle Numbers**: Type `!djbattles stats 1` or `!djbattles stats 1 2` (up to 2 battle numbers).

#### Examples of Allowed Usage

```text
!djbattles stats
!djbattles stats past
!djbattles stats @Alice
!djbattles stats @Bob 3
!djbattles stats @Charlie 1 2
!djbattles stats 5
```

#### What You Will See in Chat (Player Stats Example)

```text
📈 Battle Stats for @Alice in Battle #1:
Total Score: 48.5 pts (Manual Adjustments: +1.0 pts)
Tracks Played: 3/3 | Total Woots: 50👍 | Total Mehs: 2.5👎
```

#### What Does NOT Work / Errors to Avoid

- **Forgetting the `@` symbol when looking up a DJ**:
  - `!djbattles stats Alice`
  - _Result_: Error! User query handle must start with `@` (e.g. `!djbattles stats @Alice`).
- **Passing more than 2 battle numbers at once**:
  - `!djbattles stats @Alice 1 2 3` or `!djbattles stats 1 2 3`
  - _Result_: Error! You can query a maximum of 2 battle IDs.

---

Enjoy competing and supporting your favorite DJs! 🎧🔥

