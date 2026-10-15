# Low Poly Face Skill

A Claude skill that **animates facial expressions and simple gestures** on low-poly characters in your running copy of Blender. Build a character with the [Low Poly Model skill](../lowpolymodel/README.md), then ask Claude to make it smile, blink, talk, nod, or react, and it animates the face (and a little of the body) for you.

## What it can do

- **Emotions:** happy, joy, sad, cry, scared, shocked, curious, shy, angry, and sleepy.
- **Idle and talking:** blinking with small glances, a talking mouth, and talking with arm gestures.
- **Gestures:** nod yes, shake no, plus versions with a smile or a sad face.
- **Expressive details:** brow raises, mouth shapes, growing eyes, droopy lids, blush, sweat drops, tears, shock lines, sparkles, and floating **?** marks.
- **Easy tweaks in plain words:** "bigger smile", "slower", "no hands", "keep the face at the end", and more.

## What makes it good

- **Safe for your model:** faces never deform. Eyes, brows, lids, and effects are rigid parts on small face bones, and only the tiny mouth uses shape keys.
- **Collision-checked:** every animation is checked frame by frame, so brows don't sink into eyes and arms don't pass through the body.
- **Loopable clips:** every preset starts and ends on a neutral face, so clips loop cleanly or chain into sequences.
- **Save your favorites:** keep a tweaked expression as your own preset, and it stays available in later sessions.
- **Low token cost:** ready-made presets mean most requests take a single short call.

## Requirements

- **Claude Code.** The skill runs Python scripts on your computer to talk to Blender, so it needs Claude Code (in the terminal, desktop app, or an IDE). It won't work in the claude.ai website, which can't reach your computer's Blender.
- **Blender with the MCP for Blender add-on installed, enabled, and running.** Follow the [Blender setup guide](../README.md#guide-connect-claude-to-blender) first.
- **Python 3** on your computer.
- **A character built with the Low Poly Model skill.** This skill animates characters made by `lowpolymodel`, and it uses that skill's helper library. The zip below includes a copy of `lowpolymodel` so both are installed together.

## Download the skill

1. Open [`lowpolyface-skill.zip`](lowpolyface-skill.zip) in this folder on GitHub.
2. Click the **Download raw file** button (the download icon at the top right of the file view).
3. The zip saves to your Downloads folder.

## Add it to your Claude skills

The skill must end up at **`~/.claude/skills/lowpolyface/`** exactly, with `lowpolymodel` next to it at **`~/.claude/skills/lowpolymodel/`**, because the scripts look for themselves there.

### macOS (Terminal)

```bash
# 1. Make sure the skills folder exists
mkdir -p ~/.claude/skills

# 2. Unzip the skill into it (-o replaces an older lowpolymodel copy if you have one)
unzip -o ~/Downloads/lowpolyface-skill.zip -d ~/.claude/skills
```

### Windows (PowerShell)

```powershell
# 1. Make sure the skills folder exists
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills"

# 2. Unzip the skill into it (-Force replaces an older lowpolymodel copy if you have one)
Expand-Archive "$env:USERPROFILE\Downloads\lowpolyface-skill.zip" -DestinationPath "$env:USERPROFILE\.claude\skills" -Force
```

### Check that it worked

You should now have this folder structure:

```
~/.claude/skills/
├── lowpolyface/
│   ├── SKILL.md
│   ├── references/  (channels.md)
│   └── scripts/     (fsend.py, lpf.py)
└── lowpolymodel/
    ├── SKILL.md
    ├── examples/    (bot.py, buddy.py, chibi.py)
    ├── references/  (style-guide.md)
    └── scripts/     (bsend.py, lpm.py)
```

Make sure each `SKILL.md` is directly inside its skill folder, not inside an extra nested folder like `lowpolyface/lowpolyface/`.

Then restart Claude Code.

## How to use it

1. Open Blender and make sure the **MCP for Blender** server is running (press **N** → **MCP for Blender** tab → **Connect to MCP server**).
2. Have a character in the scene that was built with the Low Poly Model skill (for example with `/modelchar`).
3. Open Claude Code.
4. Ask for an expression, either with the skill command:

   ```
   /lowpolyface make the chibi happy
   ```

   or just in plain words:

   > *"Make my robot look shocked, then nod yes with a smile."*

5. Claude plays the animation, checks it for collisions, renders a preview sheet of a few frames, and tells you the preset, frame range, and what moves.
6. Ask for tweaks until it looks right, then save the `.blend` file yourself, or ask Claude to save it.

### Prompt ideas

- *"Make the chibi blink and talk for a few seconds."*
- *"Shy reaction, but no arm movement."*
- *"Sad with tears, then hold the face at the end."*
- *"Happy, but a bigger smile and a bit slower. Save that as my_happy."*
- *"Play blink, then talk, then nod yes with a smile."*
- *"Remove all the animation from the bot."*

## Troubleshooting

- **"Connection refused" or Claude can't reach Blender:** Blender isn't open, or the MCP for Blender server isn't started. Open Blender and start the server from the sidebar tab.
- **Claude doesn't use the skill:** check that `~/.claude/skills/lowpolyface/SKILL.md` exists, then restart Claude Code.
- **Claude can't find your character:** the character is identified by its collection name in Blender (like `Chibi` or `Bot`). Tell Claude the name, or ask it to list the characters in the scene.
- **The check reports ISSUES:** ask for a subtler version ("less intense"), or turn off arm movement ("no hands").
- **Animation looks off after you rebuilt the model:** ask Claude to re-run the face setup for that character.

---

Back to [Blender skills](../README.md) · Back to the [main README](../../README.md)
