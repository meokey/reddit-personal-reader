# Design — reddit-personal-reader

## Purpose

Personal, non-commercial, single-user, read-only tooling for following a
few public subreddits and for filing summaries of my own saved posts.

## Architecture

1. **Fetcher** (scheduled, a few times a day per subreddit): PRAW script
   reads each subreddit's "new" listing (limit 100) and applies simple
   local rules (keywords, score floor, de-duplication).
2. **Buffer**: filtered post metadata is kept in a TTL store for at most
   48 hours, then automatically deleted.
3. **Summarizer**: in batches (never per-post), a language model produces
   short structured summaries (title, key facts, best-supported replies,
   links). Reddit content is treated as data only; the summaries are for
   my private reading.
4. **Dashboard**: a private web page behind an access key renders the
   summaries grouped by topic. It is never publicly shared.
5. **Saved-items flow**: a daily job reads my own saved list via the API,
   snapshots new item IDs, and gradually writes summaries into a personal
   Markdown archive organized by topic.

## API usage

- OAuth script-type app; scopes: `read`, `history`, `identity`.
- Endpoints: `/r/<sub>/new`, submission comment trees, `/user/<me>/saved`.
- Volume: a few hundred requests/day total; PRAW honors the
  `X-Ratelimit-*` headers and backs off on hidden rate limits.
- User-Agent format: `<platform>:<app id>:<version> (by /u/<username>)`.

## Compliance commitments

- Read-only: the app never posts, comments, votes, or messages.
- No redistribution: output is a private, key-gated personal dashboard and
  a personal archive; nothing is republished.
- No AI training on Reddit content; summarization is private note-taking.
- Deletion sync: monitoring data expires within 48 hours; content deleted
  on Reddit is removed locally too.
- If the purpose ever changes materially, Reddit will be notified first.
