# SunoMV Skill

Agent skills and MCP config for [SunoMV](https://suno.bi?utm_source=github&utm_medium=sunomv-skill): turn a Suno song into a music video from Claude, ChatGPT, Cursor or any MCP client.

**Status: preview.** The MCP server is at `https://suno.bi/api/mcp` (OAuth 2.1 + PKCE, or an `sk_mv_` key from [suno.bi/user/integration](https://suno.bi/user/integration)). The creation tools below are rolling out with the next suno.bi release; if your client only lists `list_my_mvs`, they are not live yet.

Docs: [suno.bi/mcp](https://suno.bi/mcp?utm_source=github&utm_medium=sunomv-skill) · Skill page: [suno.bi/skills](https://suno.bi/skills?utm_source=github&utm_medium=sunomv-skill)

## Install

Skill only (Claude Code, Codex, Cursor, Gemini CLI, …):

```bash
npx skills add mofa-si/sunomv-skill --skill song-to-mv
```

Claude Code plugin (MCP server + skill in one go), inside Claude Code:

```
/plugin marketplace add mofa-si/sunomv-skill
/plugin install sunomv@sunomv
```

## Connect

```json
{
  "mcpServers": {
    "sunomv": { "type": "http", "url": "https://suno.bi/api/mcp" }
  }
}
```

Claude Code: `claude mcp add --transport http sunomv https://suno.bi/api/mcp`

## Tools

| Tool | What it does | Credits |
|---|---|---|
| `create_mv_project` | Import a Suno link, suno.bi song or audio / video link; align lyrics; write the storyboard | Free for members; one-time song unlock otherwise |
| `get_mv_project` | Progress, every shot and scene with status and URLs, and the next step | Free |
| `generate_stills` | Draw storyboard stills, many shots per call | Per image |
| `shoot_clips` | Turn scenes into video clips | Per second of video |
| `lipsync_clips` | Make the singer's mouth follow the vocals | Per scene |
| `export_mv` | Render the MP4; email with the link when ready | Free for members; one-time song unlock otherwise |
| `list_my_mvs` | Finished videos | Free |

Every paid tool quotes first: call it without `max_credits` to get the price, then call again with `max_credits` after the user agrees.

## Skill

[`skills/song-to-mv`](skills/song-to-mv/SKILL.md) tells the agent to go one step at a time: stills first, video only after you like them, then optional lip sync and export.

## License

MIT
