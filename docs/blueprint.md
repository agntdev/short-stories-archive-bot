# Short Stories Archive — Bot specification

**Archetype:** community

**Voice:** warm and encouraging — write every user-facing message, button label, error, and empty state in this voice.

A public Telegram bot for submitting and browsing text-only short stories with immediate public visibility. Stories are displayed chronologically with optional author attribution and include search/pagination features.

> This is the complete contract for the bot. Implement EVERY entry point, flow, feature, integration, and edge case below. The completeness review checks the bot against this document after each build pass.

## Primary audience

- general public
- story writers
- casual readers

## Success criteria

- 100+ daily story submissions
- 500+ active daily users browsing stories

## Entry points

Every feature must be reachable from the bot's command/button surface (button-first; only /start and /help are slash commands).

- **/start** (command, actor: user, command: /start) — Display welcome message with Submit/Browse/Help options
- **Submit story** (button, actor: user, callback: submit:start) — Initiate story submission flow
  - inputs: text message
  - outputs: story confirmation with ID/timestamp
- **Browse stories** (button, actor: user, callback: browse:start) — View paginated list of recent stories
  - inputs: page number
  - outputs: story list with navigation buttons
- **/search** (command, actor: user, command: /search <term>) — Search stories by keyword
  - inputs: search term
  - outputs: filtered story list

## Flows

### Story submission
_Trigger:_ submit:start

1. Display text input prompt
2. Save story with metadata
3. Show confirmation with ID

_Data touched:_ Story

### Story browsing
_Trigger:_ browse:start

1. Show 10 stories per page
2. Add Next/Prev navigation
3. Display story details

_Data touched:_ Story

### Search
_Trigger:_ /search

1. Process keyword
2. Filter matching stories
3. Show results

_Data touched:_ Story

## Data entities

Durable data (must survive a restart) uses the toolkit's persistent store, never in-memory maps.

- **Story** _(retention: persistent)_ — Submitted story with metadata
  - fields: id, text, author_name, timestamp

## Integrations

- **Telegram** (required) — Bot API messaging
- **Admin Channel** (optional) — Submission notifications
Call external APIs against their real contract (correct endpoints, ids, params); credentials from env. Do not fake responses.

## Owner controls

- Enable/disable admin channel notifications
- Set story character limit
- Configure search sensitivity

## Notifications

- Admin channel copy of each new submission

## Permissions & privacy

- All stories are public and permanent
- Author names derived from Telegram display names
- No user accounts or private data stored

## Edge cases

- Empty message submission
- Message exceeding 2000 characters
- Search with no matching results
- First/last page in browsing

## Required tests

- End-to-end submission flow with validation
- Pagination navigation between story pages
- Search functionality with edge cases

## Assumptions

- Immediate public posting of stories
- Telegram display name used for author attribution
- 10-story pagination default
