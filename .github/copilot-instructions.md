# Copilot Instructions for AI Coding Agents

## Project Overview
- This is a Model Context Protocol (MCP) server for controlling MuseScore via a WebSocket-based plugin and Python backend.
- The system enables AI agents to programmatically compose, edit, and analyze music scores in MuseScore.
- Key components:
  - `musescore-mcp-websocket.qml`: MuseScore plugin exposing a WebSocket API.
  - `server.py`: Python entry point, launches the MCP server and registers tool modules.
  - `src/tools/`: Modular Python files implementing MCP tools (navigation, notes, measures, staff, tempo, etc).
  - `src/client/`: WebSocket client logic for communicating with the MuseScore plugin.
  - `src/types/`: TypedDicts for action and parameter schemas.

## Essential Workflows
- **Startup order is critical:**
  1. Start MuseScore and open a score.
  2. Enable and run the MuseScore plugin (see README for plugin path).
  3. Start the Python MCP server (`python server.py`).
- **Development:** Use `mcp dev server.py` for hot-reload and inspection.
- **Testing:** Use the `examples/` directory for sample scores and batch operations.

## Tool/Action Patterns
- All MCP tools are registered in `server.py` via setup functions in `src/tools/`.
- Tool naming and parameters match the QML plugin's WebSocket API (see `musescore-mcp-websocket.qml`).
- Example tool: `add_note(pitch, duration, advance_cursor_after_action)`
- Batch operations: Use `processSequence` to send a list of actions for efficient multi-step edits.
- Duration and pitch are always passed as dicts (see README for reference values).

## Project Conventions
- All new tools should be added as async functions decorated with `@mcp.tool()` in the appropriate `src/tools/` module.
- Tool docstrings should clearly describe arguments and expected behavior.
- Type definitions for actions/params must be updated in `src/types/action_types.py` for new tools.
- Use the `examples/` directory for real-world test cases and regression checks.

## Integration Points
- The Python server and QML plugin communicate via WebSocket (default port 8765).
- The MCP server can be integrated with Claude Desktop or any compatible MCP client (see README for config).
- External dependencies: `fastmcp`, `websockets` (see `requirements.txt`).

## Troubleshooting
- If tools are not available, check plugin is running and server is started in correct order.
- For debugging, run MuseScore from terminal to view plugin logs.
- See README for common error messages and solutions.

## References
- See `README.md` for detailed setup, usage, and troubleshooting.
- See `src/tools/` for tool implementation patterns.
- See `src/types/` for action/parameter schemas.
- See `examples/` for sample scores and batch scripts.
