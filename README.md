# Winchester Fall Town Meeting live blog — 2026

Live reporting from Winchester's 2026 Fall Town Meeting by Winchester News, kept separate from the [45-article warrant tracker](https://winchesternews.org/20261106_winchester-town-meeting-tracker-all-45-warrant-articles-explained/).

Live page: https://wdowd-arch.github.io/winchester-fall-town-meeting-live-blog/ (embeddable with `?embed=1`).

## Files

- `index.html`: the live blog page. It reads only `live-updates.json` and re-checks it every 30 seconds while open. With `?embed=1` it drops the page chrome and posts its height to the parent frame.
- `live-updates.json`: the editorial feed. This is the only file to edit during the meeting.
- `embed-code-for-live-blog.txt`: paste into a Ghost **HTML card** in a new post. The frame resizes as updates arrive.
- `live-blog.html`: redirects to the main page, for older links.

## Ghost post

Create a new post, add an HTML card, and paste the contents of `embed-code-for-live-blog.txt`. If the post is also emailed, set that HTML card to **web only** and add a separate email-only link to the live page, because email clients do not show iframes or scripts. Preview the published post on phone and desktop; the Ghost editor may show only a placeholder.

## Posting updates

Start with `status: "upcoming"` and an empty `updates` array. When the meeting opens, set `status` to `"live"`, set `updatedAt` to the real publication time in ISO 8601 with the Eastern offset, revise `note` if needed, and add verified entries. Use `"ended"` when coverage concludes. Example entry:

```json
{
  "id": "nov5-1905-opening",
  "at": "2026-11-05T19:05:00-05:00",
  "article": 1,
  "title": "A reported headline",
  "body": "A concise, verified update. Plain text and line breaks are supported.",
  "sourceUrl": "https://www.winchester.us/example",
  "sourceLabel": "Town document"
}
```

`id`, `at`, `title`, and `body` are required; `article` (1–45), `sourceUrl` (https only), and `sourceLabel` are optional. Entries with a missing field or an unreadable `at` time are skipped. Times display in Eastern time in AP style ("7:05 p.m. Nov. 5," "7 p.m.," noon, midnight) with no time-zone label, newest first. An optional `dek` shows under the headline and must not repeat any headline word. Never post an unverified vote, quote, or floor update.

`body` is plain text with a few line-based conventions (in JSON, write a line break as `\n`):

- A blank line starts a new paragraph. A single line break stays a line break.
- `## Heading` makes a small section heading.
- A line starting with `• ` or `- ` is a bullet.
- An indented line starting with `– ` or `- ` (for example `  – item`) is a sub-bullet under the bullet above it.
- Any other indented line continues the bullet above it as a new paragraph.

Everything is inserted as text, never as HTML, so entries cannot inject markup or scripts.

An entry may also carry an optional photo, shown under the headline. Put image files in `images/` (JPEG, so older iPhones can show them) and give the real pixel size so the page does not jump while it loads:

```json
"image": {
  "src": "images/town-meeting-file-photo-1226.jpg",
  "width": 1226,
  "height": 693,
  "srcset": [
    { "src": "images/town-meeting-file-photo-800.jpg", "width": 800 },
    { "src": "images/town-meeting-file-photo-1226.jpg", "width": 1226 }
  ],
  "alt": "What the photo shows, for screen-reader users.",
  "caption": "Caption text.",
  "credit": "COURTESY PHOTO / NAME"
}
```

Only `src` is required; `srcset` is optional and lets phones load a smaller file. `src` must be a path in this repository or an https address.

GitHub Pages republishes about a minute after a push to `main`, so updates are not instant. `.github/workflows/pages.yml` publishes through GitHub Actions, which avoids the 10-builds-an-hour limit of branch publishing. It requires Settings > Pages > Source set to **GitHub Actions**; until then it skips deployment and the branch publishing continues, under that limit.

## Local preview

```
python -m http.server 8000
```

Then open http://localhost:8000/ and http://localhost:8000/?embed=1.

## Live video

The top of the page shows the YouTube live video named in `stream.youtubeId` (the 11-character ID from the YouTube link), with a link to WinCAM’s government channel underneath. Change the ID for each session, or set `"stream": null` to hide the player. Changing it reloads the player; other feed updates do not.
