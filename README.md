# Job Search Tracker

A personal job search command center, paired with an AI agent that searches job boards for me every morning.

I built this because my search was spread across spreadsheets, notes and inboxes. I wanted one place that tells me what needs attention today, which new postings are worth my time, and how each role compares on what I actually care about. That means more than title and pay: what I'd own, who I'd learn from, and how close I'd be to the founders.

## What it does

- **Overview:** applied count, response rate, interviews done and offers. Below those are overdue next steps, follow-ups due (applications with no reply after 10 days), missing thank-you notes, and the next 14 days.
- **New jobs feed:** postings the AI agent found, each with a 0–100 match score, why it fits, and what to verify. One click tracks a job into the pipeline or dismisses it.
- **Pipeline:** board and table views of every role, filtered by city, search lane, role type and status. Each role records:
  - pay, with bonus, commission and equity kept separate, and flags against a salary floor and target
  - three deciding questions: *What would I decide or own? Who would I learn from? What do I still need to verify?*
  - a fit grid scoring seven criteria (decision influence, leadership access, ownership, variety, mentorship, customer contact, routine-work load) as strong, promising, weak or unknown, with evidence
  - how the start date fits around school
- **Companies:** a watchlist of target firms, each with the roles tracked there.
- **Interviews:** every round with date, interviewers, prep, outcome and thank-you status, plus advance rate and furthest round reached.
- **People & stories:** contacts with follow-up dates, and a bank of interview stories.
- **Timeline:** the academic calendar drawn to scale, with each opportunity placed on it.

## The daily AI search agent

A scheduled Claude agent runs every morning in the cloud:

1. Reads the tracker's existing companies, roles and leads, so it never re-adds a job I've already seen or dismissed.
2. Searches broadly: public job boards (Greenhouse, Lever, Ashby) for my target companies, VC portfolio job boards (rotating several funds per day), a dozen-plus web searches across every role type and city, and public startup boards.
3. Opens each promising posting to confirm it's still accepting applications, and skips closed roles and ones requiring 3+ years of experience.
4. Scores each job 0–100 on role fit and ownership (40), level and timing (20), location (15), industry (15) and pay (10). It keeps only scores of 55 or higher, at most 15 per day and 3 per company.
5. Writes the new jobs into the tracker's database, then records when the search ran and what it covered.

The agent never applies to jobs or contacts anyone. It only reads public postings and writes to my private tracker.

The search isn't traditional code. It's an AI agent following written instructions, which decide what it searches, how it verifies postings and how it scores them. Those instructions are in [`agent/daily-search-instructions.md`](agent/daily-search-instructions.md), with my personal details replaced by placeholders.

## How it works

- **One HTML file** (`index.html`) with plain JavaScript and CSS, and no build step or framework.
- It runs as a **Claude artifact** with a document database. The page subscribes to six collections (`companies`, `roles`, `interviews`, `contacts`, `leads`, `stories`) plus a `meta/settings` document, so edits appear live on any device.
- **Access rules** keep the database private to the owner.
- **No data lives in this code.** All of my real job search records are in the private database, not in this repository.
- Light and dark themes, and it works at phone width.
- If you open `index.html` straight from this repo, it shows the empty layout and a note that saved data only loads on claude.ai, because the database is provided there.

## Run it yourself

This project runs on Claude, not as a standalone website.

1. **The tracker:** in a Claude chat, ask Claude to publish `index.html` as an artifact with the `db` (database) capability. Claude gives you a private link, and the page saves everything you enter there.
2. **The daily search:** in Claude Code, create a scheduled routine that runs each morning, and paste in the instructions from `agent/daily-search-instructions.md`. Fill in your own profile and your tracker's link first.
3. **Network access:** the routine's cloud environment needs network access turned on (Full, or an allowlist that includes the job board sites), or it can't open job postings.

## Built with

HTML, CSS, JavaScript · Claude artifacts (document database) · Claude scheduled agents for the daily search
