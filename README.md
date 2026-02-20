# Freeplane Application Control MCP Server

Control the **Freeplane application** in real-time through Claude! This MCP server uses a Groovy HTTP Bridge to give you full programmatic control over Freeplane's UI, styling, icons, connectors, and more.

## 🎯 What This Does

Unlike the file-based MCP server, this **actually controls the running Freeplane application**:

✅ **Real-time Control** - Changes appear immediately in Freeplane
✅ **Visual Features** - Set colors, icons, styles, connectors
✅ **Navigation** - Select nodes, navigate the tree, center view
✅ **Full Styling** - Fonts, colors, backgrounds, node shapes
✅ **Icons & Connectors** - Add visual elements
✅ **Live Updates** - See changes as they happen in the Freeplane UI

## Architecture

```
Claude Desktop
    ↓ MCP Protocol
Python MCP Server (server.py)
    ↓ HTTP/JSON
Groovy Bridge Server (FreeplaneHttpBridge.groovy)
    ↓ Freeplane API
Freeplane Application (running)
```

## Quick Start

### Prerequisites

- **Freeplane installed** (download from [freeplane.org](https://www.freeplane.org/))
- **Python 3.7+**
- **Claude Desktop** (or any MCP client)

### Installation

**1. Install Python dependencies:**
```bash
cd python
pip install -r requirements.txt
```

**2. Install the Groovy bridge in Freeplane:**

Copy `groovy/FreeplaneHttpBridge.groovy` to your Freeplane scripts directory:

- **Windows**: `%APPDATA%\Freeplane\1.11.x\scripts\`
- **macOS**: `~/Library/Application Support/Freeplane/1.11.x/scripts/`
- **Linux**: `~/.config/Freeplane/1.11.x/scripts/`

**3. Start Freeplane and run the bridge:**

1. Open Freeplane
2. Go to **Tools → Scripts → FreeplaneHttpBridge**
3. You should see: "Freeplane HTTP Bridge Server Started!"

**4. Configure Claude Desktop:**

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "freeplane-app-control": {
      "command": "python",
      "args": ["/path/to/freeplane-app-control-mcp/python/server.py"]
    }
  }
}
```

**5. Restart Claude Desktop**

## Usage Examples

### Check Connection

```
Is Freeplane running?
```

### Navigation & Selection

```
Select the root node and tell me about it

Navigate to the first child node

Go to the parent of the current node
```

### Creating Nodes

```
Create a new child node with text "Important Task"

Add three sibling nodes: "Planning", "Execution", "Review"
```

### Visual Styling

```
Make the current node red and bold

Set the background color of node ID_123 to yellow

Add the "idea" icon to the selected node

Create a connector from node ID_123 to node ID_456 with label "depends on"
```

### Search & Organization

```
Find all nodes containing "project"

Fold all nodes under the current selection

Center the view on the node with text "Main Topic"
```

### Attributes & Notes

```
Add an attribute "priority" with value "high" to the current node

Set a note on this node explaining the requirements

Show me all attributes of the selected node
```

## Available Tools (24 total)

### Connection & Status
- `check_connection` - Verify Freeplane bridge is running
- `get_map_info` - Get current map information

### Node Selection & Navigation
- `get_selected_node` - Get currently selected node
- `select_node` - Select node by ID
- `navigate_to_parent` - Go to parent node
- `navigate_to_child` - Go to child node

### Node Creation & Deletion
- `create_child_node` - Create child under current node
- `create_sibling_node` - Create sibling next to current node
- `set_node_text` - Change node text
- `delete_node` - Delete node and children

### Visual Styling
- `set_node_color` - Set text color
- `set_background_color` - Set background color
- `set_node_style` - Apply predefined style
- `set_font_formatting` - Set bold, italic, size

### Icons
- `add_icon` - Add icon to node
- `remove_icon` - Remove icon from node
- `list_icons` - List all available icons

### Connectors
- `add_connector` - Create visual arrow between nodes
- `remove_connector` - Remove connector

### Attributes & Notes
- `set_node_attribute` - Set custom key-value pair
- `get_node_attributes` - Get all attributes
- `set_node_note` - Set/update note
- `get_node_note` - Get note text

### Map Operations
- `fold_node` - Collapse node
- `unfold_node` - Expand node
- `center_on_node` - Center view on node
- `find_nodes` - Search for nodes

## Workflow Example

**Project Planning with Visual Organization:**

```
1. "Create a new child node 'Q1 Goals'"
2. "Add three child nodes under it: 'Revenue', 'Product', 'Team'"
3. "Set the Revenue node to green background"
4. "Set the Product node to blue background"
5. "Add a 'flag' icon to the Revenue node"
6. "Add a 'lightbulb' icon to the Product node"
7. "Create a connector from Revenue to Product with label 'drives'"
8. "Set an attribute 'status' = 'active' on the Revenue node"
9. "Add a note to the Product node with the feature list"
10. "Fold the Q1 Goals node"
```

All of this happens **live in Freeplane** as you type!

## Technical Details

### HTTP Bridge API

The Groovy bridge runs an HTTP server on `localhost:8765` with these endpoints:

- **GET /status** - Server and map status
- **POST /execute** - Execute commands
- **POST /stop** - Stop the server

### Command Format

```json
{
  "command": "set_node_color",
  "params": {
    "node_id": "ID_123456789",
    "color": "#FF0000"
  }
}
```

### Response Format

```json
{
  "success": true,
  "node": {
    "id": "ID_123456789",
    "text": "My Node",
    "color": "#FF0000"
  }
}
```

## Available Icons

Freeplane includes 100+ icons. Common ones:

- **Priority**: `full-1` through `full-9`
- **Status**: `button_ok`, `button_cancel`, `closed`, `messagebox_warning`
- **Symbols**: `idea`, `lightbulb`, `flag`, `bookmark`
- **Arrows**: `forward`, `back`, `up`, `down`
- **People**: `male1`, `female1`, `group`
- **Objects**: `pencil`, `folder`, `calendar`, `clock`

Use `list_icons` tool to see all available icons.

## Configuration

### Environment Variables

- `FREEPLANE_BRIDGE_HOST` - Bridge host (default: `localhost`)
- `FREEPLANE_BRIDGE_PORT` - Bridge port (default: `8765`)

### Changing the Port

Edit `FreeplaneHttpBridge.groovy` and change:
```groovy
final int PORT = 8765  // Change to your preferred port
```

Then update `python/server.py` environment variable or restart with:
```bash
FREEPLANE_BRIDGE_PORT=9000 python server.py
```

## Troubleshooting

### "Cannot connect to Freeplane bridge"

1. **Is Freeplane running?** Launch Freeplane first
2. **Is the bridge script running?** Go to Tools → Scripts → FreeplaneHttpBridge
3. **Port conflict?** Check if another app is using port 8765
4. **Firewall?** Allow localhost connections on port 8765

### "Node not found" errors

- Node IDs are unique per map
- Get the correct ID using `get_selected_node` or `find_nodes`
- IDs change when you reload the map

### Changes not appearing

- Make sure you're looking at the correct map in Freeplane
- Try clicking on a node to refresh the view
- Check if the bridge script is still running (Tools → Scripts menu)

### Bridge script stops

The bridge runs until you:
- Close Freeplane
- Execute the script again (stops the previous instance)
- Call the `/stop` endpoint

To restart: Tools → Scripts → FreeplaneHttpBridge

## Limitations

- **Single map**: Bridge controls the currently active map only
- **Single instance**: One bridge server per Freeplane instance
- **Local only**: Bridge listens on localhost (secure by default)
- **No undo**: Changes are immediate and use Freeplane's undo
- **Permissions**: Bridge has full access to Freeplane API

## Security Notes

- ✅ Bridge only listens on `localhost` (not accessible from network)
- ✅ No authentication needed (local-only access)
- ⚠️ Any local program can access the bridge
- ⚠️ Bridge has full control over Freeplane

**Best Practice**: Stop the bridge when not using it.

## Comparison: File-Based vs App Control

| Feature | File-Based Server | App Control Server |
|---------|-------------------|-------------------|
| Requires Freeplane | ❌ No | ✅ Yes (running) |
| Real-time updates | ❌ No | ✅ Yes |
| Visual features | ❌ Limited | ✅ Full |
| Icons & connectors | ❌ No | ✅ Yes |
| Styling & colors | ❌ No | ✅ Yes |
| File operations | ✅ Yes | ⚠️ Via Freeplane |
| Standalone | ✅ Yes | ❌ Needs Freeplane |

**Use File-Based** for: Batch processing, automation, working without Freeplane

**Use App Control** for: Interactive work, visual design, live collaboration

## Advanced Usage

### Batch Operations

Create multiple nodes at once:

```python
import requests

bridge = "http://localhost:8765/execute"

# Create a project structure
for phase in ["Planning", "Execution", "Review"]:
    requests.post(bridge, json={
        "command": "create_child",
        "params": {"text": phase}
    })
```

### Custom Scripts

Extend the bridge by adding new commands to `FreeplaneHttpBridge.groovy`:

```groovy
case "my_custom_command":
    return myCustomFunction(params)
```

### Integration with Other Tools

The HTTP bridge can be accessed by any programming language:

```javascript
// Node.js example
const response = await fetch('http://localhost:8765/execute', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
        command: 'create_child',
        params: { text: 'New Node' }
    })
});
```

## Resources

- [Freeplane Official Site](https://www.freeplane.org/)
- [Freeplane Scripting Guide](https://docs.freeplane.org/scripting/Scripting.html)
- [Freeplane API Reference](https://docs.freeplane.org/scripting/Reference.html)
- [Groovy API Tutorial](https://docs.freeplane.org/scripting/api-groovy-tutorial.html)
- [Model Context Protocol](https://modelcontextprotocol.io/)

## Contributing

Ideas for enhancement:
- WebSocket support for push notifications
- Map change events
- Formula execution
- Style templates
- Batch operations API
- Authentication layer

## License

MIT License - see LICENSE file

---

Built with ❤️ for the Freeplane community

Control your mind maps with the power of AI! 🚀
