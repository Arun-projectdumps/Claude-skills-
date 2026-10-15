# Low Poly Model Skill

A Claude skill that builds **stylized low-poly 3D characters** directly in your running copy of Blender. Describe a character in plain words, and Claude models it, rigs it with simple bones, checks it for problems, and shows you a preview.

## What it can make

- **Faceted chibi humans:** big heads, faceted hair, glossy eyes, hoodies, plus extras like glasses, headphones, or a phone in hand.
- **Smooth toy robots:** rounded glossy bodies with glowing screen faces, expressive eyes, and a wobbly antenna.
- **Faceted mascot blobs:** cute, chunky creatures with simple faces.
- **New designs in the same style:** it treats these as starting points, not templates, so you can ask for something completely new.

## What makes it good

- **Clean geometry:** only triangles and quads, closed meshes, and no messy n-gons. Characters stay light (about 800 to 3,000 triangles).
- **Animation-ready rigs:** every moving part sits on its own bone with rotation limits, so arms, heads, and antennas can move without stretching or clipping.
- **Built-in checks:** before calling a character done, the skill tests it for broken geometry, floating parts, and parts that collide at every joint limit.
- **Moody lighting:** one-step warm or night-time lighting and quick preview renders.
- **Low token cost:** a small helper library does the heavy lifting, so Claude writes short scripts instead of long ones.

## Requirements

- **Claude Code.** The skill runs Python scripts on your computer to talk to Blender, so it needs Claude Code (in the terminal, desktop app, or an IDE). It won't work in the claude.ai website, which can't reach your computer's Blender.
- **Blender with the MCP for Blender add-on installed, enabled, and running.** Follow the [Blender setup guide](../README.md#guide-connect-claude-to-blender) first.
- **Python 3** on your computer.

## Download the skill

1. Open [`lowpolymodel-skill.zip`](lowpolymodel-skill.zip) in this folder on GitHub.
2. Click the **Download raw file** button (the download icon at the top right of the file view).
3. The zip saves to your Downloads folder.

## Add it to your Claude skills

The skill must end up at **`~/.claude/skills/lowpolymodel/`** exactly, because its scripts look for themselves there.

### macOS (Terminal)

```bash
# 1. Make sure the skills folder exists
mkdir -p ~/.claude/skills

# 2. Unzip the skill into it
unzip ~/Downloads/lowpolymodel-skill.zip -d ~/.claude/skills

# 3. (Optional) Add the /modelchar shortcut command
mkdir -p ~/.claude/commands
cp ~/.claude/skills/lowpolymodel/commands/modelchar.md ~/.claude/commands/
```

### Windows (PowerShell)

```powershell
# 1. Make sure the skills folder exists
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills"

# 2. Unzip the skill into it
Expand-Archive "$env:USERPROFILE\Downloads\lowpolymodel-skill.zip" -DestinationPath "$env:USERPROFILE\.claude\skills"

# 3. (Optional) Add the /modelchar shortcut command
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\commands"
Copy-Item "$env:USERPROFILE\.claude\skills\lowpolymodel\commands\modelchar.md" "$env:USERPROFILE\.claude\commands\"
```

### Check that it worked

You should now have this folder structure:

```
~/.claude/skills/lowpolymodel/
├── SKILL.md
├── commands/modelchar.md
├── examples/        (bot.py, buddy.py, chibi.py)
├── references/      (style-guide.md)
└── scripts/         (bsend.py, lpm.py)
```

Make sure `SKILL.md` is directly inside `lowpolymodel/`, not inside an extra nested folder like `lowpolymodel/lowpolymodel/`.

Then restart Claude Code.

## How to use it

1. Open Blender and make sure the **MCP for Blender** server is running (press **N** → **MCP for Blender** tab → **Start MCP Server**).
2. Open Claude Code.
3. Ask for a character, either with the shortcut command:

   ```
   /modelchar sleepy girl with round glasses and a yellow jacket
   ```

   or just in plain words:

   > *"Make me a low-poly orange robot with a glowing screen face."*

   If you run `/modelchar` with no description, Claude will ask what to make and suggest the three base styles.

4. Claude builds the character, checks it, renders a preview, and tells you the triangle count and which bones it can move.
5. Save the `.blend` file yourself when you're happy, or ask Claude to save it.

### Prompt ideas

- *"A chibi boy with spiky hair, headphones, and a navy hoodie."*
- *"A happy toy robot with ^ ^ eyes and a blue antenna bulb."*
- *"A round green mascot blob with sleepy eyes."*
- *"Add a looping idle animation to the robot."*

## Troubleshooting

- **"Connection refused" or Claude can't reach Blender:** Blender isn't open, or the MCP for Blender server isn't started. Open Blender and start the server from the sidebar tab.
- **Claude doesn't use the skill:** check that `~/.claude/skills/lowpolymodel/SKILL.md` exists, then restart Claude Code.
- **`/modelchar` isn't recognized:** you skipped step 3 of the install. Copy `modelchar.md` into `~/.claude/commands/`, or just ask in plain words instead.

---

Back to [Blender skills](../README.md) · Back to the [main README](../../README.md)
