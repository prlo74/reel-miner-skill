# reel-miner

A Claude skill that mines saved Instagram reels, carousels and DM freebies for the tools, links and techniques they mention. It merges duplicates across creators, verifies every install command against its real source, and compiles one document. It can also stack what it keeps into one skill per domain.

Saved posts are full of names that flash past on screen, "comment WORD for the link" gates, and five creators repeating the same tip. Reel-miner reads them for you, keeps the best version of each idea, and checks it against the source.

Core rule: never one skill per reel. Pool everything, compare, keep the best, then stack.

## How it works

**Pass 1, capture.** Two routes:

- **Chrome (preferred).** Uses Claude in Chrome on Instagram web. Captions are read as text. Reels are paused and stepped through second by second with a small script that sets the video's playback position, and each frame is zoomed. This catches burned-in captions that real-time sampling misses, with no screen takeover.
- **iPhone Mirroring (fallback).** Drives iPhone Mirroring on your Mac through computer use: opens tiles, steps carousel slides, samples reels and expands captions.

Sources are a saved collection, a single open post, or DM threads you name (for freebies creators already sent you). Notes are appended to a scratch file after every post so progress survives a context limit. It is read-only throughout: it never comments, follows, likes, shares, DMs or types.

**Pass 2, compare and keep the best.** Clusters everything across posts. Repeated tools become one entry with "also in" credits. Repeated techniques keep the most specific, actionable version and list the rest as duplicates. Conflicting advice is flagged; pure promo goes to Gaps.

**Pass 3, verify.** Needs no phone. For each kept tool it opens the primary page (repo README or the tool's own site) and records the exact install command and any unusual license. Every entry is marked V (command seen on a page), I (inferred) or U (unverified). Commands are never filled in from memory.

**Pass 4, deliver.** One document: a start-here list, tables by category with Status and Kept-from columns, then Duplicates dropped, Conflicts, Gaps, and Sources (only pages actually opened). Optionally, it drafts one SKILL.md per domain that has at least three verified items.

## Design choices

- **Comment-gated posts:** it does not comment or DM. It uses the freebie if you already received it in DMs; otherwise it infers the tool from logos, URLs and README screens, labelled Verified, Inferred or Not identified.
- **Safety:** it asks before any screen takeover, reads only the DM threads you name, and does not run third-party plugin or hook installers. It offers an install checklist instead.
- **Failure handling:** covers oversized frames, stream failures, off-Space windows, bridge disconnects, and feeds that jump or stick.

## Requirements

- **Chrome route:** Claude in Chrome, signed in to Instagram web.
- **iPhone route:** a Mac linked to your Claude session (`mcp__remote-devices__*` tools) with iPhone Mirroring open, the computer-use skill, and your approval for a short on-screen takeover.
- Web search and fetch for the verification pass.

## Usage

- "Mine my saved 'Code' collection and give me a verified list."
- "Mine this reel." (with the post open in Chrome)
- "Include the freebies in my DMs from @creator."

Window coordinates in the iPhone route are examples from one run and must be recalibrated from a fresh screenshot each time.

## Install

This is a standard skill folder containing one `SKILL.md`.

- **Claude Code:** copy this folder to `~/.claude/skills/reel-miner/`.
- **Claude apps:** zip the folder and upload it as a custom skill in your Claude settings.

## Files

- `SKILL.md`: the skill definition and instructions.
