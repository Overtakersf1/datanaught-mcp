# Datanaught MCP server

Connect your own AI to Datanaught, the turn-based race to AGI from the Overtakers channel.
This repository holds the ready-built MCP server for macOS. The game's source is private.

With it, an AI client such as Claude Code or Claude Desktop can:

- **Advise you live.** It reads the game open in the Datanaught Mac app, reviews your Co-pilot's plan,
  simulates alternatives, scouts your rivals, and puts advice on your screen in a "Your AI" card.
  You decide what to play. It never moves for you, and it can't see the dice or anything a rival hides.
- **Play its own game** headless against the built-in rivals.

## Install

You need the Datanaught **Mac app** (the live link works through the Mac app's files) and macOS 15 or later.
In Terminal:

```sh
mkdir -p ~/.datanaught && curl -fsSL https://github.com/Overtakersf1/datanaught-mcp/releases/latest/download/datanaught-sim-macos.tar.gz | tar xz -C ~/.datanaught
```

Check it with `~/.datanaught/datanaught-sim --version`. Its version should match the app's
(Datanaught ⋯ menu → Connect your AI). To update, run the same command again.

If you download the file with a browser instead, macOS will refuse to open it because it isn't notarized.
Clear the download flag with `xattr -d com.apple.quarantine ~/.datanaught/datanaught-sim`.

## Connect Claude Code

```sh
claude mcp add --scope user datanaught -- ~/.datanaught/datanaught-sim mcp
```

## Connect Claude Desktop

Settings → Developer → Edit Config, add this (use your own user name in the path), then restart Claude Desktop:

```json
{
  "mcpServers": {
    "datanaught": {
      "command": "/Users/YOU/.datanaught/datanaught-sim",
      "args": ["mcp"]
    }
  }
}
```

The app's Connect your AI screen shows this with your path filled in, ready to copy.

## Try it

Open a game in the Datanaught Mac app, then ask your AI:

> Check live_status, look over my Co-pilot's plan, test a couple of alternatives, and send me advice.

Other ideas: *"Scout my rivals."* *"Review my last turn."* *"Compare all four postures over 20 trials."*
Or, without the app: *"Start a Datanaught game against the Monopolist and play my first five turns, explaining each move."*

## Tools

| Tool | What it does |
|---|---|
| `live_status` | Find the game open in the app: round, whose turn, your advice and what the player did with it |
| `live_state`, `live_copilot`, `live_actions` | The board, the Co-pilot's plan as the player sees it, and every legal move |
| `live_rivals`, `live_review_turn`, `live_game_analysis` | Rival scouting, a review of the last turn, and the game so far |
| `live_whatif` | Simulate plans to the end of the game against the rivals, on fresh dice |
| `send_advice`, `clear_advice` | Put advice on the player's screen, or take it down |
| `new_game`, `get_state`, `get_forecast`, `list_actions`, `take_action`, `end_turn`, `get_rules`, `save_game` | Play a headless game |

## Troubleshooting

- **"No live game found":** open a game in the Mac app and check that ⋯ → Live link for your AI is on.
- **The AI can't start the server:** run `~/.datanaught/datanaught-sim --version` in Terminal to see the error.
- **Versions differ:** update the server with the install command, and the app from TestFlight.
