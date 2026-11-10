# reddit-personal-reader

A read-only, personal Reddit reader that runs on my own home server.
**Single user (me), non-commercial, no interaction with other Redditors.**

## What it does

- Polls a small number of public subreddits I follow (starting with
  r/teslacanada and r/teslamotors) a few times a day for new posts about
  Tesla software updates, pricing, and recalls in Canada.
- Reads my own account's saved-items list and files short summaries of
  posts I saved, so I can find them again later.
- Presents the result on a private, access-controlled dashboard that only
  I can open.

## What it never does

- No posting, commenting, voting, or messaging — read-only, always.
- No redistribution or public display of Reddit content.
- Reddit content is never used to train AI models.
- No commercial use of any kind.

## How it works

- Python + [PRAW](https://github.com/praw-dev/praw), OAuth script-type app,
  descriptive User-Agent.
- Endpoints: subreddit "new" listings, submission comment listings, and my
  own saved-items listing. Expected volume: a few hundred requests per day
  at most, far below the 100 QPM limit.
- See [docs/DESIGN.md](docs/DESIGN.md) for the full design and data-handling
  rules.

## Data handling

- Monitoring data is stored only as metadata plus short summaries and is
  automatically deleted within 48 hours; anything deleted on Reddit is
  purged locally as well.
- Credentials live only on my own server, never in this repository.

## Status

Design complete. Implementation starts once Reddit Data API access is
approved (request under review — ticket 18718207).
