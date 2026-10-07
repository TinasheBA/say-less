---
name: say-less-relay
description: Use when the user says "send a message", "tell the devs", "let X know", "message the group", "log a bug", "raise a ticket", "put it on the board" or "update the ticket" mid-task, and a message or work item must reach Microsoft Teams or an Azure DevOps board without the user stopping to write it.
---

# Say less: relay

People lose time when they stop their own work to explain a problem in Teams or to log it on a board. This skill does that job for them. It gathers the facts, writes the message, attaches the evidence and delivers it.

## Requirements

- The `humanizer` skill.
- A Playwright MCP server with a browser profile that is already signed in to Teams web.
- The Azure CLI with the `azure-devops` extension, and a PAT in the environment variable `AZURE_DEVOPS_EXT_PAT`. The PAT needs Work Items (Read & Write).
- `relay-config.md` in this skill's folder. Copy `relay-config.example.md` and fill it in. Never commit the filled-in file.

Read `relay-config.md` before Step 1. If it is missing, ask the user for the values it needs and offer to create it.

## Step 1. Ask the four questions in one go

Use one question prompt with all four questions. Pre-fill each option from what the session already knows, so the user can confirm it with one click instead of typing.

1. **What exactly?** The behaviour seen, in one sentence, plus the IDs (work item, record, URL).
2. **For who?** A Teams chat or person, or a board, plus who to assign or tag.
3. **Where is it happening?** The environment and the screen or step.
4. **Way forward?** A proposed fix or next step, or "none, ask them".

If the session cannot pre-fill an answer, leave it open. Never guess the environment or the recipient.

## Step 2. Collect the evidence

- If a test run already produced a screenshot, use it. Look at it before you attach it.
- Otherwise take one with the Playwright MCP on the environment the user named.
- Search the target chat or board first. If someone has already raised the same problem, reply on that thread or comment on that item. Do not create a duplicate.

## Step 3. Write and humanize

1. Write the text from the four answers. Keep facts and guesses apart: write a cause that nobody has checked as a question.
2. Run the `humanizer` skill in embedded mode. For Teams, use the chat voice in `relay-config.md`. For the board, use plain English.
3. Go straight to Step 4. The user's answers to the four questions are the approval, so do not show a draft.

The text must never contain a password, a token, a connection string or a phone number.

## Step 4. Deliver

**Teams chat.** Use Teams web through the Playwright MCP.
1. Open the chat list and filter it by the chat name.
2. Open the chat and type the text in the message box.
3. Use Attach files, then Upload from this device, and pick the screenshot.
4. Send the message, then check that it shows as Sent.

**New Azure DevOps work item.** Run `az boards work-item create` with the organization and project in `relay-config.md`. Apply the board rules from the config: type, area path, parent, assignee and links. Put the steps to reproduce, the expected result and the actual result in the repro or description field. Then attach the screenshot as a comment, the same way as below.

**Comment on an existing item.**
1. Upload each image with `POST {org}/{project}/_apis/wit/attachments?fileName=<name>&api-version=7.1`.
2. Post a comment with `POST {org}/{project}/_apis/wit/workItems/{id}/comments?api-version=7.1-preview` (exactly this version). Put the text in the comment and each image as an `<img src="<attachment url>">` tag.
3. Add a comment only. Never change the state, and never overwrite the description or repro steps.

## Step 5. Report back

Send one line with where it went and the link or message time. Then show the full text that was sent, so the user can correct it later if needed. Then the user goes back to their work.

## Common mistakes

- Asking the four questions one at a time. This interrupts the user four times.
- Stating a guessed cause as fact.
- Posting a screenshot with no IDs, so the developer must come back and ask.
- Creating a new bug when the chat or board already has one for the same problem.
