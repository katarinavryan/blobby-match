# Blobby Match 🧸

**A squishmallow friendship matcher — powered by metadata and AI.**

My son has two blobbies. He has strong opinions about which squishmallows should be friends. He also, it turns out, invented a product.

This app lets him (and anyone else) figure out which squishmallows belong together — and why. It also happens to be a working demonstration of attribute-based metadata matching: the same logic that powers content discovery engines at scale.

---

## What is a blobby?

Not all squishmallows are blobbies. A blobby is a squishmallow that is **20 inches or larger** — big enough to hug properly, soft enough to be genuinely comforting, substantial enough to feel like a presence in a room.

The distinction matters for matching. A blobby isn't a toy you put on a shelf. It's a companion.

---

## The problem it solves

Squishmallows come with names, personalities, and little backstory cards. But the question my son kept asking was more specific: *who should go for a walk together? who cooks for whom? whose colours match for a beach day?*

There's no system for this. The data lives on the tag. The matching lives in a child's imagination. This bridges the gap — and in doing so, demonstrates something genuinely interesting about how structured metadata enables intelligent recommendations.

---

## How it works

1. Each blobby gets a profile: name, species, colour, personality traits, interests, and a fun fact
2. Users enter a natural language query — "who should be my blobby's best friend?", "which two would make the best cooking duo?", "who goes on a rainy day walk?"
3. The AI reads the structured attribute set across the collection and returns a match with a reason
4. Users can save matches, build their collection, and add custom blobbies over time

---

## The metadata model — this is the interesting part

Each blobby is a structured JSON object. The schema is designed to support multi-attribute reasoning — not just keyword matching, but genuine compatibility scoring across overlapping and complementary traits.

```json
{
  "id": "kellytoy-001",
  "name": "Cam",
  "species": "Cat",
  "colour": ["grey", "white"],
  "size_inches": [20],
  "squad": "Meadow Squad",
  "official_bio": "Cam loves to curl up with a good book and a cup of hot cocoa.",
  "personality": ["calm", "bookish", "cosy", "introverted", "gentle"],
  "interests": ["reading", "hot chocolate", "napping", "rainy days", "libraries"],
  "fun_fact": "Has a secret collection of bookmarks — one for every book they've ever read.",
  "best_for": ["quiet activities", "cosy indoor days", "reading companions"]
}
```

### Why this schema works

The matching logic uses four fields as primary signals:

| Field | Role in matching | Why it matters |
|-------|-----------------|----------------|
| `personality` | Core compatibility | Shared or complementary traits drive friendship matches |
| `interests` | Activity matching | Overlap predicts what they'd do together |
| `best_for` | Context filtering | Narrows matches by scenario ("rainy day", "beach trip") |
| `colour` | Aesthetic pairing | Used for colour-harmony queries |

The `official_bio` and `fun_fact` fields feed the AI's reasoning output — giving it flavour text to make the match *feel* right, not just score correctly.

This is the same architecture I built at Paramount for content discovery: fixed attributes (genre, format, duration) layered with behavioural signals and editorial metadata, used to surface the right content to the right viewer at the right time. The metadata schema for a blobby and a TV show are more similar than you'd think.

---

## Evals — how we knew the matches were working

Before shipping anything, we needed to know if the AI's matches were actually good — not just technically correct, but *right* in the way that matters.

**My son was the original eval tool.**

He has two blobbies. He knows their personalities. He has opinions. After the first 10 AI-generated matches, I read each one out to him and asked: does that make sense? would they actually be friends?

This is, functionally, human evaluation of AI output — the same process used to train and validate recommendation systems at scale. The difference is that in this case, the evaluator was seven years old, completely unbiased, and willing to tell me immediately when the AI was wrong.

His feedback shaped the prompt engineering and the weight given to different metadata fields. The matches got better. He noticed.

**What this demonstrated:**
- A structured metadata schema makes AI reasoning more reliable and auditable
- Human evals at early stages catch failures that automated scoring misses
- The "right" answer in a recommendation system is always defined by the person using it — not by the algorithm

---

## What's under the hood

- **Frontend:** Built with [Lovable](https://lovable.dev) — AI-generated frontend from a structured brief
- **Matching logic:** Claude API (Anthropic) — multi-attribute reasoning across structured JSON profiles
- **Data store:** JSON — each blobby is a structured object with fixed and flexible fields
- **No account required** — profiles stored locally, shareable via link

---

## Status

✅ Core matching logic working
✅ Profile builder with custom attributes
✅ Multi-blobby comparison
🔄 Shareable profile cards (in progress)
📋 Planned: collection view, friendship history, colour-matching for outfits

---

## What I'd build next

- **A companion version for elderly people living alone** — same metadata matching logic, applied to pairing a person's personality and interests with the right blobby. The comfort use case is real; the matching problem is identical.
- **A teacher version** — matching reading buddies or project partners by learning style and interests
- **An enterprise version** — team pairing for onboarding, using the same attribute-matching logic against role profiles and working styles
- **Export to physical card format** — so the card that comes with the blobby can be updated with their friendship history

---

## Built by

Katarina Ryan — Senior Product Leader with 15+ years building content platforms, discovery systems, and metadata architecture at scale. The idea was my son's. The metadata schema was mine. The evals were collaborative.

[LinkedIn](https://linkedin.com/in/katarinaryan) · [Portfolio](https://newstorystudio.com)
