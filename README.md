# hk-traditional-culture

A small tile board for exploring Hong Kong traditional culture: festivals, food, opera and village life.

**Started:** 29 Aug 2024


- Tap a tile to hear the term read aloud in Cantonese (uses the browser's speech synthesis, `zh-HK`).
- A short English note appears under the board.
- Related tiles light up briefly, so you can follow one idea to the next (for example Mid-Autumn Festival → mooncakes → lanterns).

Pronunciation quality depends on the Cantonese voice installed on your device or browser.

## Run locally

```bash
pnpm install
pnpm dev
```

Built with SvelteKit and Tailwind CSS.

## Content

The 20 tiles and their notes live in `src/routes/+page.svelte`. Corrections and additions are welcome.


## Progress log

This public repo is a cleaned snapshot. The full development history stays private.

### August 2024

- 29 Aug: First version of the tile board.
- 30 Aug: Added sound.
- 30 Aug: Added a simple recommendation feature and improved it.
- 30 Aug: Revised the wording.

## Licence

MIT. See `LICENSE`.
