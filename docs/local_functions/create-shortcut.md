# Create Shortcut

This document describes the **Create Shortcut**.

Unlike other features, shortcuts are **entirely client-driven** and do **not** rely on Cloud Functions or AI interaction.
They are stored directly under the user document in Firestore and are used to open the map with preconfigured filters.

Related schema: [Accounts Database Schema](../database/account.md).  
Shortcuts are returned in directly from firestore.


---

## Feature Overview

Shortcuts allow users to:

- Save reusable venue discovery filters
- Instantly open the map in a known state
- Bypass AI intent parsing
- Query Firestore directly using deterministic filters

A shortcut is best thought of as a **named map preset**.

---

## Storage Location

Shortcuts are stored per-user.

### Firestore Path

```
users/{uid}/shortcuts/{shortcutId}
```

- `uid` is the Firebase Authentication user ID
- `shortcutId` is auto-generated
- All shortcuts are private to the user

---

## Data Model

```ts
{
  name: string;

  filters: {
    types?: string[];
    amenities?: string[];
    tags?: string[];

    openNow?: boolean;
    lateNight?: boolean;

    radiusKm?: number;
  };

  createdAt: Timestamp;
}
```

---

## Example Document

**Path**
```
users/iHDTUHXP1NDqmsVb6ZoIT1jk003/shortcuts/2w7ZCNGhBpUskaXRPhwH
```

**Document**
```json
{
  "name": "Play Pool",
  "filters": {
    "amenities": ["Pool table"],
    "openNow": false,
    "lateNight": false,
    "radiusKm": 10
  },
  "createdAt": "2025-12-22T09:44:14+11:00"
}
```

---

## User Flow (UI)

The shortcut creation flow is implemented as a **multi-step modal builder**.

### Steps

1. Name — required
2. Types — optional
3. Amenities — optional
4. Tags — optional
5. Extras — open now, late night, radius

---

## Save & Usage

Shortcuts are saved directly to Firestore and later read by the map screen to execute
deterministic venue queries without invoking AI or Cloud Functions.

---
