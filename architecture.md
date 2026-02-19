---
title: Technical Architecture
permalink: /architecture
---

# Red Mosquito — Technical Architecture Blueprint

## 1. System Architecture Overview

### High-Level Component Diagram

```
┌──────────────────────────────────────────────────┐
│                    Mobile App                     │
│              React Native + Expo                  │
│                                                   │
│  ┌─────────┐ ┌──────────┐ ┌────────┐ ┌────────┐ │
│  │  Shows   │ │  Songs   │ │Profile │ │  Show  │ │
│  │ Browser  │ │ Explorer │ │  Tab   │ │ Detail │ │
│  └────┬─────┘ └────┬─────┘ └───┬────┘ └───┬────┘ │
│       │             │           │           │     │
│  ┌────▼─────────────▼───────────▼───────────▼──┐  │
│  │           State Management                   │  │
│  │     React Context + React Query              │  │
│  └────────────────┬─────────────────────────────┘  │
│                   │                                │
│  ┌────────────────▼─────────────────────────────┐  │
│  │          Local Storage (AsyncStorage)         │  │
│  │     Attendance marks, ratings, preferences    │  │
│  └───────────────────────────────────────────────┘  │
└──────────────────────┬───────────────────────────┘
                       │ HTTPS (Supabase JS Client)
                       ▼
┌──────────────────────────────────────────────────┐
│                   Supabase                        │
│                                                   │
│  ┌──────────────┐  ┌──────────────┐              │
│  │  PostgREST   │  │   Auth       │              │
│  │  (REST API)  │  │  (future)    │              │
│  └──────┬───────┘  └──────────────┘              │
│         │                                         │
│  ┌──────▼───────────────────────────────────────┐ │
│  │            PostgreSQL Database                │ │
│  │                                               │ │
│  │  shows (1,127) │ songs (627) │ venues (722)  │ │
│  │  setlist_entries (24,359) │ albums (14)       │ │
│  └───────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────┘
```

### Data Flow

1. **App Launch** → Supabase client initialized with anon key
2. **Screen Load** → React Query fetches data from Supabase REST API
3. **Data Caching** → React Query caches responses in memory (stale-while-revalidate)
4. **User Actions** (attendance, ratings) → Written to AsyncStorage locally
5. **Profile Stats** → Computed locally from AsyncStorage attendance + cached setlist data

### Third-Party Services

| Service | Purpose | Tier |
|---------|---------|------|
| **Supabase** | Database + REST API | Free tier (sufficient for 5K users) |
| **Expo EAS** | Build service + OTA updates | Free tier for builds |
| **Expo Go** | Development testing | Free |

---

## 2. Tech Stack

| Layer | Technology | Justification |
|-------|-----------|---------------|
| **Framework** | React Native + Expo SDK 54 | Cross-platform iOS/Android, managed workflow, OTA updates |
| **Language** | TypeScript | Type safety, better DX, catches errors early |
| **Navigation** | Expo Router (file-based) | Convention over configuration, deep linking built-in |
| **Data Fetching** | TanStack React Query v5 | Caching, background refresh, pagination, stale-while-revalidate |
| **Backend Client** | Supabase JS v2 | Direct PostgreSQL access via REST, typed queries |
| **Local Storage** | AsyncStorage | Simple key-value for attendance/ratings (MVP) |
| **Styling** | NativeWind v4 (Tailwind for RN) | Utility-first, matches "Dark Matter" design system, fast iteration |
| **Charts** | Victory Native | Charting library optimized for React Native (post-MVP dashboard) |
| **Maps** | react-native-maps | Google/Apple Maps integration (post-MVP venue map) |
| **Icons** | @expo/vector-icons | Bundled with Expo, comprehensive icon sets |
| **Fonts** | expo-font + Barlow | Custom font loading per design spec |

### Why These Choices

- **Expo managed workflow**: No native code to maintain. `expo build` handles iOS/Android. Solo dev-friendly.
- **Expo Router**: File-based routing means less boilerplate. Tab navigation + stack navigation out of the box.
- **React Query over Redux**: Red Mosquito is read-heavy with server state. React Query handles caching, refetching, and pagination better than Redux for this use case.
- **NativeWind**: Translates Tailwind classes to React Native styles. Faster than StyleSheet for iteration. Easy to implement the Dark Matter color palette.
- **AsyncStorage for MVP**: No auth means no server-side user data. Local storage is sufficient for attendance tracking in week 1.

---

## 3. Database Schema

### Existing Tables (Supabase — already populated)

```sql
-- 1,127 rows
CREATE TABLE shows (
  id TEXT PRIMARY KEY,
  date DATE NOT NULL,
  venue_id TEXT REFERENCES venues(id),
  tour_name TEXT,                -- e.g., "Yield Tour - European" (100% populated)
  show_notes TEXT[],             -- Postgres text array of fan-written notes
  special_status TEXT,           -- festival, benefit, mookie_blaylock, tv, promo, tv_rehearsal
  event_name TEXT,               -- Named events: Lollapalooza, Bridge School Benefit, SNL, etc.
  poster_image_url TEXT,
  total_songs INTEGER DEFAULT 0,
  setlist_status TEXT NOT NULL DEFAULT 'complete',  -- complete, partial, unknown
  source_url TEXT,
  source_pjcc_url TEXT,
  source_lf_url TEXT,
  last_updated TIMESTAMPTZ
);

-- 627 rows
CREATE TABLE songs (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  album_id TEXT REFERENCES albums(id),
  is_original BOOLEAN DEFAULT true,
  cover_artist TEXT,             -- Currently "Cover" for all covers (actual artist TBD)
  first_played DATE,
  last_played DATE,
  total_plays INTEGER DEFAULT 0, -- 530 songs with plays, 97 with 0
  total_tags INTEGER DEFAULT 0,  -- Count of tag appearances across setlist entries
  rarity_score NUMERIC(3,2),     -- 0.00 to 1.00 (currently nulled, awaiting new formula)
  rarity_tier TEXT,              -- ultra-rare, rare, uncommon, common, staple (currently nulled)
  classification TEXT,
  lyrics TEXT,
  source_url TEXT,
  last_updated TIMESTAMPTZ,
  canonical_song_id TEXT         -- Points to primary song when duplicates exist
);

-- 722 rows
CREATE TABLE venues (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  city TEXT,
  state TEXT,
  country TEXT,
  latitude NUMERIC,
  longitude NUMERIC,
  capacity INTEGER,
  indoor BOOLEAN,
  last_updated TIMESTAMPTZ,
  canonical_venue_id TEXT        -- Points to primary venue for stats aggregation
);

-- 24,359 rows
CREATE TABLE setlist_entries (
  id SERIAL PRIMARY KEY,
  show_id TEXT REFERENCES shows(id),
  song_id TEXT REFERENCES songs(id),
  set_name TEXT,                 -- 'Set 1', 'Set 2', 'Encore', 'Encore 2', etc.
  position INTEGER,
  is_debut BOOLEAN DEFAULT false,
  tags TEXT,                     -- JSON array of tag strings (e.g., performance notes)
  segue_into_next BOOLEAN DEFAULT false  -- Not yet populated
);

-- 14 rows
CREATE TABLE albums (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  release_date DATE,
  track_count INTEGER,
  album_type TEXT,               -- studio, compilation, live, reissue
  cover_image_url TEXT,
  sort_order INTEGER,
  streaming_links JSONB,
  purchase_links JSONB,
  credits JSONB,
  artwork_credits JSONB
);

-- Empty (future use)
CREATE TABLE achievements (
  -- To be designed when trophy system is implemented
);
```

### Key Indexes (recommended)

```sql
CREATE INDEX idx_shows_date ON shows(date DESC);
CREATE INDEX idx_shows_venue_id ON shows(venue_id);
CREATE INDEX idx_setlist_entries_show_id ON setlist_entries(show_id);
CREATE INDEX idx_setlist_entries_song_id ON setlist_entries(song_id);
CREATE INDEX idx_songs_rarity_tier ON songs(rarity_tier);
CREATE INDEX idx_songs_total_plays ON songs(total_plays DESC);
CREATE INDEX idx_songs_name ON songs(name);
```

### Local Storage Schema (AsyncStorage)

```typescript
// Key: "attended_shows"
// Value: JSON object mapping show_id → attendance data
{
  "19767": {
    "attended": true,
    "rating": 4,
    "markedAt": "2026-02-17T10:00:00Z"
  },
  "19768": {
    "attended": true,
    "rating": 5,
    "markedAt": "2026-02-17T10:05:00Z"
  }
}
```

### Common Queries

```sql
-- Shows list (paginated, newest first)
SELECT s.*, v.name as venue_name, v.city, v.state, v.country
FROM shows s
JOIN venues v ON s.venue_id = v.id
ORDER BY s.date DESC
LIMIT 20 OFFSET 0;

-- Shows filtered by year
SELECT s.*, v.name as venue_name, v.city, v.state, v.country
FROM shows s
JOIN venues v ON s.venue_id = v.id
WHERE EXTRACT(YEAR FROM s.date) = 2024
ORDER BY s.date DESC;

-- Show detail with setlist
SELECT se.*, sg.name as song_name, sg.rarity_tier, sg.rarity_score
FROM setlist_entries se
JOIN songs sg ON se.song_id = sg.id
WHERE se.show_id = '19767'
ORDER BY se.position;

-- Songs sorted by rarity (rarest first)
SELECT * FROM songs
ORDER BY rarity_score DESC, name ASC;

-- Song detail with recent performances
SELECT s.id, s.date, v.name as venue_name, v.city, se.set_name, se.position
FROM setlist_entries se
JOIN shows s ON se.show_id = s.id
JOIN venues v ON s.venue_id = v.id
WHERE se.song_id = 'release'
ORDER BY s.date DESC
LIMIT 10;
```

---

## 4. API Architecture

### Supabase Client (Direct Database Access)

No custom API needed for MVP. The Supabase JS client provides typed access to all tables via PostgREST.

```typescript
import { createClient } from '@supabase/supabase-js';

const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL!,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!
);

// Example: Fetch shows with venue info
const { data, error } = await supabase
  .from('shows')
  .select('*, venues(*)')
  .order('date', { ascending: false })
  .range(0, 19);
```

### Row Level Security (RLS)

For MVP (read-only public data), configure RLS to allow anonymous reads:

```sql
-- Allow anyone to read all band data
ALTER TABLE shows ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read access" ON shows FOR SELECT USING (true);

-- Repeat for songs, venues, setlist_entries, albums
```

### Rate Limiting

Supabase free tier allows 500 requests/minute. With React Query caching (5-minute stale time), this is more than sufficient for 100 concurrent users.

### Error Handling Pattern

```typescript
// Centralized error handling in React Query
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: 2,
      staleTime: 5 * 60 * 1000,    // 5 minutes
      gcTime: 30 * 60 * 1000,       // 30 minutes
      refetchOnWindowFocus: false,
    },
  },
});
```

---

## 5. Project Structure

```
red-mosquito-app/
├── app/                          # Expo Router (file-based routing)
│   ├── _layout.tsx               # Root layout (providers, fonts)
│   ├── (tabs)/                   # Tab navigator
│   │   ├── _layout.tsx           # Tab bar configuration
│   │   ├── index.tsx             # Shows Browser (Tab 1)
│   │   ├── songs.tsx             # Songs Explorer (Tab 2)
│   │   └── profile.tsx           # Profile (Tab 3)
│   ├── show/
│   │   └── [id].tsx              # Show Detail screen
│   └── song/
│       └── [id].tsx              # Song Detail screen
├── components/                   # Reusable components
│   ├── ShowCard.tsx              # Show list item
│   ├── SongRow.tsx               # Song list item
│   ├── RarityBadge.tsx           # Color-coded rarity chip
│   ├── SetlistItem.tsx           # Setlist entry row
│   ├── StarRating.tsx            # 1-5 star rating input
│   ├── YearFilter.tsx            # Horizontal year chip scroller
│   ├── SearchBar.tsx             # Reusable search input
│   └── EmptyState.tsx            # Empty/loading states
├── lib/                          # Core utilities
│   ├── supabase.ts               # Supabase client initialization
│   ├── storage.ts                # AsyncStorage helpers (attendance, ratings)
│   ├── types.ts                  # TypeScript type definitions
│   └── constants.ts              # Colors, rarity tiers, config
├── hooks/                        # Custom React hooks
│   ├── useShows.ts               # Shows data fetching
│   ├── useSongs.ts               # Songs data fetching
│   ├── useShowDetail.ts          # Single show + setlist
│   ├── useSongDetail.ts          # Single song + performances
│   ├── useAttendance.ts          # Local attendance state
│   └── useProfile.ts             # Computed profile stats
├── docs/                         # Project documentation
│   ├── prd.md                    # Product Requirements Document
│   └── architecture.md           # This file
├── requirements-input/           # Original requirements (reference)
├── prompts/                      # Project setup prompts (reference)
├── assets/                       # Static assets
│   └── fonts/                    # Barlow font files
├── app.json                      # Expo configuration
├── babel.config.js               # Babel config (NativeWind)
├── tailwind.config.js            # Tailwind/NativeWind config
├── nativewind-env.d.ts           # NativeWind TypeScript declarations
├── tsconfig.json                 # TypeScript configuration
├── package.json                  # Dependencies
├── .env.example                  # Environment variables template
├── .gitignore                    # Git ignore rules
└── global.css                    # Global Tailwind styles
```

---

## 6. Development Setup

### Prerequisites

- Node.js 18+
- npm or yarn
- Expo Go app on iOS/Android device (for testing)
- VS Code with extensions: ESLint, Prettier, Tailwind CSS IntelliSense

### Quick Start

```bash
# Install dependencies
npm install

# Start Expo dev server
npx expo start

# Scan QR code with Expo Go app to test on device
```

### Environment Variables

```bash
# .env.example
EXPO_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

---

## 7. Scaling Considerations

### Current Architecture Handles

- **100-5,000 users**: Supabase free/pro tier, React Query caching
- **24K+ setlist entries**: Paginated queries, no full-table scans
- **Read-heavy workload**: Perfect fit for PostgREST + client-side caching

### What Breaks First at 10x (50,000 users)

| Bottleneck | Symptom | Solution |
|-----------|---------|----------|
| Supabase connection limits | Timeouts during peak | Upgrade to Pro ($25/mo), connection pooling |
| API request volume | Rate limit errors | Increase stale time, prefetch on navigation |
| App store review times | Slow iteration | OTA updates via Expo (bypasses app store for JS changes) |

### Cost Optimization

- Supabase free tier covers 500MB database, 2GB bandwidth — sufficient for year 1
- React Query caching reduces API calls by ~80%
- No server-side compute needed (no custom API, no background jobs for MVP)
- Expo Go for development avoids build costs during iteration

---

## 8. Deployment Strategy

### Development

```bash
npx expo start  # Local dev with Expo Go
```

### Preview Builds (Testing)

```bash
npx eas build --profile preview --platform ios
npx eas build --profile preview --platform android
```

### Production

```bash
npx eas build --profile production --platform ios
npx eas build --profile production --platform android
npx eas submit --platform ios
npx eas submit --platform android
```

### OTA Updates (Post-Launch)

```bash
npx eas update --branch production --message "Bug fix"
```

JavaScript-only changes deploy instantly without app store review.

---

## 9. Design System Implementation

### Dark Matter Theme (NativeWind/Tailwind)

```javascript
// tailwind.config.js colors
colors: {
  background:  '#0A0A0F',   // Deep cosmic black
  card:        '#1A1A2E',   // Dark navy
  elevated:    '#141420',   // Charcoal
  primary:     '#E88F3D',   // Amber (PJ shop orange)
  'primary-light': '#F5A855',
  secondary:   '#367FA9',   // PJ Blue
  success:     '#34D399',   // Emerald
  error:       '#EF4444',   // Red
  warning:     '#F59E0B',   // Amber
  'text-primary':   '#FFFFFF',
  'text-secondary': '#B0B0C0',
  'text-muted':     '#6B6B80',
  border:      '#2A2A3E',
}
```

### Rarity Tier Colors

```typescript
const RARITY_COLORS = {
  'ultra-rare': '#E040FB',  // Electric Magenta
  'rare':       '#EF4444',  // Bright Red
  'uncommon':   '#F59E0B',  // Amber
  'common':     '#34D399',  // Emerald Teal
  'staple':     '#9CA3AF',  // Cool Grey
};
```

### Typography (Barlow)

```javascript
// Font loading in _layout.tsx
const [fontsLoaded] = useFonts({
  'Barlow-Regular':  require('../assets/fonts/Barlow-Regular.ttf'),
  'Barlow-Medium':   require('../assets/fonts/Barlow-Medium.ttf'),
  'Barlow-SemiBold': require('../assets/fonts/Barlow-SemiBold.ttf'),
  'Barlow-Bold':     require('../assets/fonts/Barlow-Bold.ttf'),
});
```
