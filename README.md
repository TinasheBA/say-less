# say-less

Claude Code skills for when you're in the middle of something and don't want to stop.

You tell the agent what's wrong in a few words. It works out the rest, does the admin, and you keep working.

## Skills

### say-less-relay

Gets a problem to your team without you writing the message or logging the ticket.

Say "send a message", "tell the devs", "log a bug" or "put it on the board" while you're busy. The agent asks four questions in one prompt, pre-filled from what it already knows:

1. What exactly is wrong?
2. Who is it for?
3. Where is it happening?
4. What's the way forward?

It then grabs a screenshot, runs the text through the `humanizer` skill and sends it. That means a Teams message, a new Azure DevOps work item, or a comment on an existing one. You get one line back saying where it went, plus the text that was sent.

You don't get a draft to approve. Your answers to the four questions are the approval.

**Requirements**

- [Claude Code](https://claude.com/claude-code)
- The `humanizer` skill
- A Playwright MCP server whose browser profile is signed in to Teams web
- The Azure CLI with the `azure-devops` extension, and a PAT with Work Items (Read & Write) in `AZURE_DEVOPS_EXT_PAT`

**Install**

1. Copy `skills/say-less-relay` into `~/.claude/skills/`.
2. In that folder, copy `relay-config.example.md` to `relay-config.md`.
3. Fill in your chat voice, Teams chat names, Azure DevOps organization, project and board rules.

`relay-config.md` holds your own team's details. It is in `.gitignore`, so keep it out of any repo.

## License

MIT
