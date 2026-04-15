# Blobby Match 🧸

**A squishmallow friendship matcher — powered by metadata and AI.**

🔗 **[Try it live](https://genre-shade-73178097.figma.site/)**

My 8 year old son asked me how does AI learn — so I created a prototype product that allows him to teach it. 
He loves Squishmallows and calls them blobbies (but only if they are over a certain size — there is the first metadata setting) and has two of them. He also has strong opinions about which Squishmallows should be friends.

This app lets him (and anyone else) figure out which Squishmallows belong together — and why. It is also a working demonstration of attribute-based metadata matching: the same logic that powers content discovery engines at scale — and an eval tool that can be used by kids to teach AI.

---

## What is a blobby?

Not all Squishmallows are blobbies. A blobby has to be at least 16 inches and soft — I did not make these rules, my son and his friends did.

---

## The problem it solves

Squishmallows come with names, personalities, and little backstory cards. We came up with a list of questions: *who should go for a walk together? who cooks for whom? whose colours match for a beach day?*- and asked the AI to answer them and then give it feedback if it got it right. 

This way the player teaches the AI to get it closer to the 'right' answer. 

---

## How it works

1. Each blobby gets a profile: name, species, colour, personality traits, interests, and a fun fact
2. The user asks a natural language question — "who should be my blobby's best friend?", "which two would make the best cooking duo?", "who would have the best rainy day?"
3. The AI reads the structured attribute set across the collection and returns a match with a reason
4. The user gives feedback — 👍 or 👎 — and the AI learns from it

That last step is the point of the app. My son isn't just using it — he's teaching it.

---

## The eval loop — this is the interesting part

Before the thumbs up/down feedback feature existed, my son was already the eval tool.

He has two blobbies. He knows their personalities. He has opinions. After the first AI-generated matches, I read each one out to him and asked: does that make sense? Would they actually be friends?

This is, functionally, human evaluation of AI output — the same process used to train and validate recommendation systems at scale. The difference is that in this case, the evaluator was 8 years old, completely unbiased, and willing to tell me immediately when the AI was wrong.

His feedback shaped the prompt engineering and the weight given to different metadata fields. The matches got better. He noticed.

The thumbs up/down buttons in the app make this loop explicit. Each piece of feedback is stored and passed back into the next match request — so the AI isn't just matching based on the data, it's matching based on what *this user* has told it is right. That's personalisation. That's how recommendation systems actually work.

**What this demonstrated:**
- A structured metadata schema makes AI reasoning more reliable and auditable
- Human evals at early stages catch failures that automated scoring misses
- The "right" answer in a recommendation system is always defined by the person using it — not by the algorithm
- You can explain all of this to an 8 year old with a thumbs up button

---

## The metadata model

Each blobby is a structured JSON object. The schema supports multi-attribute reasoning — not just keyword matching, but genuine compatibility scoring across overlapping and complementary traits.

```json
{
  "id": "kellytoy-001",
  "name": "Cam",
  "species": "Cat",
  "colour": ["grey", "white"],
  "size_inches": [20],
  "squad": "Meadow Squad",
  "personality": ["calm", "bookish", "cosy", "introverted", "gentle"],
  "interests": ["reading", "hot chocolate", "napping", "rainy days", "libraries"],
  "fun_fact": "Has a secret collection of bookmarks — one for every book they've ever read.",
  "best_for": ["quiet activities", "cosy indoor days", "reading companions"]
}
```

| Field | Role in matching | Why it matters |
|-------|-----------------|----------------|
| `personality` | Core compatibility | Shared or complementary traits drive friendship matches |
| `interests` | Activity matching | Overlap predicts what they'd do together |
| `best_for` | Context filtering | Narrows matches by scenario ("rainy day", "beach trip") |
| `colour` | Aesthetic pairing | Used for colour-harmony queries |

This is the same architecture I used in real jobs in real environments for content discovery: fixed attributes (genre, format, duration) layered with behavioural signals and editorial metadata, used to surface the right content to the right viewer at the right time. The metadata schema for a blobby and a TV show are more similar than you'd think.

---

## What's under the hood

- **Frontend:** Built with Figma Make — AI-assisted frontend from a structured brief
- **Matching logic:** Claude API (Anthropic) — multi-attribute reasoning across structured JSON profiles
- **Feedback loop:** Thumbs up/down stored in localStorage, passed back into each subsequent match request
- **Data store:** JSON — each blobby is a structured object with fixed and flexible fields
- **No account required** — everything runs in the browser

> **Image credit:** Squishmallow images sourced from the [Squishmallow fandom wiki](https://squishmallowsquad.fandom.com). All characters and images remain the property of Kellytoys. This is a prototype for portfolio and educational purposes only.

---

## Status

✅ Core matching logic working  
✅ Thumbs up/down feedback loop — stored and passed back into next match request  
✅ 30 blobby profiles with full metadata  
✅ Demo mode — works without an API key, rotates through preset matches  
📋 Planned: collection view, friendship history, colour-matching for outfits  
📋 Planned: user-added blobbies with custom profiles  

---

## What I'd build next

- **A teacher version** — matching reading buddies or project partners by learning style and interests, with the same thumbs up/down eval loop
- **An enterprise version** — team pairing for onboarding, using the same attribute-matching logic against role profiles and working styles
- **A companion version for elderly people living alone** — same matching logic, applied to pairing a person's interests and personality with the right companion. The comfort use case is real; the matching problem is identical.

---

## Built by

Katarina Ryan — Senior Product Leader with 15+ years building content platforms, discovery systems, and metadata architecture at scale. The idea was my son's. The metadata schema was mine. The evals were collaborative.

[LinkedIn](https://www.linkedin.com/in/katarina-ryan-56961314/) 
