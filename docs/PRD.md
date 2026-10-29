# Product Requirements Document — "Iltiam" (التِئام) *(working name)*

| Field | Value |
|---|---|
| Document status | Draft v1.2 — owner answers to open questions applied (see §26.1, QR-21) |
| Date | 2026-10-15 (v1.0) · 2026-10-27 (v1.1, v1.2) |
| Source | Grill Me discovery interview (35 questions, Arabic), conducted 2026-10-13 → 2026-10-15 |
| Product type | Web application + installable PWA (multi-sided marketplace) |
| Launch market | Gaza Strip, Palestine |
| Interface language | Arabic (RTL); medicine names in Arabic **and** English |

### How to read this document

Every requirement and rule carries a source tag:

| Tag | Meaning |
|---|---|
| **[C]** | **Confirmed** — explicitly answered by the product owner during the interview. |
| **[A]** | **Assumption** — inferred by the PM/BA to make the requirement complete and testable. Must be validated (see §26). |
| **[D]** | **Delegated** — the product owner asked the PM to choose ("choose the best"). Treated as a recommendation, changeable without product impact. In v1.1 the owner also delegated the fixes from the PRD review (QR-11 – QR-20). Those rules are tagged [D], may have product impact, and stay open to owner override. |

Where the interview was re-started, the **latest answer prevails** (see §26.1 for superseded answers).

---

## 1. Product Overview

**Iltiam** (working name; final name is an open question) is a dermatology-focused care platform for patients in Gaza. It connects three parties in one journey:

1. **Patient → Dermatologist**: the patient browses verified dermatology clinics, then sends an asynchronous message (free or paid, as each doctor decides), books a scheduled live text consultation (free or paid, as each doctor decides), or books an in-person visit (paid at the clinic). [C]
2. **Dermatologist → Prescription**: after any service performed through the platform, the doctor issues an electronic prescription selected from a unified, admin-managed dermatology medicine catalog. [C]
3. **Prescription → "Saydaliyati" (صيدليتي, "My Pharmacy")**: the platform shows the patient the price and the nearest pharmacies that stock the prescribed medicines, the pharmacy confirms availability and price, the patient pays the pharmacy directly through a local e-wallet, and picks up the order with a pickup code. [C]

**Unique value:** the end-to-end bridge from diagnosis to medicine in hand — price transparency and availability before the patient leaves home, which matters greatly in Gaza's conditions of medicine shortages, displacement, and unreliable connectivity.

**Business model:** patients never pay the platform. Clinics and pharmacies get a free trial, then pay a monthly subscription. No patient money flows through the platform; payments go directly from patient to clinic/pharmacy wallets. [C]

---

## 2. Problem Statement

| # | Problem | Who feels it |
|---|---|---|
| P1 | Reaching a dermatologist requires physical travel, which is costly and risky in Gaza; many skin conditions could be triaged remotely from photos. | Patients |
| P2 | After receiving a prescription, patients do not know which pharmacy has the medicine or what it costs, and must go from pharmacy to pharmacy. Shortages make this worse. | Patients |
| P3 | Clinics lack a simple digital channel to receive remote consultations, manage appointments, and issue prescriptions. | Clinics, doctors |
| P4 | Pharmacies lack a channel to receive confirmed, prepaid orders from patients with valid prescriptions. | Pharmacies |

*Note:* The problem statements are derived from the product idea as stated by the owner. No user research or market data was provided [A]. Validating them is recommended (see §25).

---

## 3. Product Goals

| ID | Goal | Type |
|---|---|---|
| G-1 | Let a patient go from "skin concern" to "prescription" without leaving home, when clinically appropriate. | User |
| G-2 | Let a patient know **before travelling** which nearby pharmacy has the prescribed medicines and at what price. | User |
| G-3 | Give clinics a lightweight tool for remote consultations, scheduling, and e-prescribing. | Partner |
| G-4 | Give pharmacies a flow for confirmed, prepaid orders. | Partner |
| G-5 | Generate recurring revenue from clinic and pharmacy subscriptions after a free trial. | Business |
| G-6 | Remain usable under unreliable power and internet (asynchronous-first design, lightweight pages). | Quality |

Quantitative targets are **not defined yet** (§22, §26).

---

## 4. Non-Goals

Out of scope for the MVP. Some may come in later phases (§24).

| ID | Non-goal | Source |
|---|---|---|
| NG-1 | Video consultations. | [C] Deferred to Phase 2 |
| NG-2 | Booking of treatment sessions or packages (laser, peeling, and so on). | [C] Deferred to Phase 2 |
| NG-3 | Non-dermatology specialties or medicines. | [C] |
| NG-4 | Processing, holding, or splitting patient payments on the platform (no escrow, no commission collection). | [C] |
| NG-5 | Charging patients any fee. | [C] |
| NG-6 | Payment proof by uploading a receipt image. | [C] Explicitly rejected |
| NG-7 | Automated e-wallet API integration. | [C] Deferred to Phase 2 |
| NG-8 | Native iOS/Android store apps. | [C] Deferred to Phase 2 |
| NG-9 | Ratings and reviews of doctors or pharmacies. | [A] Deferred (owner undecided) |
| NG-10 | Delivery of medicines (pickup only). | [A] Not discussed; pickup is the stated model |
| NG-11 | Independent doctors without a physical clinic. | [C] |
| NG-12 | English UI. | [C] Arabic UI only; medicine names bilingual |
| NG-13 | WhatsApp or SMS notifications. | [A] Not selected for MVP |
| NG-14 | Operation outside Gaza. | [C] |

---

## 5. Target Users

| User group | Description | Platform role(s) |
|---|---|---|
| Dermatology patients in Gaza | Adults (18+) who hold an account and may manage consultations for family members, including children. | Guest, Patient |
| Dermatologists | Licensed dermatology specialists, always attached to a physical clinic. | Doctor |
| Dermatology clinics | Clinics with one or more doctors. A private clinic is a one-doctor clinic. | Clinic Manager, Clinic Staff |
| Pharmacies in Gaza | Licensed pharmacies that sell dermatology medicines. | Pharmacy |
| Platform operator | Internal team that verifies partners and manages the catalog and subscriptions. | Platform Admin |

---

## 6. User Personas

*Personas are illustrative [A], built from the interview context.*

| Persona | Context | Goals | Frustrations |
|---|---|---|---|
| **Sara, 29, patient** | Displaced within Gaza, mobile phone only, intermittent internet. Has acne flare-ups. | Get a doctor's opinion from photos; know where her medicine is and its price before walking there. | Long trips to clinics; pharmacies that don't have the medicine. |
| **Abu Khaled, 45, parent** | Manages care for his wife and 3 children from one phone. | Book for his child under his own account; keep each family member's history separate. | Separate accounts and paperwork for each family member. |
| **Dr. Ahmad, dermatologist** | Runs a one-doctor private clinic with a secretary. | Answer simple cases remotely; fill in-person slots; issue prescriptions quickly. | Unpaid "quick questions"; no-shows. |
| **Lina, clinic secretary** | Manages the clinic's schedule and payments. | Manage the doctor's calendar; confirm wallet payments quickly. | Must not see patients' medical photos or content. |
| **Pharmacist Omar** | Neighborhood pharmacy, busy counter, power cuts. | Receive confirmed orders; avoid holding stock for people who never come. | Keeping prices and stock updated; unpaid reservations. |
| **Platform Admin (Nour)** | Ops team member. | Verify licenses; manage the medicine catalog; track subscriptions. | Fake or unlicensed providers; duplicate medicine names. |

---

## 7. User Needs

| ID | Need | Persona |
|---|---|---|
| UN-1 | Browse clinics and doctors without creating an account. | Patient |
| UN-2 | Send a case with photos and get a response within a known time. | Patient |
| UN-3 | Book a live chat or an in-person visit at a specific time. | Patient |
| UN-4 | Manage family members' consultations from one account. | Parent |
| UN-5 | See the price and nearest pharmacy with the prescribed medicines. | Patient |
| UN-6 | Be sure the pharmacy really has the medicine before paying. | Patient |
| UN-7 | Pick up the medicine at a convenient time, possibly through a relative. | Patient |
| UN-8 | Decide whether async messages are free, paid, or disabled. | Doctor |
| UN-9 | Delegate schedule and payment confirmation to staff without exposing medical data. | Doctor, Clinic |
| UN-10 | Maintain prices and availability for a manageable (dermatology-only) list. | Pharmacy |
| UN-11 | Ensure only licensed, physically located providers appear. | Admin, Patient |
| UN-12 | Medical data visible only to the patient and the treating doctor. | Patient |

---

## 8. User Stories

| ID | As a… | I want to… | So that… | Feature |
|---|---|---|---|---|
| US-01 | Guest | browse clinics, doctors, services, and prices without signing up | I can decide before committing | F-04 |
| US-02 | Guest | register with email and password when I book or consult | I can use services | F-03 |
| US-03 | Patient | add family members (e.g., my children) to my account | I can request care on their behalf | F-03 |
| US-04 | Patient | send an async message with text and photos to a doctor | I get an opinion without travelling | F-05 |
| US-05 | Patient | book a live text consultation slot, prepaying if the doctor charges for it | I get a real-time conversation at a set time | F-06 |
| US-06 | Patient | book an in-person appointment and pay at the clinic | I can be examined physically | F-07 |
| US-07 | Patient | pay via PalPay / Jawwal Pay / Bank of Palestine app and enter the transaction reference | the clinic or pharmacy can verify my payment | F-09 |
| US-08 | Patient | cancel a booking 24h+ in advance and get refunded | I'm not penalised for planning changes | F-15 |
| US-09 | Patient | see my prescription in Saydaliyati with nearest pharmacies and prices | I know where to get my medicines | F-12 |
| US-10 | Patient | split my prescription across pharmacies, or buy what's available now and the rest later | shortages don't block me | F-12 |
| US-11 | Patient | receive a pickup code and daily reminders | I (or a relative) can collect the order on time | F-14 |
| US-12 | Doctor | choose whether async messages are free, paid (my price), or disabled | I control my workload | F-05 |
| US-13 | Doctor | write an e-prescription from the catalog after a platform service | the patient can order it immediately | F-10 |
| US-14 | Doctor | add a free-text medicine not in the catalog and request it be added | I'm never blocked by catalog gaps | F-10, F-11 |
| US-15 | Doctor / Staff | manage availability for live chat and in-person slots | patients can book real free times | F-08 |
| US-16 | Doctor / Staff | confirm or reject a patient's payment reference | bookings only proceed when paid | F-09 |
| US-17 | Clinic Manager | register my clinic with documents and add doctors and staff | my clinic can operate on the platform | F-01 |
| US-18 | Pharmacy | register with documents | I can receive orders | F-02 |
| US-19 | Pharmacy | set daily prices and availability for catalog medicines | patients see accurate data | F-13 |
| US-20 | Pharmacy | accept or reject an order within 30 minutes | patients get a quick answer | F-13 |
| US-21 | Pharmacy | verify a pickup code and mark the order delivered | the right person gets the order | F-14 |
| US-22 | Admin | review and approve or reject clinics, doctors, and pharmacies | only verified providers appear | F-01, F-02, F-18 |
| US-23 | Admin | manage the unified dermatology medicine catalog (AR + EN) | names are consistent across doctors and pharmacies | F-11 |
| US-24 | Admin | record subscription payments and hide non-paying partners after a grace period | the business model is enforced | F-17 |

---

## 9. MVP Scope

### 9.1 In scope (MVP)

| # | Capability | Source |
|---|---|---|
| 1 | Responsive web app installable as a PWA, with push notifications | [C] |
| 2 | Arabic RTL UI; bilingual (AR/EN) medicine names | [C] |
| 3 | Guest browsing of clinics, doctors, services, and prices | [C] |
| 4 | Patient registration and login with email and password | [C] |
| 5 | Family members under one adult account | [C] |
| 6 | Clinic self-registration, document upload, admin approval | [C] |
| 7 | Clinic Manager adds doctors (admin-approved) and staff | [C] |
| 8 | Pharmacy self-registration, document upload, admin approval | [C] |
| 9 | Async message consultation (free, paid, or disabled per doctor; 24h SLA; 72h follow-up window with up to 3 patient follow-ups) | [C]/[D] |
| 10 | Booked live text consultation (free or prepaid, as the doctor decides; doctor-defined duration; 15-min wait rule) — **Should Have**, first to cut (QR-17) | [C]/[D] |
| 11 | In-person appointment booking (paid at clinic) | [C] |
| 12 | Schedule management by doctor or clinic staff | [C] |
| 13 | Manual payment verification: patient enters wallet transaction reference, provider confirms | [C] |
| 14 | E-prescription from unified dermatology catalog + free-text items + "add to catalog" request | [C] |
| 15 | Saydaliyati: GPS location (primary) or area selection (fallback); pharmacies by distance; price and last-updated | [C] |
| 16 | Partial availability: buy available items and defer the rest (Must Have); split across pharmacies (**Should Have**, first to cut — QR-17) | [C]/[D] |
| 17 | Pharmacy order confirmation within 30 min of working hours; auto-cancel otherwise | [C] |
| 18 | Pickup code (numeric + QR); pickup window set per pharmacy; daily reminders | [C] |
| 19 | Cancellation policy (24h rule) with manual refund marking | [C] |
| 20 | In-app notifications + PWA push | [C] |
| 21 | Medical data privacy (patient + treating doctor only) | [C] |
| 22 | Admin console: verification, catalog, subscriptions, partner visibility | [C]/[A] |
| 23 | Subscription status tracking: free trial → monthly; 7-day grace → hidden from new patients | [C] |

### 9.2 Out of scope (MVP)

See §4 (Non-Goals) and §24 (Future Features).

---

## 10. Core Features

Each feature lists its Functional Requirements (§11), Business Rules (defined here), Edge Cases (§18), Error States (§19), and Acceptance Criteria (§23) by ID, so each item is defined in one place only.

### Feature summary

| ID | Feature | Priority |
|---|---|---|
| F-01 | Clinic & Doctor Onboarding and Verification | Must Have |
| F-02 | Pharmacy Onboarding and Verification | Must Have |
| F-03 | Patient Account & Family Members | Must Have |
| F-04 | Guest Browsing & Doctor Discovery | Must Have |
| F-05 | Async Message Consultation | Must Have |
| F-06 | Booked Live Text Consultation | Should Have (QR-17) |
| F-07 | In-Person Appointment Booking | Must Have |
| F-08 | Schedule Management | Must Have |
| F-09 | Manual Payment Verification | Must Have |
| F-10 | E-Prescription | Must Have |
| F-11 | Unified Dermatology Medicine Catalog | Must Have |
| F-12 | Saydaliyati — Pharmacy Search & Matching | Must Have |
| F-13 | Pharmacy Inventory & Order Confirmation | Must Have |
| F-14 | Order Pickup | Must Have |
| F-15 | Cancellations & Refunds | Must Have |
| F-16 | Notifications | Must Have |
| F-17 | Partner Subscriptions | Should Have |
| F-18 | Admin Console | Must Have |

---

### F-01 — Clinic & Doctor Onboarding and Verification

- **Purpose:** ensure only licensed dermatologists working from a real, physical clinic appear to patients.
- **User stories:** US-17, US-22
- **Functional requirements:** FR-001 – FR-009
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-001 | Every doctor must belong to exactly one clinic that has a physical address. Independent doctors without a clinic are not allowed. | [C] |
| BR-002 | A private clinic is registered as a clinic with one doctor. | [C] |
| BR-003 | Clinic registration requires: a clinic license, a written address + map pin, and for each doctor a medical practice license and a dermatology specialty certificate. | [C] |
| BR-004 | A clinic is invisible to patients until a Platform Admin approves it. | [C] |
| BR-005 | Doctors added later by the Clinic Manager are invisible to patients until a Platform Admin approves their documents. | [C] |
| BR-006 | A doctor belongs to only one clinic in the MVP, enforced by a unique practice license number. | [A] |
| BR-007 | Rejection must include a reason visible to the applicant; the applicant may correct and resubmit. | [A] |
| BR-008 | A Clinic Manager may also hold the Doctor role on the same account, which one-doctor private clinics need. Medical content is reachable only through the Doctor role, for that doctor's own consultations. | [D] Resolves Q-15 |
| BR-009 | Patient payments for any doctor of a clinic go to the **clinic's** e-wallet, not to an individual doctor's wallet. | [C] (v1.2) |

- **Edge cases:** EC-01 – EC-04, EC-42
- **Error states:** ERR-01, ERR-02, ERR-03
- **Acceptance criteria:** AC-01 – AC-05, AC-05a
- **Priority:** Must Have

### F-02 — Pharmacy Onboarding and Verification

- **Purpose:** only licensed, physically located pharmacies receive orders.
- **User stories:** US-18, US-22
- **Functional requirements:** FR-010 – FR-013
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-010 | Pharmacies onboard the same way as clinics: self-registration, document upload, admin approval. | [C] |
| BR-011 | Required documents: pharmacy license, responsible pharmacist's license, written address + map pin. | [A] Derived from "same as clinics" |
| BR-012 | A pharmacy must set its working hours, which are used for the 30-minute confirmation rule and to hide closed pharmacies. | [A] Required by BR-131 |
| BR-013 | Each pharmacy registers one or more e-wallet payment identifiers (PalPay / Jawwal Pay / Bank of Palestine) shown to patients at payment time. | [A] |
| BR-014 | A pharmacy uses **one shared account** in the MVP. Multiple pharmacy staff logins are not supported. | [C] (v1.2) — resolves Q-16 |

- **Edge cases:** EC-05
- **Error states:** ERR-01, ERR-02
- **Acceptance criteria:** AC-06, AC-07
- **Priority:** Must Have

### F-03 — Patient Account & Family Members

- **Purpose:** low-friction access, with one adult managing family care.
- **User stories:** US-02, US-03
- **Functional requirements:** FR-014 – FR-019
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-020 | Browsing needs no account; an account is required to message, book, receive a prescription, or order medicine. | [C] |
| BR-021 | Patients register with email + password. | [C] |
| BR-022 | Account holders must be 18+. Minors are added as family members under the account holder. | [C] |
| BR-023 | Each consultation, booking, and prescription is linked to one specific family member (the "patient of record"). | [C] |
| BR-024 | Email must be verified before the first paid or booked service. | [A] |
| BR-025 | A mobile phone number is collected on the profile so clinics and pharmacies can contact the patient. | [A] |

- **Edge cases:** EC-06, EC-07, EC-46
- **Error states:** ERR-04, ERR-05
- **Acceptance criteria:** AC-08 – AC-10
- **Priority:** Must Have

### F-04 — Guest Browsing & Doctor Discovery

- **Purpose:** let patients compare doctors and services before registering.
- **User stories:** US-01
- **Functional requirements:** FR-020 – FR-023
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-030 | Only approved clinics and doctors with an active or trial (non-hidden) subscription are listed. | [C] |
| BR-031 | Each doctor's listing shows the services they offer and their prices: async message (free / price / disabled), live chat (free / price, duration), in-person (price, paid at clinic). | [C]/[A] |

- **Edge cases:** EC-08
- **Error states:** ERR-06
- **Acceptance criteria:** AC-11
- **Priority:** Must Have

### F-05 — Async Message Consultation

- **Purpose:** remote triage that works without both parties being online at once, which matters given Gaza's power and internet conditions.
- **User stories:** US-04, US-12
- **Functional requirements:** FR-024 – FR-031
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-040 | Each doctor sets async messaging to Free (default), Paid (doctor-defined price), or Disabled. | [C] |
| BR-041 | The doctor must send a first response within 24 hours. | [C] |
| BR-042 | Paid async: the 24h clock starts when the clinic confirms payment (F-09), or automatically when the provider window of BR-076 ends without a decision, whichever comes first. Free async: it starts when the message is sent. | [A]/[D] (QR-13) |
| BR-043 | If the doctor does not respond within 24h, the patient is notified and may consult another doctor. If the message was paid, the clinic must refund the patient. | [C] |
| BR-044 | After the doctor's first response, the thread stays open 72 hours for follow-ups. The doctor may close it earlier. After closure, any new question is a new consultation. | [C] |
| BR-045 | Messages are delivered in real time. If both parties are online, the thread behaves like a live chat. | [C] |
| BR-046 | Patients can attach photos to a message. | [C] |
| BR-047 | Photo limits: max 5 images per message, max 10 MB each, JPEG/PNG/HEIC. | [A] |
| BR-048 | During the 72h follow-up window the patient may send up to **3 follow-up messages** (photos allowed). The doctor's replies are not limited. Open-ended real-time conversation is the paid live chat (F-06). | [D] (QR-11) |
| BR-049 | The patient may withdraw a paid async case, with a full refund, until the doctor first opens it. After that it cannot be withdrawn. | [D] (QR-19) |

- **Edge cases:** EC-09 – EC-12, EC-47
- **Error states:** ERR-07 – ERR-09, ERR-25
- **Acceptance criteria:** AC-12 – AC-17, AC-16a, AC-17a
- **Priority:** Must Have

### F-06 — Booked Live Text Consultation

- **Purpose:** a guaranteed real-time conversation at a scheduled time.
- **User stories:** US-05
- **Functional requirements:** FR-032 – FR-038
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-050 | Live chat is booked into an available slot. If the doctor charges for it, it is paid **in advance** to the clinic's wallet. | [C] |
| BR-051 | The doctor defines the session duration. | [C] |
| BR-052 | A paid booking is only confirmed after the clinic confirms the payment (F-09). A free booking is confirmed on slot selection. | [C] |
| BR-053 | If the patient doesn't join within 15 minutes of the start time, the session converts to an async message and the doctor replies within 24h. No refund. | [C] |
| BR-054 | If the doctor doesn't join within 15 minutes, the patient chooses between rebooking and a full refund. | [C] |
| BR-055 | The chat closes when the session duration ends. Post-session follow-up uses the 72h rule of BR-044. | [A] |
| BR-056 | Each doctor chooses whether live chat is **Free** or **Paid** (doctor-defined price). | [C] (v1.2) — resolves Q-07; replaces the v1.1 assumption "always paid" |
| BR-057 | If a session converted to async under BR-053 gets no doctor response within 24h, BR-043 applies: the clinic refunds the patient in full. | [D] (QR-12) |

- **Edge cases:** EC-13 – EC-15, EC-41
- **Error states:** ERR-10, ERR-11
- **Acceptance criteria:** AC-18 – AC-21, AC-19a
- **Priority:** Should Have. First feature to cut if the timeline slips: async messages already arrive in real time (BR-045), within the follow-up limit of BR-048 (QR-17).

### F-07 — In-Person Appointment Booking

- **Purpose:** book a physical examination when a remote consultation isn't enough.
- **User stories:** US-06
- **Functional requirements:** FR-039 – FR-042
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-060 | In-person visits are paid at the clinic. The platform collects no payment for them. | [C] |
| BR-061 | An in-person booking is confirmed immediately on slot selection, since no prepayment is needed. | [A] |
| BR-062 | Clinic staff or the doctor can mark an in-person visit as Completed or No-show. | [A] |
| BR-063 | Each patient of record may hold at most one active in-person booking per doctor. An account with 3 no-shows in 90 days cannot book in-person visits for 30 days; the Admin can lift the block. | [D] (QR-19) |

- **Edge cases:** EC-16
- **Error states:** ERR-10, ERR-28
- **Acceptance criteria:** AC-22, AC-23, AC-23a
- **Priority:** Must Have

### F-08 — Schedule Management

- **Purpose:** publish real availability for live chat and in-person slots.
- **User stories:** US-15
- **Functional requirements:** FR-043 – FR-046
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-065 | Availability can be managed by the doctor or by Clinic Staff of the same clinic. | [C] |
| BR-066 | Slots are typed as **Live chat** or **In-person**. | [A] |
| BR-067 | A slot can be booked by only one patient. | [A] |
| BR-068 | Removing availability that already has bookings requires cancelling those bookings, which counts as a clinic cancellation (BR-092). | [A] |

- **Edge cases:** EC-17
- **Error states:** ERR-12
- **Acceptance criteria:** AC-24, AC-25
- **Priority:** Must Have

### F-09 — Manual Payment Verification

- **Purpose:** confirm direct wallet payments without the platform touching money and without automated wallet integration.
- **User stories:** US-07, US-16
- **Functional requirements:** FR-047 – FR-053
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-070 | All payments go directly from the patient's wallet to the clinic's or pharmacy's wallet. The platform never receives patient funds. | [C] |
| BR-071 | Supported methods (MVP): PalPay, Jawwal Pay, Bank of Palestine app. | [C] |
| BR-072 | The patient pays outside the platform, then enters the **transaction reference number** in the platform. | [C] |
| BR-073 | The receiving party (doctor or Clinic Staff for clinics; the pharmacy for orders) checks its wallet and marks the payment **Received** or **Not found**. | [C] |
| BR-074 | Receipt images are not used. | [C] |
| BR-075 | A transaction reference can be used only once **per wallet provider** (wallet + reference is unique). If a patient's reference is rejected as already used, they can flag "this is my payment", which creates an Admin review task. | [A]/[D] (QR-19) |
| BR-076 | Payment has two windows (resolves Q-03). **(a) Patient window:** the patient must submit the reference within **30 minutes** of holding a slot or of the pharmacy's acceptance; otherwise the hold is released. **(b) Provider window:** after the reference is submitted, the clinic must mark Received / Not found within **2 hours**, and the pharmacy within **60 minutes counted during its working hours**. For live-chat slots, the clinic must also decide before the session starts. | [D] (QR-13) |
| BR-077 | Payment amounts and currency are shown in Israeli Shekel (ILS / ₪). | [A] |
| BR-078 | If the provider window ends without a decision, the patient is protected: a paid async case is delivered and its 24h clock starts; a live-chat booking is Confirmed; a pharmacy order gets its pickup code marked "Payment unverified", and the pharmacy checks its wallet at the counter before handing over. Each case is flagged to the Admin. | [D] (QR-13) |
| BR-079 | Payment disputes (the patient says they paid, the provider marks Not found, or the amount is wrong) are settled directly between the patient and the provider. The platform does not arbitrate; it shows each party the other's phone number. | [C] (v1.2) — resolves the dispute part of Q-08 |

- **Edge cases:** EC-18 – EC-21, EC-45
- **Error states:** ERR-13 – ERR-15
- **Acceptance criteria:** AC-26 – AC-29, AC-29a, AC-29b
- **Priority:** Must Have

### F-10 — E-Prescription

- **Purpose:** turn the doctor's decision into an orderable list in Saydaliyati.
- **User stories:** US-13, US-14
- **Functional requirements:** FR-054 – FR-059
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-080 | A doctor may issue a prescription only for a service performed through the platform: an async message, a live chat, or a completed in-person visit booked on the platform. | [C] |
| BR-081 | Prescription items are selected from the unified catalog (F-11). | [C] |
| BR-082 | If a medicine is not in the catalog, the doctor may add a free-text item. It is shown to the patient as "Not available in Saydaliyati", and a catalog-addition request is sent to the Admin. | [C] |
| BR-083 | Only users with the Doctor role can create prescriptions. | [A] |
| BR-084 | Each item includes medicine, strength/form (from catalog), quantity, and dosage instructions. | [A] |
| BR-085 | A prescription cannot be edited after it is issued. The doctor may cancel it and issue a new one, provided no pharmacy order exists for it. | [A] |

- **Edge cases:** EC-22, EC-23
- **Error states:** ERR-16
- **Acceptance criteria:** AC-30 – AC-32
- **Priority:** Must Have

### F-11 — Unified Dermatology Medicine Catalog

- **Purpose:** a single naming standard, so a prescribed item matches pharmacy stock.
- **User stories:** US-23, US-14
- **Functional requirements:** FR-060 – FR-063
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-086 | The catalog contains dermatology medicines and products only. | [C] |
| BR-087 | Only the Platform Admin can create, edit, or deactivate catalog items. | [C] |
| BR-088 | Each item has an Arabic name and an English name. | [C] |
| BR-089 | Pharmacies do not add medicines. They set price and availability for catalog items only. | [C] |

- **Edge cases:** EC-24
- **Error states:** ERR-17
- **Acceptance criteria:** AC-33, AC-34
- **Priority:** Must Have

### F-12 — Saydaliyati — Pharmacy Search & Matching

- **Purpose:** show where to get the prescribed medicines, at what price, and how far away.
- **User stories:** US-09, US-10
- **Functional requirements:** FR-064 – FR-070
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-120 | Location uses device GPS as the primary method. If GPS is unavailable or denied, the patient selects an area from a predefined list. Each pharmacy has a map pin **and** is assigned to one area from that list, so an area search shows that area's pharmacies (v1.2, [C]). | [C] |
| BR-121 | Results are sorted by distance. Pharmacies stocking **all** items are listed first. | [C] |
| BR-122 | Each price shows the pharmacy's "last updated" timestamp. | [C] |
| BR-123 | If no single pharmacy has all items, the patient may either **split** the order across several pharmacies or **buy the available items** from one pharmacy and leave the rest pending on the prescription. | [C] |
| BR-124 | Pharmacies outside working hours, or hidden for non-payment, are not shown. | [C] |
| BR-125 | Free-text (non-catalog) items are never matched to pharmacies. | [C] |
| BR-126 | A prescription is valid for **30 days** from issue. After that its pending items cannot be ordered and the patient needs a new consultation. | [D] (QR-16) — pending legal confirmation (Q-14) |
| BR-127 | Each prescription item can be dispensed **once** (no refills in the MVP). An item is locked while it is in an active order, and returns to "Pending" only if that order is rejected, auto-cancelled, cancelled before payment, or cancelled for a stock-out after payment (BR-135). | [D] (QR-16); stock-out case [C] (v1.2) |
| BR-128 | Catalog items carry a **Prescription-only** flag set by the Admin. For these items the pharmacy records the collector's name at pickup and checks that the order matches the patient of record shown. | [D] (QR-16) — pending legal confirmation (Q-14) |
| BR-129 | Each pharmacy order draws items from **exactly one** prescription. Combining several prescriptions in one order is not supported. | [C] (v1.2) |

- **Edge cases:** EC-25 – EC-28, EC-40, EC-43, EC-44
- **Error states:** ERR-18, ERR-19, ERR-26, ERR-27
- **Acceptance criteria:** AC-35 – AC-39, AC-39a – AC-39c
- **Priority:** Must Have

### F-13 — Pharmacy Inventory & Order Confirmation

- **Purpose:** keep data reasonably accurate and guarantee availability before payment.
- **User stories:** US-19, US-20
- **Functional requirements:** FR-071 – FR-078
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-130 | Pharmacies are expected to update price and availability **daily**. The system sends a daily update reminder at opening time and applies the "outdated" badge of BR-134. | [C]/[D] (QR-18) |
| BR-131 | The pharmacy must accept or reject an order within **30 minutes, counted during its working hours**. Otherwise the order is auto-cancelled and the patient may choose another pharmacy. | [C] |
| BR-132 | The patient pays **only after** the pharmacy accepts. The accepted price is the price to pay. | [C] |
| BR-133 | When rejecting, a pharmacy selects a reason: out of stock, partial stock, price changed, or other. Partial acceptance is not supported in the MVP; after a "partial stock" rejection the patient can re-order a subset (FR-068). | [A]/[D] (QR-19) |
| BR-134 | If price or availability has not been updated for more than 48h, the pharmacy is flagged as "outdated" to patients. | [A] |
| BR-135 | If the pharmacy finds a stock-out **after** the patient has paid, it cancels the order and refunds the patient **in full** (BR-093). The items return to "Pending" on the prescription and can be ordered from another pharmacy (BR-127). | [C] (v1.2) — resolves Q-09 |

- **Edge cases:** EC-29 – EC-32, EC-39
- **Error states:** ERR-20, ERR-21
- **Acceptance criteria:** AC-40 – AC-44, AC-41a
- **Priority:** Must Have

### F-14 — Order Pickup

- **Purpose:** secure, flexible pickup.
- **User stories:** US-11, US-21
- **Functional requirements:** FR-079 – FR-083
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-095 | Once payment is confirmed, the patient receives a **pickup code** (short numeric + QR). | [C] |
| BR-096 | Anyone with the code may collect the order, so a relative can pick it up. | [C] |
| BR-097 | Each pharmacy sets its own pickup window, shown before the patient orders. | [C] |
| BR-098 | The patient receives a reminder **every day** until pickup or expiry. | [C] |
| BR-099 | If the window expires, the order becomes "Expired" and the pharmacy refunds the patient **in full** (BR-093). | [C] (v1.2) — resolves Q-04 |

- **Edge cases:** EC-33, EC-34
- **Error states:** ERR-22
- **Acceptance criteria:** AC-45 – AC-47, AC-47a
- **Priority:** Must Have

### F-15 — Cancellations & Refunds

- **Purpose:** a fair, simple policy, even though the platform doesn't hold funds.
- **User stories:** US-08
- **Functional requirements:** FR-084 – FR-087
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-090 | Patient cancels a paid booking **≥ 24h** before start → full refund by the clinic. | [C] |
| BR-091 | Patient cancels **< 24h** before start → no refund. | [C] |
| BR-092 | Clinic or doctor cancels → full refund, always. | [C] |
| BR-093 | Refunds are made by the clinic (or, for pharmacy orders, the pharmacy) directly to the patient's wallet. The refunding party marks "Refunded" in the platform, and the patient is notified. | [C] |
| BR-094 | Patients can cancel in-person bookings at any time without a financial effect, since nothing was prepaid. | [A] |

- **Edge cases:** EC-35, EC-36
- **Error states:** ERR-23
- **Acceptance criteria:** AC-48 – AC-50
- **Priority:** Must Have

### F-16 — Notifications

- **Purpose:** keep every party informed, even with intermittent access.
- **Functional requirements:** FR-088 – FR-090
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-100 | MVP channels are in-app notifications plus PWA push notifications. | [C] |
| BR-101 | Email is used only for account functions (verification, password reset). | [A] |

- **Events:** see FR-089.
- **Edge cases:** EC-37
- **Error states:** ERR-24
- **Acceptance criteria:** AC-51
- **Priority:** Must Have

### F-17 — Partner Subscriptions

- **Purpose:** enforce the revenue model.
- **User stories:** US-24
- **Functional requirements:** FR-091 – FR-094
- **Business rules:**

| ID | Rule | Src |
|---|---|---|
| BR-110 | Patients never pay the platform. | [C] |
| BR-111 | Clinics and pharmacies get a free trial, then a monthly subscription. There is one subscription per clinic (not per doctor) and one per pharmacy. | [C] (per-clinic: v1.2) |
| BR-112 | Trial length and subscription prices are **not defined** (Q-05, Q-06). | — |
| BR-113 | If a subscription is unpaid at the due date, a **7-day grace period** applies. After it, the partner is hidden from new patients, while existing bookings and orders continue normally. | [C] |
| BR-114 | The Admin records subscription payments manually in the MVP. | [C] |
| BR-115 | The free trial starts on the day the Admin approves the clinic or pharmacy. | [C] (v1.2) — resolves Q-02 |

- **Edge cases:** EC-38
- **Acceptance criteria:** AC-52, AC-53
- **Priority:** Should Have. Manual tracking is enough for the pilot, and enforcement can be manual if this slips.

### F-18 — Admin Console

- **Purpose:** operate the marketplace.
- **User stories:** US-22, US-23, US-24
- **Functional requirements:** FR-095 – FR-100
- **Business rules:** BR-004, BR-005, BR-087, BR-113, BR-114
- **Acceptance criteria:** AC-01 – AC-05, AC-33, AC-52, AC-53a
- **Priority:** Must Have

---

## 11. Functional Requirements

### F-01 Clinic & Doctor Onboarding
| ID | Requirement | Src |
|---|---|---|
| FR-001 | The system shall allow a Clinic Manager to self-register a clinic with name, address, map pin, phone, and clinic license document. | [C] |
| FR-002 | The system shall require, for each doctor, a practice license (document and license number, unique across the platform) and a dermatology specialty certificate. | [C]/[A] |
| FR-003 | The system shall set new clinics and doctors to status **Pending Review** and hide them from patients. | [C] |
| FR-004 | The system shall allow a Platform Admin to view the uploaded documents and Approve or Reject, with a mandatory rejection reason. | [C]/[A] |
| FR-005 | The system shall notify the applicant of approval or rejection. | [A] |
| FR-006 | The system shall allow a rejected applicant to edit and resubmit. | [A] |
| FR-007 | The system shall allow an approved Clinic Manager to invite doctors and Clinic Staff by email. | [C] |
| FR-008 | The system shall keep a newly added doctor hidden until the Admin approves their documents. | [C] |
| FR-009 | The system shall allow a Clinic Manager to deactivate a doctor or staff member. Deactivating a doctor (1) cancels their future bookings as clinic cancellations (BR-068, BR-092); (2) closes their open async and live consultations, notifies the patients, and creates a refund task for each paid consultation not yet answered; (3) keeps already-issued prescriptions valid in Saydaliyati. Transferring consultations to another doctor is out of scope for the MVP. | [A]/[D] (QR-15) |

### F-02 Pharmacy Onboarding
| ID | Requirement | Src |
|---|---|---|
| FR-010 | The system shall allow a pharmacy to self-register with name, address, map pin, area (from the predefined list, BR-120), phone, pharmacy license, and pharmacist license. | [C]/[A] |
| FR-011 | The system shall require pharmacy working hours (per weekday) and at least one e-wallet payment identifier. | [A] |
| FR-012 | The system shall apply the same Pending Review → Approve/Reject flow as FR-003 – FR-006. | [C] |
| FR-013 | The system shall allow a pharmacy to temporarily mark itself **Closed** (for example during a power outage). | [A] |

### F-03 Patient Account & Family
| ID | Requirement | Src |
|---|---|---|
| FR-014 | The system shall allow registration with email + password and email verification. | [C]/[A] |
| FR-015 | The system shall provide login, logout, and password reset by email. | [A] |
| FR-016 | The system shall require the account holder to confirm they are 18+. | [C] |
| FR-017 | The system shall allow the account holder to add, edit, and remove family members (name, date of birth, sex, relationship). Removing a member archives it: it no longer appears for new services, and its medical history is kept under the retention policy (Q-11). | [C]/[A]/[D] (QR-19) |
| FR-018 | The system shall require selecting the patient of record (self or a family member) for every consultation, booking, and order. | [C] |
| FR-019 | The system shall show the account holder each family member's history separately. | [A] |

### F-04 Guest Browsing
| ID | Requirement | Src |
|---|---|---|
| FR-020 | The system shall allow unauthenticated users to list and view approved, visible clinics and doctors. | [C] |
| FR-021 | The system shall show each doctor's offered services, prices, and next available slots. | [A] |
| FR-022 | The system shall allow filtering by area and service type. | [A] |
| FR-023 | The system shall prompt guests to register or log in when they attempt a protected action, and return them to that action afterwards. | [A] |

### F-05 Async Message
| ID | Requirement | Src |
|---|---|---|
| FR-024 | The system shall allow doctors to set async mode to Free, Paid (price), or Disabled. | [C] |
| FR-025 | The system shall allow patients to send a case description with photo attachments (BR-047). | [C] |
| FR-026 | For Paid mode, the system shall run the payment flow (F-09) before delivering the case to the doctor. | [A] |
| FR-027 | The system shall deliver messages in real time when both parties are online. | [C] |
| FR-028 | The system shall show the patient the response deadline (24h). | [A] |
| FR-029 | The system shall, at 24h without a doctor response, mark the consultation **Expired – No response**, notify the patient, and, if paid, create a refund obligation for the clinic. | [C] |
| FR-030 | The system shall close the thread automatically 72h after the doctor's first response, and allow the doctor to close it earlier. | [C] |
| FR-030a | The system shall show the patient how many follow-up messages remain (BR-048) and, at zero, disable input with options to book a live chat or start a new consultation (ERR-25). | [D] |
| FR-030b | The system shall let the patient withdraw a paid async case before the doctor opens it, and create a refund task (BR-049). | [D] |
| FR-031 | The system shall not show medical content (text, photos) to Clinic Staff. | [C] |

### F-06 Live Chat
| ID | Requirement | Src |
|---|---|---|
| FR-032 | The system shall allow doctors to set live chat to Free or Paid (price), and the session duration. | [C] |
| FR-033 | The system shall allow a patient to select an available live-chat slot and, for a paid session, hold it pending payment. | [C]/[A] |
| FR-034 | The system shall confirm a paid booking only after payment is confirmed (F-09), and a free booking immediately (BR-052). | [C] |
| FR-035 | The system shall open the chat room at the start time and show both parties a join indicator. | [A] |
| FR-036 | The system shall, if the patient hasn't joined after 15 min, convert the session to an async consultation with a 24h response deadline. | [C] |
| FR-037 | The system shall, if the doctor hasn't joined after 15 min, offer the patient Rebook or Refund. | [C] |
| FR-038 | The system shall end the live session at the configured duration and then apply the 72h follow-up window. | [A] |

### F-07 In-Person Booking
| ID | Requirement | Src |
|---|---|---|
| FR-039 | The system shall allow a patient to book an available in-person slot without prepayment. | [C] |
| FR-040 | The system shall show the clinic address, map pin, and the price payable at the clinic. | [C]/[A] |
| FR-041 | The system shall allow the doctor or staff to mark the visit Completed or No-show. | [A] |
| FR-042 | The system shall enable prescribing only for visits marked Completed. | [C]/[A] |

### F-08 Schedule
| ID | Requirement | Src |
|---|---|---|
| FR-043 | The system shall allow the doctor or Clinic Staff to define recurring weekly availability per slot type. | [C]/[A] |
| FR-044 | The system shall allow one-off blocking of dates or times. | [A] |
| FR-045 | The system shall prevent double-booking of a slot. | [A] |
| FR-046 | The system shall show the doctor and staff a calendar of bookings. | [A] |

### F-09 Payment Verification
| ID | Requirement | Src |
|---|---|---|
| FR-047 | The system shall show the patient the amount, the recipient's wallet identifier(s), and instructions. | [C]/[A] |
| FR-048 | The system shall require the patient to enter the transaction reference number and the wallet used. | [C] |
| FR-049 | The system shall reject a reference number already used with the same wallet provider, and let the patient flag the conflict for Admin review (BR-075). | [A]/[D] |
| FR-050 | The system shall present pending payments to the recipient (doctor or Clinic Staff, or the pharmacy), with Received / Not found actions. | [C] |
| FR-051 | On **Not found**, the system shall notify the patient and allow them to correct the reference once more before the hold is released. | [A] |
| FR-052 | The system shall release the held slot or order when the patient window ends without a reference, and apply BR-078 when the provider window ends without a decision (BR-076). | [D] |
| FR-053 | The system shall log who confirmed each payment and when. | [A] |

### F-10 E-Prescription
| ID | Requirement | Src |
|---|---|---|
| FR-054 | The system shall allow a doctor to create a prescription linked to an eligible service (BR-080). | [C] |
| FR-055 | The system shall provide catalog search by Arabic or English name. | [C]/[A] |
| FR-056 | The system shall allow free-text items flagged "Not available in Saydaliyati", and automatically create a catalog-addition request. | [C] |
| FR-057 | The system shall capture quantity and dosage instructions per item. | [A] |
| FR-058 | The system shall deliver the issued prescription to the patient's Saydaliyati and notify them. | [C] |
| FR-059 | The system shall allow the doctor to cancel a prescription only if no pharmacy order exists for it. | [A] |

### F-11 Catalog
| ID | Requirement | Src |
|---|---|---|
| FR-060 | The system shall allow the Admin to create, edit, and deactivate catalog items (AR name, EN name, form, strength, Prescription-only flag). | [C]/[A]/[D] |
| FR-061 | The system shall list catalog-addition requests to the Admin with Approve/Reject. | [C] |
| FR-062 | The system shall prevent duplicate items with the same EN name + form + strength. | [A] |
| FR-063 | The system shall keep deactivated items on existing prescriptions but hide them from new prescriptions. | [A] |

### F-12 Saydaliyati
| ID | Requirement | Src |
|---|---|---|
| FR-064 | The system shall request GPS location and fall back to area selection. | [C] |
| FR-065 | The system shall list open, visible pharmacies with distance, total price for the available items, availability per item, and last-updated time. | [C] |
| FR-066 | The system shall rank pharmacies with full availability first, each group by distance. | [C] |
| FR-067 | The system shall allow the patient to build a split order (one sub-order per pharmacy). **Should Have**, first to cut (QR-17): without it, the patient can still reach the same result in steps through FR-068. | [C]/[D] |
| FR-068 | The system shall allow the patient to order a subset of items and keep the remaining items "Pending" on the prescription until it expires (BR-126). | [C]/[D] |
| FR-069 | The system shall show each pharmacy's pickup window before ordering. | [C] |
| FR-070 | The system shall show a "no pharmacy available" state with the option to retry later or change area (ERR-18). When matching pharmacies exist but are all outside working hours or Closed, it shall show ERR-26 with the earliest opening time instead. | [A]/[D] (QR-19) |

### F-13 Pharmacy Inventory & Orders
| ID | Requirement | Src |
|---|---|---|
| FR-071 | The system shall allow a pharmacy to set price and in-stock status per catalog item. | [C] |
| FR-072 | The system shall provide a quick daily update screen (bulk confirm of "no change"). | [A] |
| FR-073 | The system shall timestamp every update and display "last updated". | [C] |
| FR-074 | The system shall send new orders to the pharmacy with an Accept / Reject action and a visible countdown. | [C] |
| FR-075 | The system shall count the 30-minute window only during the pharmacy's working hours. Time while the pharmacy is toggled Closed (FR-013) does not count. | [C]/[D] (QR-19) |
| FR-076 | The system shall auto-cancel an order not answered in time and notify the patient. | [C] |
| FR-077 | The system shall show the pharmacy the requested items only, with no diagnosis or other medical content. | [C] |
| FR-078 | The system shall allow the pharmacy to confirm payment (F-09) after acceptance. | [C] |

### F-14 Pickup
| ID | Requirement | Src |
|---|---|---|
| FR-079 | The system shall generate a unique pickup code (numeric + QR) after payment confirmation. | [C] |
| FR-080 | The system shall make the pickup code viewable offline once loaded (PWA cache). | [A] |
| FR-081 | The system shall allow the pharmacy to enter or scan the code and mark the order **Delivered**. | [C] |
| FR-082 | The system shall send a daily reminder until Delivered or Expired. | [C] |
| FR-083 | The system shall mark the order **Expired** at the end of the pickup window. | [C] |

### F-15 Cancellations
| ID | Requirement | Src |
|---|---|---|
| FR-084 | The system shall allow the patient to cancel bookings and show the refund outcome before confirming. | [C] |
| FR-085 | The system shall allow the clinic to cancel bookings with a reason. | [A] |
| FR-086 | The system shall create a refund task for the clinic when BR-090, BR-092, or BR-043 applies, and for the pharmacy when BR-099 or BR-135 applies, and allow it to be marked "Refunded". | [C] |
| FR-087 | The system shall show overdue refund tasks (older than 72h) to the Admin. The Admin follows up with the partner and may suspend it (FR-098) if it still does not refund. | [A] (72h) / [C] (follow-up and suspension, v1.2) |

### F-16 Notifications
| ID | Requirement | Src |
|---|---|---|
| FR-088 | The system shall provide an in-app notification center and PWA push notifications. | [C] |
| FR-089 | The system shall notify on these events: doctor reply; booking confirmed, cancelled, or reminded; payment confirmed or not found; prescription issued; pharmacy accepted, rejected, or auto-cancelled; pickup code issued; daily pickup reminder; refund marked; approval or rejection; subscription due or hidden. | [C]/[A] |
| FR-090 | The system shall send a booking reminder before each live chat and in-person appointment (timing in Q-12). | [A] |

### F-17 Subscriptions
| ID | Requirement | Src |
|---|---|---|
| FR-091 | The system shall track per partner: trial start/end, status (Trial / Active / Grace / Hidden), and next due date. | [C] |
| FR-092 | The system shall allow the Admin to record a subscription payment manually. | [C] |
| FR-093 | The system shall move a partner to Grace at the due date and to Hidden after 7 days unpaid. | [C] |
| FR-094 | The system shall keep existing bookings and orders of Hidden partners fully functional. | [C] |

### F-18 Admin Console
| ID | Requirement | Src |
|---|---|---|
| FR-095 | The system shall provide a verification queue (clinics, doctors, pharmacies). | [C] |
| FR-096 | The system shall provide catalog management and addition requests. | [C] |
| FR-097 | The system shall provide partner subscription management. | [C] |
| FR-098 | The system shall allow the Admin to suspend any partner or patient account, with a reason. | [A] |
| FR-099 | The system shall **not** give the Admin general access to medical content (messages, photos, diagnoses). The only exception is FR-100. | [A]/[D] |
| FR-100 | The system shall let a patient or doctor **report** a message or consultation (reason: abuse, harassment, impersonation, inappropriate content, other). The report shares only the reported messages and their attachments with Admins holding the **Report Reviewer** permission, for 14 days. Every view is audit-logged. | [D] (QR-14) — pending legal confirmation (Q-10) |

---

## 12. Non-Functional Requirements

| ID | Category | Requirement | Src |
|---|---|---|---|
| NFR-001 | Platform | Responsive web app; installable PWA on Android and iOS browsers with push notifications. | [C] |
| NFR-002 | Localization | Arabic UI, full RTL layout; medicine names in AR + EN; Western digits (0–9) everywhere, matching wallet references and phone numbers. Arabic-Indic digits typed by users are accepted and normalized. | [C]/[D] (QR-18) |
| NFR-003 | Performance | Largest Contentful Paint ≤ 3 s at the 75th percentile on a mid-range Android phone over a throttled 3G profile (1.6 Mbps down, 300 ms RTT), measured with Lighthouse; transferred page weight ≤ 1 MB excluding user images. | [A]/[D] (QR-18) |
| NFR-004 | Resilience | Unsent messages and form data are kept locally and retried when the connection returns. | [A] |
| NFR-005 | Offline | Pickup codes, the last-loaded prescriptions, and booking details are viewable offline. | [A] |
| NFR-006 | Images | Photos are compressed client-side before upload to ≤ 1 MB, keeping the long edge ≥ 1600 px and JPEG quality ≥ 80. | [A]/[D] (QR-18) |
| NFR-007 | Availability | 99.5% monthly uptime for the platform (excluding the user's own connectivity). | [A] |
| NFR-008 | Real-time | Chat messages are delivered in ≤ 2 s when both parties are online. | [A] |
| NFR-009 | Time | All times use the Asia/Gaza timezone; SLA timers are server-side. | [A] |
| NFR-010 | Accessibility | WCAG 2.1 AA for contrast, font scaling, and touch targets. | [A] |
| NFR-011 | Auditability | Approvals, payment confirmations, refunds, and prescription issuance are audit-logged (who, what, when). | [A] |
| NFR-012 | Scalability | Supports pilot scale (≥ 50 clinics, 100 pharmacies, 10,000 patients), verified by a load test with 500 concurrent users: p95 API response < 1 s and error rate < 1%. | [A]/[D] (QR-18) |
| NFR-013 | Tech stack | Next.js (PWA) + Supabase (PostgreSQL, Auth, Storage, Realtime). | [D] |
| NFR-014 | Browsers | Latest 2 versions of Chrome (Android), Safari (iOS), Chrome/Edge (desktop). | [A] |

---

## 13. User Flows

### UF-1 Patient: async message → prescription → pickup (core journey)
1. A guest browses doctors (F-04) and opens a doctor's profile.
2. They tap "Send a message" and are asked to log in or register (F-03). After that they return to the same doctor.
3. They select the patient of record (self or a family member).
4. They write the case and attach photos.
5. **If the doctor's async mode is Paid:** the payment screen appears, the patient pays in the wallet app and enters the reference, and the clinic confirms (F-09). **If Free:** the case is sent immediately.
6. The doctor replies within 24h. Otherwise the consultation expires, the patient is notified, and a refund task is created if it was paid.
7. The follow-up window of 72h starts. The doctor may issue a prescription (F-10).
8. The patient opens Saydaliyati, allows GPS or picks an area, and sees the ranked pharmacies (F-12).
9. The patient picks a pharmacy, or splits the order, or buys a subset → the order is sent.
10. The pharmacy accepts within 30 working minutes, or the order auto-cancels and the patient returns to step 8.
11. The patient pays the pharmacy's wallet and enters the reference. The pharmacy confirms.
12. The pickup code is issued, and daily reminders run until pickup.
13. The pharmacy scans or enters the code and marks the order **Delivered**.

### UF-2 Patient: booked live chat
1. The patient selects a doctor, then the Live chat service, then a slot. The slot is held.
2. Payment flow (F-09). Once confirmed, the booking is Confirmed and a reminder is scheduled.
3. At the start time both parties join.
   - Patient absent after 15 min → the session converts to async with a 24h reply deadline (no refund).
   - Doctor absent after 15 min → the patient chooses Rebook or Refund.
4. The session ends at its duration, then the 72h follow-up window applies, and a prescription may be issued.

### UF-3 Patient: in-person visit
1. The patient selects the doctor, then In-person, then a slot. The booking is Confirmed (no prepayment).
2. The patient visits the clinic and pays there.
3. Staff or the doctor marks the visit Completed or No-show.
4. If Completed, the doctor may issue an e-prescription and the flow continues with UF-1 steps 8–13.

### UF-4 Patient cancellation
1. The patient opens the booking and taps Cancel. The system shows the outcome: refund (≥ 24h) or no refund (< 24h).
2. When the patient confirms, the booking is Cancelled, and a refund task is created if applicable.
3. The clinic refunds the wallet and marks it Refunded. The patient is notified.

### UF-5 Clinic onboarding
1. The Clinic Manager registers the clinic with documents and the map pin. Status: Pending.
2. The Admin reviews and approves, or rejects with a reason (the applicant can fix and resubmit).
3. Once approved, the clinic sets wallet identifiers, invites doctors and staff, and sets schedules and prices.
4. Each doctor is reviewed by the Admin before going visible.
5. The trial starts. At its end the subscription is due, followed by the grace period and then hiding if unpaid.

### UF-6 Pharmacy onboarding & daily operations
1. Registration → Admin review (as in UF-5).
2. The pharmacy sets working hours and wallet identifiers, then sets price and stock for catalog items.
3. Daily: confirm or update prices and stock.
4. Handle incoming orders (Accept/Reject), confirm payments, and deliver against pickup codes.

### UF-7 Admin
1. Process the verification queue.
2. Process catalog-addition requests.
3. Record subscription payments, and monitor overdue refunds and suspensions.

---

## 14. Pages / Screens

*(Inventory only — no UI design.)*

| # | Screen | Roles |
|---|---|---|
| S-01 | Home / doctor & clinic listing (filters: area, service) | Guest, Patient |
| S-02 | Clinic profile (address, map, doctors) | Guest, Patient |
| S-03 | Doctor profile (services, prices, availability) | Guest, Patient |
| S-04 | Register / Login / Verify email / Reset password | Guest |
| S-05 | Patient profile & family members | Patient |
| S-06 | New async message (patient selector, text, photos) | Patient |
| S-07 | Slot picker (live chat / in-person) | Patient |
| S-08 | Payment instructions & reference entry | Patient |
| S-09 | My consultations & bookings (by family member) | Patient |
| S-10 | Conversation thread (async/live) | Patient, Doctor |
| S-11 | Saydaliyati — prescriptions list | Patient |
| S-12 | Saydaliyati — pharmacy results (location, ranking, split/subset) | Patient |
| S-13 | Order status & pickup code (offline-capable) | Patient |
| S-14 | Notification center | All logged-in roles |
| S-15 | Clinic registration wizard | Clinic Manager |
| S-16 | Clinic dashboard (doctors, staff, wallet IDs, settings) | Clinic Manager |
| S-17 | Schedule / calendar management | Doctor, Clinic Staff |
| S-18 | Pending payments & refund tasks | Doctor, Clinic Staff |
| S-19 | Doctor inbox (async + live sessions) | Doctor |
| S-20 | Prescription editor (catalog search + free text) | Doctor |
| S-21 | Doctor service settings (async mode/price, live price/duration, in-person price) | Doctor |
| S-22 | Pharmacy registration wizard | Pharmacy |
| S-23 | Pharmacy inventory (daily update) | Pharmacy |
| S-24 | Pharmacy orders (countdown, accept/reject, payment confirm, pickup verify) | Pharmacy |
| S-25 | Pharmacy settings (working hours, pickup window, wallet IDs, Closed toggle) | Pharmacy |
| S-26 | Admin verification queue | Admin |
| S-27 | Admin catalog & addition requests | Admin |
| S-28 | Admin partners & subscriptions | Admin |
| S-29 | Admin overdue refunds & suspensions | Admin |

---

## 15. Roles & Permissions

| Capability | Guest | Patient | Doctor | Clinic Staff | Clinic Manager | Pharmacy | Admin |
|---|---|---|---|---|---|---|---|
| Browse clinics/doctors | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Send message / book | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Manage family members | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Read medical content (messages, photos) | ❌ | Own family only | Own patients' consultations only | ❌ | ❌ [A] (✅ as Doctor if dual role, BR-008) | ❌ | ❌ [A] (reported content only, FR-100) |
| Reply to consultations | ❌ | ✅ (own) | ✅ (own) | ❌ | ❌ | ❌ | ❌ |
| Issue prescriptions | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| View prescription items | ❌ | Own | Own issued | ❌ | ❌ | Items of orders sent to it only | ❌ |
| Manage schedule | ❌ | ❌ | ✅ own | ✅ clinic doctors | ✅ [A] | ❌ | ❌ |
| Confirm clinic payments / mark refunds | ❌ | ❌ | ✅ | ✅ | ✅ [A] | ❌ | ❌ |
| Set service prices / async mode | ❌ | ❌ | ✅ own | ❌ [A] | ❌ [A] | ❌ | ❌ |
| Invite doctors/staff | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ | ❌ |
| Manage inventory, orders, pickup | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| Approve partners | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Manage catalog | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Manage subscriptions | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Suspend accounts | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ [A] |
| Report a message or consultation | ❌ | ✅ own | ✅ own | ❌ | ❌ | ❌ | ❌ |
| Review reported content (FR-100) | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ Report Reviewer only [D] |

**Admin permissions:** all Admins share the same permissions, except **Report Reviewer**, which only named Admins hold. [C] (v1.2)

**Resolved permissions:** Clinic Manager dual role (BR-008, v1.1); Admin access to reported content only (FR-100, v1.1); one shared pharmacy account (BR-014, v1.2).

---

## 16. Data Requirements

*(Logical data entities only — not an ERD.)*

| Entity | Key attributes | Notes / retention |
|---|---|---|
| User account | email, password hash, role(s), phone, status, 18+ confirmation | [C]/[A] |
| Family member | account holder, name, DOB, sex, relationship, archived flag | [C]/[D] |
| Clinic | name, address, geo-pin, phone, license doc, status, wallet IDs, subscription status | [C] |
| Doctor | user, clinic, practice license no. (unique), practice license doc, specialty certificate doc, status, service settings (async mode/price, live price/duration, in-person price) | [C] |
| Clinic staff | user, clinic | [C] |
| Pharmacy | name, address, geo-pin, phone, license docs, working hours, pickup window, wallet IDs, Closed flag, subscription status | [C]/[A] |
| Catalog item | AR name, EN name, form, strength, prescription-only flag, active flag | [C]/[D] |
| Catalog addition request | requested text, doctor, status | [C] |
| Pharmacy stock | pharmacy, catalog item, price (ILS), in-stock, updated_at | [C] |
| Availability slot | doctor, type (live / in-person), start, end, status | [C]/[A] |
| Consultation | type (async / live / in-person), patient of record, doctor, status, timestamps, SLA deadline, closes_at | [C] |
| Message | consultation, sender, text, attachments, sent_at | Medical data |
| Attachment (photo) | file ref, consultation | Medical data; private storage |
| Payment record | payer, payee, amount, wallet, reference no. (unique per wallet), status, confirmed_by, confirmed_at | [C] |
| Prescription | consultation, doctor, patient of record, items (catalog or free-text, qty, dosage, dispensed status), status, expires_at | [C]/[D] |
| Pharmacy order | prescription, pharmacy, items, accepted price, status, accept deadline, pickup code, pickup expiry, payment-unverified flag, collector name (prescription-only items) | [C]/[D] |
| Refund task | related booking/consultation, amount, status, marked_by | [C] |
| Subscription | partner, plan, trial end, due date, status, payments | [C] |
| Notification | user, event, read flag | [C] |
| Report | reporter, consultation, reported messages, reason, status, reviewer, access expiry | [D]; medical data |
| Audit log | actor, action, target, timestamp | [A] |

**Retention policy for medical data:** not defined (Q-11).

---

## 17. Integrations

| ID | Integration | MVP? | Notes |
|---|---|---|---|
| INT-1 | E-wallets (PalPay, Jawwal Pay, Bank of Palestine) | **No API.** Manual reference entry + manual confirmation | API integration in Phase 2, subject to provider availability (DEP-1). [C] |
| INT-2 | Maps / geolocation (map pin, distance) | Yes | Browser Geolocation API + a map tile provider; area list maintained by Admin. [A] |
| INT-3 | Web Push (PWA) | Yes | Note: iOS supports web push only when the PWA is installed to the home screen. [A] |
| INT-4 | Transactional email (verification, reset) | Yes | Any email provider. [A] |
| INT-5 | Backend services (DB, auth, storage, realtime) | Yes | Supabase [D] |

---

## 18. Edge Cases

| ID | Edge case | Expected behavior | Feature |
|---|---|---|---|
| EC-01 | A clinic registers with documents for some doctors missing. | The clinic can be approved. Only doctors with complete, approved documents are visible. A clinic with zero visible doctors is not listed. [A] | F-01 |
| EC-02 | The same doctor is registered by two clinics. | Blocked by BR-006 (unique practice license number). [A] | F-01 |
| EC-03 | A doctor's license expires. | Out of scope for the MVP. License expiry tracking is listed in §24. [A] | F-01 |
| EC-04 | A clinic changes address (displacement). | The address change requires Admin re-review. The clinic stays visible meanwhile, with the old address flagged. [A] | F-01 |
| EC-05 | A pharmacy relocates or closes temporarily. | Uses the Closed toggle (FR-013). Closed pharmacies are excluded from results. | F-02 |
| EC-06 | A minor wants to use the platform alone. | Not allowed. The minor must be added under an adult. | F-03 |
| EC-07 | A family member turns 18. | Remains under the parent's account. Splitting the history is out of scope. [A] | F-03 |
| EC-08 | A doctor has no free slots. | The profile shows "No availability"; async messaging is still available if enabled. | F-04 |
| EC-09 | A doctor switches async mode from Free to Paid while free cases are open. | Open cases keep their original terms. [A] | F-05 |
| EC-10 | The doctor goes offline mid-thread (power cut). | Messages queue. The 24h SLA only governs the first response; follow-ups are best-effort within 72h. [A] | F-05 |
| EC-11 | The patient sends follow-ups after the 72h closure. | The input is disabled with the prompt "Start a new consultation". | F-05 |
| EC-12 | A photo upload fails on a weak connection. | Retry with local caching (NFR-004); the message isn't sent until all attachments upload, or the user removes the failed one. | F-05 |
| EC-13 | Both the patient and the doctor fail to join. | Treated as a doctor no-show (the patient's protection prevails). [A] | F-06 |
| EC-14 | Connection drops mid-session. | The chat persists; the remaining conversation continues asynchronously. The session isn't extended. [A] | F-06 |
| EC-15 | The doctor joins late (within 15 min). | The session runs until the original end time. [A] | F-06 |
| EC-16 | The patient doesn't show for an in-person visit. | Marked No-show; no prescription can be issued. | F-07 |
| EC-17 | Staff and doctor edit the schedule at the same time. | Last write wins, with a conflict check against existing bookings (FR-045). | F-08 |
| EC-18 | The patient enters a wrong reference. | The recipient marks Not found → the patient may correct it once (FR-051). | F-09 |
| EC-19 | The patient pays the wrong amount. | The recipient marks Not found and states the reason. Settling the difference happens off-platform, between the two parties (BR-079). | F-09 |
| EC-20 | The clinic never confirms a valid payment. | When the provider window ends, BR-078 applies (the patient's service proceeds) and the case is flagged to the Admin. | F-09 |
| EC-21 | Two patients race for the same slot. | The first to *select* the slot holds it (FR-033) for the patient window of BR-076; the other sees "slot no longer available". | F-09 |
| EC-22 | The prescription has only free-text items. | Saydaliyati shows the prescription with all items marked "Not available in Saydaliyati". | F-10 |
| EC-23 | The doctor prescribes for the wrong family member. | The doctor cancels the prescription (if no order exists) and reissues. | F-10 |
| EC-24 | The Admin deactivates a catalog item that is on open orders. | Existing orders proceed; the item disappears from new prescriptions and searches. | F-11 |
| EC-25 | GPS is denied or inaccurate. | Area selection is offered. | F-12 |
| EC-26 | No pharmacy has any item. | ERR-18 state. | F-12 |
| EC-27 | A pharmacy's price data is older than 48h. | Shown with an "outdated" badge; still orderable, since the pharmacy confirms the price anyway. [A] | F-12 |
| EC-28 | The patient splits the order, and one sub-order is rejected. | The other sub-orders proceed; the rejected items return to "Pending". | F-12 |
| EC-29 | An order arrives 10 min before closing time. | 10 min count today; the remaining 20 min count after opening the next working day. | F-13 |
| EC-30 | The pharmacy accepts at a different price than listed. | The accepted price prevails; the patient may decline before paying. [A] | F-13 |
| EC-31 | The pharmacy runs out after accepting and after payment. | The pharmacy cancels the order and refunds in full (BR-135). | F-13 |
| EC-32 | The patient never pays after acceptance. | The order is released when the 30-minute patient window ends (BR-076), and its items return to "Pending". | F-13 |
| EC-33 | The patient loses their phone or code. | The code is retrievable after logging in on any device. | F-14 |
| EC-34 | The pickup window expires. | Status becomes Expired; the pharmacy refunds in full (BR-099). | F-14 |
| EC-35 | Cancellation at exactly 24h before start. | Counts as ≥ 24h (refund). [A] | F-15 |
| EC-36 | The clinic never marks a refund. | Shown to the Admin as overdue after 72h; the Admin follows up and may suspend the clinic (FR-087). | F-15 |
| EC-37 | Push notifications are not permitted, or iOS without PWA install. | In-app notifications only; the UI nudges the user to install the PWA and enable push. | F-16 |
| EC-38 | A partner is hidden while a patient is mid-booking. | The booking in progress may complete. [A] | F-17 |
| EC-39 | The pharmacy toggles Closed during an order's 30-minute countdown. | The countdown pauses and resumes when the pharmacy reopens (FR-075). [D] | F-13 |
| EC-40 | A patient searches at night and every matching pharmacy is closed. | ERR-26 with the earliest opening time, not ERR-18 (FR-070). [D] | F-12 |
| EC-41 | A live session converted to async (patient no-show) gets no doctor reply within 24h. | Full refund (BR-057). [D] | F-06 |
| EC-42 | A doctor is deactivated with open consultations. | Handled by FR-009: consultations closed, patients notified, refund tasks for unanswered paid cases. [D] | F-01 |
| EC-43 | A prescription expires with items still Pending. | Those items show as Expired and cannot be ordered (BR-126, ERR-27). [D] | F-12 |
| EC-44 | The patient tries to order an item that was already delivered. | Blocked (BR-127). [D] | F-12 |
| EC-45 | A patient's reference is rejected as already used by someone else. | The patient flags "this is my payment" and the Admin reviews (BR-075). [D] | F-09 |
| EC-46 | The account holder removes a family member who has history. | The member is archived and the history is kept (FR-017). [D] | F-03 |
| EC-47 | The patient has used all 3 follow-up messages. | Input disabled; ERR-25 with options to book a live chat or start a new consultation (BR-048). [D] | F-05 |

---

## 19. Error States

| ID | Trigger | User-facing message (intent) | Recovery |
|---|---|---|---|
| ERR-01 | Required document missing or invalid file type/size | "Please upload [document] (PDF/JPG/PNG, max 10 MB)" | Re-upload |
| ERR-02 | Registration rejected by Admin | "Your application was not approved: [reason]" | Edit & resubmit |
| ERR-03 | Map pin not set | "Set your location on the map" | Set pin |
| ERR-04 | Email already registered | "This email is already registered" | Login / reset |
| ERR-05 | Email not verified when booking | "Verify your email to continue" | Resend verification |
| ERR-06 | Listing fails to load (offline) | "You're offline — showing last loaded results" | Auto-retry |
| ERR-07 | Async messaging disabled by the doctor | "This doctor isn't accepting messages" | Book a live chat/in-person, or another doctor |
| ERR-08 | Photo upload failed | "Photo failed to upload — retry" | Retry/remove |
| ERR-09 | Message after thread closed | "This consultation is closed" | New consultation |
| ERR-10 | Slot taken | "This time is no longer available" | Pick another slot |
| ERR-11 | Live session join failure | "Connection problem — your messages will be delivered when you reconnect" | Auto-reconnect |
| ERR-12 | Schedule conflict with existing booking | "This change affects N bookings" | Cancel bookings (clinic cancellation) or abort |
| ERR-13 | Reference already used | "This transaction number was already used" | Re-enter, or flag "this is my payment" (BR-075) |
| ERR-14 | Payment marked Not found | "The clinic/pharmacy couldn't find your payment" | Correct reference once |
| ERR-15 | Payment window expired | "Your reservation expired because payment wasn't confirmed" | Start again |
| ERR-16 | Prescribing on an ineligible service | "Prescriptions can only follow a completed consultation on the platform" | — |
| ERR-17 | Duplicate catalog item | "This medicine already exists" | Edit existing |
| ERR-18 | No pharmacy has the items | "No nearby pharmacy currently has these medicines" | Change area / retry later / notify me (future) |
| ERR-19 | Location unavailable | "We couldn't get your location — choose your area" | Area picker |
| ERR-20 | Pharmacy rejected order | "The pharmacy couldn't fulfil your order: [reason]" | Choose another pharmacy |
| ERR-21 | Pharmacy didn't respond in time | "The pharmacy didn't respond in time" | Choose another pharmacy |
| ERR-22 | Invalid pickup code | "Code not valid for this pharmacy" | Re-enter / check order |
| ERR-23 | Cancel after session start | "This booking can no longer be cancelled" | — |
| ERR-24 | Push permission denied | "Turn on notifications to get doctor replies quickly" | Settings guidance |
| ERR-25 | Follow-up limit reached | "You've used your 3 follow-up messages" | Book a live chat / start a new consultation |
| ERR-26 | All matching pharmacies closed | "Nearby pharmacies with these medicines are closed now. The first opens at [time]" | Retry later / change area |
| ERR-27 | Prescription expired | "This prescription has expired" | Start a new consultation |
| ERR-28 | In-person booking blocked after no-shows | "In-person booking is paused on your account until [date]" | Use async or live chat / contact support |

---

## 20. Security & Privacy Requirements

| ID | Requirement | Src |
|---|---|---|
| SEC-01 | Medical content (messages, photos, diagnoses, prescriptions) is accessible **only** to the patient account holder and the treating doctor of that consultation. Exceptions: pharmacies see order items as defined in SEC-02; Report Reviewers see reported content as defined in FR-100. | [C]/[D] (QR-20) |
| SEC-02 | Pharmacies see only the items, quantities, and patient of record name for orders sent to them, with no diagnosis or photos. | [C] |
| SEC-03 | Clinic Staff see appointments and payments, but no medical content. | [C] |
| SEC-04 | Authorization is enforced server-side (row-level security), not only in the UI. | [A] |
| SEC-05 | Medical photos are stored in private storage and served via short-lived signed URLs; they are never public. | [A] |
| SEC-06 | All traffic uses TLS 1.2+; data at rest is encrypted. | [A] |
| SEC-07 | Passwords: minimum 8 characters; hashed with an industry-standard algorithm; login rate-limited. | [A] |
| SEC-08 | Partner documents (licenses) are visible only to the uploading partner and Admins. | [A] |
| SEC-09 | Audit log for access-sensitive actions (NFR-011). | [A] |
| SEC-10 | Location is used only at search time and is not stored beyond the session. | [A] |
| SEC-11 | Patients can request account deletion. Medical-record retention obligations apply (Q-11). | [A] |
| SEC-12 | Applicable Palestinian telemedicine, e-prescription, and data-protection regulations must be confirmed before launch (RISK-3). | [A] |
| SEC-13 | Reported-content access (FR-100) is limited to the reported messages, expires after 14 days, and every view is audit-logged. | [D] |

---

## 21. Analytics & Tracking

Events to instrument (no third-party sharing of medical data):

| Event | Purpose |
|---|---|
| `guest_view_doctor`, `signup_completed` | Acquisition funnel |
| `async_sent` (free/paid), `async_first_response` (latency), `async_expired_no_response` | SLA compliance |
| `live_booked`, `live_patient_noshow`, `live_doctor_noshow`, `inperson_booked`, `inperson_completed` | Service usage |
| `payment_reference_submitted`, `payment_confirmed` (latency), `payment_not_found` | Payment friction |
| `prescription_issued` (catalog vs free-text ratio) | Catalog coverage |
| `saydaliyati_search` (GPS vs area), `no_pharmacy_result` | Availability gaps |
| `order_sent`, `order_accepted` (latency), `order_rejected` (reason), `order_auto_cancelled` | Pharmacy responsiveness |
| `order_delivered`, `order_expired` | Fulfilment |
| `booking_cancelled` (≥/<24h, by whom), `refund_marked` (latency) | Policy health |
| `inventory_updated` (per pharmacy per day) | Data freshness |
| `partner_status_changed` (Trial/Active/Grace/Hidden) | Revenue |

---

## 22. Success Metrics / KPIs

**Targets were not set by the product owner** ("not defined yet"). These are the proposed KPIs to track; targets are an open question (Q-13).

| KPI | Definition |
|---|---|
| Active partners | # approved, visible clinics and pharmacies |
| Registered patients | # verified patient accounts |
| Async SLA compliance | % first responses within 24h |
| Prescription → order conversion | % prescriptions with ≥ 1 delivered order |
| Pharmacy acceptance rate | % orders accepted within 30 working minutes |
| Pickup completion | % paid orders delivered before expiry |
| Payment confirmation time | median time from reference entry to confirmation |
| Inventory freshness | % pharmacies updated in the last 24h |
| Trial → paid conversion | % partners paying after trial |
| Catalog coverage | % prescription items from catalog (vs free text) |

---

## 23. Acceptance Criteria

### F-01 Clinic & Doctor Onboarding
- **AC-01:** Given a Clinic Manager on the registration form, when they submit without a clinic license, then submission is blocked and ERR-01 is shown.
- **AC-02:** Given a submitted clinic, when it is in Pending Review, then it does not appear in any patient or guest listing.
- **AC-03:** Given a pending clinic, when an Admin rejects it without a reason, then the system blocks the action until a reason is entered.
- **AC-04:** Given an approved clinic, when the Manager adds a new doctor, then that doctor is not visible to patients until an Admin approves the doctor's practice license and specialty certificate.
- **AC-05:** Given a clinic registration, when no map pin is set, then submission is blocked (ERR-03).
- **AC-05a:** Given a doctor with an unanswered paid async case, when the Clinic Manager deactivates the doctor, then the case is closed, the patient is notified, and a refund task is created.

### F-02 Pharmacy Onboarding
- **AC-06:** Given a pharmacy registration, when working hours or wallet identifier are missing, then submission is blocked.
- **AC-07:** Given an approved pharmacy toggled to Closed, when a patient searches Saydaliyati, then that pharmacy is not listed.

### F-03 Patient Account
- **AC-08:** Given a guest viewing a doctor, when they tap "Send message", then they are asked to log in/register and, after success, return to the same doctor's message screen.
- **AC-09:** Given a patient with two children added, when starting a consultation, then they must select one patient of record before proceeding.
- **AC-10:** Given registration, when the user does not confirm 18+, then the account cannot be created.

### F-04 Browsing
- **AC-11:** Given a clinic in Hidden subscription status, when a guest browses, then the clinic and its doctors are not listed.

### F-05 Async Message
- **AC-12:** Given a doctor with async mode Disabled, when a patient opens the profile, then "Send message" is unavailable (ERR-07).
- **AC-13:** Given a doctor with async mode Free, when a patient sends a case with 3 photos, then the doctor receives it immediately and the patient sees a 24h response deadline.
- **AC-14:** Given a doctor with async mode Paid, when the patient submits a case, then it is not delivered to the doctor until the clinic marks the payment Received.
- **AC-15:** Given an async case with no doctor reply, when 24h elapse, then the status becomes "Expired – No response", the patient is notified, and, if paid, a refund task is created for the clinic.
- **AC-16:** Given the doctor's first reply at time T, when T + 72h is reached, then the thread closes and the patient cannot send further messages (ERR-09).
- **AC-16a:** Given an open follow-up window where the patient has sent 3 follow-ups, when they try to send a 4th, then input is disabled and ERR-25 is shown; the doctor can still reply.
- **AC-17:** Given an open async thread, when a Clinic Staff user opens the clinic dashboard, then no message text or photos are visible to them.
- **AC-17a:** Given a paid async case with payment Received and not yet opened by the doctor, when the patient withdraws it, then the case is cancelled and a refund task is created. Once the doctor has opened it, withdrawal is unavailable.

### F-06 Live Chat
- **AC-18:** Given a held paid live-chat slot, when payment is not confirmed, then the booking is not Confirmed and the slot is not shown as available to others while the hold lasts.
- **AC-19:** Given a confirmed live session, when the patient has not joined 15 minutes after start, then the session converts to async with a 24h response deadline and no refund task is created.
- **AC-19a:** Given a live session converted to async under BR-053, when 24h pass without a doctor reply, then a refund task is created for the clinic.
- **AC-20:** Given a confirmed live session, when the doctor has not joined 15 minutes after start, then the patient is offered Rebook or Refund, and choosing Refund creates a refund task.
- **AC-21:** Given a live session with a 20-minute duration, when 20 minutes elapse from start, then the live session ends and the 72h follow-up window begins.

### F-07 In-Person
- **AC-22:** Given an available in-person slot, when the patient books it, then it is confirmed without any payment step and shows the clinic address and price payable at the clinic.
- **AC-23:** Given an in-person visit marked No-show, when the doctor tries to create a prescription for it, then the action is blocked (ERR-16).
- **AC-23a:** Given an account with 3 in-person no-shows in the last 90 days, when it tries to book an in-person slot, then booking is blocked and ERR-28 is shown.

### F-08 Schedule
- **AC-24:** Given Clinic Staff of clinic X, when they edit doctor Y's schedule at clinic X, then the change is saved; given staff of clinic Z, then access is denied.
- **AC-25:** Given a slot booked by patient A, when patient B tries to book the same slot, then B receives ERR-10.

### F-09 Payment Verification
- **AC-26:** Given a payment screen, when the patient submits without a reference number, then submission is blocked.
- **AC-27:** Given PalPay reference "12345" already used, when another PalPay payment is submitted with "12345", then ERR-13 is shown; the same reference with Jawwal Pay is accepted.
- **AC-28:** Given a pending payment, when the recipient marks Not found, then the patient is notified and can correct the reference exactly once.
- **AC-29:** Given a confirmed payment, when viewed in the audit log, then the confirming user and timestamp are recorded.
- **AC-29a:** Given a submitted reference for a paid async case, when 2 hours pass without the clinic's decision, then the case is delivered to the doctor, the 24h clock starts, and the case is flagged to the Admin.
- **AC-29b:** Given a held slot, when 30 minutes pass without a submitted reference, then the hold is released and the slot is available again.

### F-10 E-Prescription
- **AC-30:** Given a completed platform consultation, when the doctor issues a prescription with 2 catalog items, then it appears in the patient's Saydaliyati and the patient is notified.
- **AC-31:** Given a doctor adding a medicine not in the catalog, when they enter it as free text, then it appears to the patient as "Not available in Saydaliyati" and a catalog-addition request appears in the Admin queue.
- **AC-32:** Given a Clinic Staff user, when they attempt to create a prescription, then access is denied.

### F-11 Catalog
- **AC-33:** Given an Admin, when creating an item with the same EN name, form, and strength as an existing item, then ERR-17 is shown.
- **AC-34:** Given a catalog item, when displayed to patients, doctors, or pharmacies, then both Arabic and English names are shown.

### F-12 Saydaliyati
- **AC-35:** Given GPS permission denied, when the patient opens pharmacy results, then an area picker is shown and results are sorted by distance from the chosen area.
- **AC-36:** Given 3 prescribed items, pharmacy A (2 km) with all 3 and pharmacy B (1 km) with 2, when results load, then A is listed before B.
- **AC-37:** Given each listed pharmacy, when results load, then each shows total price for available items and a "last updated" timestamp.
- **AC-38:** Given no pharmacy with all items, when the patient chooses Split, then one sub-order per selected pharmacy is created; when they choose Subset, then unordered items remain "Pending" on the prescription.
- **AC-39:** Given a pharmacy outside its working hours, when a patient searches, then it is not listed.
- **AC-39a:** Given a prescription issued 31 days ago with pending items, when the patient opens it in Saydaliyati, then ordering is unavailable and ERR-27 is shown.
- **AC-39b:** Given a prescription item already delivered, when the patient tries to order it again, then the action is blocked.
- **AC-39c:** Given matching pharmacies that are all outside working hours, when the patient searches, then ERR-26 is shown with the earliest opening time, not ERR-18.

### F-13 Pharmacy Orders
- **AC-40:** Given a new order at 10:00 during working hours, when the pharmacy has not responded by 10:30, then the order auto-cancels and the patient is notified (ERR-21).
- **AC-41:** Given an order received 10 minutes before closing, when the pharmacy reopens next working day, then the remaining 20 minutes count from opening time.
- **AC-41a:** Given an order with 20 minutes left on its countdown, when the pharmacy toggles Closed for 1 hour and then reopens, then 20 minutes remain.
- **AC-42:** Given a sent order, when the pharmacy has not yet accepted, then the patient sees no payment option.
- **AC-43:** Given a pharmacy viewing an order, when they open it, then they see only items, quantities, and patient of record name, with no diagnosis or photos.
- **AC-44:** Given a pharmacy price update, when saved, then the "last updated" timestamp shown to patients reflects the save time.

### F-14 Pickup
- **AC-45:** Given a pharmacy-confirmed payment, when confirmation is saved, then a unique pickup code (numeric + QR) is shown to the patient and remains viewable offline.
- **AC-46:** Given a paid order not yet collected, when each day passes within the pickup window, then the patient receives one reminder per day.
- **AC-47:** Given a pharmacy entering a code belonging to another pharmacy's order, when submitted, then ERR-22 is shown and the order isn't delivered.
- **AC-47a:** Given an order with a Prescription-only item, when the pharmacy marks it Delivered, then the collector's name is required.

### F-15 Cancellations
- **AC-48:** Given a paid booking starting in 30 hours, when the patient cancels, then the system shows "full refund", and on confirmation a refund task is created.
- **AC-49:** Given a paid booking starting in 10 hours, when the patient cancels, then the system shows "no refund" before confirmation and no refund task is created.
- **AC-50:** Given a clinic cancelling any paid booking, when confirmed, then a refund task is created regardless of timing.

### F-16 Notifications
- **AC-51:** Given a patient with push enabled, when the doctor replies, then the patient receives a push notification and an in-app notification.

### F-17 Subscriptions
- **AC-52:** Given a partner whose subscription is due today and unpaid, when 7 days pass without the Admin recording payment, then the partner becomes Hidden from new patients.
- **AC-53:** Given a Hidden clinic with a confirmed booking next week, when the date arrives, then the booking proceeds normally.

### F-18 Admin Console
- **AC-53a:** Given a patient who reports a message, when a Report Reviewer opens the report, then only the reported messages are visible, the view is audit-logged, and access ends after 14 days.

---

## 24. Future Features

| Phase | Feature | Origin |
|---|---|---|
| 2 | Video consultations | [C] deferred |
| 2 | Treatment session/package booking | [C] deferred |
| 2 | Automated e-wallet integration (API) | [C] deferred |
| 2 | Native mobile apps (store) | [C] deferred |
| 2 | Ratings & reviews (doctors, pharmacies) | [A] deferred |
| 2 | WhatsApp / SMS notifications | [A] |
| 2 | Automated subscription billing | [A] |
| 3 | English UI | [A] |
| 3 | "Notify me when available" for out-of-stock medicines | [A] |
| 3 | Delivery of medicines | [A] |
| 3 | License expiry tracking & re-verification | [A] |
| 3 | Expansion to the West Bank / other specialties | [A] |
| 3 | Admin analytics dashboard | [A] |

---

## 25. Risks & Dependencies

| ID | Risk / Dependency | Impact | Mitigation |
|---|---|---|---|
| RISK-1 | Unreliable power/internet in Gaza | Missed SLAs, failed uploads | Async-first design, offline caching, retry queues (NFR-004/005) |
| RISK-2 | Pharmacies don't update prices/stock daily | Stale data, rejected orders | Dermatology-only catalog, quick-update screen, "last updated" + outdated badge, pharmacy confirmation before payment |
| RISK-3 | Regulatory: telemedicine and e-prescription legality in Palestine/Gaza; prescription-only medicines | Launch blocker | Legal review before launch (Q-14) |
| RISK-4 | Manual payment confirmation is slow or errors | Abandoned bookings/orders | Clear payment window, single-use references, Admin visibility of overdue tasks |
| RISK-5 | Refunds depend on clinics/pharmacies acting in good faith (platform holds no money) | Patient trust | Overdue-refund monitoring, Admin follow-up and suspension (FR-087, FR-098) |
| RISK-6 | Medicine shortages | No-match results | Split/subset orders; free-text items |
| RISK-7 | Partner acquisition during free trial; conversion to paid | Revenue | Define trial length and pricing early (Q-05/06) |
| RISK-8 | Problem/demand not validated with user research | Product-market fit | Pilot with a small set of clinics/pharmacies |
| RISK-9 | iOS web push limited to installed PWAs | Missed notifications | In-app center, install prompts |
| DEP-1 | E-wallet providers' merchant API availability | Phase 2 automation | Contact providers |
| DEP-2 | Map/geo data quality for Gaza areas | Distance accuracy | Admin-maintained area list |
| DEP-3 | Initial catalog content (dermatology medicines, AR/EN names) | Launch blocker | Admin/pharmacist to compile before launch |

---

## 26. Open Questions

### 26.1 Quality review findings

| # | Check | Finding | Resolution |
|---|---|---|---|
| QR-1 | Conflicts | Original idea: "pick up **any time**" vs later rule "pickup window set per pharmacy". | Resolved by owner: per-pharmacy window + daily reminders (BR-097/098). |
| QR-2 | Conflicts | Original brief: "web application" vs later "make it an app". | Resolved: Web + PWA now; native app in Phase 2. |
| QR-3 | Conflicts | In the first (abandoned) interview round, video consultation was chosen for MVP; in the restarted round, video was deferred. | Latest answer prevails: video = Phase 2. |
| QR-4 | Conflicts | Payment verification was first "deferred", then decided in the restart as reference number + manual confirmation. | Latest answer prevails. |
| QR-5 | Missing rules | Payment-confirmation window length, pickup-expiry refund, pharmacy post-payment stock-out are undefined. | Q-03 resolved in v1.1 (QR-13); Q-04 and Q-09 resolved in v1.2 (QR-21). |
| QR-6 | Undefined permissions | Clinic Manager dual role; pharmacy multi-user; Admin access to medical content for disputes. | Q-15 resolved and Q-10 proposed in v1.1 (QR-14, QR-15); Q-16 resolved in v1.2 (QR-21). |
| QR-7 | Incomplete flows | Disputes (patient says they paid, clinic says no) have no resolution path beyond one correction. | Resolved in v1.2: settled directly between the parties (BR-079). |
| QR-8 | MVP consistency | All MVP features trace to confirmed answers; F-17 automated enforcement is Should Have (manual fallback). | OK |
| QR-9 | Unnecessary complexity | Split orders across pharmacies add complexity; confirmed by owner, kept. Ratings removed. | Superseded by QR-17: split orders are now Should Have. |
| QR-10 | Assumptions | All [A] items must be validated; key ones listed below. | — |
| QR-11 | Conflicts (v1.1 review) | Free async with real-time delivery (BR-045) and a 72h window made paid live chat redundant. | Patient follow-ups capped at 3 (BR-048, FR-030a). [D] |
| QR-12 | Conflicts (v1.1 review) | Live session converted to async: BR-053 (no refund) vs BR-043 (refund if unanswered). | BR-057: full refund if the doctor doesn't answer within 24h. [D] |
| QR-13 | Missing rules (v1.1 review) | The provider controlled when the 24h SLA started; payment window undefined (Q-03). | Patient and provider windows with patient protection (BR-076, BR-078). [D] |
| QR-14 | Undefined permissions (v1.1 review) | No one could review abuse inside medical content. | Reports and a Report Reviewer permission (FR-100, SEC-13). [D] Legal confirmation pending. |
| QR-15 | Undefined permissions (v1.1 review) | A deactivated doctor's patients were left without a path; Clinic Manager dual role undefined. | FR-009 and BR-008. [D] |
| QR-16 | Missing rules (v1.1 review) | Prescriptions reusable without limit; prescription-only medicines collectable by anyone. | BR-126 – BR-128. [D] Legal confirmation pending. |
| QR-17 | MVP size (v1.1 review) | 17 of 18 features were Must Have. | F-06 live chat and FR-067 split orders moved to Should Have and built last (§27). [D] |
| QR-18 | Unverifiable requirements (v1.1 review) | NFR-002, NFR-003, NFR-006, NFR-012, and BR-130 could not be tested. | Rewritten with measurable thresholds. [D] |
| QR-19 | Missing edge cases (v1.1 review) | Closed toggle during countdown, night search, in-person no-shows, reference collisions, async withdrawal, family member removal, partial acceptance. | FR-075, FR-070, BR-063, BR-075, BR-049, FR-017, BR-133; EC-39 – EC-47; ERR-25 – ERR-28. [D] |
| QR-20 | Internal inconsistencies (v1.1 review) | EC-02 relied on a license number not defined anywhere; SEC-01 vs SEC-02; EC-21 vs FR-033 on when a slot hold starts. | Aligned in FR-002, BR-006, SEC-01, EC-21. [D] |
| QR-21 | Owner answers (v1.2, from the domain-model review) | Open questions on wallets, refunds, subscriptions, live-chat price, pharmacy accounts, areas, and Admin permissions. | BR-009, BR-014, BR-050, BR-052, BR-056, BR-079, BR-093, BR-099, BR-111, BR-115, BR-120, BR-127, BR-129, BR-135, FR-010, FR-032 – FR-034, FR-086, FR-087, §15. Resolves Q-02, Q-04, Q-07, Q-08, Q-09, Q-16; partly Q-06. [C] |

### 26.2 Unresolved questions

| ID | Question | Owner | Blocks |
|---|---|---|---|
| Q-01 | Final product name (current working name: التِئام / Iltiam). | Product owner | Branding |
| Q-02 | Exact subscription start: when does the free trial begin (approval date?) **Resolved in v1.2:** on the Admin approval date (BR-115). | Product owner | — |
| Q-03 | Payment-confirmation window: how long does a slot/order stay held awaiting the provider's confirmation, and is there escalation if the provider never responds? **Resolved in v1.1** by BR-076 and BR-078 [D]. | Product owner | — |
| Q-04 | When a paid pharmacy order expires uncollected, is the patient refunded (fully/partially) or not? **Resolved in v1.2:** full refund by the pharmacy (BR-099). | Product owner | — |
| Q-05 | Free-trial length. | Product owner | F-17 |
| Q-06 | Subscription prices (same for clinics and pharmacies? per doctor?). **Partly resolved in v1.2:** one subscription per clinic, not per doctor (BR-111). Prices still open. | Product owner | F-17 |
| Q-07 | Can a doctor offer booked live chat for free, or is it always paid? **Resolved in v1.2:** the doctor chooses (BR-056). | Product owner | — |
| Q-08 | Dispute and refund enforcement: what happens if a clinic/pharmacy doesn't refund or a payment is disputed? Admin role? **Resolved in v1.2:** for refused refunds the Admin follows up and may suspend the partner (FR-087); payment disputes are settled directly between the parties (BR-079). | Product owner | — |
| Q-09 | Pharmacy discovers stock-out after payment: policy and consequences? **Resolved in v1.2:** cancel and refund in full (BR-135). | Product owner | — |
| Q-10 | Can Admins access medical content (e.g., for abuse reports or disputes)? **Proposed in v1.1:** reported content only (FR-100) [D]; legal confirmation pending. | Legal | SEC |
| Q-11 | Medical-record retention period and deletion rules. | Legal | SEC-11 |
| Q-12 | Booking reminder timing (e.g., 24h and 1h before). | Product owner | F-16 |
| Q-13 | 3-month success targets (clinics, pharmacies, patients, conversions). | Product owner | §22 |
| Q-14 | Regulatory requirements for telemedicine, e-prescriptions, and prescription-only medicines in Gaza. | Legal | Launch |
| Q-15 | Can a Clinic Manager also be a doctor in the same account? **Resolved in v1.1:** yes (BR-008) [D]. | Product owner | — |
| Q-16 | Do pharmacies need multiple staff logins? **Resolved in v1.2:** no, one shared account (BR-014). | Product owner | — |
| Q-17 | Ratings & reviews: confirm deferral to Phase 2 (owner was undecided). | Product owner | §24 |
| Q-18 | Minimum booking notice (how soon before a slot can it be booked?). | Product owner | F-08 |
| Q-19 | Can the same prescription be ordered more than once (refills)? Prescription validity period? **Proposed in v1.1:** no refills, 30-day validity (BR-126, BR-127) [D]; legal confirmation pending. | Legal | F-10, F-12 |
| Q-20 | Who compiles the initial dermatology catalog and areas list? | Product owner | Launch |

### 26.3 Key assumptions to validate

Highest impact first: BR-024 (email verification), BR-025 (phone collected), BR-047 (photo limits), BR-134 (48h outdated flag), BR-077 (ILS currency), FR-051 (one correction), FR-087 (72h refund overdue), FR-099 (Admin no medical access), NFR-003 (performance budget), NFR-013 (tech stack).

v1.1 delegated rules to validate in the pilot: BR-048 (3 follow-ups), BR-063 (no-show block), BR-076 (30 min / 2 h / 60 min windows), BR-126 (30-day validity), FR-100 (14-day report access).

---

## 27. Recommended Development Phases

| Phase | Name | Scope | Exit criteria |
|---|---|---|---|
| 0 | Foundations & validation | Resolve Q-05/06, Q-14 (Q-03 resolved in v1.1; Q-04 in v1.2); legal confirmation of BR-126, BR-128, and FR-100; compile catalog & areas list; confirm wallet process with 2–3 pilot partners | Legal go-ahead; catalog v1 ready |
| 1 | Partner onboarding | F-01, F-02, F-11, F-18 (verification + catalog), Arabic RTL PWA shell | Pilot clinics/pharmacies approved and configured |
| 2 | Consultations | F-03, F-04, F-05, F-07, F-08, F-09 (clinic side), F-16 | Patients complete async and in-person flows end-to-end |
| 3 | Prescription → Saydaliyati | F-10, F-12, F-13, F-09 (pharmacy side), F-14 | Full journey UF-1 passes in pilot |
| 4 | Policies, business model & Should Have features | F-15, F-17, analytics (§21); then, if time allows, F-06 live chat and FR-067 split orders (QR-17) | Refund & subscription flows live; KPIs reporting |
| 5 | Closed pilot | Limited clinics/pharmacies/patients in Gaza; fix based on feedback | KPIs measured against targets (Q-13) |
| 6 | Phase 2 features | §24 Phase 2 items | — |

---

*End of document.*
