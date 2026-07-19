# Generated Decks

16 PowerPoint files, one per session, generated from the session `slides/outline.md` specs by [`../build_decks.py`](../build_decks.py).

**326 slides · 68 diagrams · 16:9 · speaker notes on every content slide.**

```
decks/
├── 01-what-ai-is.pptx  …  16-agi-and-quantum.pptx
└── mermaid/
    └── NN-slug/slide-NN-N.mmd     # diagram sources, one file per diagram
```

## Regenerating

```bash
cd output/training
python build_decks.py          # requires python-pptx
```

The decks are **build artefacts** — edit the `outline.md` files, not the `.pptx`, then rebuild. Any manual PowerPoint edits are lost on the next run.

## What's in each deck

| Element | Where it comes from |
|---|---|
| Title slide | session README H1 + block name |
| Headline | the `## Slide N — <claim>` heading (a claim, not a label) |
| Bullets | the `On-slide text` field, split on `·`, capped at 6 per the spec |
| Speaker notes | the `Speaker notes` field → Notes pane, never on the slide |
| Footer left | `Session N · AI Training Series` |
| Footer right | the source/licence tag, **only on slides derived from a source** |
| Diagram box | placeholder + the Mermaid source in the notes |

## ⚠️ The one manual step: rendering diagrams

No Mermaid renderer was available in the build environment, so **the 68 diagrams are placeholders**, not images. Each placeholder shows the diagram's node labels so the slide reads sensibly, and the full Mermaid source sits in that slide's speaker notes and in `mermaid/`.

To finish them, pick one:

```bash
# Option A — batch render with the Mermaid CLI
npm install -g @mermaid-js/mermaid-cli
for f in decks/mermaid/*/*.mmd; do mmdc -i "$f" -o "${f%.mmd}.png" -b transparent -w 1600; done
# then drop each PNG onto its placeholder box
```

```
# Option B — one at a time, no install
paste the .mmd contents into https://mermaid.live , export PNG/SVG, place on the slide
```

Render in the deck palette (accent `#2563EB`, slate `#1F2A37`, grey `#6B7280`) and **add alt text** — the placeholders already carry it, so replacing the shape means re-adding it.

## Before presenting

Per [`../powerpoint_instructions.md`](../powerpoint_instructions.md) §5:

- [ ] Diagrams rendered and placed (above).
- [ ] Apply the corporate template/font if there is one — `build_decks.py` uses Segoe UI and a neutral palette by design.
- [ ] Check no LINK-ONLY material got embedded (see each session's `resources/sources.md`).
- [ ] Rehearse for 45 minutes; the decks are specced to that, but sessions 7 and 8 are deliberately thin because the lab carries them.
- [ ] Refresh anything marked *"verify at delivery"* — pricing, model names, OWASP version, EU AI Act dates.
- [ ] Session 15: confirm the candour level with the requester first.
