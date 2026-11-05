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

`id`, `at`, `title`, and `body` are required; `article` (1–45), `sourceUrl`, and `sourceLabel` are optional. Times display in Eastern time, newest first. Text is rendered as text, so entries cannot inject HTML. Never post an unverified vote, quote, or floor update.

GitHub Pages republishes about a minute after a push to `main`, so updates are not instant. `.github/workflows/pages.yml` publishes through GitHub Actions, which avoids the 10-builds-an-hour limit of branch publishing. It requires Settings > Pages > Source set to **GitHub Actions**; until then it skips deployment and the branch publishing continues, under that limit.

## Local preview

```
python -m http.server 8000
```

Then open http://localhost:8000/ and http://localhost:8000/?embed=1.
