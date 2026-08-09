# DJ Battle Moderator & Admin Guide

This guide is designed for **Room Moderators, Bouncers, Managers, and Admins** (`minRole: "bouncer"` or higher). It covers how to create, manage, adjust, stop, cancel, and configure **DJ Battles** using the `!djbattle` command.

---

## 1. Moderator Permissions & Golden Rules

As a moderator, you have full control over starting and managing DJ battles in the room. Please keep these important rules in mind:

> [!IMPORTANT]
> **Key Rules for Moderators**:
>
> 1. **Mandatory `@` Handles**: All DJ handles MUST start with `@` (e.g. `@Alice @Bob`).
> 2. **Participant Range**: A battle must have **between 2 and 10 distinct DJs**.
> 3. **Participating in Battles**: Authorized moderators ARE allowed to join battles as competitors and are allowed to stop or cancel battles they compete in (an audit notice will be posted in chat).
> 4. **No Self-Adjusting Scores**: Participating moderators are **strictly forbidden** from manually adjusting scores in any battle where they are competing.
> 5. **Double Quotes for Reasons**: Custom adjustment reasons MUST be enclosed in double quotes (e.g. `"Bonus for epic transition"`).

---

## 2. Quick Moderator Cheat Sheet

| Task                          | Command Example                             | Crucial Rule                                |
| :---------------------------- | :------------------------------------------ | :------------------------------------------ |
| **Start 1v1 Battle**          | `!djbattle start @Alice @Bob`               | Handles **must** start with `@`.            |
| **Start 3-way 5-song battle** | `!djbattle start @Alice @Bob @Charlie 5`    | Cap must be `1` to `10`.                    |
| **Stop a battle**             | `!djbattle stop 1`                          | Tie-breaker hierarchy applied.              |
| **Award bonus points**        | `!djbattle adjust @Alice 1.5 "Awesome set"` | Reason **must** have double quotes `"..."`. |
| **Deduct points**             | `!djbattle adjust @Bob -0.5 "Mic noise"`    | Max change is `-10.0` to `+10.0`.           |
| **Cancel active battle**      | `!djbattle cancel 1`                        | Deletes battle without saving.              |
| **Delete past battle record** | `!djbattle delete 5`                        | Requires numeric ID.                        |
| **Toggle track broadcasts**   | `!djbattle announce on`                     | OFF by default.                             |

---

## 3. Moderator Commands (`!djbattle`)

All management actions use the `!djbattle` command followed by a sub-command name (`start`, `stop`, `adjust`, `cancel`, `delete`, or `announce`).

---

### Command 1: `!djbattle start` (Start a New Battle)

Use this command to initialize a new DJ battle in the room.

#### How to Use It

- **Basic 2-DJ Battle (3 songs default)**:
  `!djbattle start @dj1 @dj2`
- **Multi-DJ Battle with Custom Song Cap**:
  `!djbattle start @dj1 @dj2 @dj3 5` (sets song cap to 5 songs per DJ)
- **Battle with Custom Song Cap and Custom Title/Notes**:
  `!djbattle start @dj1 @dj2 3 Hip-Hop Heavyweights` (sets 3 songs per DJ with notes "Hip-Hop Heavyweights")

#### Parameter Breakdown

1. **DJs (`@dj1 @dj2 ...`)**: List of 2 to 10 DJ handles. **Must begin with `@`**.
2. **Song Cap (`1` to `10`)** _(Optional)_: Single integer between `1` and `10`. Defines how many songs each DJ gets. If omitted, defaults to **3 songs**.
3. **Notes / Title** _(Optional)_: Any text following the DJs and optional song cap will be displayed on scoreboards and announcements.

#### Examples of Allowed Usage

```text
!djbattle start @Alice @Bob
!djbattle start @Alice @Bob @Charlie
!djbattle start @Alice @Bob 5
!djbattle start @Alice @Bob 3 Synthwave Showdown
!djbattle start @Alice @Bob @Charlie 7 Friday Night Finals
```

#### What You Will See in Chat

```text
⚔️ DJ Battle #1 Started! (Track Cap: 3 songs/DJ | Notes: Synthwave Showdown)
Participants: @Alice and @Bob
Play your best songs! Track scores (Woots - Mehs) will be recorded automatically.
```

#### What Does NOT Work / Errors to Avoid

- **Forgetting the `@` symbol on any DJ handle**:
  - `!djbattle start Alice Bob`
  - _Result_: Error! `A DJ battle requires at least 2 participant handles starting with '@' (e.g. !djbattle start @Alice @Bob).`
- **Fewer than 2 DJs or more than 10 DJs**:
  - `!djbattle start @Alice`
  - _Result_: Error! `A DJ battle requires at least 2 participant handles starting with '@' (e.g. !djbattle start @Alice @Bob).`
- **Using invalid song caps**:
  - `!djbattle start @Alice @Bob 15`
  - _Result_: `15` is greater than 10, so the bot treats `"15"` as part of the battle notes string and defaults the song cap to 3.
- **Mentioning an offline/non-existent user handle**:
  - `!djbattle start @Alice @GhostUser`
  - _Result_: Error! `Could not find user: @GhostUser`

---

### Command 2: `!djbattle stop` (Conclude a Battle)

Use this command to manually end an active battle and announce final standings and winners.

#### How to Use It

- **Single active battle running**: Type `!djbattle stop` (automatically stops the running battle).
- **Multiple active battles running**: Type `!djbattle stop 1` (must specify the numeric battle ID).

#### Examples of Allowed Usage

```text
!djbattle stop
!djbattle stop 1
!djbattle stop 14
```

#### What You Will See in Chat

```text
🛑 DJ Battle #1 Stopped! (Audit Notice: Command executed by participant @ModAlice)
🏆 Winner: @Alice
#1 @Alice: 24.0 pts (3/3 songs, avg 8.0, 0 mehs)
#2 @Bob: 18.0 pts (3/3 songs, avg 6.0, 1 mehs)
```

#### What Happens When Stopped?

1. **Zero-Track Cancellation Rule**: If 0 songs were played by all DJs **AND** no manual points were added/subtracted, the bot automatically cancels and cleans up the battle instead of saving an empty result:
   `🚫 DJ Battle #1 Cancelled (No tracks played or score adjustments made). (Audit Notice: Command executed by participant @ModAlice)`
2. **Tie-Breakers Applied**: Computes rankings based on Total Score $\rightarrow$ Avg Song Score $\rightarrow$ Fewest Mehs.
3. **Audit Notice**: If a participating moderator stops their own battle, chat displays: `(Audit Notice: Command executed by participant @ModAlice)`

#### What Does NOT Work / Errors to Avoid

- **Running `!djbattle stop` when multiple battles are active without specifying an ID**:
  - `!djbattle stop` (when Battle #1 and Battle #2 are both running)
  - _Result_: Error! `Multiple active battles found: #1 (@Alice vs @Bob), #2 (@Charlie vs @Dave). Please specify a battle ID.`

---

### Command 3: `!djbattle adjust` (Add or Subtract Points)

Use this command to manually award bonus points or deduct points from a DJ during an active battle.

#### How to Use It

- **Single active battle (or defaulting scope)**:
  `!djbattle adjust @dj <points> "[reason]"`
- **Specific battle ID**:
  `!djbattle adjust @dj <points> <battle_id> "[reason]"`
- **All active battles for that DJ**:
  `!djbattle adjust @dj <points> all "[reason]"`

#### Parameter Breakdown

1. **Target DJ (`@dj`)**: Must start with `@`.
2. **Points Delta (`-10.0` to `+10.0`)**: Points to add (e.g. `1.5`, `2`) or subtract (e.g. `-1.0`, `-0.5`).
3. **Scope (`battle_id` or `all`)** _(Optional if DJ is in 1 battle)_: Specify battle ID or `"all"`. Required if the DJ is competing in multiple active battles.
4. **Reason (`"your text"`)** _(Optional)_: Must be enclosed in **double quotes `"`**. If omitted, defaults to `"Manual adjustment by @ModName"`.

#### Examples of Allowed Usage

```text
!djbattle adjust @Alice 1.5 "Great crowd interaction"
!djbattle adjust @Bob -1.0 "Played overplayed track"
!djbattle adjust @Alice 2.0 1 "1st place round bonus"
!djbattle adjust @Alice 0.5 all "Theme night bonus"
```

#### What You Will See in Chat

```text
✅ Adjusted @Alice's score by +1.5 pts in Battle #1 (Reason: Great crowd interaction). New score: 25.5 pts.
```

#### What Does NOT Work / Errors to Avoid

- **Forgetting double quotes around the reason**:
  - `!djbattle adjust @Alice 1.5 Great track`
  - _Result_: Error! `Adjustment reason must be enclosed in double quotes (e.g. "1st place bonus").`
- **Exceeding point limits**:
  - `!djbattle adjust @Alice 15.0 "Bonus"`
  - _Result_: Error! `Score adjustment delta must be a number between -10.0 and +10.0.`
- **Participating moderator adjusting own battle**:
  - `@ModAlice` tries running `!djbattle adjust @Bob 1.0 1` (where `@ModAlice` is competing in Battle #1).
  - _Result_: Error! `You cannot adjust score in Battle #1 because you are a participant.`

---

### Command 4: `!djbattle cancel` (Cancel & Abort Battle)

Use this command to cancel an active battle without saving any history or scores (cascade delete).

#### How to Use It

- **Single active battle**: Type `!djbattle cancel`
- **Specific battle ID**: Type `!djbattle cancel 1`

#### Examples of Allowed Usage

```text
!djbattle cancel
!djbattle cancel 2
```

#### What You Will See in Chat

```text
🚫 DJ Battle #2 Cancelled! (Audit Notice: Command executed by participant @ModAlice)
```

---

### Command 5: `!djbattle delete` (Permanently Delete Past Battle Data)

Use this command to permanently remove an existing **completed past** battle record from the database.

> [!NOTE]
> `!djbattle delete` cannot be used on **active** battles. To finish or abort an active battle, use `!djbattle stop` or `!djbattle cancel`.

#### How to Use It

- Type `!djbattle delete <battle_id>` (numeric ID is required).

#### Examples of Allowed Usage

```text
!djbattle delete 5
```

#### What You Will See in Chat

```text
🗑️ DJ Battle #5 deleted successfully.
```

#### What Does NOT Work / Errors to Avoid

- **Omitting the battle ID**:
  - `!djbattle delete`
  - _Result_: Error! `Battle # not found.`
- **Attempting to delete an active battle**:
  - `!djbattle delete 1` (when Battle #1 is currently active)
  - _Result_: Error! `Cannot delete active Battle #1. Use '!djbattle stop' to conclude or '!djbattle cancel' to abort active battles.`

---

### Command 6: `!djbattle announce` (Toggle Per-Track Chat Messages)

Use this command to turn per-track live score updates in chat ON or OFF for the room.

> [!NOTE]
> **Default Setting**: Per-track announcements are **OFF by default** to prevent chat spam.
> **Note**: Final victory messages when a battle finishes are ALWAYS broadcast in chat regardless of this setting.

#### How to Use It

- **Check current setting**: `!djbattle announce`
- **Enable per-track messages**: `!djbattle announce on` (or `enable`, `true`, `1`)
- **Disable per-track messages**: `!djbattle announce off` (or `disable`, `false`, `0`)

#### Examples of Allowed Usage

```text
!djbattle announce
!djbattle announce on
!djbattle announce off
```

#### What You Will See in Chat

```text
📢 DJ Battle announcements enabled! Track results will be posted automatically.
```

---

Thank you for hosting DJ Battles and keeping the community fun and fair! 🎧🎉

