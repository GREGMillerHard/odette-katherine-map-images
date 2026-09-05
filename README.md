# HONIORA site — deployment notes

## What's in this package

- `index.html` — the entire site (markup, CSS, and JS all in one file).
- Every image, video, and icon file — hero shots, the hero glass video (`hero-glass.mp4`/`.webm` for desktop, `hero-glass-lite.mp4`/`.webm` for the lite site), the "In The Mix" Stack section banner (`matrix-stack-header.jpg`), nav hex buttons, pillar icons, the nav bar wordmark (`honiora-logo.png`), the Mānuka vista banner, the Rita Rocks feature photos (`rita-ora-feature.jpg` for desktop, `rita-ora-feature-lite.jpg` for the lite site), the HoniBlog founder photo (`blog-founder-happy-place.jpg`), the HoniBlog Article 02 video still (`blog-kiwi-slimland.jpg`), the HoniBlog Article 03 video still (`blog-jenerise-origins.jpg`), the HoniBlog Article 04 video still (`blog-manuka-benefits.jpg`), and all 14 ingredient photos used in the Stack section (including the new `bifidobacterium_adolescentis.jpg`) — sits flat in the same folder as `index.html`, referenced as plain filenames like `hero-glass.jpg` or `cGP-Pro_blackcurrant.jpg`. There is no `ingredients/` subfolder — everything is one level, beside `index.html`.

## How to host it

This is a static site with no build step. Upload the whole folder as one flat directory — `index.html` and every image/video file beside it, no subfolders. If any image lands in a different folder than `index.html`, or in a subfolder, it will fail to load even though the page itself renders fine, since every image path in the HTML is relative to where `index.html` lives.

For GitHub Pages specifically: commit the folder as-is, flat, to the repo (or the branch/folder GitHub Pages is configured to serve from). Do not nest the images into subfolders or rename files — the HTML references exact filenames.

## Version

This package corresponds to HONIORA_site_v247, updated 2026-09-05.

Recent changes in this version:
- 14/The Company: swapped the order of the "01" and "02" boxes so the "Who we are" company spiel now comes first, with the team portrait photo directly after it. Numbers travel with position (Who we are is now 01, the portrait is now 02), 03 (map) and 04 (360 view) unchanged.

Recent changes in this version:
- 11/The Taste heading: "HONIORA SKU2 Organoleptic Profile." &rarr; "HONIORA Organoleptic Profile." (dropped "SKU2").

Recent changes in this version:
- Removed the "About these references" note under the Clinical Thresholds ingredient table.

Recent changes in this version:
- Moved Braincurrant next to cGP-Pro in the ingredient scroller (they're both New Zealand blackcurrant-based, so they now sit as a pair).
- Fixed the real reason the ingredient tile images were slow on the lite site: all 14 photos in that row (creatine, Mānuka crystal, Livaux kiwifruit, Feiolix feijoa, Braincurrant, cGP-Pro, S7, Careflow mango, VasoDrive-AP, Bifidobacterium, Landkind salidroside, yuzu, passionfruit, thaumatin) were the same 820x820 file at every screen size, despite showing at well under 250px wide on a phone. Each now has a lite-specific version sized for that display width, picked automatically the same way as the other lite images on the page. Combined size for all 14 dropped from about 1.97MB to about 290KB on the lite site, roughly an 85% cut.
- Rita Rocks: on the lite site, the pull-quote/byline/"Read the full feature" block now sits directly under her photo, full width, instead of overlapping down the left side of it. Desktop is unchanged (still the text-over-photo treatment).
- Mānuka banner caption reverted to always one line, lite site included: it now shrinks to fit the width (down to a 10px floor if it has to) rather than wrapping to two lines. Confirmed one line down to a 320px-wide phone.
- Found the real cause of the lite site's slow/stalling images: several photos on the page were the full desktop file at every screen size, several times larger than they're ever displayed on a phone. `hero-packshot-cutout.png` (the five-tube pack shot) was 1.9MB for a box that displays around 240px wide on mobile; `manuka-coast.jpg` was 1.1MB for a similar story; the globe map was 772KB in two places, one of which had no lazy-loading at all and was downloading on every visit whether the map was ever opened or not. All four now have a lite-specific version sized and compressed for how big they actually render on a phone (hero-packshot-cutout: 1.9MB &rarr; ~120KB, manuka-coast: 1.1MB &rarr; ~130KB, globe: 772KB &rarr; ~60KB), picked automatically via the same `<source media>` technique used everywhere else on the site, plus the missing lazy-load fixed on the globe's lightbox copy.
- The 09/Stack "In The Mix" banner now has its own lite version too, using the MIX_LITE.jpg you sent (1000x615, 95KB) instead of the full 2678px desktop file.

Recent changes in this version:
- Added the "In The Mix" banner graphic (`matrix-stack-header.jpg`) to the top of the 09 / The Stack section, above the "The HONIORA recipe" heading. Full width up to 1040px, centred, scales down cleanly on the lite site. Used the file from the GitHub link (near-identical to what was pasted in chat, just a different JPEG export of the same design).

Recent changes in this version:
- Store section headline: "FOUNDERS OFFER." &rarr; "HONI FOUNDERS LIMITED OFFER."
- Store section intro line: "hold it for you" &rarr; "reserve it for you", "Sign up to the HONIORA protocol" &rarr; "Sign up for our HONIORA protocol".
- Ritual section intro line: added "delicious" before "and ready to drink" (fixed from the pasted "delicous" to the correct spelling).
- "Fast-dissolving, easy to absorb" section paragraph rewritten per the new copy: now specifies "The 2 HONIORA tablets", "in your water", and ends "so your gut absorbs the actives quickly and they reach your bloodstream."
- Checked "08 / Clinical Thresholds" and its headline against the pasted copy: already word-for-word identical on the live page, nothing to change there.

Recent changes in this version:
- The hero video is back on the lite site, and fast: a new, purpose-made encode, `hero-glass-lite.mp4` / `hero-glass-lite.webm`, downscaled to 400px wide (the size it actually displays at on a phone) and re-compressed at a lower bitrate. 267KB / 388KB versus the desktop file's 2.6MB / 1.2MB, a roughly 85-90% cut. Autoplay is back on for every device: the lite site picks this small file automatically (the same `<source media>` technique already used for the Mānuka banner and Rita Rocks lite images), so it loads and starts playing quickly without competing with everything else on the page.

Recent changes in this version:
- Found why the lite site was slow, and why the tube, the beekeeper banner and Rita Rocks seemed not to load: the hero video (tablets fizzing in the glass) had `autoplay` and `preload="auto"` on every device, so it started pulling down several megabytes the instant the page opened even on a phone, eating the bandwidth everything further down the page was waiting on. It now only preloads and plays on desktop and the 1001-1400px tablet layout; on the lite site it downloads nothing at all and just shows its static poster frame permanently, freeing up the connection for the images that actually need it. Also found and removed a second, older piece of code that was calling `.play()` on this same video unconditionally on every device (an earlier iOS Safari fix) -- it would have silently started downloading the video on the lite site regardless of the new gating above, so it had to go too.

Recent changes in this version:
- Found the real cause of the jump/double-up right where the beekeeper (Mānuka) banner ends and the ingredient scroll section begins: the banner's image was lazy-loaded, and its "finished loading" event was wired to force a ScrollTrigger recalculation. Since that image sits right before the pinned scroll section, it was finishing its lazy load exactly while the visitor was scrolling into that section, and recalculating a pinned ScrollTrigger mid-scrub snaps it to a corrected position, which reads as a jump or a momentary doubled-up frame. The banner image now loads eagerly (no more `loading="lazy"` on it) and no longer triggers a recalculation on load, so it's fully loaded and settled long before anyone scrolls near it. This is the exact same failure mode already avoided for the ingredient tile images themselves (see the code comment next to it), just missed on the banner image at the time.

Recent changes in this version:
- Lite site: the gold keylines (tube to ingredient) no longer deploy at all below the 1000px breakpoint. They still build and play normally on desktop and the 1001-1400px tablet layout, unchanged.
- Checked HONIRITA.jpg (sent for Rita Rocks on the lite site) against what's already in the package: byte-for-byte identical to the deployed `rita-ora-feature-lite.jpg`. Already live, nothing to change.
- Lite site: the Mānuka banner caption ("FROM AOTEAROA : THE LAND OF THE LONG WHITE CLOUD") now breaks onto two centred lines below 1000px instead of being squeezed down to fit one. Desktop and tablet are unaffected, still one line.
- Lite site: tapping one of the two gold hex buttons in the mobile nav bar (Reserve Protocol / Reserve Test Tube) now grows downward into the open header space instead of growing equally up and down from its centre, so the top of the hex no longer gets cropped by the header's own top edge. Also nudged down slightly on top of that for a bit more breathing room from the header.

Recent changes in this version:
- Checked ALIGN.png against the live site: it's a plain reference shot of the tube and ingredient list side by side (no lines drawn in it), and it matches the current layout exactly, tube on its side with the cap end toward the list, dash mark before each ingredient. Since it's a target for alignment rather than a finished asset, the keylines stay live-built rather than switching to a static image. That's still the right call: the JS version is already pixel-matched to this exact layout at every screen size including mobile, and keeps working if the ingredient list ever changes, which a flat picture wouldn't.
- Checked the two Mānuka image links sent this session (150_Manuka_05-1600x1067.jpg for desktop, LITE_LWC.jpg for the lite site) against what's already in this package: both are byte-for-byte identical to the files already deployed (MANUKA_PAN.jpg and MANUKA_PAN_lite.jpg). Nothing to change there, both sizes are already live.

Recent changes in this version:
- Article 03's video still is finally the real one: a new file, `blog-jenerise-womens-health.jpg` (1238x2200), a genuine Shorts frame of Rachael Jennings mid-sentence with the on-screen "Creatine" caption and the Jenerise wordmark, from the new video. Sized the card's aspect-ratio to this image's own exact dimensions so nothing gets cropped by the fill-the-box treatment the other blog cards use. The old placeholder file, `blog-jenerise-origins.jpg`, is no longer referenced anywhere but is left in the folder rather than deleted, in case it's wanted elsewhere.

Recent changes in this version:
- Article 03 byline corrected from "Rachael Jenner" to "Rachael Jennings" in the title, the image alt text, and the aria-label. This matches the surname already used elsewhere on the page for her father, Steve Jennings (the Jenerise Cr.01 founder), in the Cr-01 ingredient section and the AskHoni knowledge base. The still image is still the placeholder noted in v233, unchanged: it's the old Jenerise video's frame, not a still from the new one.

Recent changes in this version:
- HoniBlog Article 03 replaced with the new video: title is now "Rachael Jenner @ Jenerise UK: Creatine — The Lifelong Supplement for Women's Health & Longevity", and the watch link now points to the new short (youtube.com/shorts/gMFTkdwvEbs). NOT done: the video still image. It's still the old frame from the previous Jenerise video, since fetching a new frame means reaching YouTube directly and that's blocked from this environment. Marked clearly with an HTML comment at that spot in the file so it isn't missed. To finish this, send a still image (a screenshot from the new short) or a link to one hosted somewhere reachable (GitHub has worked for every other image this session) and I'll drop it in.

Recent changes in this version:
- Hero keylines, round two. Found and fixed a real alignment bug: the ingredient list carries this page's own scroll-reveal treatment (fades and slides up into place, at a slightly different moment for the headline, the tag line, the list, and the CTAs), so the first geometry measurement was taken before that settled and every line was landing about 26px above its ingredient. Lines now re-measure and correct themselves the moment that settling finishes, confirmed by direct pixel comparison against each ingredient's own tick mark: 0px off on desktop and the lite site, under half a pixel on mobile (rounding, invisible).
- Also found and fixed a second, more serious bug from the first attempt at this correction: a safety timer meant to catch slow-loading fonts was unconditionally re-hiding the lines a couple of seconds after page load, even if the visitor had already scrolled up and was actively looking at them. Rebuilding the geometry (on resize, on that settle, as a backstop) now always reapplies whatever state was actually showing, instead of assuming hidden.
- Belt and suspenders against "not loading" happening a third way: the whole script now runs inside a try/catch that logs to the browser console instead of failing silently, and people with "reduce motion" set on their device now get the lines permanently drawn and visible with no animation, rather than no lines at all, so a system accessibility setting can no longer look identical to a broken feature. If it's still not showing after this, a hard refresh clears a stale cached copy of the page, and the browser console (right-click, Inspect, Console tab) will show a "[hero-keylines]" message if something on that specific device is still going wrong.

Recent changes in this version:
- HoniBlog Article 03 title changed from "Jenerise: Creatine's Origins — Essential for All, Not Just Athletes!" to "Rachael Jenner @ Jenerise: Creatine's Origins — [line break] Essential for All, Not Just Athletes!", matching the line break supplied. Nothing else on that article card (video, link, meta line) changed.

Recent changes in this version:
- Fixed: the gold hero keylines from v229 weren't loading. The likely cause was the GSAP/ScrollTrigger CDN script (loaded from cdnjs.cloudflare.com) failing or being blocked somewhere in the chain, since that's exactly what happened in my own test environment when I re-checked it. Rebuilt the whole feature on plain vanilla JS and CSS instead (IntersectionObserver plus a CSS transition), with zero dependency on that or any other external library, so it can no longer be taken down by a blocked or slow third-party script. Re-verified end to end with GSAP deliberately unavailable, confirming it still works: 13 lines build, draw in on scroll-up, and reset on scroll-down.
- Tightened the alignment: every line now ends at the exact same left edge and vertical offset already used by that ingredient's own small gold tick mark (the existing dash to the left of each ingredient's name), so each line lands precisely on its ingredient rather than an approximation of its position. Re-verified at 390px, 1200px and 1600px with a screenshot at each: every line visibly terminates right at its ingredient's tick, tube to ingredient, correctly at every size including the lite and stacked mobile layouts.

Recent changes in this version:
- New: gold "keylines" in the hero. Thin gold lines now connect the tube's cap end to each of the 13 active ingredients in the list beside it, drawing in one by one as the visitor scrolls back UP through the hero after having scrolled further down the page. They reset to hidden if you scroll back down away from the hero, so the reveal replays each time you scroll up into it again. Built with GSAP ScrollTrigger (already loaded on this page for the "the unit" horizontal-scroll section), so it only turns on if that library loads and the visitor doesn't have "reduce motion" set in their OS, in which case the page just looks like it did before, no broken half-state. Positions are measured from the real, live layout rather than assumed, so this works unmodified on the lite site and the stacked mobile layout too, not just desktop. Checked at 390px, 1200px and 1600px: 13 lines build correctly at each, draw in on scroll-up, and reset on scroll-down.
- Mānuka Honey section: the golden bee mark above the headline is bigger (clamp 160-250px, was 128-200px). Scoped to just this bee, so the separate "1% for the Bees" badge elsewhere on the page, which shares the same CSS class, is untouched at its original size. Headline text changed from "Mānuka honey / Does far more than sweeten." to "Mānuka Honey / A biological powerhouse".

Recent changes in this version:
- The hero title, "HONIORA" and "MĀNUKA CREATINE PROTOCOL" beneath it, is bigger at every screen size. Color and both typefaces (the serif logo, the sans tag line) are unchanged, only size went up, by roughly 15% across desktop, lite, and mobile. The tag line still auto-matches the logo's exact rendered width above 1400px (unaffected, that script just picked up the new size), and I bumped its own separate size rules for the lite tier (1001-1400px) and both mobile tiers so it stays in the same proportion to the logo everywhere. Checked 390px, 820px, 1200px and 1600px: no wrapping, no overflow, no horizontal scrollbar introduced.
- The Mānuka vista banner caption ("FROM AOTEAROA : THE LAND OF THE LONG WHITE CLOUD") now uses the same light-weight sans-serif treatment as the site's other headlines (h1/h2: Arial/Helvetica Neue, weight 300) instead of a bold weight. Only the font-family and font-weight changed; the fill-to-width script, size, color and shadow are untouched.
- On the ingredient scroller, the Braincurrant tile moved from beside Landkind (near the end) to directly after Feiolix. Its description text also changed to match the wording supplied: it now ends "...naturally rich in anthocyanins. Natural support for focus, memory and mood balance." (was "...The supplier cites support for focus, memory and mood balance."). Name, dose, and image are unchanged. Confirmed only one copy exists and the tile order now reads Cr-01, MGO, Livaux, Feiolix, Braincurrant, Eriomin, cGP-Pro, S7, Careflow, VasoDrive-AP, Bifidobacterium adolescentis, Landkind, Yuzu, Passionfruit, Thaumatin.

Recent changes in this version:
- On the ingredient scroller, the Bifidobacterium adolescentis tile moved again, this time to right after VasoDrive-AP (its previous spot was beside Feiolix). Only its position changed, photo/dose/description untouched. Confirmed only one copy exists and the tile order now reads Cr-01, MGO, Livaux, Feiolix, Eriomin, cGP-Pro, S7, Careflow, VasoDrive-AP, Bifidobacterium adolescentis, Landkind, Braincurrant, Yuzu, Passionfruit, Thaumatin.

Recent changes in this version:
- The Mānuka vista banner caption is now "FROM AOTEAROA : THE LAND OF THE LONG WHITE CLOUD" (was "FROM THE LAND OF THE LONG WHITE CLOUD"). The longer wording doesn't fit on one line at a legible size on narrow phones, so the fill script was extended: it still spans the full width as one line down to 640px, and below that, where holding one line would force the text under a 14px floor, it now lets the line wrap to two instead of shrinking further or quietly overflowing past the image edge (which the old script would have done with this longer string). Checked 320px through 1920px: no overflow anywhere, wraps cleanly to two centred lines at 320-430px, one full-width line everywhere above 640px.

Recent changes in this version:
- On the ingredient scroller (the horizontal photo strip under the Stack section), the Bifidobacterium adolescentis tile moved from after S7 (its original spot, added there in an earlier version) to right beside Feiolix, immediately before Eriomin. Only its position changed, its photo, dose, and description are all untouched. Confirmed only one copy exists in the markup and the tile order now reads Cr-01, MGO, Livaux, Feiolix, Bifidobacterium adolescentis, Eriomin, cGP-Pro, S7, Careflow, VasoDrive-AP, Landkind, Braincurrant, Yuzu, Passionfruit, Thaumatin.

Recent changes in this version:
- The Mānuka vista banner photo is replaced: new file `MANUKA_PAN.jpg` (1600x1067), beekeepers tending hives on a clifftop over the ocean, swapped in for the old sunset honeycomb-frame shot. Lite gets its own dedicated crop, `MANUKA_PAN_lite.jpg` (1091x1067, a tighter, more square framing of the same scene), swapped in below 1000px via `<picture>`/`<source>` the same way the Rita Rocks feature already does it — desktop stays on the full photo. Added an aspect-ratio rule for the lite breakpoint so the reserved space matches that crop's real ratio instead of the desktop photo's, avoiding a layout jump while it loads.
- "FROM THE LAND OF THE LONG WHITE CLOUD" now runs the full width of the banner in one line, at every screen size, instead of being right-aligned in a corner (and instead of wrapping to three lines on the old mobile treatment). A runtime script measures the text's true rendered width and scales its font-size up or down so it always spans edge to edge (minus a small side margin), re-measuring on resize and after fonts load, the same measure-and-scale approach used elsewhere on this page for the hero tag line. Checked 390px through 1920px: it fills the full width at every one with margin to spare, no overflow. Below about 390px the scaling hits a 14px legibility floor rather than continuing to shrink, so on anything narrower it would stop spanning true edge-to-edge to stay readable — worth knowing if this needs to support very old/small phone widths.

Recent changes in this version:
- "FROM THE LAND OF THE LONG WHITE CLOUD" (the caption on the Mānuka vista banner photo) is bigger: desktop went from a 16-34px range to 20-44px, lite/mobile from 14-20px to 18-26px. While making it bigger, found and fixed a real pre-existing bug: on desktop, this caption was rendering at 0px tall (confirmed via computed styles, not just a screenshot guess) because it inherits `line-height:0` from its parent banner container (`.manukapan{line-height:0}`, there to remove the thin gap under the banner photo) and never had its own line-height to override it — so the text was actually collapsing invisibly at the very top of the banner, mostly hidden behind the nav bar, on every version of the site before this one, not something this font-size change introduced. Mobile was unaffected, it already had its own `line-height:1.25` for its wrapped multi-line layout. Added `line-height:1.2` to the desktop rule, confirmed the caption now has real height (53px at 1600px wide, was 0px) and is fully visible across the top of the photo, checked at 390px and 1600px, no overflow off either edge of the image.

Recent changes in this version:
- Fixed the tube's vertical centering on desktop. The previous version centered it with a small fixed margin near the video, but the desktop column actually stretches to match the full height of the ingredient list, stats, and CTA button on the right (`.hero-lower` uses `align-items:stretch`), so that fixed margin left a huge dead gap below the tube once the page rendered at full height. The tube now uses auto top/bottom margins instead, which soak up whatever space is actually left in the stretched column and center it there for real, wherever that column happens to end. Confirmed at 1600px: space above and below the tube now match, and the ingredient list, stats, and Explore button are all still clear of it. Lite (≤1000px) is unaffected, it never had this stretched-column situation to begin with.

v220 changes:
- The gold tube shot is now rotated 90° clockwise (lying on its side, cap to the right) and sized to a "life size" proportion relative to the glass: a note on that judgment call, since a screen has no fixed physical size, true life-size in centimeters isn't something a web page can guarantee across devices. What's implemented instead is a realistic real-world ratio — a ~13cm effervescent tube next to a ~15cm pint glass — so the tube reads as roughly true-to-scale beside the glass rather than a shrunken icon. Worth verifying against the actual tube and glass dimensions if exact scale matters for this placement. It's centered with equal margin above and below in the space between the glass and the copy that follows (on lite that's literally the "UPGRADE YOUR RITUAL" headline below it; on desktop the copy runs alongside this column rather than under it, so the same equal-margin treatment centers it in the column's own whitespace instead). Checked 390px, 820px, 1000px, 1401px, and 1600px: no overlap with the ingredient list or headline copy at any of them.
- The gold tube product shot sits directly under the fizzing glass video in the hero, on both desktop and lite. New file `hero-pack-tube.png`: background fully removed and recomposited on pure white (no grey halo or vignette anywhere near the edges, checked at pixel level), cropped tight to the tube itself.
- "MĀNUKA CREATINE PROTOCOL" now matches "HONIORA"'s exact rendered width on desktop. A runtime script measures both lines' true text width (off-screen DOM probes, accounting for font, weight, style, and letter-spacing, not just the elements' box width) and scales the tag line's font-size to match. Checked 1401px through 2560px: rendered widths land within a third of a pixel of each other at every width. Lite is untouched — confirmed the script leaves the tag's font-size alone below 1401px, where its own separate clamp rule still governs.

v218 changes:
- "UPGRADE / YOUR / RITUAL" is doubled in size on desktop (144px cap, up from 72px). Lite is untouched, its own separate size rule wasn't touched. The runtime fit script already in place handles the bigger size everywhere it's needed: the tightest part of the desktop range (just above 1400px) still shrinks it down to fit its column, same as before, just off a bigger starting point, and the column itself is capped at 660px wide so it stops growing past a certain viewport width rather than running unbounded. Checked 1001px through 2560px, three lines every time, nothing overflows.

v217 changes:
- The Rita Rocks feature now uses a dedicated vertical photo (`rita-ora-feature-lite.jpg`, new file in this package) on the lite site only, swapped in with a `<picture>`/`<source>` so desktop keeps loading the original landscape photo (`rita-ora-feature.jpg`) completely unchanged. The new photo is a true 4:5 portrait, shot with a plain marble column down the left side and Rita positioned centre-right, so it needed no cropping at all, just its own aspect-ratio reserved for layout. The quote and byline now overlay directly on this photo too (same left-side gradient-fade treatment desktop already used), instead of the old approach of stacking that text in a plain block below a cropped copy of the desktop photo. The overlay column is narrow and fades out well before it reaches her, confirmed at 360px, 390px, and 430px. Desktop re-verified unchanged: still the original photo, same crop, same layout.

v216 changes:
- On the lite site, the two mobile hex nav buttons (Reserve Protocol, Reserve Test Tube) lost the "i" info badge added a few versions back. In its place, the whole button now doubles in size on hover/focus, growing outward away from the logo (not over it) rather than from dead centre, so it never climbs on top of the wordmark. The info card itself is unaffected for anyone hovering with a real pointer (trackpad on a tablet-width screen already got it on hover before this change); it's specifically the tap-a-badge path for pure touch that's gone, replaced by the size cue. Checked at 390px: badge is gone, both buttons scale cleanly on hover with no overlap.
- Fixed a real layout bug on the lite site: the Founders Offer section (the pricing intro just above the plan cards) had a large empty gap between the offer text and the five-tube pack photo below it, on every phone and tablet width up to 820px. Cause: the text column's CSS gave it `flex-basis: 480px`, meant to set its *width* when it sits side by side with the photo on wider screens, but on narrower screens where that row stacks into a column, flex-basis controls *height* instead, so the same 480px was forcing the text block to a minimum 480px tall regardless of how short its actual text was, no matter the content. Reset that for the stacked layout so the block just takes its natural height, closing the gap. Checked up to 820px, the photo now sits right below the text at every width in that range, and the side-by-side desktop layout above 820px is completely unaffected (that flex-basis:480px is doing its original, correct job there).

v214 changes:
- "UPGRADE / YOUR / RITUAL" is now italic, on both desktop and the lite site. Everything else about it (three forced lines, size, the runtime script that keeps a single word from ever overflowing its column) is unchanged. Checked 320px through 2560px, still fits cleanly at every width.

v213 changes:
- The hero headline's big lead line is back to "UPGRADE YOUR RITUAL" (reverted from the "UPGRADE YOUR DAY" wording tried in v212) and now always breaks onto exactly three lines, UPGRADE, YOUR, and RITUAL, one word per line, on both desktop and the lite site, and set bigger again on top of that (desktop's cap went 58px to 72px, lite's went 50px to 62px). A script keeps it safe at every width: it measures the widest of the three lines against its real column width at runtime and only scales the font down, never up, if a single word would otherwise run past the edge, since the desktop and lite layouts give this line very differently sized columns and a single hand-tuned size can't be trusted to fit both. Checked 320px through 2560px: three lines every time, nothing overflows or overlaps the glass image, and re-verified on resize and after the webfont loads.

v211 changes:
- On the lite (mobile) site, the two hex nav buttons flanking the logo (Reserve Protocol, Reserve Test Tube) now have the same info popouts the desktop hex buttons already show on hover. Since phones don't have real hover, each hex got a small "i" badge in its corner: tap it to open the info card (same wording as desktop — "Monthly Protocol" / "Single Pack"), tap it again, tap elsewhere, or hit Escape to close it. Tapping the hex image itself is unchanged, it still goes straight to Reserve (#store), the badge is a separate tap target so the info card never gets in the way of buying. Devices with real hover (trackpad on a tablet-width screen) also get it on hover, no tap needed. Checked at 360px, 390px, and 430px: both cards open fully on-screen with no overflow, and confirmed desktop's existing hover popouts are completely unchanged.

v210 changes:
- On the lite site, the "HUGE pills and/or chalky, messy powders / Every morning?" line above the hero headline is gone. The headline now starts straight at "UPGRADE YOUR RITUAL." on phones and tablets, no gap left behind. Desktop is untouched, still shows the full headline exactly as before, since it's the same shared paragraph and this only hides that one line below the site's usual 1400px lite/desktop split. Checked 320px through 1600px.

v209 changes:
- In the 09 The Stack table, Cr-01 Creatine's row got two real citations in place of the "Ingredient science citation to be added, verify before launch" placeholder. Checked both before using them: Gutierrez-Hellin et al. 2025 in Nutrients, "Creatine Supplementation Beyond Athletics: Benefits of Different Types of Creatine for Women, Vegans, and Clinical Populations," a narrative review confirming this is a real, indexed paper directly about creatine's benefits beyond sports. The second, a 2026 paper in The Journal of Nutritional Physiology, "Creatine supplementation and brain health," I confirmed is real and indexed (also listed on ScienceDirect and a university repository), but the full text is paywalled everywhere I could check, so I couldn't read the actual findings, only the title, which is why the citation on-site is labeled by year and journal rather than claiming specific results from it. Also dropped the opening sentence about creatine being the largest single ingredient by weight, and added "trusted and" ahead of "heavily researched," both per your edit.

v208 changes:
- Resolved the hypotonic vs isotonic contradiction from the last version. You confirmed the drink is hypotonic at the actual serving size (2 tablets in 300 ml, which is what the site's own dosing instructions already say). I ran a rough order-of-magnitude check against the real formula, summing the dissolved particles from every major active (sodium and potassium from the bicarbonates, magnesium, creatine, taurine, D-allulose, the citrate/malate left over from the effervescent reaction) and came out around 250 mOsm/L, below blood plasma's roughly 285 to 295 mOsm/kg, which is directionally consistent with hypotonic. That's an estimate from ingredient masses, not a lab osmometer reading, so treat it as a sanity check, not a certified number. The one remaining site mention that said "Isotonic electrolyte matrix" now says "Hypotonic electrolyte matrix," matching the other spot that already said "slightly hypotonic drink." I didn't add the more specific "accelerates absorption of creatine, amino acids, and botanical extracts" claim from what you sent, hypotonic solutions generally do empty the stomach faster than isotonic ones, that's well established, but I don't have a source tying that specifically to this formula's ingredients, so I left it as the general "sits easy" language already there rather than a specific absorption-speed claim.

v207 changes:
- Repositioned HONIORA as a daily health booster rather than a gym electrolyte-replacement or pre-workout product. Went through the whole site for sports/gym-coded language and softened or reframed about a dozen spots: the "Cellular Energy" and "Clean Hydration" hero pillar blurbs (dropped "clinical saturation payloads," "neuromuscular performance," "precision osmology," "isotonic fluid delivery system," "GI distress"), the Strength/Gut/Heart/Brain matrix section lede (dropped "power output" emphasis and "The HONIORA protocol performs"), the Dissolve section lede (dropped "high performance"), the "Rapid hydration and nutrient delivery" heading and body (now "Fast-dissolving, easy to absorb," dropped "rushing nutrients into your bloodstream"), the VasoDrive-AP heading (now "Better blood flow, better circulation," was "Upgraded blood flow and muscle delivery"), the creatine section heading and body (now "Cellular energy, every day"), a section header ("The Depth of a Natural Active Antipodean Matrix," was "The Power of..."), and every mention of creatine as an "ergogenic nutrient" or for "sports nutrition/sports science" (five places: the Strength quadrant paragraph, the stack-matrix row, the Ask Honi creatine answer twice, the ishot caption, and the Jenerise origin-story lede), reframed instead around cellular energy and healthy aging, matching your own "Essential for All, Not Just Athletes" video framing. Landkind's "training stress and recovery" / "training load" language (four places: ishot caption, stack-matrix row, ingredient detail block, Ask Honi answer) is now "everyday stress." Left alone on purpose: the "Strength" category name itself (core to the site's Strength/Gut/Heart/Brain architecture, renaming it is a much bigger job than tone), "effervescent engine" as a phrase (your own term for the fizzing mechanism, not gym language), and a few spots that describe actual study findings ("exercise recovery," "exercise-induced muscle damage" in Braincurrant's citations) since rewording those would misstate what the cited research covered. The disputed "hypotonic" vs "isotonic" wording from the last version is still there, still unresolved, still needs your call on which one's correct.

v206 changes:
- Removed the "no independent citation has been added yet, so I won't invent one" style disclaimer from Ask Honi's answers for Taurine, Zinc Bisglycinate, and Landkind Pure Salidroside. For Taurine, the whole source line was that disclaimer and nothing else, so the source line is gone entirely for that one now. For Zinc and Landkind, the disclaimer was trimmed off the end of their manufacturer line, leaving just the manufacturer information. Nothing else about these three entries changed.

v205 changes:
- Expanded the science behind Bifidobacterium adolescentis in the Ask Honi answer for it, covering how its short-chain fatty acids (like butyrate) connect to the blood-brain barrier and to creatine uptake, not just the gut. You supplied four science-style claims about this; each was independently checked before anything went on-site, same as every other ingredient here. What held up: gut inflammation has been shown (in a rat/mouse study) to cut expression of a nutrient transporter at the blood-brain barrier, and a real 2024 human study found a probiotic reducing a gut compound that otherwise blocks the intestinal creatine transporter, raising circulating creatine, plus a solid 2023 review on how short-chain fatty acids protect the blood-brain barrier generally. What didn't: no evidence turned up for gut bacteria improving taurine absorption specifically, so that claim was dropped, and the creatine-transporter finding is from a different Bifidobacterium species (B. animalis, not our B. adolescentis) tested in older sarcopenia patients, so it's flagged on-site as a different strain and population rather than presented as evidence for this product. Also left out: the suggestion that this ties in with cGP-Pro and VasoDrive-AP specifically, since that particular combination hasn't actually been studied together. Three new citations added to the Ask Honi panel for this entry, six total now, all checked against the actual paper content.

v204 changes:
- On the lite site, the hero glass loop video is 20% smaller (399px tall cap, down from 499px). It stays perfectly centred at every width, 320px through 1000px, since the sizing and centring fix from v199 carried over unchanged, only the target size moved.

v203 changes:
- On the lite site, the three fine-print notes under the pricing plans ("Free USA shipping, always.", "First Drop, 2026.", "Founding list only.") no longer have huge empty gaps between them on an iPhone. The cause: those three paragraphs share a CSS rule meant for the desktop layout, where they sit side by side and each one has a 220px minimum width. On phones the layout switches to stacking them vertically, but that same 220px number was still being applied, and on a stacked (vertical) layout the browser reads it as a minimum height instead of a width, forcing every paragraph into a 220px-tall box no matter how short its actual text was. Reset that so each note's height on mobile matches its own text again. Checked 320px through 1600px: the desktop side-by-side layout is unchanged, and every phone width now shows the three notes stacked tightly with even spacing.

v202 changes:
- On the lite site, "2 fizzy tablets & 1 glass of water & 60 sec." (shortened from "2 fizzy tablets, 1 glass of water & 60 seconds." per your latest wording) now stays on one line at every phone and tablet width, 320px to 1400px. It gets its own smaller, width-aware font size instead of sharing the size used by the "HUGE pills..." line above it, since fitting a fixed phrase onto exactly one line at every width needed a tighter, purpose-built size rather than the general body copy size. Desktop is untouched; the text itself is shared between lite and desktop since it's the same element, so the shortened wording shows there too, but desktop keeps its own larger size and normal wrapping.

v201 changes:
- On the lite site, "UPGRADE YOUR RITUAL.", "HoniOra's Solution:", and "2 fizzy tablets, 1 glass of water & 60 seconds." no longer wrap onto more lines than they do on desktop. This wasn't really a font-size problem: .hero-lower reserves a big chunk of right-side padding (430px) for the desktop side-by-side image column, and that leftover reservation was still in effect even after the layout had stacked into one column on phones, squeezing the headline copy into a sliver as narrow as 130px wide. Cleared that reservation once the layout is actually stacked (1000px and below), and eased it partway in the 1001-1400px gap where the nav already goes mobile but the hero layout hadn't caught up yet. HONIORA never wrapped to begin with, so it's untouched. Checked every 20px from 320px to 1400px against desktop's own line counts, no violations left anywhere in that range.

v200 changes:
- Ask Honi's answer thread now shows a real, visible scrollbar on both lite and desktop, so it's obvious there's more to see when an answer runs long. It actually already scrolled (mouse wheel and drag both worked), but with no visible scrollbar and a long answer running right up against the row of suggested questions below it, it read as cut off rather than scrollable. Also added proper momentum scrolling for mobile Safari and stopped scroll from leaking through to the page behind the chat once you hit the top or bottom of an answer.

v199 changes:
- All 14 ingredient-scroll circle photos are now the same visual size. The image frames were already identical, but the actual circle drawn inside each one filled a different amount of that frame (from about 65% up to 98%), so some ingredients looked noticeably bigger than others. Rescaled and recentred every one to a consistent 88% fill. Also squared up thaumatin.jpg, which was 820x786 instead of 820x820, the only one that wasn't a true square file.
- On the lite (mobile) site: the hero glass loop video is 75% bigger (285px tall cap to 499px), and it's now genuinely centred instead of sitting left of centre. The old centring relied on padding that only existed on the right side of its container (reserved for the desktop text column), so this column had nothing balancing it on the left. Fixed by cancelling that reserved space out for this column specifically, and capped the new size responsively so it still centres cleanly without overflowing on narrower phones. Every current iPhone width (375px and up) gets the full 499px.
- The lite (mobile) site now always opens on screen one, the hero. A reload or a shared link with something like #tablet in the URL will no longer land mid-page on a phone. Desktop link behaviour is unchanged.

v198 changes:
- Fixed the real cause of the oversized gap between "HUGE pills and/or chalky, messy powders" and "Every morning?" (and every other line in that hero body text block). It wasn't the line-height on those lines themselves, it was an invisible layout quirk: since only "UPGRADE YOUR RITUAL." is a proper block element, the rest of the paragraph was inheriting that headline's own large 48px/1.15 line spacing as a baseline "strut" for every line, even though the visible text is only 22px. Wrapped that trailing block in its own container with its own font-size and line-height so it no longer borrows the headline's spacing. Line gaps are now the size they were actually styled to be, both lines and paragraph breaks. "HoniOra's Solution:" keeps its original large size, unaffected.

v195 changes:
- Tightened the gap between "UPGRADE YOUR RITUAL." and "HUGE pills and/or chalky, messy powders" in the hero headline. That line break was a block break plus a 6px margin stacked on top, reading as more than one line break. Removed the margin so it's now a single line break, same as the rest of that headline.

v193 changes:
- Tightened the Bifidobacterium adolescentis ingredient-scroll caption: dropped "that survives compression and the effervescent reaction" so it now reads "Microencapsulated live probiotic that ferments the formula's Livaux and Mānuka prebiotic fibres into short-chain fatty acids and supports the gut lining's tight junctions." Also dropped "own" before Livaux.

v192 changes:
- Replaced the Bifidobacterium adolescentis ingredient-scroll photo with the version you sent: a cleaner, sharp-edged circular crop of the same bacteria micrograph, matching the other ingredient tiles' hard-edge circle style more closely than the soft-feathered version used before. Same filename, so no other part of the page needed touching.

v191 changes:
- Corrected D-Allulose back to 110 mg/tab (220 mg/serving). v190 had it at 60 mg, based on a misread of the MBR PDF's table on my part, not an actual formula change. You then sent a clean CSV export of the same Rev 5.7 bill of materials confirming D-Allulose is 110.00 mg/tab, unchanged from Rev 5.6 — corrected on the one site location that carries this figure (stack-matrix table).

v190 changes:
- Formula updated to Rev 5.7 per the new MBR PDF you uploaded (HONIORA_MBR_MakersNutrition_v5.7_140k_38mm.pdf): added Bifidobacterium adolescentis, a microencapsulated probiotic, 50 mg/tab (10 billion CFU per 2-tablet serving). Ingredient count 27→28 everywhere it's stated (hero stat, stat card, Store plan card, AskHoni KB, stack-matrix closing note), unit tablet weight 7,610.00→7,660.00 mg, daily serving 15,220→15,320 mg. Added a new stack-matrix table row, a new AskHoni chat entry, a new item in the hero ingredient list and the header ticker, and a new circular ingredient-scroll photo (you supplied a colourised bacteria micrograph; cropped and soft-edge-masked to match the site's existing circular-photo treatment, same style as the Landkind tile). Science citations independently verified, not taken from your pasted marketing copy: a 2025 human/mouse study (Int J Mol Sci, DOI 10.3390/ijms262412142) on a seafarer cohort showing the species restores gut-lining tight-junction proteins; a 2024 genomic/colitis-model study (Front Microbiol, DOI 10.3389/fmicb.2024.1496280) with the same tight-junction finding plus reduced inflammatory markers; and a 2025 pediatric IBS RCT of a specific strain, PRL2019 (Microorganisms, DOI 10.3390/microorganisms13030627) — flagged on-site as a different strain and a children's population, not a direct match for this product. The "crowds out pathogens" and "second GLP-1 pathway" style claims from your pasted copy were not used as-is since I couldn't independently verify them for this species; the copy instead states the SCFA-fermentation and tight-junction mechanisms that are verified.

**Data flag, still open, please check with Makers Nutrition:** with D-Allulose correctly at 110 mg/tab, summing all 28 per-tablet line items comes to 7,610.00 mg, not the 7,660.00 mg the MBR states as its own header/total-row unit weight — a 50 mg/tab gap. This is the same unresolved gap flagged at Rev 5.6 (it was never about D-Allulose), carried forward unchanged. The site is synced to the stated total, 7,660.00 mg, as the working assumption, same as every revision since Rev 5.6, but this hasn't been confirmed with Makers Nutrition.

v189 changes:
- Rebuilt all four gold nav hex buttons (Reserve Protocol, Reserve Test Tube, Ask Honi, Join List) from scratch: cropped everything outside the dark keyline border away, so the beveled outer band and the leftover shadow blob from the old artwork are gone, leaving a clean tight hexagon with just the border. Added a real soft drop shadow underneath each button (CSS-based, so it sits correctly under the button at all times, not just on hover). Checked at 360px through 1600px, no overflow.

v188 changes:
- Reduced the font size of two lines in the hero sub-copy back down: "HUGE pills and/or chalky, messy powders / Every morning?" and "2 fizzy tablets, 1 glass of water & 60 seconds." are now back to a smaller secondary-text size (16px on desktop, scaling down to 13px on narrow phones). "HoniOra's Solution:" and "UPGRADE YOUR RITUAL." stay large and matched to each other, so the solution line still stands out clearly between the now-smaller surrounding lines. Checked from 360px through 1600px, no overflow.

v187 changes:
- The full four-line sub-copy ("HUGE pills and/or chalky, messy powders / Every morning? / HoniOra's Solution: / 2 fizzy tablets, 1 glass of water & 60 seconds.") is now the same size as "UPGRADE YOUR RITUAL." above it, at every breakpoint (confirmed the computed sizes match exactly). Also tightened the line spacing on that block since a size this large needed less breathing room between lines than the old small-print value. Checked from 360px through 1600px, no overflow.

v186 changes:
- "HoniOra's Solution:" in the hero sub-copy is now the same font size as "MĀNUKA CREATINE PROTOCOL" at every breakpoint (confirmed the computed sizes match exactly, from 15px on narrow phones up to 30px on tablet), so it stands out from the rest of that sentence instead of blending in at the smaller body size. Checked from 360px through 1600px, no overflow or wrapping issues.

v185 changes:
- The line breaks in "HUGE pills and/or chalky, messy powders / Every morning? / HoniOra's Solution: / 2 fizzy tablets, 1 glass of water & 60 seconds." now show everywhere, including desktop. Previously they only showed on mobile/tablet and desktop collapsed it into one flowing paragraph; now the four lines are always visible. Checked at 360px, 390px and 1600px, no overflow.

v184 changes:
- "Upgrade your ritual." is now "UPGRADE YOUR RITUAL." (all caps).
- The sub-copy now capitalizes "Every" ("...powders Every morning?..."), matching the wording you sent. Checked at 360px, 390px, and 1600px, no overflow or wrapping issues.

v183 changes:
- Checked the font on "Upgrade your ritual." against "MĀNUKA CREATINE PROTOCOL": they were already the same family and weight (Arial, bold), computed and confirmed in the browser, so no font change was needed there.
- Increased the size of "Upgrade your ritual.": up to 48px on desktop (was 40px) and up to 42px on mobile/tablet (was 36px), checked from 360px through 1600px wide with no wrapping or overflow issues.
- Updated the hero sub-copy under it to: "HUGE pills and/or chalky, messy powders every morning? HoniOra's Solution: 2 fizzy tablets, 1 glass of water & 60 seconds." (was "HUGE pills and chalky, messy powders every morning? HoniOra's Solution: Two fizzy tablets, a glass of water and sixty seconds."). Kept the same mobile-only line breaks the previous version had (desktop still reads as one flowing paragraph).

v182 changes:
- Replaced the S7 Plant Blend ingredient photo (was a small blue logo graphic, S7.png) with the circular leaf photo you provided (s7_leaf.jpg), sized and framed the same way as the rest of the ingredient grid (820x820, matching the Careflow/Cr-01/cGP-Pro style) instead of the old smaller logo treatment.

v181 changes:
- Fixed a glitch in the matte gold hex buttons from v180: the soft drop-shadow blur baked into the original plaque images had picked up a gold tint when recoloured, showing up as faint gold flecks/streaks in the transparent area around each hex. Rebuilt all four buttons straight from the original source photos with a tight, clean edge, so the transparent background around each hex is genuinely clean now, no flecks, no stray tint.
- Replaced the Careflow Mango Fruit ingredient photo with the new mango-on-mint-circle image you provided, cropped to just the circle and its contents (mango, stem and leaf) with the surrounding white background cut away, matching the tight circular-thumbnail style the other ingredient photos use.

v180 changes:
- All four nav hex buttons (Reserve Protocol, Reserve Test Tube, Ask Honi, Join List) recoloured from white/cream to a flat matte gold. The dark trim line and text stayed as they were for contrast, so the labels are still easy to read. This is a straight recolour of the plaque images themselves (not a CSS filter), applied on both the desktop 4-hex layout and the mobile 2-hex layout, so it's consistent everywhere the buttons appear.
- Checked the spacing between hex 1/2 (Reserve Protocol/Reserve Test Tube) and hex 3/4 (Ask Honi/Join List) on desktop: they were already identical at every width from 1401px up (both pairs use the same tightened gap), so no spacing change was needed there, just confirmed it stayed correct after the recolour.

v179 changes:
- On phones (≤560px), page content now has a wider side margin: 26px, up from 20px. Text blocks, citation lists, ingredient cards and the taste-score table all sit further from the screen edges instead of running close to them. Nothing else changed (the nav bar and hex buttons keep their own separate spacing, untouched by this). Checked from 360px through 1450px, no overflow anywhere.

v178 changes:
- On the mobile nav menu, "Join the Founding List" is now "Join Founding List" (fits on one line at every phone width instead of wrapping).
- Moved "Ask Honi" and "About HoniCo" to the bottom of the RIGHT-hand mobile menu column, after HoniBlog. The left column is now 8 items (Reserve the Protocol through Taste), the right column 8 (2 Tabs through About HoniCo). Checked from 360px through 1000px, no overflow.

v177 changes:
- Moved The Stack, Gut · Heart · Brain, Taste, Ask Honi, and About HoniCo to the bottom of the LEFT-hand column of the mobile menu (previously they'd been placed at the bottom of the right-hand column). The left column is now 10 items, the right column 6. Checked from 360px through 1000px: the longer column still fits comfortably with no overflow.

Recent changes in this version:
- Fixed the hex nav sizing so it actually shows up on vertical (portrait) phones, not just when rotated to landscape. Previously the v175 size increase only applied once the screen got wide enough to enter the "tablet" nav layout, which portrait phones almost never reach, so portrait phones looked unchanged. Now portrait phones get their own two-step growth curve: hexes stay at their existing safe size on the narrowest phones (360-390px, where there's no room to grow without touching the burger icon), then grow meaningfully from 391px up to 560px wide, capping around 106-118px on the widest phones, versus 65px before. Checked from 360px to 560px, no overlap with the burger anywhere.
- Fixed landscape phones getting oversized. Rotating a phone to landscape used to push it past the 560px "phones" width and into the tablet-sized nav (up to 160px hexes, 110px bar), which was far too big for a short landscape screen. Landscape phones (width taller than they are, but under 500px tall) now get their own compact sizing, 70px bar and hexes around 40-43px, similar to portrait. Tablets and desktops in landscape (which have much more height, 600px+) are unaffected and keep the full-size nav. Checked several real phone landscape sizes (iPhone SE through iPhone Pro Max) plus iPad/desktop landscape to confirm the cutoff doesn't catch anything it shouldn't.
- Reordered the right-hand column of the two-column mobile menu: The Stack, Gut · Heart · Brain, Taste, Ask Honi, and About HoniCo now sit together at the bottom of that column, in that order, after the other links (2 Tabs, Ritual, Join the Founding List, Rita Rocks, Lemonwater?, HoniBlog), which keep their previous relative order above them. The left column is unchanged.

v175 changes:
- On the lite (mobile) site, the two hex nav buttons are bigger again on the main lite range (750px-wide and up): the cap is now 160px, up from 138px. On phones (≤560px) they hold at their existing size: tested raising the cap there too, but the vw-driven curve never actually reaches even the old cap within that width range, so there was nothing to gain, and the narrowest phones (360-390px) are already at the physical limit where hex, logo, and burger icon can coexist without touching.
- The nav bar itself is taller again: 110px on the main lite range (up from 92px), 88px on phones (up from 76px), with the hero's top padding and section scroll-offsets adjusted to match so nothing sits under the taller bar.
- The mobile burger slide-out menu is now two columns instead of one long list, so it reads in half the scroll depth. Left column: Reserve the Protocol, Protocol, Mānuka Honey, Science, Function. Right column: everything else, in its previous order (2 Tabs, Ritual, Join the Founding List, Gut · Heart · Brain, The Stack, Rita Rocks, Lemonwater?, Taste, Ask Honi, About HoniCo, HoniBlog). Link targets, colours, and scroll behaviour are unchanged, just the layout.
- The Rita Ora feature photo is now cropped to a 4:3 frame on the lite/mobile and tablet layout (up to 1000px wide): Rita stays fully in view on the left, and the baked-in "HONIORA / Super Star Rita Ora" text stays fully in view, uncropped, top-right. The crop only trims empty background off the left edge of the original photo. Desktop (1001px+) keeps the original full-width photo with the text overlay panel, completely unchanged.
- Checked the whole nav (hex size, bar height) from 360px up through 1450px: no overlap anywhere, no horizontal overflow, logo stays centred, and desktop nav is unaffected.

v174 changes:
- On the lite (mobile) site, the burger slide-out menu list is reordered: "Reserve the Protocol" and "Join the Founding List" (previously near the bottom of the 16-link list) now sit at positions 3 and 4, right after "2 Tabs" and "Ritual". Everything else keeps its previous relative order, just shifted down to make room. Nothing else about the menu (styling, scroll behaviour, link targets) changed.

v173 changes:
- On the lite (mobile) site, the two hex nav buttons ("Reserve Protocol" and "Reserve Test Tube") are up to 50% bigger: 138px on the main lite range (750px-wide screens and up, from 92px) and up to 93px on phones (from 62px, by roughly 800px-wide screens, past the point where phones hand off to the main lite layout). On the narrowest phones (360-430px) they hold close to their existing size, same physical-space tradeoff as previous rounds: the screen just isn't wide enough for a 50%-bigger hex plus the logo plus the burger icon without them touching. Also reduced the gap between the glass video and the "Upgrade your ritual." headline underneath it on the stacked mobile/tablet hero layout, from about 57px down to about 18px. Checked from 360px up through 1400px: no overlap with the logo or burger anywhere, no horizontal overflow, logo stays perfectly centred, and desktop (1401px+) is unchanged.

v172 changes:
- Removed the line break in the "Introducing Jenerise Cr.01" intro paragraph, between "...Olympic athletes in 1992." and "Jenerise Cr-01 Creatine Monohydrate is built to stricter...". It now flows as one continuous paragraph instead of two forced lines.

v171 changes:
- Removed item "03" from the Creatine "What it does" list ("Room for a real creatine dose... Two 38 mm tablets carry it in one dissolve, and twenty-six other named actives besides."). The old item 04 ("Paired with the vascular and gut stack...") is renumbered to 03, so the section now runs 01 through 03 with no gap.

v170 changes:
- Removed item "05" from the Creatine "What it does" list ("Sourced from creatine's own pioneer... Cr-01 comes from Jenerise, founded by Steve Jennings..."). The section now runs 01 through 04. Nothing else in that section changed, and the Jenerise/Steve Jennings sourcing story is still told elsewhere on the page (the Cr-01 ingredient row and the Stack matrix), just not repeated a second time here.

v169 changes:
- On the lite (mobile) site, the nav bar itself is taller (92px, up from 78px on the main lite range; 76px, up from 64px, on phones), the HONIORA wordmark is bigger (58px, up from 48px on the main lite range; 38px, up from 36px, on phones), and the two hex buttons are bigger too (92px, up from 78px on the main lite range; up to 62px, up from 56px, by 500px-wide phones). On the narrowest phones (360-430px) the hexes hold roughly their existing size rather than growing, since the bigger logo now takes up more of the space between it and the burger icon and there just isn't room for both to grow at that width without them touching. Checked from 360px up through 1300px: no overlap with the logo or burger anywhere, no horizontal overflow, logo stays perfectly centred, and desktop (1401px+) is unchanged.

v168 changes:
- The mobile-nav "Reserve Protocol" and "Reserve Test Tube" hexes are bigger again (78px, up from 64px on the main lite range, and up to 56px on phones, from 50px), and a real cascade bug is fixed along the way: the phone-only size and spacing rules were losing to the wider lite-range rules because of a specificity mismatch, so shrinking the hexes down for narrow phones was silently not working. Fixed the selector specificity and re-tuned the phone-width sizing and spacing so the bigger hexes still clear the burger icon and the logo with no overlap and no horizontal overflow, checked from 360px up through 1300px. Also centred the "15,220 mg / 5,000 mg / 15" stats row under the hero on mobile: it used to wrap raggedly, left-aligned, once the layout stacks to one column, now it stacks as one centred group like everything else in that section, on desktop it's unchanged.

v167 changes:
- The mobile nav now puts one hex on each side of the logo (Reserve Protocol on the left, Reserve Test Tube on the right) instead of both together on one side. The logo itself is now always truly centred on the bar (same 1fr/auto/1fr grid technique the desktop nav uses), so it no longer drifts off-centre depending on hex sizing. Also fixed a bug this introduced along the way, where the mobile hexes were briefly showing up as a stray second row underneath the desktop nav above 1400px; that's cleaned up, desktop is back to a single clean row. In "01 / The Ritual" hero copy, added line breaks after "messy powders," "every morning," and "HoniOra's Solution:" on mobile only, desktop keeps the original flowing paragraph. Checked from 360px up through the full lite range and on desktop, no overlap with the logo or burger and no horizontal overflow at any normal phone width.

v166 changes:
- The mobile-nav "Reserve Protocol" and "Reserve Test Tube" hexes are bigger again (64px, 50px on phones, up from 56px/44px) and now sit close beside the logo at every lite/mobile width, not just on narrow phones: they used to be pinned to the screen's left edge, so the gap to the logo grew wider the wider the screen got (up to ~220px on tablet-width "lite" views). They now move as one group with the logo, centred together, so the gap to the logo stays a small, consistent ~20-28px everywhere from 375px up to the 1400px desktop handoff. Checked for overlap against the logo and burger icon at 375, 390, 430, 560, 900 and 1300px, and confirmed the hexes still tap through to the store section.

v165 changes:
- Fixed a real bug in the mobile slide-out menu: the 16-link list is taller than most phone screens, but the menu panel had no scrolling enabled, so it just vertically centred the list and clipped whatever didn't fit, links cut off with no way to reach them. The panel now scrolls properly (tested that every link from "2 Tabs" down to "HoniBlog" is reachable), with room at the top to clear the still-visible header and room at the bottom so the last link isn't flush against the screen edge. Checked on several phone sizes (375, 390, 430px, including a short-screen iPhone SE-sized viewport), no horizontal overflow introduced.

v164 changes:
- The mobile-nav "Reserve Protocol" and "Reserve Test Tube" hexes (added in v163) are bigger and better spaced apart: 40 percent taller (56px, 44px on phones, up from 40px/32px) with real breathing room between them (14px/10px gap, up from a slight overlap). Checked at 375, 390, 430, 900 and 1300px: still clears the centred logo with room to spare and no horizontal overflow.

v163 changes:
- On the mobile (lite) nav, the "Reserve Protocol" and "Reserve Test Tube" hex buttons now sit directly on the nav bar itself, top left, next to the centred logo, instead of only being reachable as a text link inside the slide-out burger menu. Both tap through to the store section. Checked at 375px, 390px, 430px and the wider "lite" widths up to 1400px: no overlap with the logo or burger icon, and no horizontal overflow. The full-size desktop hex buttons above 1400px are unchanged.

v162 changes:
- The "HONIORA" wordmark in the nav bar (`honiora-logo.png`) had a solid white box baked into the image. Its background is now genuinely transparent (proper alpha channel, letters re-matted so there's no white fringe at the edges), so it blends cleanly into the header no matter what's showing behind it as you scroll, instead of showing a hard white rectangle. Checked against several backdrop colours and against the live header in both its transparent and "stuck" (scrolled) states.

v161 changes:
- Ask Honi now gives a targeted answer to "is it safe with blood pressure medication" (and close variants): just the medication/FDA-disclaimer line, instead of the full allergy-and-pregnancy safety paragraph it used to return for any safety-adjacent question. General safety questions (allergies, pregnancy, gluten) still get the full paragraph, which still includes the medication line too. Verified in the browser against several phrasings of both.

v160 changes:
- Added HoniBlog Article 04: "Dr Vicki Petersen: The Powerful Benefits of Mānuka Honey." It's now the top article in the HoniBlog stack (newest first, per the standing rule in the HTML comment above the article list). Same treatment as Articles 02 and 03: a static still from the YouTube Short in a frame the same size as the image, with a play button that opens the video directly on YouTube in a new tab (https://youtube.com/shorts/d7Bky7AMQQw). Checked on desktop, image and link both correct, and the article stack order (04, 03, 02, 01) confirmed.

v159 changes:
- Added HoniBlog Article 03: "Jenerise: Creatine's Origins — Essential for All, Not Just Athletes!" Same treatment as Article 02: a static still from the YouTube Short in a frame the same size as the image, with a play button that opens the video directly on YouTube in a new tab (https://youtube.com/shorts/RnSxf7aFNxA). Checked on desktop, image and link both correct.

v158 changes:
- Fixed a real hero-section layout bug reported on a MacBook Air: at common laptop widths (roughly 1000px to 1600px), the "Upgrade your ritual" copy and the actives list were squeezed into a tiny, wrapped, near-centered column instead of the intended wide left-aligned layout. Root cause was the hero's text block sharing a CSS class with the page's standard side-padding, which was eating width it shouldn't have inside the hero's own layout. Fixing that width alone then exposed a second, previously-hidden problem: with the text column properly widened, the glass image (which is intentionally shifted right to sit close to the copy) started visually overlapping the headline and ingredient list. Both are now fixed together: the text column has its full intended width, and the glass image and text no longer overlap at any desktop width from 1001px up to 2560px+ (spot-checked at 1001, 1280, 1366, 1401, 1440, 1600, 1920, 2560). Also re-checked mobile (375, 390, 430px): type and images are centred as intended and there is no horizontal overflow/side-scroll anywhere on the page.

v156 changes:
- In "05 / The Creatine Dose," the two body paragraphs reworded: "Creatine is one of the most trusted and heavily researched ingredients in wellness and sports nutrition." and "Sourced from creatine's own UK pioneer, Steve Jennings, who first introduced creatine monohydrate to Olympic athletes in 1992. Jenerise Cr-01 Creatine Monohydrate is built to stricter and higher standards than its competitors: independently tested and verified by SGS at 99.96% purity."

v155 changes:
- In "05 / The Creatine Dose," the two paragraphs under the heading ("Creatine is one of the most heavily researched..." and "Sourced from creatine's own pioneer...") are now left-aligned (ragged right edge) instead of centered. The heading above them stays centered. Checked on desktop and mobile, clean.

v154 changes:
- Fixed the "05 / The Creatine Dose" heading from v153: on wider screens (confirmed on an iMac) each of the three intended lines was wrapping again into two, six lines total instead of three, because the heading's normal font size was too large for the line "By the Creatine OG Steve Jennings." to fit on one row. The heading now uses a smaller, dedicated size for this specific three-line heading, so it renders as exactly the three intended lines at every desktop width (checked 1401px to 2560px). On phones the last line still wraps once, since 350px is genuinely too narrow to fit that full phrase at a readable size.

v153 changes:
- In "05 / The Creatine Dose," the heading now reads across three lines: "Introducing Jenerise Cr.01," then "Super Creatine Monohydrate," then "By the Creatine OG Steve Jennings." in gold. Checked on desktop and mobile, no overflow.

v152 changes:
- The "HoniBlog" nav link (desktop nav and mobile menu) now scrolls precisely to "16 / HoniBlog," landing it right at the top of the frame directly under the header, instead of partway down the section.

v151 changes:
- In "01 / The Ritual," the countdown circle's caption now breaks onto two lines: "Seconds to full dissolution" on its own line, "&middot; no stirring" below it.

v150 changes:
- Fixed the Article 02 video: the embedded, autoplay-in-place player from v149 threw YouTube error 153 for this specific video, because its owner has embedding disabled (a setting on the video itself, not something a site can work around). The play button now opens the video directly on YouTube in a new tab instead, which works reliably. Same still image and frame, same play-button look.
- HoniBlog stack reordered: Article 02, "SLIMLAND: Why Kiwis are a GOATED Fruit," now sits first, with Article 01, "Six habits for healthy ageing," below it. Left a note in the code so future articles get added at the top of the stack, newest first, going forward.

v149 changes:
- Added HoniBlog Article 02, "SLIMLAND: Why Kiwis are a GOATED Fruit," directly below Article 01. It shows the supplied still inside a frame sized to match the image, with a play button overlaid. Clicking play swaps the still for an embedded, autoplaying YouTube player (https://youtube.com/shorts/tiKZwCxxdMA) at the exact same frame size, no layout jump. Checked on desktop and mobile, no overflow.

v148 changes:
- The "About HoniCo" nav link (desktop nav and mobile menu) now scrolls precisely to "14 / The Company &middot; About HoniCo.", landing it right at the top of the frame directly under the header, instead of partway down the section.

v147 changes:
- In "03 / M&#257;nuka Honey," the heading now reads "M&#257;nuka honey" on its own line, with "Does far more than sweeten." on the line below in gold. The copy underneath now reads "The 1,000 mg MGO 850+ M&#257;nuka Honey Crystals in HONIORA are a concentrated daily dose of very high MGO goodness, efficient bioactives and significantly reduced honey sugars."

v146 changes:
- The "M&#257;nuka Honey" nav link (desktop nav, mobile menu, and footer) now scrolls precisely to the top of the golden bee mark, instead of landing partway down the section with empty space above it. Verified on desktop: the bee now sits right at the top of the frame, directly under the header, with no lingering gap.

v145 changes:
- In "04 / Look around HoniCo HQ," the embedded VR360 panorama is now 30% taller (300px &rarr; 390px), giving the walkthrough noticeably more room. Checked for overflow/overlap around the frame, clean.

v144 changes:
- In "About HoniCo" &rarr; "Who we are," the closing sentence now stands on its own line in bold: "HoniCo is dedicated to supporting Bees and Apiculture, and we pledge 1% of all profits to Bee welfare and research initiatives." (Fixed a real bug this surfaced along the way: the bold tag was rendering as only medium-weight, not properly bold, because the section's paragraph style set a lighter base weight that the browser's default "bolder" keyword doesn't jump past on its own; fixed by setting the weight explicitly.)

v143 changes:
- In "About HoniCo" &rarr; "02 / Who we are," the bee-mark image replaced with the supplied "1% for the Bees" badge (background made transparent so it sits cleanly on the section's cream background, not as a black box).
- The "Who we are" copy replaced with the new text supplied. The unrelated bee-mark image used in the M&#257;nuka Honey section further up the page was left untouched.

v142 changes:
- The hero stat "27 / Named actives, zero blends" changed to "15 / Full dose Natural actives, Zero blends."

v141 changes:
- "S7® Plant Matrix" added to the bottom of the hero named-actives list, now 12 items.

v140 changes:
- Removed the original 7-item hero claims list ("5g Cr-01 creatine mono...", "Isotonic hydration matrix...", etc). The 11-item named-actives list now sits directly under "Born in New Zealand > Made in USA," where the removed list used to be.

v139 changes:
- The "4D" desktop nav link removed. The "4D" (Gut, heart, brain) section itself is untouched and still reachable via the hero's "Gut, heart, brain" button and the footer link.

v138 changes:
- Tightened the spacing between the two paired hex buttons on each side of the nav (Reserve Protocol/Reserve Test Tube on the left, Ask Honi/Join List on the right), independent of the gap between the hex buttons and the text nav links, which is unchanged. Checked for overflow/overlap at every desktop width from 1401px to 1920px, all clean.

v137 changes:
- "Rita Rocks" moved to the left-hand nav cluster, between Function and Cr.01. "Ritual" moved to the right-hand cluster, between Mānuka Honey and About HoniCo.
- Fixed a real bug this surfaced: with "Rita Rocks" added to the left cluster, its links ran wide enough to overlap the HONIORA logo at 1401px. Fixed by tightening nav link spacing and letter-spacing slightly; checked clean (no overlap, no overflow) from 1401px to 1920px.
- The hero line now reads "HUGE pills and chalky, messy powders every morning?" ("Still swallowing" and "mixing" dropped).

v136 changes:
- "Mānuka Honey" moved to the front of the right-hand nav cluster, now sitting directly beside the logo, between it and "Rita Rocks." Checked for overflow at every desktop width from 1401px to 1920px, all clean.

v135 changes:
- Both hero bullet lists (the original 7-claim list and the new 11-item named-actives list) made bold.

v134 changes:
- The hero copy now reads "Still swallowing HUGE pills and mixing chalky, messy powders every morning?"
- The "2 HONIORA tablets + 300ml cold water + 60 seconds = 27 Named Actives..." line replaced with an 11-item bulleted list of named actives, leading with MGO 850+ Mānuka Honey crystals: Cr-01 Creatine Monohydrate, Livaux® gold kiwi, Feiolix® feijoa, Braincurrant®, cGP-Pro® blackcurrant, VasoDrive-AP® peptide, Careflow® mango, Eriomin® lemon, Landkind® Rhodiola rosea, Thaumatin® Talin. Styled to match the existing claims list right above it.

v133 changes:
- Widened the gap between the hero glass video and the hero text column by 10px. Clean with clear breathing room from 1440px up; at 1401px the text still lightly touches the glass's edge, an improvement over v132 but not fully clear at that one narrow width.

v132 changes:
- The hero text column (Upgrade your ritual, the HONIORA headline, the bullet list, everything in that block) moved left 25px net from where it started this session.

v131 changes:
- The hero glass video (the fizzing tablets in the glass) moved 100px right and 75px down on desktop. Checked for overflow at every desktop width from 1401px to 1920px, all clean; the mobile layout, which already had its own separate centering rule, is unaffected.

v130 changes:
- "4D" moved to the left-hand nav cluster, now the last item, closest to the logo.
- The "HoniCo" desktop nav link renamed to "About HoniCo" (matches the mobile menu's existing wording). Checked for overflow at every desktop width from 1401px to 1920px, all clean.

v129 changes:
- Swapped "Stack" and "Rita Rocks": Stack now sits in the left-hand nav cluster (last item, closest to the logo), Rita Rocks in the right-hand cluster (2nd item, after 4D). Checked for overflow at every desktop width from 1401px to 1920px, all clean.

v128 changes:
- HoniBlog's Article 01 now has the real founder photo (`blog-founder-happy-place.jpg`), cropped to lose the rock on the sand at the bottom, with the caption "HoniOra Founder Greg Miller-Hard in his happy place at the end of his land" overlaid bottom-right on the photo itself (white italic text, drop shadow for legibility), matching how photo captions work elsewhere on the site.
- "Rita Rocks" moved from the right-hand nav cluster to the left, now sitting directly beside the HONIORA logo.
- The 4 hex nav buttons made noticeably bigger again (62px → 82px tall) — moving Rita Rocks left rebalanced the two nav clusters enough to afford the extra size without reintroducing the overflow bug from v127; verified safe at every desktop width from 1401px up to 2560px.

v127 changes:
- Built the HoniBlog section (new "16 / HoniBlog" section): Article 01, "Six habits for healthy ageing." Linked from the nav.
- Fixed a real bug this surfaced: a 6-link nav cluster was overflowing off-screen at normal laptop widths; fixed by tightening nav spacing rather than raising the mobile breakpoint.

v126 changes:
- The 4 hex nav buttons (Reserve Protocol, Reserve Test Tube, Ask Honi, Join the List) each show an info popout on hover/focus.

v125 changes:
- Added a "HoniCo" link to the desktop nav bar (right-hand cluster, after Mānuka Honey), linking to the existing "About HoniCo" section.

v124 changes:
- "Upgrade your ritual." (the lead line above the HONIORA / MĀNUKA CREATINE PROTOCOL headline) made significantly bigger.
- Text updated to: "Upgrade your ritual. Still swallowing giant pills and mixing chalky, messy powders every morning? HoniOra's Solution: Two fizzy tablets, a glass of water and sixty seconds." ("those" dropped from "giant pills").
- On mobile (the lite site), the HONIORA logo in the header is now dead center. Previously it was flex-positioned against the burger menu button using space-between, which pushed it to the left instead of centering it; the burger button is now positioned independently so the logo can sit at true center.

v123 changes:
- The new line above the HONIORA / MĀNUKA CREATINE PROTOCOL headline made bold throughout, with the first sentence set larger than the rest.

v122 changes:
- Reworded the new line above the HONIORA / MĀNUKA CREATINE PROTOCOL headline.

v121 changes:
- Added a new line of copy directly above the HONIORA / MĀNUKA CREATINE PROTOCOL headline.
- Fixed a real, pre-existing mobile bug uncovered while placing that new line: the whole hero text column (headline, tagline, bullet list, everything) was carrying a desktop-only 50px leftward offset that was never reset for mobile, so on phones it sat 50px too far left and clipped content off the left edge. That offset is now cleared on mobile; the desktop layout is unaffected.

v120 changes:
- On the 1st scroll (the auto-scrolling hero ticker), "Balanced Hydration Matrix" moved from last to 2nd position, right after Cr-01 Creatine.

v119 changes:
- Mobile top-nav logo doubled in size (48px, 36px on phones under 560px).
- Fixed the mobile glass video sitting slightly off-center: it was inheriting a 50px rightward offset meant only for the desktop layout, which was never cleared for mobile. Now sits dead center on phones; desktop is unaffected.

v118 changes — reworked the mobile hero, on request for a "lite" mobile layout (built into the same responsive index.html rather than a second file, so there's nothing extra to host):
- Mobile hero order changed: glass video, then the logo/tagline/bullet-list/stats text block, then the Pure Origin pillar leading the other three pillars (Cellular Energy, Vascular Support, Clean Hydration) — previously the pillars sat between the video and the text block.
- Glass video enlarged 50% on mobile (190px → 285px max-height).
- Fixed a real autoplay/loop bug: the script meant to force-start the hero video on browsers that block autoplay was querying a leftover class name (`.glassbox`) that no longer exists anywhere in the page, so that fallback silently did nothing. It now points at the real video element, so the loop reliably starts on phones that need the manual nudge (notably iOS Safari).

v117 changes — a full mobile audit ("is the mobile site screen-width only, and is everything centered"):
- Fixed real horizontal overflow on phones: the four-label "Strength → Gut → Heart → Brain" loop diagram and the ingredient-table group headings ("Effervescent & organoleptic system" etc.) were both set to stay on one line no matter how narrow the screen, so on phones they silently pushed the whole page a few dozen pixels wider than the screen. Both now wrap properly on narrow screens; the page no longer scrolls sideways at any phone width tested (375–430px).
- Fixed a real bug, not just cosmetic: the entire 09 TheStack ingredient table (all 27 actives) was wrapped in a single scroll-reveal animation that only turns visible once 12% of its own height has scrolled into view. That threshold was written for normal-sized blocks — this one block is far taller than any phone screen, so that 12% was mathematically impossible to reach and the whole section stayed invisible on mobile. The reveal now triggers off a small pixel amount instead of a fixed percentage, so it still displays at the same point for every normal-sized block on the page, but now works correctly for this one too.
- Confirmed all page sections and images are properly centered (the `.wrap` container was already using `margin:0 auto`) — the "not centered" feeling was actually the sideways scroll bug above; once that's fixed, everything sits centered as intended.

v116 changes:
- Added a "Cr.01" nav link between Function and the logo, linking to the "Introducing Jenerise Cr.01" creatine section.

v115 changes:
- Nav bar logo enlarged 50% on both desktop and mobile.
- Nav bar spacing widened (the gap between the logo and the hex-button/nav-link clusters on either side) to keep everything comfortably clear at the larger logo size; the breakpoint where the nav collapses to the mobile burger menu moved from 1360px to 1400px so the wider logo always has room before it does.

v114 changes:
- Nav bar wordmark replaced: the "HONIORA" text logo is now the supplied logo image (`honiora-logo.png`), sized to match the previous text logo exactly, on both the desktop-centered and mobile nav bars.
- Hero bullet list and closing line: added missing end-of-line periods for consistent punctuation.

v113 changes:
- Fixed the pinned horizontal-scroll "unit" section (the claims list and the ingredient-photo band): the ingredient images were each triggering a layout refresh the instant they finished loading, and because they load lazily while that section is pinned, those refreshes were firing mid-scroll and causing the scroll position to snap or jump. The unnecessary refreshes are removed; the section now scrubs smoothly start to finish.
- NZ map icon in the Pure Origin pillar enlarged 30% relative to the other three pillar icons.
- 07 Function table reduced in width and centered.
- "No Maltodextrin" removed from the claims list in the pinned scroll section.
- Ask Honi popup: bee logo removed, header shrunk, backdrop-click-to-close confirmed working.
- Fixed the pinned scroll section covering the Mānuka beekeeper banner image below it.
- The two right-column pillar copy blocks (Cellular Energy, Clean Hydration) now range left, matching the left column.
- Ask Honi's Hobbiton answer updated to the latest wording and corrected to the HONI*ORA* brand styling.

Earlier changes:
- Pillar icon images (NZ map, mitochondria, heart, H2O droplet) reverted to their original size after a temporary doubling.
- "4D" nav link moved to the right of the HONIORA logo in the nav bar; nav spacing re-verified across desktop widths.
- Braincurrant® copy in the Stack section rewritten.
- Ingredient images moved out of the `ingredients/` subfolder and flattened into the same folder as `index.html`; all `src` paths updated to match.
