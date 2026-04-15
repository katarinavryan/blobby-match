# Blobby Match — Data Schema

This document defines the data structures used by Blobby Match. It exists so that anyone building the frontend (in Loveable or elsewhere) knows exactly what shape the data takes and what the API returns.

---

## Squishmallow Profile Object

Each squishmallow in the collection is represented as a JSON object with the following fields.

```json
{
  "id": "kellytoy-001",
  "name": "Cam",
  "species": "Cat",
  "colour": ["grey", "white"],
  "size_inches": [8, 12, 16, 20],
  "squad": "Meadow Squad",
  "official_bio": "Cam loves to curl up with a good book and a cup of hot cocoa.",
  "personality": ["calm", "bookish", "cosy", "introverted", "gentle"],
  "interests": ["reading", "hot chocolate", "napping", "rainy days", "libraries"],
  "fun_fact": "Has a secret collection of bookmarks — one for every book they've ever read.",
  "best_for": ["quiet activities", "cosy indoor days", "reading companions"]
}
```

### Field reference

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | yes | Unique identifier. Format: `kellytoy-NNN` for seed data, `user-[timestamp]` for user-added. |
| `name` | string | yes | The squishmallow's name. |
| `species` | string | yes | What kind of creature/object they are. |
| `colour` | string[] | yes | Array of colours. Used for colour-matching queries and UI display. |
| `size_inches` | number[] | no | Available sizes. Not used for matching logic. |
| `squad` | string | no | Official squad group. Defaults to "My Squad" for user-added. |
| `official_bio` | string | no | Short official description. Used as flavour text in the UI. |
| `personality` | string[] | yes | Core personality traits. Primary matching signal. |
| `interests` | string[] | yes | Things the squishmallow enjoys. Primary matching signal. |
| `fun_fact` | string | no | A single fun fact. Displayed in profile cards. |
| `best_for` | string[] | no | Suggested use cases or activity types. |

---

## Match Result Object

Returned by the `findMatch()` function after calling the Claude API.

```json
{
  "query_type": "pair",
  "matches": [
    { "name": "Cam", "species": "Cat" },
    { "name": "Hoot", "species": "Owl" }
  ],
  "reason": "Cam and Hoot are both quiet and love learning — Cam through books, Hoot through deep conversations. They'd spend a perfect rainy afternoon together, Cam reading while Hoot asks thoughtful questions about every chapter.",
  "activity_suggestion": "A cosy reading afternoon with tea and biscuits, trading book recommendations."
}
```

### Field reference

| Field | Type | Notes |
|-------|------|-------|
| `query_type` | "pair" \| "single" \| "group" | How many squishmallows the answer involves. |
| `matches` | Array<{ name, species }> | The matched squishmallow(s). Name and species only — look up full profile from collection by name. |
| `reason` | string | 2–3 sentence warm explanation, written for a child to enjoy. |
| `activity_suggestion` | string | A specific thing these squishmallows could do together. |

---

## User Collection

The user's personal collection is stored as a JSON array in browser localStorage under the key `blobby-match-collection`.

```json
[
  { ...squishmallow profile object },
  { ...squishmallow profile object }
]
```

On first load, if no collection exists, the app should offer to import from the seed dataset (`squishmallows-seed.json`) or start fresh.

---

## API Call Shape

The frontend calls the matching logic via a thin backend endpoint (or directly via the Claude API in local/Loveable environments). The request and response shapes are:

### Request

```json
{
  "collection": [ ...array of squishmallow profile objects ],
  "query": "Who should go for a walk together?"
}
```

### Response

```json
{
  "query_type": "pair",
  "matches": [ { "name": "Wendy", "species": "Frog" }, { "name": "Brenda", "species": "Bear" } ],
  "reason": "Wendy loves jumping through puddles and Brenda loves peaceful forest walks — together, they'd turn any woodland path into an adventure.",
  "activity_suggestion": "A morning walk through the woods, ending with honey and lily pad hopping."
}
```

---

## Environment Variables

The matching logic requires one environment variable. **Never commit this to GitHub.**

```
ANTHROPIC_API_KEY=your_key_here
```

For local development: create a `.env` file in the project root. The `.gitignore` should exclude `.env`.

For Loveable: add the API key in the Loveable project settings under Environment Variables.

---

## Squads reference

The seed data uses these official squad names. User-added squishmallows default to "My Squad" but can be assigned any squad.

| Squad | Description |
|-------|-------------|
| Meadow Squad | Gentle, nature-loving characters |
| Pond Squad | Water-loving, outdoor characters |
| Forest Squad | Woodland creatures and earthy characters |
| Arctic Squad | Cold-weather characters |
| Night Squad | Nocturnal and late-night characters |
| Food Squad | Food-shaped characters |
| Farm Squad | Farm animal characters |
| Aquatic Squad | Ocean and underwater characters |
| Easter Squad | Spring and Easter characters |
| Fantasy Squad | Magical and mythical characters |
| Mythical Squad | Legendary creatures |
| Safari Squad | African wildlife characters |
| Winter Squad | Winter and Christmas characters |
| Harvest Squad | Autumn and harvest characters |
| My Squad | Default for user-added characters |

---

## Notes for Loveable frontend build

The Loveable frontend should:

1. **Load the collection** from localStorage on startup. If empty, prompt user to browse the seed data or add their own.
2. **Display squishmallow cards** — show name, species, colour swatches, 2–3 personality tags, and the fun fact.
3. **Accept a free-text query** — a text input with example prompts like "who should be friends?", "who would make a great cooking team?", "match colours for a beach day".
4. **Display the match result** — highlight the matched squishmallows, show the reason in a speech bubble or card, and show the activity suggestion.
5. **Allow adding new squishmallows** — name, species, colour picker, personality tags (selectable from a list + freeform), interests (same), optional fun fact.
6. **Save to localStorage** — all user additions persist in the browser. No account required.

The matching API call should be sent to a `/api/match` endpoint that wraps `matching-logic.js`. In a Loveable prototype, this can be a direct client-side call if an API key is available in the environment — for a demo, that's fine.
