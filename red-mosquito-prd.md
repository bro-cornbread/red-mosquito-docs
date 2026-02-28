---
title: Product Requirements Document
permalink: /
---

# Red Mosquito — Product Requirements Document

## 1. Problem Validation

### The Problem

Pearl Jam has played over 1,127 shows across 35+ years, with 627 unique songs, 722 venues, and 24,000+ individual setlist entries. Fans who attend multiple shows have no consolidated, mobile-friendly way to:

- Track which songs they've heard live and which they're still chasing
- Understand how rare a song is relative to the full catalog
- Record their attendance history with personal ratings
- View trends in the band's setlist choices over the decades
- Earn recognition for milestones in their concert journey

### Evidence This Problem Exists

- Pearl Jam has one of the most dedicated fanbases in rock music, with many fans attending 10-100+ shows
- Existing fan sites (pearljam.com, fivehorizons.com, pearljamconcertchronology.com) provide raw data but no personal tracking or mobile experience
- The band's famous "no two setlists alike" philosophy creates a natural collector/completionist dynamic
- Fan communities on Reddit, forums, and social media frequently discuss attendance tracking and song chasing

### Current Workarounds

- Fans maintain personal spreadsheets or text files listing shows attended
- Some use generic concert tracking apps (Setlist.fm) that lack Pearl Jam-specific depth
- Others browse fan sites on mobile browsers — functional but clunky
- No existing solution combines personal tracking with band-wide statistics and gamification

### Cost of Not Solving

- Hours spent manually cross-referencing setlists across multiple fan sites
- No recognition or reward for dedication and attendance milestones
- Lost personal history when spreadsheets get corrupted or lost
- Missed connections between personal experience and broader catalog patterns

---

## 2. Market Analysis

### Direct Competitors

| Competitor | Strengths | Weaknesses |
|-----------|-----------|------------|
| **Setlist.fm** | Broad artist coverage, community-contributed data | Generic — not Pearl Jam-focused, no rarity scoring, no achievements |
| **Pearl Jam official app** | Official brand, tour info | Limited stats, no personal tracking, no gamification |
| **livefootsteps.org** | Deep PJ statistics, rarity scores | Web-only, desktop-focused, no mobile experience, no personal tracking |

### Indirect Competitors

| Solution | Why It Falls Short |
|----------|-------------------|
| Personal spreadsheets | Manual maintenance, no visualization, no gamification |
| Generic concert apps | Lack depth for a single-artist focused experience |
| Fan forums/Reddit | Information scattered, not structured or personalized |

### Why Existing Solutions Fail

No single tool combines: (1) comprehensive Pearl Jam-specific data, (2) personal concert tracking, (3) rarity scoring, (4) achievement/trophy system, and (5) a polished mobile experience. Red Mosquito fills this gap.

### Market Size

- Pearl Jam has sold 85M+ records worldwide
- Average of 500K-1M+ unique ticket buyers per tour cycle
- Active online fan community estimated at 50K-100K engaged fans
- Serviceable Obtainable Market (SOM): 5,000 active users in year 1

---

## 3. Value Proposition

### Unique Angle

Red Mosquito is the only app purpose-built for Pearl Jam fans that combines complete live catalog data with personal tracking and gamification. It turns concert attendance into a rewarding, data-rich journey.

### Key Differentiators

1. **Song Rarity System** — Every song scored and color-coded by frequency of play (5 tiers from Ultra-Rare to Staple)
2. **Personal Concert Tracking** — Mark attendance, rate shows, see your unique song collection grow
3. **Trophy/Achievement System** — Trophies across 5 tiers tied to real concert milestones
4. **Comprehensive Data** — 1,127 shows, 627 songs, 722 venues, 24,359 setlist entries — all in your pocket
5. **Stats Dashboard** — Customizable widgets surfacing band-wide trends and personal stats
6. **Dark Matter Visual Identity** — A polished, on-brand dark theme that feels like it belongs in the PJ universe

### User Benefit Statement

"Know every song, track every show, earn every trophy — your Pearl Jam journey, quantified."

---

## 4. MoSCoW Feature Prioritization

### MUST Have (MVP — Week 1)

| Feature | Description |
|---------|-------------|
| **Shows Browser** | Searchable list of all 1,127 concerts with year filter chips |
| **Show Detail** | Full setlist grouped by set, rarity badges, debut markers, tour name, show notes, setlist status indicator |
| **Songs Explorer** | Searchable/sortable song catalog with rarity filters |
| **Song Detail** | Stats, lyrics (collapsible), recent performances |
| **"My Show"** | Mark show attendance + 1-5 star rating (local storage) |
| **Simple Profile** | Show count, unique songs heard, basic stats |

### SHOULD Have (Post-MVP — Weeks 2-4)

| Feature | Description |
|---------|-------------|
| **Authentication** | Email/password accounts for cloud-synced personal data |
| **Stats Dashboard** | Home tab with Band Stats and Personal Stats modes; 14 widgets (see §7.1) |
| **Venue Map** | Interactive world map with density-colored pins (see §7.5) |
| **Venue Detail** | Venue stats, capacity, indoor/outdoor, list of all shows at venue (see §7.6) |
| **Trophy System** | Achievement system with 5 tiers themed around Seattle venues (see §8) |
| **Album Detail** | Track listing with live play counts, streaming links, credits (see §7.7) |
| **Unified Search** | Cross-type search with 2-char minimum, grouped results with count badges (see §7.8) |
| **Settings** | Sign in/out with confirmation, app version, preference placeholders (see §7.9) |

### COULD Have (v2+)

| Feature | Description |
|---------|-------------|
| **Poster Gallery** | 3-column masonry grid of 1,000+ show posters with year filters and date overlays (see §7.10) |
| **Offline Caching** | Full offline support with local data persistence and background refresh |
| **Advanced Dashboard** | Reorderable widgets, add/remove, edit mode |

### WON'T Have (Out of Scope)

| Feature | Reason |
|---------|--------|
| Social features | Adds complexity; focus on personal experience first |
| Push notifications | Requires backend infrastructure beyond MVP |
| Setlist predictions | ML/AI feature deferred to future |
| Collectible digital badges | Blockchain/NFT complexity not warranted |
| Apple Watch / Wear OS | Platform-specific development overhead |

---

## 5. User Stories

### MVP — Browse & Discover

| ID | Story | Acceptance Criteria | Success Metric |
|----|-------|-------------------|----------------|
| S-01 | As a fan, I want to browse all Pearl Jam concerts so I can explore touring history | Shows load with date, venue, city, song count; pull-to-refresh works | 80% of sessions include show browsing |
| S-02 | As a fan, I want to filter shows by year so I can focus on a specific era | Year chips filter instantly; "All" resets; selected chip is visually distinct | 50%+ of browse sessions use a filter |
| S-03 | As a fan, I want to see a song's rarity badge so I know how special it is | Color-coded badge displays on every song row matching the 5-tier system | Users can correctly identify rarity tiers |
| S-04 | As a fan, I want to view a show's full setlist so I can relive the concert | Setlist grouped by set name; songs show position, rarity, debut badge; setlist status indicator | Avg time on show detail > 30 seconds |
| S-05 | As a fan, I want to search for any song so I can find it quickly | Search filters as user types (2+ chars); results update in real-time | Search used in 30%+ of sessions |
| S-06 | As a fan, I want to sort songs by rarity, play count, or name | Sort toggles work; list re-renders correctly | Sort used in 20%+ of song browse sessions |

### MVP — Personal Tracking

| ID | Story | Acceptance Criteria | Success Metric |
|----|-------|-------------------|----------------|
| S-07 | As a fan, I want to mark a show as attended | "My Show" toggle persists locally; visual state changes | 60%+ of registered users mark at least 1 show |
| S-08 | As a fan, I want to rate shows I attended (1-5 stars) | Star rating only appears after marking attendance; persists locally | 40%+ of attended shows get rated |
| S-09 | As a fan, I want to see my unique songs heard count | Profile shows accurate count derived from attended shows' setlists | Users check profile weekly |
| S-10 | As a fan, I want to see how many shows I've attended | Profile shows count + percentage of all PJ shows | Drives repeat attendance marking |

### Post-MVP — Dashboard & Stats

| ID | Story |
|----|-------|
| S-11 | As a fan, I want a customizable stats dashboard so I see the data I care about most |
| S-12 | As a fan, I want separate band stats and personal stats views so I can switch contexts |
| S-13 | As a fan, I want to see which songs are played most often so I know what to expect at my next show |
| S-14 | As a fan, I want to see shows on a map so I can visualize the band's global reach |
| S-15 | As a fan, I want to see a song's performance history so I know when and where it was last played |

### Post-MVP — Personal Tracking (Enhanced)

| ID | Story |
|----|-------|
| S-16 | As a fan, I want to see which album songs I've heard live so I can track album completion |
| S-17 | As a fan, I want to see my rarest catches so I can appreciate the special moments |
| S-18 | As a fan, I want to see a timeline of my attended shows so I can visualize my journey |
| S-19 | As a fan, I want to see which songs I've heard the most so I know my personal staples |
| S-20 | As a fan, I want to see how many cities, states, and countries I've seen shows in |
| S-21 | As a fan, I want AI-generated insights about my concert patterns |

### Post-MVP — Achievements

| ID | Story |
|----|-------|
| S-22 | As a fan, I want to earn trophies for attendance milestones so I feel rewarded |
| S-23 | As a fan, I want to see all available trophies so I know what I'm working toward |
| S-24 | As a fan, I want trophies organized by difficulty tier so I can appreciate the progression |
| S-25 | As a fan, I want to see my trophy progress on my profile |

### Post-MVP — Discovery & Detail

| ID | Story |
|----|-------|
| S-26 | As a fan, I want to see album details with streaming links so I can listen to songs I discovered |
| S-27 | As a fan, I want to browse show posters in a visual gallery |
| S-28 | As a fan, I want to read a song's lyrics while viewing its stats |
| S-29 | As a fan, I want to see which tour a show belonged to |
| S-30 | As a fan, I want to read fan-written show notes about what made a concert memorable |

### Post-MVP — Account & Auth

| ID | Story |
|----|-------|
| S-31 | As a visitor, I want to browse all band statistics without creating an account |
| S-32 | As a user, I want to reset my password via email |
| S-33 | As a user, I want my session to persist across app restarts |
| S-34 | As a user, I want to sign out with a confirmation prompt |

---

## 6. MVP Scope Definition

### Minimum Feature Set for Launch

The MVP delivers a read-heavy experience where fans can explore Pearl Jam's complete live catalog and begin tracking their personal concert history — all stored locally on-device.

**Screens (6):**
1. Shows list (with search + year filter)
2. Show detail (setlist + rarity + "My Show")
3. Songs list (with search + sort + rarity filter)
4. Song detail (stats + recent shows)
5. Profile (attendance stats)
6. Tab navigation shell

**Data:** All 1,127 shows, 627 songs, 722 venues, 24,359 setlist entries served from existing Supabase database. Tour names populated for all shows. Show notes imported for 609 shows (1990-2004).

**Personal data:** Stored locally via AsyncStorage (attendance marks, ratings). No account required.

### What Can Be Manual/Fake Initially

- Song lyrics: can be added incrementally post-launch
- Poster images: show placeholders where images aren't available
- Rarity scores: currently nulled — new formula to be designed

### Timeline

- **Week 1:** Full MVP — all 6 screens functional and polished
- **Week 2-3:** Auth, dashboard, venue map, trophies
- **Week 4+:** AI insights, poster gallery, offline mode

---

## 7. Screen & Feature Specifications

### 7.1 Stats Dashboard (Home Tab)

The home screen is a customizable dashboard with two modes: **Band Stats** and **My Stats**. Users switch between modes via a toggle at the top.

#### Band Stats Widgets (7)

| Widget | Type | What It Shows |
|--------|------|---------------|
| **Shows by Year** | Bar chart | How many shows per year from 1990 to present |
| **Rarity Distribution** | Pie chart | Breakdown of songs by rarity tier |
| **Top 10 Openers** | Ranked list | Most common first songs played at shows |
| **Top 10 Closers** | Ranked list | Most common final songs (last encore) |
| **Most Played Songs** | Ranked list | Top 15 songs by total play count |
| **Album Completion** | Progress bars | Percentage of each album's songs that have been played live |
| **Shows by Country** | Bar chart | Geographic distribution of concerts |

#### Personal Stats Widgets (7 + AI Insights) — requires account + attended shows

| Widget | Type | What It Shows |
|--------|------|---------------|
| **My Rarity Catches** | Pie chart | Songs heard by rarity tier — see how special your catches are |
| **My Albums Completed** | Progress bars | Album songs heard live — track album completion |
| **Geographic Reach** | Stat card | Count of cities, states, and countries where you've seen shows |
| **Most Seen Songs** | Ranked list | Songs heard most across all attended shows |
| **Rating Distribution** | Bar chart | Breakdown of show ratings given (how many 5-star, 4-star, etc.) |
| **My Show Timeline** | Bar chart | Attended shows per year — visualize your fan journey |
| **AI Insights** | 3-card carousel | Personalized AI-generated observations (see below) |

**AI Insights details:**
- Each insight has a title, body, category label, and icon
- Categories: effort, rarity, geographic, loyalty, milestone, fun-fact
- User can dismiss individual insights or refresh for new ones
- Insights expire after 7 days and regenerate

**Note:** My Shows count and Unique Songs Heard are already displayed in the Profile screen and don't need dedicated dashboard widgets. Additional widgets to be brainstormed in a future session.

### 7.2 Shows Browser

A searchable, filterable list of every Pearl Jam concert.

- **Header** shows total concert count
- **Year filter chips** scroll horizontally (All, 2025, 2024, ... 1990)
- **Attended filter** (when signed in) — toggle to show only attended shows
- Each show card displays:
  - Concert date (long format)
  - Venue name
  - City, State/Province, Country
  - Song count
  - Poster thumbnail (if available)
- **Pull-to-refresh**
- Tap any show to open its detail screen

### 7.3 Show Detail

Everything about a single concert.

- **Poster image** (if available)
- **Date, venue, location, song count**
- **"My Show" toggle** — mark/unmark attendance
- **Star rating** (1-5) — only when marked as attended
- **Full setlist** grouped by set (Set 1, Set 2, Encore, etc.):
  - Position number
  - Song name (tappable — navigates to song detail)
  - "DEBUT" badge if this was the song's first-ever performance
  - Rarity badge
- **Tour name** (always available — populated for all 1,127 shows)
- **Setlist status indicator** — if the setlist is partial or unknown, display a notice
- **Show notes** (available for 609 shows, primarily 1990-2004 era)

### 7.4 Song Detail

Everything about a single song.

- **Song name** (large heading)
- **Rarity badge** + **"Cover" badge** (if applicable)
- **Album name**
- **Stats row:** Total plays, first played date, last played date
- **Lyrics** (collapsible — show first 8 lines with "Show more" toggle)
- **Recent Performances** — list of last 10+ shows where this song was played, showing date, venue, and set name. Each row tappable to show detail.

### 7.5 Venue Map

An interactive world map showing every venue where Pearl Jam has performed.

- **Map pins** colored by show density:
  - Brighter/larger for venues with 5+ shows
  - Lighter/smaller for 1-4 shows
- **Tap a pin** to see venue name, location, and show count
- **Tap the callout** to navigate to venue detail
- **Legend** explaining pin colors
- Dark-themed map styling that matches the app's visual identity

### 7.6 Venue Detail

- **Venue name and full location** (city, state, country)
- **Stats:** Show count, capacity (if known), indoor/outdoor
- **List of all shows** at this venue with dates and song counts

### 7.7 Album Detail

- **Album cover art** (or placeholder)
- **Album name, release year, type, track count**
- **Track list** with:
  - Track number
  - Song name (tappable)
  - Cover artist (if cover)
  - Total live plays
  - Rarity badge
- **Streaming links** — tap to open in Spotify, Apple Music, etc.
- **Credits** — producer, engineer, recorded at (collapsible)

### 7.8 Unified Search

Unified search across the entire database.

- **Search bar** with 2-character minimum
- **Results grouped by type:** Songs, Shows, Venues
- Each group shows a count badge and relevant details
- Tap any result to navigate to its detail screen

### 7.9 Settings

- **Sign In / Sign Out** (with confirmation dialog on sign out)
- **App version**
- Future: notification preferences, theme options, data export

### 7.10 Poster Gallery

A visual grid of all 1,000+ show posters.

- **3-column masonry grid** of poster images
- **Year filter chips** at top
- **Date overlay** on each poster
- Tap to navigate to show detail
- Smooth scrolling performance even with 1,000+ images
- Header showing total poster count

---

## 8. Trophy System

### Concept

A gamification layer that rewards fans for concert attendance milestones, rarity catches, geographic reach, and catalog completion. Trophies are organized into 5 tiers themed around iconic Seattle music venues.

### Tiers

| Tier | Venue Theme | Color | Difficulty |
|------|-------------|-------|------------|
| Common | The Showbox | Lavender-grey | Entry-level milestones |
| Uncommon | The Moore Theatre | Emerald | Moderate dedication |
| Rare | The Off Ramp | PJ Blue | Serious commitment |
| Epic | The Paramount | Electric Magenta | Deep devotion |
| Legendary | The Crocodile | Gold | Extraordinary achievement |

### Trophy Categories

Trophies reward fans across these dimensions:
- **Attendance milestones** — show count thresholds
- **Rarity catches** — hearing rare and ultra-rare songs live
- **Geographic reach** — cities, states, countries visited
- **Album completion** — hearing every song from a studio album live
- **Special moments** — debuts, specific dates, iconic venues

### Display

- Grid of all trophies organized by tier
- Unlocked trophies show full color; locked trophies show dimmed/grayscale with lock icon
- Tap any trophy to see a detail modal with: name, tier, description, unlock date (if earned)
- Trophy progress shown on Profile screen

**Note:** Specific trophy definitions and requirements are TBD — to be designed in a dedicated session.

---

## 9. Component Design Patterns

### Cards
Rounded corners (12px border radius), dark card background (`#1C1C20`), subtle border (`#2A2218`)

### Chips
Rounded pill shape (16-20px border radius), outline in default state, filled with primary color when active

### Badges
Small rounded rectangles with tier color background and white text. Used for rarity tiers, "Cover" labels, "DEBUT" markers.

### Bar Charts
Crimson (`#BF2038`) bars on dark background, grey axis labels, responsive to container width

### Pie Charts
Rarity tier colors with legend below the chart

### Ranked Lists
Numbered items with horizontal progress bars showing relative value (e.g., play count relative to most-played)

### Progress Bars
Rounded track with gradient fill, used for album completion and trophy progress

---

## 10. Future Roadmap

These are not in scope for the current release but are on the roadmap for future consideration:

| Feature | Description |
|---------|-------------|
| **Social features** | Share setlists, compare stats with friends, leaderboards |
| **Push notifications** | Alerts when a rare song is played at tonight's show |
| **Setlist predictions** | Predict tonight's setlist based on historical patterns |
| **Collectible badges** | Verifiable digital trophies that fans can own and display outside the app |
| **Concert photos** | User-uploaded photos tied to attended shows |
| **Apple Watch / Wear OS** | Quick-glance widgets for setlist tracking during shows |
| **Home screen widget** | iOS/Android home screen widget showing next show or recent stats |

---

## 11. Data Overview

### 11.1 Data Counts

| Table | Rows | Description |
|-------|------|-------------|
| **shows** | 1,127 | Every Pearl Jam concert from 1990-10-22 to 2024-11-21 |
| **songs** | 627 | Every song Pearl Jam has performed or recorded (530 with live plays) |
| **venues** | 722 | Every venue where PJ has performed |
| **setlist_entries** | 24,359 | Every song at every show, with set name, position, debut/tag markers |
| **albums** | 14 | Studio albums from Ten (1991) to Dark Matter (2024) |

### 11.2 Song Qualification Rules

A song is included in the database if it meets any of these criteria:

1. **Performed live by Pearl Jam** — Appears in at least one setlist entry. This accounts for 530 of 627 songs.
2. **On a Pearl Jam studio album** — Even if never performed live (e.g., Vitalogy/Gigaton interludes like "Bugs", "Aye Davanita", "Never Destination"). 4 songs fall in this category.
3. **In the source catalog** — 93 additional songs with 0 plays exist in the database from the original data import (covers, Eddie Vedder solo songs, collaborations). These are candidates for filtering/hiding in the app.

**What counts as a "play":** A song appearing in any `setlist_entries` row for a show. Tags (e.g., "with Mike McCready on lead vocals") are tracked but don't affect the play count. Segues between songs are tracked via `segue_into_next` (not yet populated).

**Song classification:** Songs have a `classification` field (original, cover, etc.) and an `is_original` boolean. The `cover_artist` field currently contains the generic string "Cover" rather than actual artist names — this is a known data gap.

### 11.3 Data Enrichment Status

| Field | Status | Coverage |
|-------|--------|----------|
| **tour_name** | Populated | 1,127/1,127 (100%) — 28 named tours with regional suffixes (54 distinct values) |
| **show_notes** | Populated | 609/1,127 (54%) — Fan-written show notes from Fivehorizons & PJCC (1990-2004 era) |
| **setlist_status** | Populated | complete (1,079), partial (12), unknown (36) |
| **special_status** | Populated | festival (55), benefit (38), mookie_blaylock (23), tv (23), promo (2), tv_rehearsal (1) |
| **event_name** | Partial | 119 shows with named events (Lollapalooza, Bridge School Benefit, SNL, etc.) |
| **rarity_score / rarity_tier** | Not populated | Nulled — awaiting new formula design |
| **song lyrics** | Partial | Some songs have lyrics; to be expanded |
| **poster_image_url** | Partial | Some shows have poster URLs |

### 11.4 Data Sources

All data originates from public fan sites. No official Pearl Jam data agreements exist.

| Source | Coverage | What We Use |
|--------|----------|-------------|
| **pearljam.com** | 1990-present | Shows, songs, albums, setlists — primary source of record |
| **Fivehorizons.com** | 1990-2004 | Show notes, attendance figures, durations (745 shows documented) |
| **PearlJamConcertChronology.com** | 1990-2002 | Show notes, partial setlists, photos/posters (594 entries, 3,400+ images) |
| **TwoFeetThick.com** | 2005-2012 | Setlists recovered via Wayback Machine (229 shows) |

### 11.5 Known Data Gaps

- **97 songs with 0 plays** — 4 are album interludes, 93 are non-PJ songs. May be hidden in app.
- **cover_artist** — All cover songs say "Cover" instead of the actual original artist name.
- **album_id** — 76.3% of songs have no album association (covers, B-sides, one-offs).
- **segue_into_next** — All false; segue data exists in source sites but hasn't been imported.
- **Show notes post-2004** — 518 shows (2005-2024) have no notes. No fan site covers this era comprehensively.
- **36 shows with unknown setlists** — Genuinely undocumented early-era shows (1990-1993).

---

## 12. Business Model

### Initial Strategy: Free

Red Mosquito launches as a **free app** — no paywall, no ads. The goal is to build a dedicated user base and validate demand.

### Future Monetization Options (Post-Validation)

| Model | Description |
|-------|-------------|
| **Freemium** | Free browsing, paid personal tracking (AI insights, advanced stats) |
| **Tip jar** | Optional one-time support from fans |
| **Premium trophies** | Exclusive achievement tiers for supporters |

### Cost Structure

| Service | Monthly Cost |
|---------|-------------|
| Supabase (Pro) | $25 |
| Expo EAS Build | $0-29 |
| Apple Developer | $8.25 (annual / 12) |
| Google Play Developer | $2.08 (one-time / 12) |
| **Total** | ~$65/month |

Well within the $50-200/month budget.

---

## 13. Success Metrics

| Metric | 1 Month | 3 Months | 12 Months |
|--------|---------|----------|-----------|
| Downloads | 100 | 500 | 5,000 |
| Weekly Active Users | 30 | 150 | 1,500 |
| Shows Marked Attended | 200 | 2,000 | 20,000 |
| App Store Rating | 4.0+ | 4.3+ | 4.5+ |
| Retention (30-day) | 25% | 35% | 40% |

---

## 14. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Data accuracy issues | Medium | High | Cross-reference multiple sources; community error reporting |
| Low initial adoption | Medium | Medium | Share in PJ fan communities; target Reddit, fan forums |
| API rate limits (data sources) | Low | Medium | Cache aggressively; update data in batches |
| App store rejection | Low | High | Follow guidelines strictly; no copyrighted images without permission |
| Pearl Jam IP concerns | Medium | High | Use public data only; no official branding; clearly mark as fan-made |
