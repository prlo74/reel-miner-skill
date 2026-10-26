# reel-miner

A Claude skill that mines a saved Instagram folder of reels and carousels for the tools, links and install steps they mention, then compiles them into a verified document.

Saved posts about developer tools are full of names that flash past on screen and "comment WORD for the link" gates. Reel-miner reads them for you and checks each one against its real source.

## How it works

**Pass 1, capture.** Drives iPhone Mirroring on your Mac through computer use. It opens each saved tile, steps through carousel slides, samples reels every few seconds, expands captions, and appends notes to a scratch file after every post so progress survives a context limit. It is read-only: it never comments, follows, likes, shares or types.

**Pass 2, verify.** Needs no phone. For each tool it searches the web, opens the primary page (repo README or the tool's own site), and records the exact install command. Every entry is marked V (command seen on a page), I (inferred) or U (unverified). Commands are never filled in from memory.

**Pass 3, deliver.** Produces one document: a start-here list, tables by category (plugins and skills, MCP servers, sites and repos, agents and apps, learning resources, workflows), a Gaps section, and a Sources section listing only pages actually opened. Every table has a Status column.

## Design choices

- **Comment-gated posts:** it does not comment or DM. It infers the tool from logos, URLs and README screens, and labels each answer Verified, Inferred or Not identified.
- **Safety:** it asks before taking over the screen, releases control when done, and does not run third-party plugin or hook installers on your machine. It offers an install checklist instead.
- **Failure handling:** covers stream failures, off-Space windows, bridge disconnects, and feeds that jump or stick.

## Requirements

- A Mac linked to your Claude session (`mcp__remote-devices__*` tools) with iPhone Mirroring open on the saved folder grid.
- The computer-use skill and your approval for iPhone Mirroring access and a short on-screen takeover.
- Web search and fetch for the verification pass.

## Usage

"Pull the tools out of my saved Instagram 'Code' folder and give me a verified list."

The skill was developed against a 16-tile folder about Claude Code. Window coordinates in the skill are examples from that run and must be recalibrated from a fresh screenshot each time.

## Install

This is a standard skill folder containing one `SKILL.md`.

- **Claude Code:** copy this folder to `~/.claude/skills/reel-miner/`.
- **Claude apps:** zip the folder and upload it as a custom skill in your Claude settings.

## Files

- `SKILL.md`: the skill definition and instructions.
