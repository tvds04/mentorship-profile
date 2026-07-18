# Mentorship Profiles

## What this project is
Spotify-style "artist profile" pages for mentors in the Technology Consulting Workshop (TCW) mentorship program. The design was recreated in HTML from `Reference Image.png` (a mobile Spotify artist page mockup).

## Files
- `index.html` - THE TEMPLATE, and also what GitHub Pages serves at the repo root. A single self-contained HTML file (all CSS inline, icons are inline SVG). Verified visually against the reference image. `profile-template.html` was retired - use `index.html` as the only source of truth going forward (or once a real `profiles/` index page exists, point `index.html` there instead).
- `Reference Image.png` - the original design being matched.
- `Sample Headshot.jpg` - placeholder headshot used in the template hero image.
- `TCW Logo.jpg` - the real TCW logo, used in the small tile next to the Following button (same for every profile).
- `Lizzy McAlpine.jpg` - favorite-artist card image (from Wikimedia Commons, photo by Philip Romano). Each new profile needs its own favorite-artist image; Wikipedia's API is a reliable source: `https://en.wikipedia.org/w/api.php?action=query&titles=<Artist Name>&prop=pageimages&format=json&pithumbsize=800` returns a thumbnail URL to curl.
- `template-preview.png` - full-page browser screenshot of the finished template, for quick comparison without rendering.

## How to create a new profile (for future agents)
1. Copy `index.html` to a new file, e.g. `profiles/firstname-lastname.html`.
2. Edit ONLY the spots marked with `<!-- EDIT-ME: ... -->` comments:
   - headshot image path (appears twice: hero + favorite artist card image)
   - mentor name (h1)
   - hometown ("From ...")
   - majors line
   - intern experience items (title + season; add/remove `.exp-item` blocks, keep `.num` sequential)
   - latest interests (one `.interest-item` div per interest)
   - favorite artist name + image
   - mentoring style blurb (`.card-desc`)
   - favorite song + artist (now-playing bar)
3. Do NOT touch the CSS or the icon SVGs - they are pixel-matched to the reference.
4. The layout is a fixed 540px-wide column (`.phone`). Headshot crop is controlled by `object-position` on `.headshot` - adjust the `20%` vertical value if a new headshot's face is cut off.

## Verification workflow (learned the hard way)
- Playwright MCP blocks `file://` URLs. Serve the folder first:
  `python -m http.server 8471` (run in background from the project dir), then open `http://localhost:8471/<file>.html`.
- Screenshot with `browser_take_screenshot` + `fullPage: true`, then Read the PNG and compare against `Reference Image.png` side by side.
- Kill the server afterward: find PID via `netstat -ano | grep 8471`, then `Stop-Process -Id <pid> -Force`.

## Learnings / gotchas
- `position: sticky` bars (now-playing, bottom nav) look right when scrolling but render mid-page in a fullPage screenshot and overlap content. The reference is one tall static capture, so those bars are in normal document flow at the bottom. Keep them that way.
- The shuffle icon uses Spotify's actual shuffle SVG path (viewBox 0 0 16 16). Hand-drawn approximations look noticeably wrong - reuse real glyph paths for brand icons.
- The background `python -m http.server` task reports exit code 127 to the harness when killed, even though it served fine - ignore that failure notice.
- Filenames with spaces work fine in `src=""` attributes but must be %20-encoded in URLs.
- No Unicode characters in code (user preference) - all icons are inline SVG, the "..." menu is three CSS dot spans.

## Possible next steps (not yet requested)
- A `profiles/` folder + a small index page listing all profiles.
- A generator script (or agent instructions) that takes a JSON of profile data and stamps out HTML files.
- Git repo (ask user first - gh CLI is available).
