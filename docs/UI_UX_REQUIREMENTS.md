# UI/UX Requirements — Iltiam (التِئام)

| Item | Value |
|---|---|
| Status | Draft 1.0, ready for design |
| Date | 2026-10-24 |
| Source | UX discovery interview (Arabic, MCQ format), 2026-10-22 to 2026-10-24, on top of the approved PRD ([PRD.md](PRD.md) / [PRD.ar.md](PRD.ar.md)) |
| Scope | Information architecture, navigation, layout, content hierarchy, forms, interactions, UI states, responsive behavior, accessibility, language, visual identity |
| Out of scope | Visual mockups, code, and any feature not in the PRD |

### How to read this document

Each decision is tagged with its source:

| Tag | Meaning |
|---|---|
| **[C]** | **Confirmed** by the product owner during the interview. |
| **[D]** | **Delegated**: the product owner asked the UX researcher to decide using best-practice recommendations ("answer all questions with recommendations"). Can be changed by the designer without product impact. |
| **[T]** | **Tentative**: confirmed, but must be validated in usability testing before it is final. |
| **[PRD]** | Inherited directly from the PRD; IDs (F-, BR-, FR-, NFR-, S-) refer to it. |

This document adds **no new features**. Where a UI element is listed, it exists to serve a PRD requirement, which is cited.

---

## 1. Design Principles

| # | Principle | Why | Source |
|---|---|---|---|
| P1 | **Mobile-first, low-bandwidth-first.** Every screen is designed at 360px width and must meet the 3G budget (NFR-003: first useful screen ≤ 3s, ≤ 1MB excluding user images). | Gaza: phones are the main device, connectivity is weak. | [C] |
| P2 | **Offline is a normal state, not an error.** | Power and network cuts are frequent (G-6). | [C] |
| P3 | **Always show "where am I and what happens next".** Every waiting state shows its stage and remaining time. | The product is full of timers (24h, 72h, 15min, 30min). | [C] |
| P4 | **Money is explicit.** Price, who receives it, and refund rules are visible before any commitment. | Payments happen outside the platform (BR-070). | [D] |
| P5 | **Medical privacy by layout.** Screens for clinic staff are built without any slot for medical content, not just hidden content. | UN-12, §15. | [PRD] |
| P6 | **Calm and warm.** Soft burgundy/pink palette, generous spacing, no aggressive red except for real errors. | Brand direction. | [C] |

---

## 2. Information Architecture

### 2.1 App structure

| Decision | Detail | Source |
|---|---|---|
| **One app for all roles** | Single PWA, single URL, single install. After login, the user lands in a role-specific shell with its own navigation and flows. | [C] |
| **One account per person** | A person with two roles (clinic manager who is also a doctor) uses one account. | [C] |
| **Device strategy** | All role shells are mobile-first and fully responsive. Admin shell is optimized for desktop (still usable on mobile). | [C] |
| **Code-splitting by role** | Partner/admin screens must not be loaded in the patient bundle (protects NFR-003). | [D] |

### 2.2 Role → landing screen

| Role | Lands on | Source |
|---|---|---|
| Visitor | Home (doctor list) | [C] |
| Patient | Home (doctor list) | [C] |
| Doctor (incl. doctor-manager) | Cases | [D] |
| Clinic staff | Today's Patients | [D] |
| Clinic manager (non-doctor) | Clinic Management | [D] |
| Pharmacy | Orders | [D] |
| Admin | Pending Tasks | [C] |
| Partner in "Under Review" / "Rejected" status | Application Status screen (see §6.8) | [D] |

### 2.3 Sitemap

```
Visitor / Patient
├── Home: doctor list ............................ S-01
│   ├── Clinic page .............................. S-02
│   └── Doctor page .............................. S-03
│       ├── New async message .................... S-06
│       ├── Slot picker (live chat / in-person) .. S-07
│       └── Payment instructions + transaction ID  S-08
├── My Consultations ............................. S-09
│   └── Conversation ............................. S-10
├── My Pharmacy (صيدليتي)
│   ├── Prescriptions list ....................... S-11
│   ├── Pharmacy results ......................... S-12
│   └── Order status + pickup code ............... S-13
├── Notifications ................................ S-14
└── My Account: profile + family members ......... S-05
    Auth (login/register/verify/reset) ........... S-04

Doctor
├── Cases (async inbox + live sessions) .......... S-19
│   └── Conversation → Prescription editor ....... S-10, S-20
├── Schedule ..................................... S-17
├── Payments (pending + refund tasks) ............ S-18
├── Notifications ................................ S-14
└── My Account: service settings ................. S-21
    └── Clinic Management (only if also manager) . S-16

Clinic Staff
├── Today's Patients (in-person list, no medical content)
├── Schedule ..................................... S-17
├── Payments ..................................... S-18
├── Notifications ................................ S-14
└── My Account

Clinic Manager (non-doctor)
├── Clinic Management ............................ S-16
├── Schedule ..................................... S-17
├── Payments ..................................... S-18
├── Notifications ................................ S-14
└── My Account
    Registration wizard .......................... S-15

Pharmacy
├── Orders (30-min timer, accept/reject, payment confirm) S-24
├── Pickup (scan / enter code) ................... S-24
├── Inventory (daily update) ..................... S-23
├── Notifications ................................ S-14
└── My Account: settings (hours, pickup window, wallets, "Closed") S-25
    Registration wizard .......................... S-22

Admin (desktop sidebar)
├── Pending Tasks (home)
├── Verification Queue ........................... S-26
├── Medicine List + addition requests ............ S-27
├── Partners & Subscriptions ..................... S-28
└── Overdue Refunds & Suspensions ................ S-29
```

---

## 3. Navigation

### 3.1 Global pattern

| Rule | Detail | Source |
|---|---|---|
| Mobile | Fixed **bottom tab bar**, max 5 tabs, icon + Arabic label always visible (never icon-only). | [C] |
| Desktop (≥ 1024px) | Bottom bar becomes a **right-side** vertical nav (RTL). Same items, same order. | [D] |
| Admin | Right-side sidebar on desktop; collapses to a top menu button on mobile. | [D] |
| Badges | Tabs show a count badge for items needing action (e.g. unanswered cases, pending payments, new orders). | [D] |
| Tab order | Items are listed right-to-left in the order given below (first item = rightmost). | [D] |
| Back navigation | Every non-root screen has a back arrow in the top bar pointing **right** (RTL). Browser/OS back must work identically. | [D] |
| Deep links | Every notification opens the exact target screen (case, order, payment). | [D] |

### 3.2 Bottom tabs per role

| Role | Tabs (right → left) | Source |
|---|---|---|
| Visitor | Home · My Pharmacy · Login | [D] — protected tabs prompt login (BR-020) |
| Patient | Home · My Consultations · My Pharmacy · Notifications · My Account | [C] |
| Doctor | Cases · Schedule · Payments · Notifications · My Account | [C] |
| Clinic staff | Today's Patients · Schedule · Payments · Notifications · My Account | [C] |
| Clinic manager (non-doctor) | Clinic · Schedule · Payments · Notifications · My Account | [D] |
| Pharmacy | Orders · Pickup · Inventory · Notifications · My Account | [C] |
| Admin | Sidebar: Pending Tasks · Verification · Medicine List · Partners & Subscriptions · Overdue Refunds | [C] home / [D] order |

Notes:
- Prescriptions are **not** a doctor tab. They are written and viewed only inside the patient conversation. [C]
- A doctor who is also clinic manager finds **Clinic Management** inside **My Account**. No role switcher. [C]
- Clinic staff's **Today's Patients** lists in-person appointments (name, time, doctor) with "Completed / No-show" actions (BR-062). It contains **no medical content**. [C]

### 3.3 Family members (patient)

| Decision | Detail | Source |
|---|---|---|
| Model | **No global profile switcher.** Lists show all family members together, each item labeled with the member's name. | [T] |
| Filter | Filter chips above lists in My Consultations and My Pharmacy: `All · Me · <member> · …`. | [T] |
| Selection at action time | Every new message, booking, or order starts with **"Who is this for?"** (required, no default pre-selected when the account has family members; defaults to "Me" when there are none). | [T] |
| Validation | Run a usability test comparing this with a global switcher; measure wrong-member submissions. | [T] |

---

## 4. Screen Requirements & Content Hierarchy

Ordering in each table = visual priority, top to bottom.

### 4.1 Patient

**S-01 Home — doctor list** [C]
1. Search field (doctor or clinic name).
2. Filter chips: Area, Service (Message / Live chat / In-person) — FR-020..023.
3. Doctor cards, each showing:
   - Doctor name + clinic name + area.
   - **Service prices** (e.g. "Message: Free · Chat: 30₪ · In-person: 50₪"). Disabled services shown as "Unavailable". [C]
   - **Nearest available slot** (e.g. "Next: today 4:00 PM"). Must be computed server-side and returned with the list to keep payload small. [C]
- List paginates with "Load more" (not infinite scroll), to control data usage. [D]
- No doctor photos required in MVP; if used, lazy-loaded and ≤ 30KB each. [D]

**S-02 Clinic page** [PRD]
Clinic name → address + "Open in maps" link (no embedded map, to save data [D]) → phone → list of doctors (same card as S-01).

**S-03 Doctor page** [C]
1. Header: name, clinic, area.
2. **Three stacked service cards**, each with: name, one-line explanation (e.g. "Reply within 24 hours"), price, and a primary button. A disabled service is greyed with "Unavailable".
   - Message: Free / price / Unavailable (BR-040).
   - Live chat: price + duration (BR-051); button → S-07.
   - In-person: price + "Pay at the clinic" (BR-060); button → S-07.
3. Clinic info + "Open in maps".
- Visitor tapping any service → login/register → returns to the **same doctor page** with the service pre-selected (UF-1 step 2). [PRD]

**S-06 New async message** [PRD]
"Who is this for?" → description text → photo attach (max 5, 10MB each, JPEG/PNG/HEIC, compressed on device — BR-047, NFR-006) → price summary (if paid) → Send.
- Draft is auto-saved locally (NFR-004). [D]
- Photo thumbnails show per-image upload progress and a retry on failure. [D]

**S-07 Slot picker** [PRD]
"Who is this for?" → horizontal day strip (next 14 days [D]) → available time slots as large tappable chips → summary (service, date/time in Asia/Gaza, price, cancellation rule) → Confirm.
- Days with no slots are shown disabled, not hidden. [D]

**S-08 Payment** [C]
**One screen with numbered steps**, state preserved if the user leaves to the wallet app and returns:
1. Choose wallet (PalPay / Jawwal Pay / Bank of Palestine).
2. Copy recipient wallet ID and **exact amount** (each with a "Copy" button).
3. Pay from your wallet app.
4. Come back and enter the **transaction number** → Submit.
- Show who receives the money ("Paid directly to <clinic/pharmacy>", BR-070) and the confirmation deadline (Q-03, once decided). [D]
- After submit → status timeline (§5.2) at stage "Awaiting confirmation".

**S-09 My Consultations** [C]
Filter chips (family) → segmented: Active / Past [D] → items showing member name, doctor, type, status timeline summary, remaining time.

**S-10 Conversation (async / live)** [PRD]
- Top: doctor/patient name, member name, status + countdown (reply deadline, 72h follow-up window, or live session end).
- Messages with per-message state: Sending ⏳ / Sent ✓ / Failed (tap to retry). [D]
- Prescription appears as a distinct **prescription card** in the thread with "Open in My Pharmacy". [D]
- When closed: composer is replaced by "Conversation closed — start a new consultation". (BR-044)
- Live chat: 15-minute waiting state with countdown for both sides (BR-053/054); if the doctor no-shows, the patient sees the two choices "Rebook" / "Refund". [PRD]

**S-11 My Pharmacy — prescriptions** [PRD]
Filter chips (family) → prescription cards: member, doctor, date, item count, status (not ordered / partially ordered / fully ordered). Free-text items marked "Not available in My Pharmacy" (BR-082).

**S-12 Pharmacy results** [PRD]
1. Location bar: "Using your location" or selected area, with "Change". GPS permission is requested only here, with a short explanation before the system prompt; on denial, show the area list immediately (BR-120). [D]
2. Results as a **list** (no map by default, to save data [D]), sorted by distance, pharmacies with **all items first** (BR-121). Each card: name, distance, total price, items available (e.g. "3 of 4"), "Last updated" time (BR-122), "Stale data" tag after 48h (BR-134), pickup window (BR-097).
3. Actions: Order all here / Buy available only / Split across pharmacies (BR-123).

**S-13 Order status + pickup code** [PRD]
Status timeline → after payment: **large pickup code** (short number + QR, max brightness hint), pharmacy name/address, pickup deadline. Must render offline (NFR-005). [PRD]

**S-05 My Account** [PRD]
Profile (name, email, phone — BR-025) → Family members (add/edit) → Notifications settings → Logout. For a doctor-manager, a "Clinic Management" entry appears here. [C]

### 4.2 Doctor

**S-19 Cases** [C]
Sorted by **urgency** (closest deadline first). Each item: member name, type (Message / Live), deadline countdown ("3h left to reply"), unread indicator. Segments: Needs reply / Open follow-up / Upcoming live sessions / Closed. [D]

**S-20 Prescription editor** (opened from inside a conversation only) [C]
Search the medicine list (Arabic or English) → pick form/strength → quantity → usage instructions → add more items → "Add item not in list" (free text, triggers an addition request — BR-082) → review → Issue. Issued prescriptions are read-only; "Cancel and reissue" only while no pharmacy order exists (BR-085).

**S-21 Service settings** (inside My Account) [PRD]
Message mode (Free / Paid + price / Off), live chat price + duration, in-person price.

### 4.3 Clinic (doctor, staff, manager)

**Today's Patients** (staff) [C] — date header, list of today's in-person appointments by time: patient name, doctor, status, buttons "Completed" / "No-show".

**S-17 Schedule** [PRD] — week view on desktop, day view on mobile [D]; slot type shown by color + label (Live chat / In-person, BR-066); removing a booked slot shows a warning that bookings will be cancelled with full refund (BR-068).

**S-18 Payments** [PRD] — two lists: "Awaiting confirmation" (patient name, amount, wallet, transaction number, time; actions **Received** / **Not found**) and "Refunds to send" (amount, patient wallet, action **Refunded**). No medical content.

**S-15 Clinic registration wizard / S-16 Clinic Management** [PRD] — see §6.8.

### 4.4 Pharmacy

**S-24 Orders** [C] — new orders first with a prominent **30-minute countdown** (counted in opening hours, BR-131); order shows items, quantities, prices, member name; actions Accept / Reject (reason required: Not available / Price changed / Other — BR-133); then payment confirmation (Received / Not found).

**Pickup tab** [C] — opens directly to **camera QR scan** with a "Enter code manually" field below; result shows order items and a single **"Mark as delivered"** button. Designed for one-handed use at the counter. [D]

**S-23 Inventory** [PRD] — searchable list of medicine-list items; each row: name (AR + EN), price input, available toggle. Sticky "Confirm today's prices" button that updates the "last updated" time even when nothing changed (BR-130). [D]

**S-25 Settings** (inside My Account) [PRD] — opening hours, pickup window, wallet IDs, and a prominent **"Closed now"** toggle.

### 4.5 Admin (desktop-optimized)

**Pending Tasks (home)** [C] — count cards: verification requests, medicine addition requests, overdue refunds, subscriptions due / in grace period. Each card opens its section.

**S-26..S-29** [PRD] — data tables with search, status filter, and a detail side panel (document viewer for licenses; approve / reject with **required reason**, FR-004). On mobile, tables collapse to cards. [D]

---

## 5. Interaction Patterns

### 5.1 Forms

| Rule | Source |
|---|---|
| One column, label **above** the field, never placeholder-as-label. | [D] |
| Inline validation **on blur**, plus a summary at the top on submit that links to each error. | [D] |
| Required fields marked; optional fields labeled "(optional)". | [D] |
| Input types matched to content: `tel` for phone, `numeric` for transaction ID / price / pickup code, `email` for email. | [D] |
| Phone, email, wallet IDs, transaction numbers and codes are entered and displayed **LTR** inside RTL layout. | [D] |
| Long forms (registration wizards) split into steps with a progress indicator ("Step 2 of 4"); progress auto-saved locally. | [D] |
| All drafts (messages, forms) survive app close and connection loss (NFR-004). | [PRD] |
| Destructive or money-related actions (cancel booking, reject order, mark "Not found", remove booked slot) need a confirmation sheet stating the consequence (e.g. refund or no refund — UF-4). | [D] |

### 5.2 Status timeline (waiting states) [C]

Every consultation, booking, and order shows a horizontal (mobile: vertical) **stage timeline + remaining time**:

```
✓ Sent → ✓ Payment confirmed → ● Waiting for doctor reply (18h left) → ○ Prescription
```

- Countdowns use server time (NFR-009) and show relative time ("18h left") with the absolute time on tap.
- When a deadline passes, the stage shows the outcome and the next action (e.g. "Doctor didn't reply — consult another doctor; refund requested").

### 5.3 Feedback

| Event | Pattern | Source |
|---|---|---|
| Success of a quick action | Toast at the bottom, 4s, with undo where possible. | [D] |
| Success of a key milestone (booking confirmed, order paid, pickup code issued) | Full success screen with the next step. | [D] |
| Async state change from the other party | In-app notification + PWA push + badge (F-16). | [PRD] |
| Copy to clipboard | "Copied" toast. | [D] |

### 5.4 Images

Client-side compression before upload (NFR-006), thumbnail preview, remove (×) per image, tap to view full screen with pinch-zoom (doctor side for diagnosis). [PRD]/[D]

---

## 6. UI States

Every data screen must define all five states.

### 6.1 Loading [D]
- **Skeletons** shaped like the final content (cards, list rows), never full-screen spinners.
- If loading exceeds 8s on a slow network: "Taking longer than usual…" with Retry.
- Cached content is shown immediately, then refreshed (stale-while-revalidate), with a subtle "Updating…" indicator.

### 6.2 Empty [D]
Each empty state has: one-line explanation + one primary action. Examples:

| Screen | Message (intent) | Action |
|---|---|---|
| My Consultations | No consultations yet. | Find a doctor |
| My Pharmacy | Prescriptions from your doctor will appear here. | Find a doctor |
| Pharmacy results | No open pharmacy has these items now. | Change area / Buy available only |
| Doctor Cases | No cases waiting. | — |
| Pharmacy Orders | No new orders. | Update inventory |
| Home (filters) | No doctors match. | Clear filters |

### 6.3 Error [D]
- Errors say **what happened + what to do**, in plain Arabic; no codes shown to users.
- Inline field errors under the field; screen-level errors as a banner with Retry.
- Error color + ⚠ icon + text, never color alone (§8).
- PRD error cases (ERR-01..ERR-24) each need a written message in the copy deck.

### 6.4 Offline [C]
- Thin persistent banner at the top: "No connection — showing saved data".
- Outgoing messages queued with "Waiting to send ⏳" and sent automatically on reconnect.
- **Available offline**: pickup codes, last loaded prescriptions, booking details (NFR-005).
- Actions that need the server (payment submit, accept order) are disabled with an explanation, while their form input is kept.

### 6.5 Success
See §5.3.

### 6.6 Partner account statuses [D]
"Under review", "Rejected (reason + Edit & resubmit)", "Approved — next setup steps", "Hidden (subscription grace ended)" are each a full status screen with clear next steps (BR-007, BR-113).

### 6.7 Permissions [D]
Location, camera (pickup scan), notifications: always ask **in context**, after a short in-app explanation, and always provide a working fallback (area list, manual code entry, in-app notification center).

### 6.8 Registration wizards (S-15, S-22) [D]
Steps: Account → Business details + map pin → Documents upload → Review & submit → Status screen. Document upload accepts camera or file, shows previews, and allows replacing a document after rejection.

---

## 7. Responsive Behavior

| Breakpoint | Layout | Source |
|---|---|---|
| < 600px (base) | Single column, bottom tab bar, full-width cards, bottom sheets for pickers/confirmations. | [C] |
| 600–1023px | Single column, max content width 640px centered; bottom bar stays. | [D] |
| ≥ 1024px | Right-side navigation; list-detail split views for Cases, Orders, Payments, and admin tables. | [D] |

- Minimum supported width: 320px, no horizontal scroll. [D]
- Admin screens are designed desktop-first but must remain usable on mobile (tables → cards). [C]
- Browsers per NFR-014. PWA install prompt shown after the first successful action, not on first visit; iOS users get install instructions because push requires install (RISK-9). [D]

---

## 8. Accessibility (WCAG 2.1 AA — NFR-010)

| Requirement | Detail | Source |
|---|---|---|
| Contrast | Body text ≥ 4.5:1, large text and UI components ≥ 3:1. The dusty rose (`#B5677D`) is **not** used for body text on the light background. | [PRD]/[D] |
| Color independence | Status, errors, slot types always use icon/label + color. | [D] |
| Touch targets | ≥ 44×44px, ≥ 8px apart. | [PRD] |
| Text scaling | Layout works at 200% text size; no fixed-height text containers. | [PRD] |
| Base font size | 16px body, 14px minimum for secondary text. | [D] |
| Screen readers | Semantic HTML, labeled controls, `lang="ar" dir="rtl"` on root, `lang="en" dir="ltr"` on English medicine names; countdowns announced politely, not every second. | [D] |
| Focus | Visible focus ring (burgundy, 2px) for keyboard users on desktop. | [D] |
| Motion | Respect `prefers-reduced-motion`; no essential information conveyed by animation. | [D] |
| Timeouts | No session-expiry data loss; drafts preserved (NFR-004). | [D] |

---

## 9. Language & Content

| Rule | Detail | Source |
|---|---|---|
| UI language | Arabic only, RTL (NG-12). | [PRD] |
| Register | Simple Modern Standard Arabic with a warm, friendly tone; short sentences; no medical jargon in patient screens. | [D] |
| Addressing | Gender-neutral phrasing where possible (e.g. imperative verbs avoided in favor of nouns: "إرسال" not "أرسلي/أرسل"). | [D] |
| Numerals | **Western digits (0–9)** everywhere, consistently (prices, times, codes, transaction IDs) — matches wallet apps and reduces entry errors (NFR-002). | [D] |
| Currency | `₪` after the number, e.g. "30 ₪" (BR-077). | [D] |
| Date/time | 12-hour clock with ص/م, Asia/Gaza time zone, relative dates for recent items ("today", "yesterday"). | [D] |
| Medicine names | Arabic name primary, English name secondary below/after it in LTR (BR-088). | [PRD] |
| Mixed direction | Wallet IDs, emails, phones, codes, English names isolated with `dir="ltr"` / Unicode isolation. | [D] |
| Icons | Directional icons (back, next, progress arrows) mirrored for RTL; non-directional icons (search, camera, check) not mirrored. | [D] |
| Copy deck | All strings (including ERR-01..24 messages and empty states) are maintained in one Arabic copy file reviewed by the product owner before build. | [D] |

---

## 10. Visual Identity (initial direction)

The product name is still provisional (Q-01). There is no final logo. The direction below is based on a color reference the product owner shared (not a logo).

### 10.1 Mood [C]
Calm, comfortable, and beautiful — **soft burgundy and dusty pink**.

### 10.2 Color tokens [C] (exact shades may be refined by the designer while keeping contrast)

| Token | Value | Use |
|---|---|---|
| `primary` | `#5E1F3A` (deep burgundy) | Primary buttons, headings, active nav, focus ring |
| `accent` | `#B5677D` (dusty rose) | Decorative details, borders, illustrations, inactive accents — **not** body text |
| `background` | `#F6E8E4` (blush) | App background |
| `surface` | `#FFFFFF` [D] | Cards, sheets, inputs |
| `text` | `#2A1A20` [D] | Body text |
| `error` | `#C2410C` (orange-red) | Errors only, always with ⚠ icon + text |
| `success` / `warning` / `info` | To be defined by the designer [D] | Must be distinguishable from `primary` and `error`, each with icon + text |

- Flat colors in the app UI. Gradients/textures are limited to splash/marketing surfaces and must be CSS-only (no image backgrounds) to protect NFR-003. [D]
- Dark mode: **not in MVP**; tokens are defined so it can be added later. [D]

### 10.3 Typography [D]
- The product owner rejected IBM Plex Sans Arabic, Tajawal, and Cairo.
- **Recommendation:** **Readex Pro** for all UI text (Arabic + Latin in one family, designed for readability, variable font, free on Google Fonts). Optional: **Alexandria** for large headings only, if the designer wants more character. Requires product owner sign-off on a sample screen.
- Constraints for any final choice: Arabic + Latin support, legible at 14px, free license, self-hosted and subset, ≤ 2 weights loaded (Regular + SemiBold), `font-display: swap`.

### 10.4 Shape & spacing [D]
- Rounded corners: 12px cards, 10px buttons/inputs, full-pill chips.
- 8px spacing grid; generous whitespace to support the calm mood.
- Soft, low shadows only for floating elements (sheets, toasts).

---

## 11. Performance-Driven UI Rules (NFR-003, G-6)

| Rule | Source |
|---|---|
| No embedded maps by default; use "Open in maps" links. | [D] |
| Icon set as inline SVG sprite, not an icon font. | [D] |
| Images lazy-loaded with fixed aspect-ratio placeholders. | [D] |
| Lists paginate with "Load more". | [D] |
| App shell cached by the service worker; content cached for offline per NFR-005. | [PRD] |

---

## 12. Open UX Questions

| # | Question | Blocks |
|---|---|---|
| UX-Q1 | Validate family-member model ([T], §3.3) in usability testing. | S-06, S-07, S-09, S-11 |
| UX-Q2 | Final font approval (Readex Pro recommendation, §10.3). | Visual design |
| UX-Q3 | Final product name and logo (PRD Q-01). | Visual identity |
| UX-Q4 | Payment confirmation deadline to show on S-08 (PRD Q-03). | S-08 copy |
| UX-Q5 | Expired pickup outcome copy (PRD Q-04). | S-13 |
| UX-Q6 | Whether pharmacies have multiple staff accounts (PRD Q-16) — affects Pickup/Orders concurrency UI. | S-24 |
| UX-Q7 | Success / warning / info color values. | Visual design |

---

## 13. Decision Log (interview)

| # | Topic | Decision | Tag |
|---|---|---|---|
| 1 | App structure | One app; role-specific flows after login | [C] |
| 2 | Devices | Mobile-first for all; admin preferably desktop | [C] |
| 3 | Patient tabs | Home · My Consultations · My Pharmacy · Notifications · My Account | [C] |
| 4 | Family members | Unified lists + filter chips + "Who is this for?" at action time | [T] |
| 5 | Doctor tabs | Cases · Schedule · Payments · Notifications · My Account | [C] |
| 6 | Prescriptions for doctor | Inside the conversation only, no separate tab | [C] |
| 7 | Doctor who is also manager | One account; Clinic Management inside My Account | [C] |
| 8 | Clinic staff tabs | Today's Patients · Schedule · Payments · Notifications · My Account | [C] |
| 9 | Pharmacy tabs | Orders · Pickup · Inventory · Notifications · My Account | [C] |
| 10 | Admin home | Pending Tasks with count cards | [C] |
| 11 | Patient home | Doctor list (not clinic list) | [C] |
| 12 | Doctor card | Service prices + nearest available slot | [C] |
| 13 | Doctor page | Three stacked service cards; disabled = greyed | [C] |
| 14 | Payment screen | One screen, numbered steps, state preserved | [C] |
| 15 | Waiting states | Stage timeline + remaining time | [C] |
| 16 | Offline | Thin banner + auto-send queue; pickup code stays visible | [C] |
| 17 | Brand | No existing identity; ideas only | [C] |
| 18 | Mood & colors | Soft burgundy + dusty pink; palette from reference image | [C] |
| 19 | Font | Readex Pro recommended (owner rejected Plex / Tajawal / Cairo) | [D] |
| 20 | Everything else (forms, states, responsive, a11y, language, numerals) | Delegated to UX recommendations | [D] |
