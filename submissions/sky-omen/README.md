# Sky Omen

**One sentence:** Sky Omen turns every land-defense event into a live $RAREFRIENDS economy — your Friend stakes RF (the game's in-world $RAREFRIENDS unit) into a pari-mutuel pool against a whole cohort of neighboring lands, while a self-sustaining treasury, streak/loss pools, passive structures and a mark-fusing Mutation Cauldron all circulate that same RF back out, modeling what a real ongoing token economy around $RAREFRIENDS could look like rather than a one-off purchase.

- **Category:** Economy Potential — best potential for a token economy paired with $RAREFRIENDS(also Token Activity)
- **Builder:** Alex — [X/Twitter](https://x.com/so_givemeasigh) · [OpenSea](https://opensea.io/givemeasigh) · [sighsighsigh89@gmail.com](mailto:sighsighsigh89@gmail.com)
- **SDK:** FriendSDK v0.1.2
- **Source code:** https://github.com/givemeasigh89/sky-omen
- **Game rules (full detail):** [`games/omen/README.md`](https://github.com/givemeasigh89/sky-omen/blob/main/games/omen/README.md)
- **Playable preview:** https://givemeasigh89.github.io/sky-omen/ (see "Publish a playable preview" below if this link isn't live yet)

## Run it

From a checkout of this repo (a fork of the FriendSDK):

```sh
npm ci
npm run build
npm run dev:game -- games/omen
```

Open the printed URL (normally `http://localhost:4173`), connect a wallet on Robinhood mainnet holding a Rare Friends Generations NFT (generation ≥ 1), select that Friend, and play.

**Wallet/network requirements:** a wallet connected on Robinhood mainnet, holding a Rare Friends Generations NFT (generation ≥ 1). No real RF/$RAREFRIENDS is spent anywhere — this is a simulated preview economy (see "Rules and rewards" below).

## Publish a playable preview

```sh
npm run build
npx friendsdk build games/omen
npx friendsdk check games/omen
```

This writes a static build to `games/omen/.friendsdk/`. To host it as the playable preview link above:

1. On GitHub, go to this repo's **Settings → Pages**.
2. Under "Build and deployment", set **Source** to "Deploy from a branch".
3. You need the contents of `games/omen/.friendsdk/` published at the repo root of a `gh-pages` branch (with a `.nojekyll` file next to them, so GitHub Pages doesn't ignore the SDK's own dot-folders). The simplest way without a terminal: create a new branch named `gh-pages` on GitHub, then use "Add file → Upload files" to upload everything from your local `games/omen/.friendsdk/` folder into it, plus an empty file named `.nojekyll`.
4. Set Pages' branch to `gh-pages` / root, save, and the preview goes live at `https://givemeasigh89.github.io/sky-omen/` within a minute or two.

## Play

Pick one of the 3 offered defenses (Carrots, Radio, Dome, Scarecrow or Mirror — the roster rotates), then tap anywhere clear on the plot to choose where it goes. Your Friend walks there; once arrived, a thought bubble lets you pick a stake (quick chips, a slider, or an exact number) and confirm. A single countdown covers the whole event — "Time to choose" while picking, "Impact in Ns" once staked — and when it resolves, one of the three defenses is drawn as the win, one as neutral, one as the loss, with a pari-mutuel payout, a treasury yield bonus, and a secret per-event jackpot split among everyone who staked, win or lose.

Two permanent buildings stand on the plot: the **🧪 Mutation Cauldron** (fuse 3 of your own marks that share the same outcome into one stronger mark) and the **📖 Mutation Codex** (a tech-tree readout of what each of the 3 fused lineages currently grants). Click either building's own glyph to walk over and open it.

## Rules and rewards

| Rule | Exact value |
| --- | --- |
| Starting balance | 20,000 RF (simulated, resets on reload or restart) |
| Stake range | 100 – 100,000 RF, in steps of 100 |
| Decision window | one countdown (compressed from the design's 24h) per event |
| Win payout | stake back + pari-mutuel share of the losing pool, plus a treasury yield, plus a share of that event's secret jackpot |
| Neutral payout | stake returned in full, plus treasury yield, plus jackpot share |
| Loss payout | 0 (or a resilience-charge refund), plus a jackpot share regardless |
| Structure upkeep | every non-crater mark already on the plot pays a flat RF income every event |
| Mutation Cauldron | fuse 3 marks sharing an outcome into 1 stronger Tier-1 mark; fuse 3 same-lineage Tier-1s into a Tier-2 Grand Mutation |

This is a simulated preview economy — no RF is real, nothing is redeemed against a contract, and reloading the page resets the run. `game.json` ships a valid `ChanceGameDefinition` because the runtime requires one, but this game does not call `buy`/`play`/`settle`/`redeem` against it; its own economy (a variable-stake, cross-participant pool with a treasury, passive structures and mutations) is implemented entirely in `sim.ts`. See [`games/omen/README.md`](https://github.com/givemeasigh89/sky-omen/blob/main/games/omen/README.md) for the full numbers and design reasoning.

## Checks, credits and limitations

- `npx friendsdk check games/omen` — build validity: passes.
- `npx tsc` (project typecheck) — passes.
- Verified live with Playwright against the actual FriendSDK dev server: placement, staking, event resolution, streaks, the World/Log/Settings menus, the Mutation Cauldron (fusing, Tier-2 grand mutations, availability during every game phase) and the Mutation Codex all confirmed working end to end.
- Built entirely with FriendSDK's own `GameWorld` renderer, `GameMenu`/`GameFrame` chrome and Friend identity/sprite system — no custom rendering pipeline. The only hand-drawn assets are this game's own economy marks (a planted carrot, a locator station, a dome, a scarecrow, a mirror, a crater, and the Mutation Cauldron's fused-gem glyphs), drawn to match the SDK's own monochrome hand-drawn style.
- Limitation: the neighboring cohort each event is simulated (26–58 simulated plots), not live multiplayer — called out plainly in the UI itself (the World and Streaks menus), never presented as real other players.
