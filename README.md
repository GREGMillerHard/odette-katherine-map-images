# HONIORA site — deployment notes

## What's in this package

- `index.html` — the entire site (markup, CSS, and JS all in one file).
- Every image, video, and icon file — hero shots, nav hex buttons, pillar icons, the nav bar wordmark (`honiora-logo.png`), the Mānuka vista banner, the HoniBlog founder photo (`blog-founder-happy-place.jpg`), the HoniBlog Article 02 video still (`blog-kiwi-slimland.jpg`), the HoniBlog Article 03 video still (`blog-jenerise-origins.jpg`), the HoniBlog Article 04 video still (`blog-manuka-benefits.jpg`), and all 13 ingredient photos used in the Stack section — sits flat in the same folder as `index.html`, referenced as plain filenames like `hero-glass.jpg` or `cGP-Pro_blackcurrant.jpg`. There is no `ingredients/` subfolder — everything is one level, beside `index.html`.

## How to host it

This is a static site with no build step. Upload the whole folder as one flat directory — `index.html` and every image/video file beside it, no subfolders. If any image lands in a different folder than `index.html`, or in a subfolder, it will fail to load even though the page itself renders fine, since every image path in the HTML is relative to where `index.html` lives.

For GitHub Pages specifically: commit the folder as-is, flat, to the repo (or the branch/folder GitHub Pages is configured to serve from). Do not nest the images into subfolders or rename files — the HTML references exact filenames.

## Version

This package corresponds to HONIORA_site_v167, updated 2026-09-01.

Recent changes in this version:
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
