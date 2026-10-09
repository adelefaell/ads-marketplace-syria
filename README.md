# ads-marketplace-syria


# Syria's used-goods marketplace

This is a mobile app (Android first, iOS supported) where people in Syria buy and sell second-hand goods. It connects sellers and buyers phone to phone, without a middleman's fee.

## The problem

Syrians trade used goods (phones, furniture, appliances, cars, clothing, kids' items) mostly through scattered social-media groups. Those channels have:

- No structure: no categories, filters, price search or location search.
- No trust signals: no verified identity, no way to report a bad seller.
- Listings that never expire, so buyers chase items already sold.
- No tools for local conditions: dual currency (USD and SYP), patchy connectivity, Arabic-first use.

## What we are building

A focused classifieds marketplace with a simple loop: **post in under a minute, find nearby, contact the seller.**

### For buyers

- Browse a feed and a category tree (11 parent categories, 13 sub-categories, Arabic names).
- Search by text with autocomplete, plus filters: category, condition, price range and currency, location, and per-category attributes.
- Sort by newest, price, or nearest.
- Search by place: pick a city or area, use the phone's location, or drop a pin on a map with a radius (whole city, or 5 to 200 km). A results map shows listings as price pins.
- Save listings, save searches, and get notified when a new match appears.
- Reveal the seller's contact number only when you ask. It is never public by default.

### For sellers

- A guided posting flow: photos (camera first), price, category, details, publish.
- Price in **USD or SYP**, exactly as entered, with no conversion.
- The listing stores the city and an approximate point (about 1 km). The exact address is never stored.
- Drafts save on every change, and publishing works offline: it queues and retries.
- Listings live for 30 days, then expire. Sellers can renew them, mark them reserved or sold, or republish.
- A profile with several saved locations and "my listings" management.

### Trust and safety

- **Phone OTP is the only login.** Possession of the phone number is the account. Numbers are validated as Syrian mobile numbers (E.164), and OTP codes are hashed, attempt-limited and single-use.
- Phone numbers are masked everywhere in logs and tools, and never shown publicly.
- Report listings and sellers. A separate **staff moderation panel** (web, with email, password and TOTP) handles reports, with an audit log of every staff action.
- Verified-store badge for trusted shops.
- Deleting an account blocks instant re-registration of the same number for a cooldown period.

## Made for the Syrian context

- **Arabic-first, RTL throughout.** English is supported. The app follows the device language and flips live. A numeral setting offers Western (0-9) or Eastern (٠-٩) digits.
- **Works on weak networks.** It keeps cached catalog data, shows connection state (online, weak, offline), queues publishes and retries.
- **Dual currency** (USD and SYP) with no forced conversion.
- **Privacy by default.** Coarse location only, rounded coordinates, and no exact addresses.
- **No cards and no borders** in the UI. The design is calm, high-contrast and readable. It uses IBM Plex Sans Arabic, with 48 px minimum touch targets and 56 px for primary actions.

## Status (October 2026)

| Area | State |
| --- | --- |
| Auth (phone OTP, first-login profile, account deletion) | Shipped |
| Listings: create, edit, lifecycle, renew, expiry, favorites | Shipped |
| Search (Postgres, with Meilisearch option), autocomplete, facets | Shipped |
| Location picker, map search, radius | Shipped |
| Notifications and push (favorites, saved-search matches, expiry) | Shipped |
| Moderation panel (reports, audit log) | Shipped |
| Chat | UI on mock data (dev builds only); backend not built yet |
| Featured / paid promotion (Sham Cash) | Parked |
| Full Arabic copy rewrite by a native speaker | Pending before production |

## Technical shape

- **App:** Expo SDK 57, React Native 0.86, expo-router, TypeScript, unistyles, React Query, zustand. Native UI goes through wrappers in `src/components/ui`.
- **API:** NestJS 12 and Express 5, better-auth (phone OTP), Drizzle on Postgres 17, Meilisearch for search, Cloudflare R2 for images.
- **Shared:** `packages/shared` holds zod schemas, types and phone helpers used by both app and API.
- **Admin:** `admin/` is the staff moderation web panel (React, Vite, shadcn/ui).
- **Environments:** development, staging (allowlist-gated signup) and production, with boot guards that stop test OTP settings from reaching production.

## What's next

1. Build the chat backend (real-time messaging) and ship Messages to users.
2. Native-speaker Arabic copy pass, including search synonyms and stopwords.
3. Staging validation, then production launch with the OTP delivery provider and the account-deletion cooldown enabled.
4. Later: featured listings, stronger authentication for sensitive actions (passkeys or TOTP), and Redis-backed rate limiting when scaling.
