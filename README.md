# 🎵 Prosperity Music — Canonical D1 Database Schema

Official open-source database schema for **Prosperity Music** running on **Cloudflare D1 (SQLite at the Edge)**.

---

## ⚡ Multi-Tier CDN & Live Sync Architecture

All user personal workers and the mobile Flutter app dynamically fetch and apply this declarative schema at runtime:

```http
# Primary (Live GitHub Raw with Cache-Busting)
GET https://raw.githubusercontent.com/prosperity-music/databaseschema/main/schema.json

# Secondary (Global Edge Multi-CDN - Zero Rate Limit)
GET https://cdn.jsdelivr.net/gh/prosperity-music/databaseschema@main/schema.json
```

---

## 📊 Database Tables (18 Tables)

| # | Table Name | Purpose | Key Features |
|---|------------|---------|--------------|
| 1 | `profiles` | User profile & cloudflare credentials | Unique usernames, avatars, bios, tokens |
| 2 | `canvas_cache` | Spotify live video canvases | Instant 0ms cache for song video backgrounds |
| 3 | `liked_songs` | User liked music tracks | Song metadata, cover art, duration, unique (user_id, song_id) |
| 4 | `liked_albums` | User saved albums / EPs | Album metadata, artwork, added timestamps |
| 5 | `liked_playlists` | User bookmarked public playlists | Playlist title, artwork, owner reference |
| 6 | `playlists` | Custom user playlists | Playlist metadata, public/private toggle |
| 7 | `playlist_tracks` | Tracks inside user playlists | Track positions, metadata, unique constraint |
| 8 | `playlist_collaborators` | Shared / collaborative playlists | Roles ('owner', 'editor', 'viewer') |
| 9 | `listening_history` | Stream play history | Recent songs, played_at timestamps |
| 10 | `podcast_history` | Podcast listen & resume history | Episode positions, progress percentage, completion status |
| 11 | `search_history` | User search queries | Recent searches, timestamp ordering |
| 12 | `user_clips` | Custom song snippets / rings | Start & end millisecond markers |
| 13 | `user_downloads` | Offline downloaded track metadata | Quality (320kbps), duration, timestamps |
| 14 | `user_footprints` | AI personalization behavioral footprint | Listening habits, language affinities JSON |
| 15 | `user_settings` | Cloud synced app preferences | Sync themes, audio quality, UI options JSON |
| 16 | `user_blacklists` | Blocked artists, songs, or genres | Filtered from recommendation feeds |
| 17 | `followers` | Social followers list | Follower username, avatar, and relationship |
| 18 | `following` | Accounts this user follows | Target user profiles, instant social queries |

---

## 🔄 How Auto-Sync Works in App

1. **New User Registration:** When a new user registers their Cloudflare Worker, the app immediately generates and executes all `CREATE TABLE IF NOT EXISTS` and index creation queries derived from `schema.json`.
2. **App Launch Auto-Check:** Every time the mobile app starts, it checks the latest `version` in `schema.json`. If `remoteVersion > localVersion`, the app executes an automated non-blocking D1 schema migration.
3. **Zero Interruption:** Operates non-blockingly without delaying user playback or navigation.
