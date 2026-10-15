# Blender Skills

This folder contains Claude skills that work with **Blender** through the **MCP for Blender** add-on (formerly called BlenderMCP). They let Claude build and edit 3D models directly in your running copy of Blender, so you can describe what you want and watch it appear in the viewport.

---

## Guide: connect Claude to Blender

This guide sets up the MCP server on your computer so Claude can talk to Blender. You only need to do it once.

### 1. Requirements

| | macOS | Windows |
| --- | --- | --- |
| **Claude** | [Claude Code](https://claude.com/claude-code) or [Claude Desktop](https://claude.ai/download) | [Claude Code](https://claude.com/claude-code) or [Claude Desktop](https://claude.ai/download) |
| **Blender** | [Blender 3.0 or newer](https://www.blender.org/download/) | [Blender 3.0 or newer](https://www.blender.org/download/) |
| **Python** | [Python 3.10 or newer](https://www.python.org/downloads/) | [Python 3.10 or newer](https://www.python.org/downloads/) (tick **"Add Python to PATH"** during install) |
| **uv** (runs the MCP server) | `brew install uv` (needs [Homebrew](https://brew.sh)) | See step 2 below |
| **MCP for Blender add-on** | [github.com/ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp) | [github.com/ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp) |

### 2. Install uv

Install uv with the official installer, not with `pip install uv`.

**macOS** (Terminal):

```bash
brew install uv
```

**Windows** (PowerShell):

```powershell
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Then add uv to your PATH so Claude can find it:

```powershell
$localBin = "$env:USERPROFILE\.local\bin"
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
[Environment]::SetEnvironmentVariable("Path", "$userPath;$localBin", "User")
```

Close and reopen your terminal afterwards. Check that it worked by running `uvx --version`.

### 3. Download the add-on file

1. Go to [github.com/ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp).
2. Open the file **`addon.py`**.
3. Click the **Download raw file** button (the download icon at the top right of the file view) and save it somewhere you'll find it, like your Downloads folder.

### 4. Install the add-on in Blender

1. Open Blender.
2. Go to **Edit → Preferences** (on macOS, you can also use **Blender → Settings**).
3. Click **Add-ons** in the left sidebar.
4. Install the file:
   - **Blender 4.2 or newer:** click the small **dropdown arrow (˅)** at the top right of the Add-ons panel and choose **Install from Disk…**
   - **Blender 4.1 or older:** click the **Install…** button at the top of the Add-ons panel.
5. Find and select the **`addon.py`** file you downloaded, then click **Install from Disk** (or **Install Add-on**).
6. In the add-on list, search for **"MCP for Blender"**.
7. ✅ **Make sure the checkbox next to "Interface: MCP for Blender" is ticked.** The add-on won't run unless it's enabled.
8. If it doesn't show up in the list, restart Blender and search again.

### 5. Connect the MCP server to Claude

Claude starts the MCP server for you whenever it needs it, so you just register it once.

**Claude Code** (Terminal on macOS, PowerShell on Windows):

```bash
claude mcp add blender uvx mcp-for-blender
```

Run `claude mcp list` to check that `blender` shows up.

**Claude Desktop:** go to **Settings → Developer → Edit Config**, add the following, save, and restart Claude Desktop:

```json
{
    "mcpServers": {
        "blender": {
            "command": "uvx",
            "args": ["mcp-for-blender"]
        }
    }
}
```

> Don't also start `uvx mcp-for-blender` yourself in a separate terminal. Claude launches it, and running two copies at once can cause connection problems.

### 6. Start the server inside Blender

1. In Blender's 3D viewport, press **N** to open the sidebar.
2. Open the **MCP for Blender** tab.
3. Click **Connect to MCP server** (older versions label it **Start MCP Server** or **Connect to Claude**). Newer versions start it automatically when Blender opens.

The add-on now listens on `127.0.0.1:9876`, and Claude can talk to Blender. Try asking Claude: *"What objects are in my Blender scene?"*

### Troubleshooting

- **Claude can't connect to Blender:** make sure Blender is open, the add-on's checkbox is ticked, and the server is started in the **MCP for Blender** tab.
- **`spawn uvx ENOENT` error (Claude Desktop):** Claude can't find uv. Use the full path to uvx as the `command` in the config. You can find it with `which uvx` (macOS) or `where uvx` (Windows).
- **Still stuck:** restart Blender first, then Claude.

---

## Skills in this folder

| Skill | Folder | What it does |
| --- | --- | --- |
| Low Poly Model | [`lowpolymodel`](lowpolymodel) | Builds stylized low-poly 3D characters (chibi humans, toy robots, mascot blobs) with clean geometry and simple bone rigs. Run it with `/modelchar`. |

### Installing a skill

1. Finish the guide above so Claude can reach Blender.
2. Download the skill's `.zip` from its folder and unzip it into `~/.claude/skills/`.
3. Restart Claude and ask for what you want, for example: *"make me a low-poly robot with a glowing screen face."*

## Tips

- Keep Blender open the whole time Claude is working. The skills talk to it live.
- Save your `.blend` file yourself when you're happy with the result. The skills don't save it for you unless you ask.

---

Back to the [main README](../README.md).
