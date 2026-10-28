---
name: "reel-miner"
description: "Use when the user wants tools, links and techniques mined from saved Instagram reels, carousels or DM freebies, deduped across creators into a verified doc and stacked skills."
---

# Reel miner

Turns saved Instagram posts (reels, carousels, and freebies creators already sent by DM) into one verified, de-duplicated playbook: a doc, plus optional skills stacked by domain. Core rule: never one skill per reel. Pool everything, compare, keep the best version, then stack.

Proven on a 16-tile "Code" folder via iPhone Mirroring, and on single reels via Chrome seek-sampling.

## Choose the capture route
1. **Chrome (preferred).** If Claude in Chrome tools are present and Instagram web is open and signed in, use them. No screen takeover, and captions come through as clean text.
2. **iPhone Mirroring (fallback).** Use only when no browser tools exist or the content is app-only. Needs the Mac linked (mcp__remote-devices__ tools). If missing, tell the user how to link the chat and stop.
If neither route is available, say so and stop.

## Scope the sources (ask once if unclear)
- **Saved collection:** instagram.com/<username>/saved/ then the collection by name. If the user did not name one, ask which.
- **Single post open in the tab:** mine it first, then ask whether to continue to the whole collection.
- **DM freebies:** files and links creators already sent (often after a comment-gate). Ask which threads to read and read only those. Never open other conversations.
- Reading only throughout: never comment, like, follow, share, DM or type anything on Instagram.

## Pass 1A: capture via Chrome
1. **Post text:** get_page_text on the post gives creator, caption and comments. Captions often hold the links; comments rarely matter.
2. **Collection grid:** use read_page/find to list post links (/p/ or /reel/), scrolling to load more. Write the full list to the notes file before opening any post.
3. **Reels: seek, don't watch.** Real-time sampling misses most burned-in captions. Pause and seek with javascript_tool:
   - Setup: `window.__v=[...document.querySelectorAll('video')].find(x=>x.getBoundingClientRect().width>0); __v.pause(); [__v.duration, JSON.stringify(__v.getBoundingClientRect())]`
   - Per frame: `__v.currentTime=T`, wait 0.5 s, zoom the video rect. Batch 8-16 frames per browser_batch.
   - Step 1 s for coverage; step 0.5 s around lists, tool names or anything that matters (caption phrases change every 0.5-1 s).
   - Zoom at scale 0.3. If a frame returns "too large", re-zoom only the graphic and caption band (top ~55% of the rect) or drop to 0.25. Frames with a face are the ones that overflow.
   - Burned-in captions are the transcript; audio is not available. Record captions in order with timestamps.
4. **Carousels:** advance with the next-slide control or read slide alt text via read_page; zoom each slide.
5. Open each post in the same tab and return to the grid afterwards.

## Pass 1B: capture via iPhone Mirroring (fallback)
- Load the computer-use skill first. Call computer_request_access for iPhone Mirroring (com.apple.ScreenContinuity). The real phone window is the large one; a small "Welcome" window may also exist.
- Ask before taking over the screen. Background computer_app_* tools can scroll but cannot click or drag the phone canvas, so a short takeover (computer_request_full_control) is needed. If declined, say what you can't do. Release with computer_release_full_control and computer_release_lock at the end.
- Take one screenshot (scale 0.6) to find the phone window; recalibrate whenever it moves. Last run: phone at about x 565-808, y 172-697, back button near (588,221).
- Open a tile with a left click. Display lags 2-3 s: always wait 3 s before zoom. Zoom at scale 0.4-0.9 depending on text.
- Carousels: per slide, 3 x computer_scroll at amount 100 at the phone centre, wait 3 s, zoom. A small scroll only budges the slide. Batch 7 slides per computer_batch.
- Reels: zoom every 2-3 s for the reel's length. If it ends ("Watch more reels / Watch again"), tap Watch again and resample at 1-2 s; item 1 of a list often needs a second pass.
- Captions: tap "more", wait, zoom; if it won't expand, don't loop, move on. Tap Back (top-left chevron) to return; a second Back may be needed if the viewer switched to Reels mode. Screenshot to confirm you are on the grid before clicking tiles.
- Failure handling: "Failed to start stream" or window off the current Space -> computer_app_bring_to_current_space, then one screenshot per call. Bridge disconnected -> retry once; if still failing, say the Mac may be asleep or the desktop app closed, and do web verification meanwhile. Reload tools with ToolSearch after reconnecting. Feed jumps or sticks -> Back to the grid and reopen the tile.

## Notes file (both routes)
Append after every post to a scratchpad markdown file: URL, creator, date, caption links, on-screen text with timestamps, tool names, techniques/claims (one line each), CTA word, anything unreadable. Context can run out mid-job; the notes file is the memory.

## Comment-gated posts
Creators say "comment WORD and I'll send the link". Do not comment or DM. If the user already received that freebie in DMs, use it (see Scope). Otherwise work out the tool from what is on screen (logos, URLs in screen recordings, README pages, co-author tags) and record confidence: Verified, Inferred, or Not identified.

## Pass 2: compare and keep the best
Cluster everything across posts before verifying anything:
- **Same tool from several creators** -> one entry, with "also in: @a, @b".
- **Same technique or claim** (e.g. "hook in the first 2 seconds") -> one entry. Keep the most specific, actionable version (concrete steps, numbers, a working example, a primary source). Name whose version was kept and why; move the rest to a Duplicates list.
- **Conflicting advice** -> keep both, flag the conflict, and note which side has evidence.
- **Pure promo** (course pitches, "comment X" with nothing shown) -> Gaps, not entries.
- Count recurrences. Recurrence signals consensus, not quality.

## Pass 3: verify (no phone needed)
For each kept tool, WebSearch then WebFetch the primary page (repo README or the tool's own site) for the exact install command and requirements. Search results are titles only; open pages. Mark each entry V (command seen on a page; say if a third-party mirror), I (inferred) or U (unverified). Never fill a command from memory. Docs-mirror redirects need a second fetch at the redirect URL. Note the license when it is not plain MIT/Apache.

## Pass 4: deliver
- **Doc** (Docs type): start-here list, then tables per category (plugins/skills, MCP servers, sites and repos, agents/apps, learning resources, workflows/prompts, techniques). Every table has Status and Kept-from columns. Then Duplicates dropped, Conflicts, Gaps, and Sources (only pages actually opened).
- **Stack (offer, don't assume):** group kept techniques by domain (e.g. Hooks, Script, Edit, Captions, Sound for content; an animation stack for web). For each domain with at least 3 verified items, draft one SKILL.md and present it with propose_skills (max 3 per call). One skill per domain, never one per reel. Each skill cites its source posts and uses only V-status commands.
- **Installs:** running /plugin commands is not possible from here, and plugin/hook installers are third-party code run with the user's permissions, so do not execute them. Offer an install checklist file and, with a connected folder, inert shallow clones. If folder access is denied, do not work around it.
- **Final reply:** lead with the main caveat, tag claims [Certain]/[Likely]/[Guessing] if the user asked for that, and one line linking the doc.
