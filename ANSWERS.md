# LRF Portal — Written Answers

## Part 1: What is LRF?

The LUMS Religious Festival (LRF) is the flagship annual event of the LUMS Religious Society (LRS), and it bills itself as Pakistan's premier inter-collegiate Islamic festival. Where LRS runs the year-round program at LUMS — halaqas, Tajweed and Seerah classes, Quranic study circles, scholarly lectures, and the large Ramadan Sehr-o-Iftaar gatherings — LRF is the moment each year when that work opens outward to the rest of the country. It is a multi-day, campus-wide competition and gathering that pulls in delegations from universities across Pakistan rather than staying an internal LUMS activity.

By its third iteration (LRF 3.0, held January 30 to February 1, 2026), the festival had grown into an eight-category competition spanning research and essay writing (Sareer-e-Khama), ethical entrepreneurship pitching with investor mentorship (Tajdeed-e-Tijarat), photography and digital storytelling (Tasveer-e-Tajalli), a fast-paced Islamic knowledge quiz (Fehm-e-Islam), Islamic calligraphy and sacred geometry (Tajalli-e-Khat-o-Rang), an MUN-style debate simulating OIC diplomacy (Hujjat-e-Haq), Quranic recitation and Naat (Mizmaar-e-Dawood), and a grand finale for finalists (Clash of Champions). Earlier iterations (LRF 1.0 and 2.0) followed a similar template, refining the category list and scale each year — LRF 2.0, for instance, drew over 300 delegates from top institutions nationwide.

LRF matters to LUMS in three ways. First, it is one of the largest student-organized events on campus, giving LUMS visible standing as a hub for Islamic scholarship and youth leadership in Pakistani higher education — the kind of reputational and community-building role that few student societies achieve at this scale. Second, it operationalizes LRS's stated pillars — Faith, Awareness, Integrity — by turning abstract values into a concrete, judged, competitive experience: participants are not just listening to a lecture, they are researching, pitching, reciting, and debating. Third, it is explicitly a talent and leadership pipeline: winners and top finalists return for "Clash of Champions," social evenings pair competition with reflection and guest speakers, and official study guides are distributed ahead of time so participation is a genuine intellectual and spiritual exercise, not just a logistics exercise.

In short, LRF is LRS's outward-facing, competitive, multi-day festival that brings the religious society's year-round mission to a national stage — combining scholarship, entrepreneurship, the arts, debate, and recitation under one roof, for one weekend, at LUMS.

## Part 2A: System Thinking

### Core data entities

- **User** — a single login identity shared across all three roles. Fields: user ID, name, email (often institutional), phone, password hash / auth provider ID, role, status (active/suspended), created-at.
- **Role assignment** — a join between User and a Role (Participant, Director, EC/Convening Council), plus, for Directors, the specific Event(s) they are scoped to. This is separate from User because a person's role can change between festival cycles (e.g., a past participant becomes a Director the following year) without losing their history.
- **Event** — a single competitive category or session (e.g., Fehm-e-Islam, Tajdeed-e-Tijarat). Fields: title, description, category type (quiz/pitch/recitation/etc.), format (individual/team), date/time, venue, capacity, registration deadline, status (draft/published/closed/completed), owning Director(s), study-guide attachment.
- **Registration / Booking** — links a Participant (or a Team) to an Event. Fields: registration ID, participant ID, event ID, team ID (nullable), timestamp, status (pending/confirmed/waitlisted/cancelled), payment/accommodation add-ons if any.
- **Team** — for team-based categories, groups multiple Participants plus a team name and an institution/university field, since delegates come from outside LUMS.
- **Announcement** — a message published by a Director or EC member, scoped either to one Event's registrants, all participants, or the whole platform. Fields: title, body, scope, publish time, author.
- **Institution / Delegation** — since LRF is inter-collegiate, participants are grouped by home university, which matters for both registration (delegation-level accommodation bookings) and analytics (which institutions are participating).
- **Analytics snapshot / Audit log** — derived, read-only views the EC + Convening Council use: registrations per event over time, capacity utilization, delegation counts, Director activity. Not user-editable.

### Authentication

All three roles authenticate through one shared login (email/password, with LUMS students able to use their institutional email; external delegates from other universities register with their own institutional or personal email and get verified before their registration is confirmed). Authentication is a single mechanism — the platform does not have three separate login systems — because the interesting complexity here is authorization (what you can do), not authentication (who you are).

### Role assignment

Every account starts as a Participant by default; this is the "public" role anyone can self-register into. Director access is granted, not self-selected — an existing EC/Convening Council member promotes a user to Director and scopes them to one or more specific events (a Director for Fehm-e-Islam does not automatically manage Tajdeed-e-Tijarat). EC + Convening Council is the smallest, highest-trust tier and is assigned only by an existing EC member (or seeded manually during setup) since it grants global settings and the power to create other Directors. This creates a simple three-tier trust ladder: self-service → peer-promoted → admin-promoted, and it means the system needs exactly one extra table (role assignment, scoped by event where relevant) rather than three parallel user systems.

### What each role sees first (post-login landing)

- **Participant** → the festival landing/home screen: what's happening now, upcoming events they can register for, and their own bookings/announcements. This is the screen designed in Part 2B.
- **Director** → an event management dashboard scoped to only the event(s) they own: registration counts, a participant/team list, and a shortcut to publish an announcement to their registrants.
- **EC + Convening Council** → a platform-wide control panel: aggregate analytics across all events (registrations, capacity, delegation breakdown), a Director-management screen (promote/demote, assign to events), and global settings (festival dates, registration windows, category list).

### One complete user flow: discovery → registration

1. A student at another university hears about LRF (via Instagram, a university flyer, or word of mouth) and visits the portal's public marketing page, which lists this year's categories and dates without requiring login.
2. They create an account (sign-up: name, email, phone, home institution) and verify their email; on first login they land as a Participant on the festival home screen.
3. They browse the event catalog, filter by category type (e.g., "individual" vs "team"), and open Fehm-e-Islam to read its description, format, and study guide.
4. They click Register. If the category is team-based, they either create a new team (naming it, setting their institution) and invite teammates by email, or join an existing team via an invite code; if individual, registration is immediate.
5. The system checks the event's registration deadline and capacity; if capacity is full, the participant is placed on a waitlist instead of confirmed, and told so immediately.
6. On successful registration, the participant receives a confirmation (in-app and email) and the event now appears in their "My Bookings" list; any announcements the Fehm-e-Islam Director later publishes (schedule changes, room assignment, study material updates) automatically reach this participant.
7. As the festival date approaches, the participant returns to the same landing screen, which now foregrounds "your upcoming events" and any new announcements rather than the general catalog — the same screen adapts to where they are in the journey.

## Part 2B: Design Concept — Participant Landing Page

### Visual tone, theme, and design language

The landing page should feel like the moment you walk under LRF's entrance banner: warm, dignified, and quietly energetic rather than loud or corporate. The design language draws on Islamic geometric pattern work — repeating eight-point stars and interlocking line motifs — used sparingly as background texture and section dividers, never as heavy ornamentation that competes with content. The palette pairs a deep teal/emerald (evoking mosque tilework and LRS's "Faith" pillar) with a warm gold/amber accent (used for calls to action and highlights, evoking the festival's competitive, celebratory energy), on an off-white/parchment base so the page reads as calm and legible rather than dark or somber. Typography pairs a confident serif or calligraphic-influenced display face for headings (nodding to the festival's calligraphy category and Islamic art heritage) with a clean, highly legible sans-serif for body text and UI elements, so the page stays functional as a piece of software, not just a poster.

### Sections and elements

1. **Header bar** — LRF wordmark/logo, a slim geometric pattern rule beneath it, and a persistent nav (Home, Events, My Bookings, Announcements) plus the participant's avatar/name in the corner, so identity and orientation are immediate.
2. **Hero band** — a short, warm greeting ("Assalamu Alaikum, [Name]") with the current festival edition name and dates, a countdown or "Day 1 of 3" status if the festival is live, and one primary call-to-action button ("Explore Events" or "Continue Registration") in the gold accent — this answers "where am I and what should I do next" in the first three seconds.
3. **This year at a glance** — a compact strip of stats (delegations attending, categories open, days remaining) that communicates scale and excitement without requiring the participant to click anywhere; this is where the "over 300 delegates" energy lives.
4. **Featured / open-for-registration events** — a horizontally scrollable or grid set of event cards (Fehm-e-Islam, Tajdeed-e-Tijarat, Mizmaar-e-Dawood, etc.), each with its category icon, a one-line description, registration deadline, and a status pill (Open / Closing Soon / Full), letting the participant browse and register directly from the home screen rather than hunting through a menu.
5. **My bookings / your journey** — once a participant has registered for at least one event, a dedicated card shows their confirmed events, team info if applicable, and any pending actions (e.g., "invite teammates" or "upload your submission"); before any registration, this section instead gently prompts them to get started.
6. **Announcements feed** — a short, timestamped list of the latest updates relevant to the participant (global festival announcements plus anything from events they've joined), so nothing important is missed without needing a separate inbox.
7. **Spirit-of-LRF strip** — a closing visual section using a photo or pattern band with a short line drawn from LRS's own language ("Faith • Awareness • Integrity") and a nod to the social evenings and community-building side of the festival, so the page ends by reaffirming *why* LRF exists, not just what to click next.
8. **Footer** — contact/helpline info, LRS's social links, and quick links (About LRS, Study Guides, Support).

### How this reflects LRF's identity

Every element is chosen to answer the brief's three requirements at once. It orients the participant instantly (header + hero make the "where am I" question unmissable). It makes them feel the spirit of LRF specifically because the visual language borrows from the festival's own competitive categories — the geometric patterning nods to Tajalli-e-Khat-o-Rang (calligraphy/sacred geometry), the warm greeting and social-evening strip nod to Mizmaar-e-Dawood and the community/spiritual side, and the stats strip nods to the inter-collegiate scale that makes LRF distinct from an ordinary campus event. And it guides them naturally toward action, because the page is structured as a funnel — greeting, scale, browse, register, track — rather than a static poster, which is exactly what a participant needs from a portal landing page as opposed to a marketing flyer.
