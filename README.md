# LRF Portal — Participant Landing Page (Design Concept)

This repository contains the Part 2B deliverable for the LRF Portal design task: a single, self-contained HTML/CSS/JS mockup of the **participant landing page** — the screen a Participant sees immediately after logging into the LRF (LUMS Religious Festival) portal.

## What this is

LRF is the flagship annual festival of the LUMS Religious Society (LRS), an inter-collegiate Islamic festival bringing together delegates from universities across Pakistan for competitions spanning Islamic scholarship, ethical entrepreneurship, recitation, calligraphy, debate, and content creation.

This project is **not** the full-stack LRF portal (participant registration, director dashboards, EC/Convening Council admin panel, backend, database). It is a high-fidelity front-end concept for the one screen specified in the task brief: the participant home screen, designed to immediately orient a logged-in participant, communicate the spirit of the festival, and guide them toward registering for events, tracking bookings, and staying updated via announcements.

The reasoning behind the system as a whole (data entities, authentication, role assignment, what each of the three roles — Participant, Director, EC + Convening Council — sees first, and a full discovery-to-registration user flow) and the design rationale behind this specific page are written up in [`ANSWERS.md`](./ANSWERS.md).

## What's in this repo

```
lrf-portal/
├── index.html      # The participant landing page (self-contained: HTML + CSS + JS in one file)
├── ANSWERS.md       # Part 1 (What is LRF), Part 2A (system thinking), Part 2B (design concept writeup)
└── README.md        # This file
```

## How to view it

No build step, no dependencies, no server required.

1. Clone or download this repository.
2. Open `index.html` directly in any modern browser (double-click it, or drag it into a browser window).

Optionally, you can serve it locally:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

Or view it live via GitHub Pages if enabled on this repository (Settings → Pages → deploy from the `main` branch).

## Design summary

- **Palette:** deep teal/emerald (mosque tilework, the "Faith" pillar) + warm gold/amber accent (celebratory, competitive energy) on a parchment/off-white base.
- **Typography:** Playfair Display (serif, headings — nods to the festival's calligraphy category) paired with Poppins (sans-serif, body/UI) for legibility.
- **Motifs:** repeating eight-point-star geometric patterns used sparingly as texture, echoing Islamic sacred geometry and the Tajalli-e-Khat-o-Rang (calligraphy & sacred art) category.
- **Page structure:** sticky header with role-aware profile chip → hero greeting with live countdown → at-a-glance festival stats → open-for-registration event catalog with filters → "My Bookings" journey tracker → announcements feed → closing "spirit of LRF" section → footer.
- **Interactivity included:** nav active states, event-category filter chips, a mobile nav toggle, and a live JS countdown to the festival start date — enough to demonstrate the page is a working UI, not a static poster.

The content (event names, delegate counts, dates) is illustrative placeholder data modeled on real LRF categories and past editions, since this task scopes only the front-end design of one screen, not a live backend.
