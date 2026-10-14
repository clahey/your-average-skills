# your-average-skills

Your Average Chris' collection of [Claude Code](https://docs.claude.com/en/docs/claude-code) skills.

## Install

```
/plugin marketplace add clahey/your-average-skills
/plugin install your-average-skills@your-average-skills
```

## What's in here

- [`conversation-cruft`](plugins/your-average-skills/skills/conversation-cruft/SKILL.md) — strips
  conversation narration out of docs and code comments (the "originally we did X, but then
  realized Y" kind of writing) while keeping genuine rationale and decision-history content.

More skills get added here over time.

## Layout

This repo is a [Claude Code plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugins).
`.claude-plugin/marketplace.json` lists the plugin; the plugin itself lives under
`plugins/your-average-skills/` with each skill in its own directory under `skills/`.

## License

MIT, see [LICENSE](LICENSE).
