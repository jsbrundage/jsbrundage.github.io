# jsbrundage.com/play — Site Structure
## How to Deploy the AI D&D System on Your Website

---

## File-to-URL Mapping

| File | URL Path | Purpose |
|------|----------|---------|
| `play_main_hub.md` | `/play` | Main entry point. DM prompt + world brief + adventure hooks + efficiency coaching. |
| `play_create_character_guide.md` | `/play/create` | Guided character creation with clickable menus and dice rolling. |
| `play_solo.md` | `/play/solo` | Solo play companion — NPC companions, difficulty calibration, tone examples, session resumption. |
| `play_help.md` | `/play/help` | DM troubleshooting — coaching stuck players, class-specific prompts, situation handling. |
| `play_dm_rules.md` | `/play/dm-rules` | Compact 5e DM rules reference — conditions, actions, rests, vision, environmental hazards. |
| `play_encounters.md` | `/play/encounters` | Random encounter tables by region with narrative hooks that create story threads. |
| `play_npcs.md` | `/play/npcs` | NPC reference — fetched on demand during sessions. |
| `play_world.md` | `/play/world` | Expanded town details and sensory palettes — fetched on demand. |
| `play_lighthouses.md` | `/play/lighthouses` | Lighthouse network lore — fetched when the adventure involves the network. |

---

## How It Works

### The User Flow

```
FIRST VISIT:
  Reader finds jsbrundage.com/play
  → Reads instructions on page
  → Opens Claude.ai
  → Tells Claude: "Read jsbrundage.com/play/create and help me make a character"
  → Claude fetches the page, walks them through character creation
  → Player downloads their character sheet
  → Done. Character saved.

FIRST SESSION:
  Player starts new Claude chat
  → Tells Claude: "Read jsbrundage.com/play and run a D&D session"
  → Uploads their character sheet
  → Claude fetches the page, reads the DM prompt + world brief
  → Claude launches an adventure hook
  → They play
  → At a natural stopping point, Claude generates a session handoff
  → Player downloads the handoff
  → Done. Progress saved.

CONTINUING:
  Player starts new Claude chat
  → Same fetch command
  → Uploads character sheet + session handoff
  → Claude picks up where they left off
  → Repeat
```

### What Claude Fetches and When

**Always fetched (by user request):**
- `/play` — Every session start
- `/play/create` — Character creation only

**Recommended first session (by Claude):**
- `/play/solo` — Solo play rules, companion guidelines, tone calibration
- `/play/dm-rules` — Mechanical reference for accurate rulings
- `/play/help` — Coaching techniques for new players (fetch if player shows signs of confusion)

**Fetched on demand (by Claude during play):**
- `/play/npcs` — When Claude needs named NPCs for a town
- `/play/world` — When Claude needs detailed location descriptions
- `/play/lighthouses` — When the adventure involves the lighthouse network
- `/play/encounters` — When Claude needs a random encounter on the road or coast
- `/play/dm-rules` — When Claude needs to resolve a specific rules question

Claude is instructed in the main `/play` prompt to fetch these resources only when needed, keeping context lean.

---

## Page Format

Each file is written in **Markdown** with dual-audience structure:

1. **Human-readable header** — Instructions for visitors (how to use the page, what they need)
2. **Claude-readable body** — Structured content optimized for AI parsing (clear headers, explicit instructions, embedded mechanical data)

When rendered as a web page, the human instructions appear first and look natural. When Claude fetches the page via `web_fetch`, it reads the full markdown and uses the structured content to run sessions.

### Hosting Options

**Option A: Render as web pages**
Use your existing site's CMS to render the markdown as HTML pages. Add basic styling so they look good for human visitors. Claude reads the underlying content regardless of styling.

**Option B: Serve as raw markdown**
Host the `.md` files directly (e.g., via GitHub Pages, a static file server, or a CMS that serves raw files). Claude reads markdown natively. Simpler to maintain.

**Option C: Hybrid**
Render pretty HTML pages for human visitors at `/play`, but also serve the raw markdown at `/play/index.md` (or similar). Link the pretty page to the raw version with a note: "Copy this link for Claude: jsbrundage.com/play/raw"

Recommendation: **Option A or B.** Claude handles both HTML and markdown well. The key is that the content is accessible at the URL without requiring login or JavaScript rendering.

---

## Token Budget

| Page | Approximate Size | Context Impact |
|------|-----------------|---------------|
| `/play` (hub) | ~8,000 tokens | Always loaded — this is the session foundation |
| `/play/create` | ~7,000 tokens | Only during character creation (separate chat) |
| `/play/solo` | ~5,500 tokens | Recommended on first session — solo play rules + tone examples |
| `/play/help` | ~5,000 tokens | Loaded on demand when player seems stuck or confused |
| `/play/dm-rules` | ~4,500 tokens | Loaded on demand for rules lookups |
| `/play/encounters` | ~6,000 tokens | Loaded on demand for random encounters |
| `/play/npcs` | ~5,000 tokens | Loaded on demand during play |
| `/play/world` | ~4,500 tokens | Loaded on demand during play |
| `/play/lighthouses` | ~3,500 tokens | Loaded on demand during play |

A typical session loads `/play` (~8K tokens) plus 1-2 on-demand resources (~4-6K tokens each). Total prompt overhead: ~12-20K tokens, leaving the vast majority of Claude's context window for actual gameplay.

Compare this to loading all project files (~2.4MB / ~600K tokens) — this system is dramatically more efficient.

---

## Content Licensing

The mechanical data (ability scores, class features, race traits, monster stats) in these documents comes from the **D&D 5e Systems Reference Document (SRD 5.1)**, released by Wizards of the Coast under the **Creative Commons Attribution 4.0 International License (CC-BY-4.0)**.

Each page includes an attribution line:
> *Uses D&D 5e rules under the Creative Commons SRD 5.1 (CC-BY-4.0)*

The setting content (Thornwick Protectorate, NPCs, locations, storylines) is your original creative work.

---

## Future Expansion

When you're ready to add more content, the architecture supports it cleanly:

| Future Page | URL | What It Would Contain |
|------------|-----|----------------------|
| `/play/adventures` | Full adventure modules beyond the starter hooks (multi-session arcs) |
| `/play/levelup` | Detailed level-up guides for each class (levels 2-5+) |
| `/play/party` | Rules for multi-player sessions (when ready) |
| `/play/primer` | The full Master Greytome Primer (newsletter bonus, also fetchable for deep lore) |
| `/play/items` | Magic items and treasure appropriate by level for the Thornwick setting |
| `/play/factions` | Deep dive on factions, allegiances, and political dynamics |

Each new page follows the same pattern: human-readable header + Claude-optimized body. The main `/play` page references new resources with fetch URLs so Claude knows they exist.

---

## Maintenance

When you update your campaign world (new towns, new NPCs, resolved storylines), update the relevant page:

- **New NPC?** → Add to `/play/npcs`
- **New location?** → Add to `/play/world`
- **New adventure hook?** → Add to `/play` (main hub)
- **Lighthouse arc progress?** → Update `/play/lighthouses`

Because Claude fetches these pages fresh each session, updates are immediately available to all players. No app updates, no downloads, no versioning headaches.

---

## Testing

Before publishing, test each page:

1. Open Claude.ai
2. Say: "Please fetch this URL and tell me what you see: [your URL]"
3. Verify Claude reads the content correctly
4. Run through a character creation to confirm the flow works
5. Start a test session to confirm adventure hooks launch properly
6. Verify on-demand fetches work ("fetch jsbrundage.com/play/npcs and tell me about Cordelia")

If Claude can't fetch the URL, check:
- Is the page publicly accessible (no login required)?
- Is the content rendered as text/HTML/markdown (not behind JavaScript that requires client-side rendering)?
- Does the URL return content directly (not a redirect chain)?

---

*Deployment guide for the jsbrundage.com/play AI D&D system.*
