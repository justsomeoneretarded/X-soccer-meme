# X Soccer Meme

An English-language soccer meme poster for a global audience. It looks for
match reactions, club banter and fan humor, then previews or posts one
approved photo or video per run.

- Collects approved public Telegram channels or Reddit communities in
  `sources.json`. The Telegram list contains the supplied channels. Enable a source only after you
  have permission to reuse the media you plan to post. Public previews
  and community membership give you a way to browse the content.
- Expires candidates eight hours after the source post and gives new items
  a two-hour freshness half-life. Source-relative views and reactions inform
  ranking. Reddit retrieval uses its newest listing and can be blocked on
  cloud runners.
- Requires a positive Gemini or Groq vision verdict plus a minimum score.
  Gemini starts with Flash-Lite. Groq fallback uses the vision models
  available to its configured key.
  The judge rejects unrelated media, plain footage, betting promotions and
  abusive content. Video screening uses a still and caption. Choose feeds
  you trust to supply authorized, suitable full clips and audio.
- Uses a short original X caption and links to the source. Deduplicates
  source URLs and exact media bytes. Re-encoded media can match imperfectly.
- Saves reservations to this repo's own `autopost-state` branch before
  publishing. Unconfirmed sends are set aside and never automatically retried.

## Configured sources

The November 18 audit found recent downloadable media from
[`soccer_memes`](https://t.me/s/soccer_memes),
[`Football_FunTroll`](https://t.me/s/Football_FunTroll) and
[`FootballMemesBanter`](https://t.me/s/FootballMemesBanter).
`trollingfootball` exposed older media. `Troll_Football_Telegram`,
`troll_footy` and `FOOTYMEMES_Comments` had no usable media during the check.
All supplied handles are in the configuration, so a channel can contribute
when a fresh public post becomes available.

## Preview and go live

1. Add approved names to `sources.json` as bare handles or subreddit names.
2. Add `GEMINI_API_KEY` in **Settings → Secrets and variables → Actions**.
   Optionally add `GROQ_API_KEY` as a backup. Run **Actions → Soccer memes**
   with **Only test the AI keys/models** checked to verify access.
3. Run **Soccer memes** with **Create isolated posting history** checked once,
   leaving publishing off. Review the report and try again when sources
   have qualifying fresh material.
4. Save the **meme account's** session as `TWEETKIT_COOKIE` in this repo,
   then run one live test. TweetKit uses an unofficial X session, so it can
   stop working if the session expires.
5. After confirming a successful post, set the repository Actions variable
   `AUTO_PUBLISH` to `true` for a GitHub schedule every 10 minutes. Scheduled
   runs are skipped until the variable is enabled. GitHub can delay jobs.

```sh
python -m venv .local/runtime
.local/runtime/bin/python -m pip install -r requirements.txt
.local/runtime/bin/python -m unittest discover -s tests -v
```

Reddit clips with separate audio require FFmpeg. Live Actions runs prepare it
when Reddit sources are configured. Telegram MP4s use the original video.
