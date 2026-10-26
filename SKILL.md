---
name: "reel-miner"
description: "Use when the user wants the tools, links and install steps extracted from a saved Instagram folder of reels or carousels by driving iPhone Mirroring, then compiled into a verified doc."
---

# Reel miner

Turns a saved Instagram folder (reels + carousels) into a compiled, verified list of tools, sites, skills and install steps. Proven on a 16-tile "Code" folder about Claude Code.

## Preconditions
- The user's Mac is linked (mcp__remote-devices__ tools present) and iPhone Mirroring is open on the folder grid. If tools are missing, tell the user how to link the chat and stop.
- Load the computer-use skill first. Call `computer_request_access` for iPhone Mirroring (`com.apple.ScreenContinuity`). The real phone window is the large one; a small "Welcome" window may also exist.
- Ask before taking over the screen. Background `computer_app_*` tools can scroll but cannot click or drag the phone canvas, so a short on-screen takeover (`computer_request_full_control`) is needed. If the user declines, say what you can't do. Release with `computer_release_full_control` and `computer_release_lock` at the end.

## Pass 1: capture (full-screen control)
1. Take one screenshot (scale 0.6) to find the phone window. Coordinates shift if the window moves, so recalibrate from a fresh screenshot. In the last run the phone sat at about x 565-808, y 172-697 with the back button near (588,221).
2. Open a tile with a left click. The display lags 2-3 s: always `wait` 3 s before `zoom`. Zoom the phone region, scale 0.4-0.9 depending on how much text matters.
3. **Carousels:** per slide, scroll right with 3 x `computer_scroll` at amount 100 at the phone centre, wait 3 s, zoom. A small scroll only budges the slide. Batch 7 slides in one `computer_batch`.
4. **Reels:** zoom every 2-3 s for the length of the reel (30-60 s). Tool names flash by, so if the reel ends ("Watch more reels / Watch again"), tap Watch again and resample at 1-2 s. A reel can need two passes to catch item 1 of a list.
5. **Captions:** tap "more" at the end of the caption line, wait, zoom. If it doesn't expand, don't loop; move on. Tap Back (top-left chevron) to return. Replaying a reel can switch the viewer into Reels mode, so a second Back tap may be needed. Take a screenshot to confirm you are on the grid before clicking tiles.
6. Never tap the comment box, Follow, Like or Share, and never type. Reading only.
7. **Save notes after every post** (append to a scratchpad markdown file): creator, on-screen text, tool names, CTA word, anything unreadable. Context can run out mid-job; the notes file is the memory.

## Comment-gated posts
Creators say "comment WORD and I'll send the link". Do not comment or DM. Work out the tool from what is on screen (logos, URLs in screen recordings, README pages, co-author tags) and record how sure you are: Verified, Inferred, or Not identified.

## Failure handling
- "Failed to start stream" / window off the current Space: `computer_app_bring_to_current_space`, then one screenshot per call.
- Bridge disconnected: retry once. If it still fails, tell the user the Mac may be asleep or the desktop app closed. Meanwhile do web verification, which needs no phone. After it reconnects, reload tools with ToolSearch.
- Feed jumps or sticks: Back out to the grid and reopen the tile.

## Pass 2: verify (no phone needed)
For each tool, use WebSearch then WebFetch on the primary page (repo README or the tool's own site) to get the exact install command and requirements. Search results are titles only; open pages. Mark each entry: V (command seen on a page; say if it was a third-party mirror), I (inferred), U (unverified). Never fill a command from memory. Redirects from docs mirrors need a second fetch at the redirect URL.

## Pass 3: deliver
- One doc (Docs type) the user can keep: start-here list, then tables per category (plugins/skills, MCP servers, sites and repos, agents/apps, learning resources, workflows/prompts), a Gaps section and a Sources section listing only pages actually opened. Every table has a Status column.
- If asked to "download" or install: running `/plugin` commands is not possible from here, and plugin/hook installers are third-party code run with the user's permissions, so do not execute them. Offer an install checklist file and, with a connected folder, inert shallow clones. If folder access is denied, do not work around it.
- Final reply: lead with the main caveat, tag claims [Certain]/[Likely]/[Guessing] if the user asked for that, and one line linking the doc.
