# Challenge 01: audit of the current phone reader

Notes on the 14 screens in `../after/`, before any redesign.

## How I made these notes

1. I went through all 14 screens myself first. Items marked **[mine]** are what I found on my own.
2. Then I did a second pass with an AI assistant (Claude) and asked it what I had missed.
   Items marked **[AI pass]** came from that. I opened every one against the screenshot,
   kept only the ones I could see and agree with, and moved anything I couldn't confirm
   from an image into "Open questions" instead of listing it as a problem.
3. I dropped one of my own first findings. I had flagged "conversion to paid slipped" as a
   grammar error. It is normal business language, and it is what the speaker said.
   The transcript text is the user's content, not the UI. It is not ours to fix.

**How I measured.** The screenshots are taken at 2x, so 2 image pixels = 1 CSS pixel.
I read sizes from the pixel grid and checked contrast with the WCAG formula. Contrast read
from a screenshot is approximate (text edges are smoothed), so I only call out clear cases.
A screenshot only shows the *visible* size of an icon, not its real tap area.
So for touch targets I say "looks small"; I can't prove the hit area from an image.

---

## Problems that run across many screens

These matter more than any single screen, because fixing one fixes many.

1. **Sentences are cut in half across timestamps.** [mine] Segments break mid-sentence:
   "...several people have a hard" (0:03) / "stop at the top of the hour." (0:07), and
   "...three of the five deals we expected" (0:13) / "in the next quarter closed early." (0:18).
   The timestamps mark audio chunks, not thoughts. That hurts reading, copying and quoting.
2. **The transcript gets half the screen on a small phone.** [AI pass]
   At 320px the screen is 558px tall. The header and tabs take 118px (21%), and the preview bar,
   player and "Audio unavailable" line take 158px (28%). The transcript gets 282px (50%).
   At 390px it gets 67%. With search open, it drops to 52% even at 390px.
3. **Justified text with hyphenation.** [mine] Body text is justified on a narrow column,
   which leaves wide gaps ("Thanks   everyone   for   joining") and splits words ("be-cause",
   "seg-ment"). Desktop (screen 14) uses left-aligned text, which reads better.
4. **The player offers what it can't do.** [AI pass] Every reader screen shows "Audio
   unavailable", but the big green play button looks active, the time says "00:00 / 00:00"
   while the header says "1m 44s", and "Play from here" is offered on text selection.
   The skip buttons are greyed (contrast 1.67:1) but play and speed are not.
5. **An orphan dot in the header.** [AI pass] The meta line reads "1m 44s · EN ·", ending in
   a stray dot. Desktop shows "1m 44s · EN · Sep 12, 2026, 12:46 PM · ≈ $0.01 est.", so the
   phone hides the date and cost but keeps their separator. On screens 12 and 13 the line
   is a lone "·".
6. **Three time formats.** [mine] "0:00" in the gutter, "00:00 / 00:00" in the player,
   "1m 44s" in the header.
7. **Controls shown when they can't be used.** [AI pass] Tabs, search and download stay visible
   on the load-failure (12) and sign-in (13) screens, where there is nothing to search or
   download.
8. **Panels open in different ways.** [AI pass] Search opens a row under the tabs. Download,
   More and Reading settings open bottom sheets. None of the sheets has a title bar with a
   close button, only a drag handle. The checkbox is on the left in Download but on the
   right in Reading settings.
9. **Developer words in user copy.** [mine] "HTTP 503", file extensions as button labels
   (".srt", ".vtt"), and monospace type for buttons and footers.
10. **Phone and desktop say different things.** [AI pass] The same upsell is "Keep reading →"
    on the phone and "Download with email" on desktop. The sign-up subtitle is "Your first
    transcript is free. No credit card." on the phone and "Keep every transcript in your
    library. Packs from $6." on the desktop home page.

---

## Screen by screen

### 01 · Reader at 320px
- **Keep:** clear tab underline; timestamps in a quiet monospace gutter; body text has strong
  contrast (14.8:1).
- **Problems:**
  - Sentence cut across 0:03 / 0:07. [mine]
  - The transcript gets 50% of the screen (see cross-screen #2). [AI pass]
  - The preview bar copy truncates to "You're reading the free preview (first 48%)...." with
    four dots. The useful half, why to sign up, is cut off. [AI pass]
  - Justified text with "be-cause" hyphenation. [mine]
- **Why it matters:** this is the smallest phone, where every pixel counts. Half the screen is
  chrome, and the reader sees about five segments at a time.

### 02 · Reader at 375px
- **Keep:** same structure as 01; tabs and icons fit on one row.
- **Problems:**
  - Same sentence split. [mine]
  - "seg-ment" hyphenation and wide justified gaps. [mine]
  - Visible icon glyphs are small: search ~13px, download ~14x15px, back arrow ~13px,
    "⋯" ~12x2px. They may have larger tap areas; that can't be confirmed from an image. [AI pass]
- **Why it matters:** small, faint icons are hard to find and hit with a thumb.

### 03 · Reader at 390px
- **Keep:** more room; the preview bar shows the full first sentence.
- **Problems:**
  - Sentence cut at 0:13 "...deals we expected" / 0:18 "in the next quarter closed early." [mine]
  - The preview bar still truncates: "Create a free account to read..." [AI pass]
- **Why it matters:** even on a common phone width, the key upsell sentence is never fully visible.

### 04 · Long transcript, scrolled
- **Keep:** the header scrolls away, as the README says, giving the text more room.
- **Problems:**
  - Large empty area below the end note: the text stops about halfway and the rest is blank. [mine]
  - The end note ("End of free preview — 48% read. Sign up free to read, download and keep the
    rest.") is plain text with no button. The real button is in the sticky bar below it,
    which repeats the same message, truncated to "(firs...". [AI pass]
- **Why it matters:** the moment a reader hits the end is the best moment to convert them. Here
  the message is said twice and the action is in neither place where the eye is.

### 05 · Preview boundary
- **Problem:** `05-preview-boundary.png` is byte-for-byte the same file as `04-scrolled.png`
  (same hash). The preview boundary state is not actually shown. [AI pass]
- **Why it matters:** the README describes this state ("nothing hides under it"), but there is no
  screen to check that against. I treat 04 as the boundary screen.

### 06 · Search
- **Keep:** match count "1/1", yellow highlight on the match, a clear close button.
- **Problems:**
  - The match is shown in a separate card at the top, but the transcript below still starts
    at 0:00. Search doesn't scroll the transcript to the match. [mine]
  - The up/down arrows stay active with only one result. [AI pass]
  - Search row + result card push the transcript down to 52% of the screen. [AI pass]
  - Visible glyphs for the arrows (~5x12px) and the close × (~7x7px) are tiny. [AI pass]
- **Why it matters:** search exists to get you to the moment. Showing the line out of context,
  away from the lines around it, only does half the job.

### 07 · Download sheet
- **Keep:** grouping by purpose (Transcript, Translated subtitles, Google Drive, Summary);
  large full-width rows.
- **Problems:**
  - Buttons are labelled only with file extensions (".txt", ".docx", ".srt", ".vtt", ".md").
    A non-technical user doesn't know what .srt or .vtt is. [mine]
  - Only ".txt" has an icon; the other rows don't. [AI pass]
  - "Choose language…" uses a different, larger font from every other control. [AI pass]
  - Helper copy is jargon: "for .txt and .docx; subtitles always carry timing". [AI pass]
  - At least nine options in one sheet, and it runs off the bottom of the screen (".md" is cut off). [AI pass]
  - No title bar with a close button. [AI pass]
- **Why it matters:** most people want one thing (a readable file, or captions). Here they have to
  decode extensions to find it.

### 08 · More menu
- **Keep:** destructive "Delete recording" is red and last.
- **Problems:**
  - It offers "Sign in" next to "Rename" and "Delete recording". A signed-out user is shown
    actions that need an account. [mine]
  - Nine items with no grouping. [AI pass]
  - "Timestamps: shown" duplicates the toggle inside "Reading settings". [AI pass]
  - "Home" is in an overflow menu while the back arrow is on screen. [AI pass]
  - "Copy transcript" is hidden here, but on desktop "Copy" is a visible button. [AI pass]
- **Why it matters:** a menu that mixes navigation, settings, account and destructive actions
  makes every item harder to find, and some of these shouldn't be shown at all.

### 09 · Reading settings
- **Keep:** the footnote "These settings only change this view. Copies and downloads are
  unaffected." is clear, honest copy.
- **Problems:**
  - The README says this sheet holds "Timestamps, speakers, text size", but there is no speakers
    setting on the screen. [AI pass]
  - The footnote is light grey: about 4.3:1 contrast, borderline against the 4.5:1 minimum
    for small text (measured from the screenshot, so approximate). [AI pass]
  - The same "Show timestamps" toggle also lives in the More menu. [AI pass]
- **Why it matters:** two places for one setting means two places to keep in sync, and users don't
  know which one is "real".

### 10 · Text selection
- **Keep:** two clear, useful actions ("Copy selection", "Play from here"); dark popover stands out.
- **Problems:**
  - The popover covers the next line ("0:09 First, the numbers.") and part of its timestamp. [AI pass]
  - "Play from here" is offered while audio is unavailable. [AI pass]
  - Because segments are half-sentences, selecting a "line" copies a fragment
    ("stop at the top of the hour."). [AI pass]
- **Why it matters:** quoting a line is a core phone use. A fragment with no timestamp isn't a quote.

### 11 · Keep reading → sign-up
- **Keep:** Google sign-in first; "No credit card" removes a worry.
- **Problems:**
  - Nothing says what you were doing. The user tapped "Keep reading", but the modal doesn't
    mention the transcript ("Sign up to read the rest of long.m4a"). [AI pass]
  - The password field has no "Show" toggle; the desktop sign-up dialog has one. [AI pass]
  - "Forgot password?" sits on the Sign up tab, where it doesn't apply. [AI pass]
  - "OR WITH EMAIL" contrast is 2.9:1, below the 4.5:1 minimum, in spaced monospace caps. [AI pass]
  - The close × glyph is about 9x9px. [AI pass]
- **Why it matters:** this is the conversion moment. Losing context here makes it feel like a wall,
  not a next step.

### 12 · Load failure
- **Keep:** a single clear "Try again" button.
- **Problems:**
  - The message shows a raw server code: "Couldn't load transcript (HTTP 503)." The failure may be
    on the server, but how it is *shown* is a UI choice. A user doesn't know what 503 means,
    or whether their transcript is lost. [mine, corrected after AI pass]
  - Tabs, search and download stay active with nothing loaded. [AI pass]
  - The header meta line is a lone "·". [AI pass]
  - No reassurance ("your transcript is safe") and no next step besides retry. [AI pass]
- **Why it matters:** an error screen is where trust is won or lost.

### 13 · Not on this account
- **Keep:** a short message with a direct sign-in link.
- **Problems:**
  - The title says "Loading…" but the page has already decided: sign in to view. It is no longer
    loading; the label is stuck. [AI pass; I first thought it was fine]
  - The state is called "not on this account", but the copy only covers "not signed in". What does
    someone signed into a *different* account see? [AI pass]
  - The "Sign in" link is the browser's default blue underline, unlike anything else in the app,
    and is a small tap target. [AI pass]
  - Tabs, search, download and "⋯" stay visible. [AI pass]
- **Why it matters:** a shared link that opens on "Loading…" forever looks broken.

### 14 · Desktop at 1280px
- **Keep:** left-aligned text; "Click a line to play. Drag to select." teaches the interaction;
  visible Copy and Download buttons; the date and cost in the meta line.
- **Problems:**
  - Two "long.m4a" entries in the sidebar, both "QUEUED", while the main pane shows a finished
    transcript. [AI pass]
  - The upsell is a large box above the transcript, so desktop readers see the pitch before the
    content. The phone puts it in a bar at the bottom. [AI pass]
  - A floating "^ • v" control overlaps the bottom-right of the transcript, with no label. [AI pass]
- **Why it matters:** the README says the phone design must not break desktop. Desktop also has
  good ideas the phone lacks (helper line, visible Copy).

---

## Open questions

Things the screens don't show. I'd test them on the live product or ask, not guess.

- Does search also look inside the locked part of a free preview? If it finds a match there,
  what does the user see?
- Can a signed-out preview user open the Download sheet (07)? Screen 04 says you must sign up
  to download, so what happens when they tap a format?
- When you select text on a real phone, does the system's own Copy/Share menu appear on top of
  the app's popover (10)?
- What does someone signed into a *different* account see on screen 13?

---

## The README's eight questions

My answers so far. Lines marked **DECIDE** are design choices I still have to make.

1. **The one thing a phone user came to do, and how many taps.** Read the transcript and find
   one moment in it: 0 taps to start reading, 3 to search (search icon, type, then manually
   scroll to the match, since search doesn't jump there). Copying the whole transcript takes
   2 taps (⋯ → Copy transcript). **DECIDE:** make search jump to the match in place, and move
   Copy out of the menu.
2. **The Keep reading bar and the player at the bottom.** Together they take 158px, 28% of a
   320px screen, and the player can't even play. With the keyboard up, the bar, player and
   keyboard would leave almost no room. **DECIDE:** one compact bottom bar; hide the player
   when audio is unavailable; hide the bar while the keyboard is up.
3. **The nine More items.** Keep: Rename, File details, Delete (signed in only), Quiz.
   Move to screen: Copy. Remove: Home (back arrow covers it), "Timestamps: shown" (already in
   Reading settings), Sign in (belongs in the preview bar, not a menu). **DECIDE** the final list.
4. **Search, download, settings: should they feel like one?** Yes. One sheet style, with a title
   and a close button, opening from the bottom.
5. **The reader while still processing.** It needs: the file name, a plain status ("Transcribing
   — about 2 minutes for this file"), a progress hint, what you can do meanwhile (leave the
   page and come back), and lines appearing as they're ready if the system supports it.
   (Check the API docs: `progress` is "a rough hint, not a promise".)
6. **A meeting with four speakers at 320px.** The current screens show one speaker only. Needs a
   short speaker label or colour per turn, without stealing width from the text.
   Note: the API docs say "Don't rely on a per-line speaker label in the transcript";
   speakers are named in the summary. **DECIDE** how to show speakers honestly with that.
7. **After selecting a line.** Copy the line *with its timestamp*, play from there (only when
   audio works), and select whole sentences, not half-segments.
8. **Should desktop change?** Partly. Keep its layout; align the copy and upsell wording with the
   phone, and fix the duplicate "QUEUED" entries.

---

## My top 5, worst first

1. **Sentences split across timestamps:** it hurts reading, copying and quoting on every screen.
2. **The transcript gets half the screen at 320px:** the main job is squeezed by chrome and a
   player that can't play.
3. **Signed-out users are shown Rename and Delete:** wrong actions for the state.
4. **Search doesn't take you to the match:** it shows the line out of context instead.
5. **Error and access screens speak in system words:** "HTTP 503", and "Loading…" that never ends.
