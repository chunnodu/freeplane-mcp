# Quick Start Guide - Freeplane App Control

Get the Freeplane Application Control MCP Server running in 5 minutes!

## Step 1: Install Freeplane

If you don't have Freeplane installed:

1. Download from **https://www.freeplane.org/**
2. Install and launch Freeplane
3. Open or create a mind map

## Step 2: Install the Groovy Bridge

**Find your Freeplane scripts directory:**

- **Windows**: `%APPDATA%\Freeplane\1.11.x\scripts\`
- **macOS**: `~/Library/Application Support/Freeplane/1.11.x/scripts/`
- **Linux**: `~/.config/Freeplane/1.11.x/scripts/`

**Copy the bridge script:**

```bash
# From the project directory:
cp groovy/FreeplaneHttpBridge.groovy /path/to/freeplane/scripts/
```

**Or manually:**
1. Open `groovy/FreeplaneHttpBridge.groovy` in a text editor
2. Copy all the contents
3. Create a new file in your Freeplane scripts directory named `FreeplaneHttpBridge.groovy`
4. Paste and save

## Step 3: Start the Bridge

1. **In Freeplane**: Go to **Tools → Scripts → FreeplaneHttpBridge**
2. You should see a dialog: "Freeplane HTTP Bridge Server Started!"
3. The status bar should show: "HTTP Bridge running on port 8765"

**Leave Freeplane open with the bridge running!**

## Step 4: Install Python Dependencies

```bash
cd python
pip install -r requirements.txt
```

## Step 5: Test the Connection

```bash
python server.py
```

In another terminal:

```bash
curl http://localhost:8765/status
```

You should see JSON with Freeplane information!

## Step 6: Configure Claude Desktop

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

Add:

```json
{
  "mcpServers": {
    "freeplane-app": {
      "command": "python",
      "args": ["/absolute/path/to/freeplane-app-control-mcp/python/server.py"]
    }
  }
}
```

Replace `/absolute/path/to/` with the actual path!

## Step 7: Restart Claude Desktop

Close and reopen Claude Desktop.

## Step 8: Try It Out!

In Claude, try:

```
Is Freeplane running?
```

```
Create a child node with text "Hello from Claude!"
```

```
Make the current node bold and red
```

```
Add a lightbulb icon to this node
```

## Troubleshooting

### "Cannot connect to Freeplane bridge"

✅ **Is Freeplane running?** - Launch it
✅ **Is the bridge script running?** - Tools → Scripts → FreeplaneHttpBridge
✅ **Correct port?** - Should be 8765

### Server not appearing in Claude

✅ **Correct path?** - Must be absolute, not relative
✅ **Python in PATH?** - Try `which python` or `where python`
✅ **Restart Claude?** - Close completely and reopen

### Bridge script not in menu

✅ **Correct directory?** - Check scripts folder path
✅ **File name exact?** - Must be `FreeplaneHttpBridge.groovy`
✅ **Restart Freeplane** - Close and reopen to reload scripts

## Common Commands

**Navigation:**
- "Show me the current node"
- "Go to the root node"
- "Navigate to the first child"

**Creation:**
- "Create 3 child nodes: A, B, C"
- "Add a sibling before this node"

**Styling:**
- "Make this red and bold"
- "Set background to yellow"
- "Add the flag icon"

**Search:**
- "Find all nodes with 'important'"
- "Search for project nodes"

## Next Steps

- Read the [full README](README.md) for all features
- Try visual styling with colors and icons
- Create connectors between nodes
- Explore the 24 available tools

## Stopping the Bridge

**To stop the HTTP bridge:**

1. In Freeplane: Tools → Scripts → FreeplaneHttpBridge (runs it again, stopping the previous instance)
2. Or close Freeplane

**Security Note:** The bridge only listens on localhost, but stop it when not in use as a best practice.

---

**Having issues?** Check the [README](README.md) troubleshooting section!
