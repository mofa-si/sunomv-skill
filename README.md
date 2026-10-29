# SunoMV Skill

Agent skills and MCP config for [SunoMV](https://suno.bi?utm_source=github&utm_medium=sunomv-skill): turn a Suno song into a music video from Claude, ChatGPT, Cursor or any MCP client.

**Status: early preview.** The MCP server is live at `https://suno.bi/api/mcp` (OAuth 2.1 + PKCE, or an `sk_mv_` key from [suno.bi/user/integration](https://suno.bi/user/integration)). Right now it exposes one read-only tool, `list_my_mvs`. The `song-to-mv` skill and the paid creation tools ship next.

## Connect

```json
{
  "mcpServers": {
    "sunomv": { "type": "http", "url": "https://suno.bi/api/mcp" }
  }
}
```

Claude Code: `claude mcp add --transport http sunomv https://suno.bi/api/mcp`

## Planned workflow

Works step by step, so you only pay for what you keep:

1. Import a Suno song (link, audio URL or suno.bi song id) and get storyboard stills.
2. Happy with the stills? Animate the scenes you want into video clips.
3. Optional lip sync for singing shots.
4. Export; you get a suno.bi link and an email when the video is ready.

Every paid step quotes the credits first and only charges after you confirm.

## License

MIT
