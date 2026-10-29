---
name: song-to-mv
description: Turn a song into a lyric music video with SunoMV, one approved step at a time — storyboard stills first, then animated scenes, optional lip sync, then the final MP4 by email. Use when the user wants a music video, MV, lyric video or TikTok / Reels / Shorts video for a Suno song (suno.com link), a suno.bi song, or any audio / video link, or right after generating a song with the Suno MCP. Triggers - "make a music video for this song", "turn my Suno song into an MV", "lyric video", "做个 MV", "把这首歌做成视频".
---

# Song to MV

Uses the SunoMV MCP server (`https://suno.bi/api/mcp`). If its tools are missing, ask the user to connect it:
`claude mcp add --transport http sunomv https://suno.bi/api/mcp` (other clients: add the same URL as an HTTP MCP server). Sign-in is OAuth in the browser.

## The rule: one step, then ask

The user pays per step and judges by eye. After every step, give them the project link (`projectUrl`), say what changed, and ask before the next step. Never chain paid steps on your own.

## Steps

1. **Storyboard** — `create_mv_project(song, style?, aspectRatio?, story?)`
   - `song`: the Suno link (from the user or the Suno MCP), a suno.bi song page / id, or an audio / video link.
   - `aspectRatio`: `9:16` (default, TikTok / Reels / Shorts) or `16:9` (YouTube).
   - `story`: the user's own idea, if they gave one. Otherwise leave it out.
   - `status: "aligning_lyrics"` → wait about 30 seconds and call again with the same song. Calling again never starts over.
2. **Stills** — `generate_stills(projectId)` draws every undrawn shot. Pass `shots` to draw or redraw specific ones.
   Poll `get_mv_project` about once a minute until `progress.stillsDrawing` is 0, then send the user the project link to review.
3. **Video** — only after the user likes the stills: `shoot_clips(projectId, scenes?)`. Scenes are `C1, C2, …` on screen and `0, 1, …` in the tool.
   Without `scenes` it shoots every scene that has no video yet. Scenes marked `placeholder` (lyrics or a still only) are shot only when named.
   It never reshoots a scene that already has video; for changes, send the user to the editor on the project page.
4. **Lip sync (optional)** — `lipsync_clips(projectId, scenes, tier?)` for sung scenes the user wants tighter. `economy` (default) edits the mouth on the existing video; `precise` costs more.
5. **Export** — `export_mv(projectId)` renders the MP4 in the background. The user gets an email with the link in a few minutes; `list_my_mvs` shows it too.

`get_mv_project(projectId)` returns every shot and scene with status and image / video URLs, plus `nextStep`. Read it before naming shots or scenes.

## Credits

Every paid step quotes first:

1. Call the tool **without** `max_credits` → it returns `quote` and charges nothing.
2. Tell the user the price (`quote.totalCredits`, plus anything in `quote.skipped` and why).
3. Only after they agree, call again with `max_credits` = `quote.totalCredits`.

Prices match the buttons on suno.bi. Members pay from their monthly allowance first. If credits run out, send the user to https://suno.bi/shop.

## Pairing with the Suno MCP

Suno's MCP makes the song; SunoMV makes the video. When a Suno tool returns a song link or id, pass it straight to `create_mv_project`.
