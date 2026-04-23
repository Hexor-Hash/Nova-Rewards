# Pull Request: Nova Rewards — Feature Batch

## Overview

This PR implements four features across the backend and frontend to improve performance, user experience, and developer tooling.

---

## Changes

### #576 — Redis Caching Layer

- `GET /campaigns` and `GET /campaigns/:merchantId` now cache responses with a **60s TTL** via `ioredis`
- Cache is invalidated on campaign creation and on reward issuance (`POST /rewards/distribute`)
- Added `cache_hits_total` and `cache_misses_total` Prometheus counters exposed at `/metrics`
- Integration tests verify cache hit/miss behavior and invalidation

Closes #576

---

### #595 — Redemption Flow UI

- New `RedemptionFlow` component with a 5-step flow:
  1. Campaign selector showing available options with exchange rates
  2. Amount input with real-time balance validation
  3. Confirmation step displaying tokens to burn, perk value, and exchange rate
  4. Freighter signing modal for on-chain burn transaction
  5. Success screen with transaction hash and Stellar Explorer link

Closes #595

---

### #601 — User Profile Page

- New `/profile` page with:
  - Wallet address display with copy-to-clipboard button
  - Profile form: display name, email, avatar upload
  - Notification preferences toggles (email / in-app per event type)
  - Account stats: total rewards earned, total redeemed, member since
  - Toast notifications for success and error feedback

Closes #601

---

### #610 — UI Component Library

- New `components/ui/` library with Storybook stories for all components:
  - **Button** — variants: `primary`, `secondary`, `ghost`, `danger` · sizes: `sm`, `md`, `lg` · loading state
  - **Input** — `text` / `number`, label, error state, helper text, accessible `aria-*` attributes
  - **Select** — label, error state, helper text
  - **Card** — variants: `default`, `elevated`, `bordered`
  - **Badge** — variants: `success`, `warning`, `error`, `info`
  - **Modal** — accessible, focus-managed, Escape-dismissible
  - `index.js` barrel export for all components

Closes #610

---

## Files Changed

| File | Change |
|------|--------|
| `novaRewards/backend/routes/campaigns.js` | Redis caching with 60s TTL |
| `novaRewards/backend/routes/rewards.js` | Cache invalidation on reward issuance |
| `novaRewards/backend/middleware/metricsMiddleware.js` | Cache hit/miss counters |
| `novaRewards/backend/tests/cacheInvalidation.test.js` | New integration tests |
| `novaRewards/frontend/components/RedemptionFlow.js` | New redemption flow component |
| `novaRewards/frontend/pages/profile.js` | New user profile page |
| `novaRewards/frontend/components/ui/` | New UI component library + Storybook stories |
