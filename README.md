Card battler multiplayer browser game template for Net Forge, the open-source multiplayer browser game engine built for AI coding agents.

![INKWRIT, the card battler template, in play](docs/screenshot.png)

This is a template for a card battler that runs in the browser: **INKWRIT**, a complete multiplayer game you can start in one command and change in one file. A one-against-one online card battler: decks shuffled on the server, hidden hands, mana, units and spells, and a gauntlet of AI opponents for a player alone.

## What you get

- 2 players duel each other, or one player runs a gauntlet of four duels against AI opponents of rising strength.
- Decks the server shuffles and keeps secret, so a client never sees the other hand.
- Mana (ink) that rises each turn, units and spells, guards and rush, and a draft of one of three cards after each duel.
- An authoritative room at 30 Hz; card rules are integer state, so a tick costs a few microseconds.
- **Authoritative multiplayer.** A room host owns the game state; every browser tab is a player. Locally, `npm run play` runs that host on your computer.
- **Shared client/server simulation.** The same ECS and Rapier physics code steps the world on the server and predicts your own player in the browser, with delta snapshots.
- **three.js renderer with WebGPU**, and a WebGL2 fallback.
- **Bots and a headless playtest.** `npm run test` plays the game with bots and reports what happened.
- **TypeScript, MIT licensed.** The library models, sprites and sounds the game uses are CC0.
- **Ready for coding agents.** `AGENTS.md`, `CLAUDE.md`, Cursor rules, Copilot instructions and the Net Forge MCP server in `.mcp.json`, `.cursor/mcp.json` and `.vscode/mcp.json`.

Measured with `npm run test` (2 bots, 1 minute of game time, Node 22, Apple M2 Pro):

| Playtest report | Value |
|---|---|
| Ticks simulated at 30 Hz | 1,800 |
| Duel reached in the gauntlet | 4 |
| Earn rules fired (play money) | 2 |
| Server cost per tick (mean) | under 0.005 ms |
| Softlock | none |

Every number above is play money and a local run. Longer runs: `npm run test -- --minutes 4 --bots 4`.

## Start in one command

```sh
npx create-netforge my-game --template cards --yes
cd my-game
npm run play
```

Or press **Use this template** on GitHub, clone your new repository, and run:

```sh
npm install && npm run play
```

`npm run play` builds the game, starts a local room and opens http://127.0.0.1:4663. Open the same link in another tab for a second player. Click a card to play it; click your creature, then a target, to attack. Open the link in a second tab and the two of you duel each other.

You need Node.js 20 or newer. Use `npm run play -- --no-open` to print the link instead, `--port 4700` if the port is taken, and `--host 0.0.0.0` to let phones on your network join. Local play uses play money: no wallet, no chain and no hosted servers.

## Change the rules

The whole game is `src/game.ts`: declarations against the typed API in `@netforge/engine` (the only import it may make). The template package `@netforge/template-cards` provides the renderer, netcode, physics, controls, HUD and bots.

```sh
npm run test        # compile src/game.ts and play it with bots; fails on a broken rule or a softlock
npm run typecheck   # the TypeScript check of src/game.ts
npm run play        # play the change (stop and start it again after each edit)
```

The template guide walks through the usual changes:

- [Add a card](https://forge.netnet.capital/docs/templates/card-battler#1-add-a-card)
- [Tune the table and the AI](https://forge.netnet.capital/docs/templates/card-battler#2-tune-the-table-and-the-ai)
- [Add an earn rule](https://forge.netnet.capital/docs/templates/card-battler#3-add-an-earn-rule)
- [Check it with bots](https://forge.netnet.capital/docs/templates/card-battler#4-check-it-with-bots)

`llms.txt` holds the rules the build enforces and recipes for common changes. Or ask your coding agent in plain words; `AGENTS.md` tells it where the rules live and how to test them.

## Links

- Template guide: https://forge.netnet.capital/docs/templates/card-battler
- Net Forge engine: https://github.com/netnetdev/net-forge
- Docs: https://forge.netnet.capital/docs
- npm: [create-netforge](https://www.npmjs.com/package/create-netforge), [@netforge/template-cards](https://www.npmjs.com/package/@netforge/template-cards), [@netforge/engine](https://www.npmjs.com/package/@netforge/engine)

Pinned to Net Forge 0.2.0. Not affiliated with other projects named NetForge. Local play runs the game's own code on your computer with your permissions; play only games whose code you trust.

## License

MIT ([LICENSE](LICENSE)). The library assets are CC0 1.0.
