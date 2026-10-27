# User Flows — Iltiam (التِئام)

| Field | Value |
|---|---|
| Source | [PRD.md](PRD.md) — Draft v1.0 (2026-10-15) |
| Date | 2026-10-22 |
| Scope | Confirmed MVP journeys only (PRD §9.1 and §13) |
| Format | Mermaid `flowchart TD` |
| Arabic version | [user-flow.ar.md](user-flow.ar.md) |

## How to read this document

- Each diagram covers one journey and is followed by an **explanation**, the **related PRD requirements**, and its own **open questions**.
- No business rule was added beyond the PRD. Steps based on a PRD **assumption [A]** are tagged `[A]` in the diagram, and steps that depend on an **open question** carry its number (e.g. `Q-03`).
- Screen (`S-xx`), error (`ERR-xx`), and edge-case (`EC-xx`) IDs are taken from the PRD as-is.

### Legend

| Shape / colour | Meaning |
|---|---|
| `([ ... ])` blue | Entry or exit point |
| `[ ... ]` | User action or system response |
| `{ ... }` | Decision point |
| `[[ ... ]]` | Shared sub-flow (drawn in its own diagram) |
| Green | Success path |
| Red | Error state or failure path |
| Yellow | Waiting / pending state |

### Flow index

| # | Flow | Roles | PRD features |
|---|---|---|---|
| UF-0 | Role-based navigation | All | §14, §15 |
| UF-A | Sign-up, login & family members | Guest, Patient | F-03, F-04 |
| UF-1 | Async message consultation | Patient, Doctor | F-05 |
| UF-2 | Booked live text consultation | Patient, Doctor | F-06 |
| UF-3 | In-person appointment | Patient, Doctor, Clinic Staff | F-07 |
| UF-B | Manual payment verification (sub-flow) | Patient, Doctor/Staff, Pharmacy | F-09 |
| UF-C | Issuing an e-prescription | Doctor | F-10, F-11 |
| UF-D | Saydaliyati: order & pickup | Patient, Pharmacy | F-12, F-13, F-14 |
| UF-4 | Cancellation & refund | Patient, Clinic, Admin | F-15, F-08 |
| UF-5 | Clinic & doctor onboarding, subscription | Clinic Manager, Doctor, Staff, Admin | F-01, F-08, F-17 |
| UF-6 | Pharmacy onboarding & daily operations | Pharmacy, Admin | F-02, F-13 |
| UF-7 | Admin console | Platform Admin | F-11, F-17, F-18 |

---

## UF-0 — Role-based navigation

```mermaid
flowchart TD
    START(["Open app - web or PWA"]) --> AUTH{"Logged in?"}

    AUTH -->|"No"| GUEST["Guest: home<br/>clinic and doctor listing S-01"]
    GUEST --> BROWSE["Browse clinic S-02 and doctor S-03<br/>services, prices, availability"]
    BROWSE --> PROT{"Attempted a protected action?<br/>message, book, or order medicine"}
    PROT -->|"No"| BROWSE
    PROT -->|"Yes"| LOGIN[["Login or register S-04<br/>see UF-A"]]
    GUEST --> PARTNER{"Register as a partner?"}
    PARTNER -->|"Clinic"| CREG[["Clinic registration wizard S-15<br/>see UF-5"]]
    PARTNER -->|"Pharmacy"| PREG[["Pharmacy registration wizard S-22<br/>see UF-6"]]
    LOGIN --> ROLE

    AUTH -->|"Yes"| ROLE{"Account role?"}

    ROLE -->|"Patient"| PAT["Home S-01 • Profile and family S-05<br/>My consultations and bookings S-09<br/>Saydaliyati S-11 • Notifications S-14"]
    ROLE -->|"Doctor"| DOC["Inbox S-19 • Schedule S-17<br/>Payments and refunds S-18<br/>Prescription editor S-20 • Service settings S-21"]
    ROLE -->|"Clinic Staff"| STAFF["Schedule S-17<br/>Payments and refunds S-18<br/>no medical content"]
    ROLE -->|"Clinic Manager"| MGR["Clinic dashboard S-16<br/>Schedule S-17 [A] • Payments S-18 [A]"]
    ROLE -->|"Pharmacy"| PH["Daily inventory S-23 • Orders S-24<br/>Settings S-25"]
    ROLE -->|"Platform Admin"| ADM["Verification queue S-26 • Catalog S-27<br/>Partners and subscriptions S-28<br/>Overdue refunds and suspensions S-29"]

    PAT --> EXIT(["Log out"])
    DOC --> EXIT
    STAFF --> EXIT
    MGR --> EXIT
    PH --> EXIT
    ADM --> EXIT

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    class START,EXIT entry
```

### Explanation

- A **guest** browses approved clinics, doctors, services, and prices without an account (BR-020). On any protected action they are asked to log in or register, then returned to the same action (FR-023).
- A guest can also start **partner registration** (clinic or pharmacy) through the dedicated wizard.
- After login, each user lands on the screens their role allows, per the permissions matrix (§15):
  - **Clinic Staff** see the schedule and payments only, **never medical content** (SEC-03, FR-031).
  - The **Admin** has no access to medical content in the MVP (FR-099 [A]).
  - **Clinic Manager** access to the schedule and payment confirmation is an assumption [A].
- Notifications (S-14) are available to all logged-in roles.

### Related PRD requirements
BR-020, FR-020, FR-023, §14 (S-01 – S-29), §15, SEC-01, SEC-03, FR-031, FR-099.

### Open questions
- **Q-15:** Can a Clinic Manager also be a doctor in the same account? If so, a role switch or merged menu is needed.
- **Q-16:** Can a pharmacy have multiple staff logins? (Affects the pharmacy menu.)
- **Q-10:** Can the Admin access medical content to resolve disputes?

---

## UF-A — Sign-up, login & family members

```mermaid
flowchart TD
    START(["Guest attempts a protected action<br/>or opens S-04 directly"]) --> HAS{"Has an account?"}

    HAS -->|"Yes"| LIN["Enter email and password"]
    LIN --> LOK{"Credentials valid?"}
    LOK -->|"No"| LERR["Login error<br/>with password reset option"]
    LERR --> FORGOT{"Forgot password?"}
    FORGOT -->|"Yes"| RESET["Send reset link by email [A]"]
    RESET --> LIN
    FORGOT -->|"No"| LIN

    HAS -->|"No"| REG["Enter email, password, and mobile number [A]"]
    REG --> AGE{"Confirmed 18 or older?"}
    AGE -->|"No"| AGEERR["Account cannot be created<br/>minors are added as family members under an adult"]
    AGEERR --> ABORT(["Exit without an account"])
    AGE -->|"Yes"| EXISTS{"Email already registered?"}
    EXISTS -->|"Yes"| ERR04["ERR-04 This email is already registered"]
    ERR04 --> LIN
    EXISTS -->|"No"| CREATED["Account created, verification email sent [A]"]
    CREATED --> LOK2

    LOK -->|"Yes"| LOK2["Logged in"]
    LOK2 --> RET["Return to the original action FR-023"]

    RET --> NEEDV{"Is the action a booked or paid service?"}
    NEEDV -->|"No"| POR
    NEEDV -->|"Yes"| VER{"Email verified? [A]"}
    VER -->|"No"| ERR05["ERR-05 Verify your email to continue<br/>with resend option"]
    ERR05 --> VER
    VER -->|"Yes"| POR

    POR{"Who is the patient of record?<br/>FR-018"} -->|"Me"| SELF["Select account holder"]
    POR -->|"Family member"| FAMQ{"Member already added?"}
    FAMQ -->|"Yes"| FAM["Select family member"]
    FAMQ -->|"No"| ADDF["Add member: name, date of birth,<br/>sex, relationship S-05"]
    ADDF --> FAM
    SELF --> DONE(["Continue the service linked to one patient"])
    FAM --> DONE

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    class START,ABORT,DONE entry
    class LOK2,CREATED success
    class LERR,AGEERR,ERR04,ERR05 error
```

### Explanation

1. The guest reaches this flow when trying to message a doctor, book, or order medicine, or by opening the login screen directly.
2. **Registration** is by email and password only (BR-021). The account holder must confirm they are **18+**, or the account cannot be created (AC-10). A minor cannot use the platform alone and is added as a family member (EC-06).
3. If the email is already registered, ERR-04 is shown and the user is sent to login or password reset.
4. After login the user is **returned to the action they started** (FR-023, AC-08).
5. Email verification is required before the first paid or booked service (BR-024 — assumption [A]).
6. Every consultation, booking, and order requires selecting the **patient of record** — the account holder or a family member (BR-023, FR-018, AC-09). A new member can be added mid-flow.

### Related PRD requirements
US-02, US-03, BR-020 – BR-025, FR-014 – FR-019, FR-023, AC-08 – AC-10, EC-06, EC-07, ERR-04, ERR-05.

### Open questions
- Email verification before the first service (BR-024) and collecting a phone number (BR-025) are assumptions to validate (§26.3).
- The PRD only states "login rate-limited" (SEC-07); the lockout message and timing are undefined.

---

## UF-1 — Async message consultation

```mermaid
flowchart TD
    START(["Patient on doctor profile S-03"]) --> MODE{"Doctor's async mode?"}
    MODE -->|"Disabled"| ERR07["ERR-07 This doctor isn't accepting messages"]
    ERR07 --> ALT(["Exit: book live chat or in-person<br/>or choose another doctor"])
    MODE -->|"Free or Paid"| TAP["Tap Send a message"]
    TAP --> AUTHQ[["Login and select patient of record<br/>UF-A"]]
    AUTHQ --> WRITE["Write case and attach photos S-06<br/>up to 5 photos, 10 MB each [A]"]
    WRITE --> UP{"All photos uploaded?"}
    UP -->|"No"| ERR08["ERR-08 Photo failed to upload<br/>cached locally, retry"]
    ERR08 --> FIX{"Retry or remove photo?"}
    FIX -->|"Retry"| UP
    FIX -->|"Remove"| WRITE
    UP -->|"Yes"| PAID{"Is the message paid?"}

    PAID -->|"Free"| SENT["Case sent immediately<br/>24h clock starts at send time"]
    PAID -->|"Paid"| PAY[["Manual payment verification<br/>UF-B — recipient: clinic"]]
    PAY --> PAYOK{"Payment confirmed?"}
    PAYOK -->|"No"| PAYFAIL(["Exit: case not delivered to doctor"])
    PAYOK -->|"Yes"| SENT2["Case delivered to doctor<br/>24h clock starts at payment confirmation [A]"]

    SENT --> WAIT["Waiting for doctor reply<br/>deadline shown to patient"]
    SENT2 --> WAIT
    WAIT --> REPLY{"Doctor replied within 24h?"}
    REPLY -->|"No"| EXP["Status: Expired - No response<br/>patient notified"]
    EXP --> WASPAID{"Was it paid?"}
    WASPAID -->|"Yes"| REF["Refund task created for clinic<br/>see UF-4"]
    WASPAID -->|"No"| OTHER
    REF --> OTHER(["Exit: patient may consult another doctor"])

    REPLY -->|"Yes"| OPEN["Patient notified of reply<br/>72h follow-up window starts"]
    OPEN --> CHAT["Message exchange S-10<br/>real-time when both online"]
    CHAT --> RX{"Doctor issues a prescription?"}
    RX -->|"Yes"| RXF[["Issue prescription UF-C"]]
    RXF --> CHAT
    RX -->|"No"| CLOSE{"Doctor closed thread<br/>or 72h elapsed?"}
    CLOSE -->|"No"| CHAT
    CLOSE -->|"Yes"| CLOSED["Thread closed"]
    CLOSED --> NEWQ{"Patient sends a new question?"}
    NEWQ -->|"Yes"| ERR09["ERR-09 This consultation is closed<br/>start a new consultation"]
    ERR09 --> START
    NEWQ -->|"No"| DONE(["Consultation ends"])

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class START,ALT,PAYFAIL,OTHER,DONE entry
    class SENT,SENT2,OPEN success
    class ERR07,ERR08,EXP,ERR09 error
    class WAIT pending
```

### Explanation

1. **Entry:** the doctor profile. The doctor sets async mode to Free (default), Paid (own price), or Disabled (BR-040). If disabled, the patient is offered live chat, in-person, or another doctor (ERR-07).
2. The patient logs in (or registers), selects the **patient of record**, writes the case, and attaches photos (BR-046). Photo limits are an assumption [A] (BR-047). A failed upload is cached and retried; the message isn't sent until all photos upload or the failed one is removed (EC-12).
3. A **free message** is sent immediately and the 24h clock starts. A **paid message** goes through manual payment (UF-B) first, and the clock starts at payment confirmation (BR-042 [A]).
4. **No reply within 24h:** status becomes "Expired – No response", the patient is notified and may consult another doctor; if paid, a refund task is created for the clinic (BR-043, FR-029).
5. **After the first reply:** the thread stays open 72h for follow-ups; the doctor may close it earlier (BR-044). Messages are real-time when both are online (BR-045).
6. The doctor may issue a prescription during the consultation (UF-C).
7. After closure, input is disabled and the patient is prompted to start a new consultation (EC-11, ERR-09).
8. **Privacy:** case text and photos are visible only to the patient and the treating doctor — not Clinic Staff, not the Admin (FR-031, SEC-01).

### Related PRD requirements
US-04, US-12, BR-040 – BR-047, FR-024 – FR-031, AC-12 – AC-17, EC-09 – EC-12, ERR-07 – ERR-09, SEC-01, SEC-05, NFR-004, NFR-006.

### Open questions
- **Q-03:** Length of the payment-confirmation window for paid messages, and what happens if the clinic never confirms.
- Starting the 24h clock at payment confirmation (BR-042) is an assumption to confirm.
- **EC-10:** The 24h SLA covers only the first reply; follow-ups are best-effort. Should the patient see any expected response time after the first reply? (Not defined in the PRD.)

---

## UF-2 — Booked live text consultation

```mermaid
flowchart TD
    START(["Patient on doctor profile S-03"]) --> SVC["Select Live chat service<br/>price and duration set by doctor"]
    SVC --> AUTHQ[["Login and select patient of record<br/>UF-A"]]
    AUTHQ --> AVAIL{"Any slots available?"}
    AVAIL -->|"No"| NOAV["Show No availability EC-08"]
    NOAV --> NOAVX(["Exit: async message if enabled<br/>or another doctor"])
    AVAIL -->|"Yes"| SLOT["Pick a slot S-07"]
    SLOT --> FREE{"Slot still available?"}
    FREE -->|"No"| ERR10["ERR-10 This time is no longer available"]
    ERR10 --> SLOT
    FREE -->|"Yes"| HOLD["Slot held pending payment"]
    HOLD --> PAY[["Manual payment verification<br/>UF-B — recipient: clinic"]]
    PAY --> PAYOK{"Payment confirmed within window? Q-03"}
    PAYOK -->|"No"| ERR15["ERR-15 Reservation expired, payment not confirmed<br/>slot released"]
    ERR15 --> RESTART(["Exit: start again"])
    PAYOK -->|"Yes"| CONF["Booking Confirmed<br/>notification and reminder scheduled Q-12"]

    CONF --> CANQ{"Cancelled by either party before start?"}
    CANQ -->|"Yes"| CANF[["Cancellation and refund UF-4"]]
    CANQ -->|"No"| ROOM["Chat room opens at start time<br/>join indicator for both [A]"]

    ROOM --> JOIN{"Who joined within 15 min?"}
    JOIN -->|"Both"| LIVE["Live session S-10<br/>until original end time EC-15"]
    JOIN -->|"Doctor only - patient absent"| TOASYNC["Session converts to async message<br/>doctor replies within 24h - no refund"]
    TOASYNC --> ASYNCF[["Continue as async consultation UF-1"]]
    JOIN -->|"Patient only or nobody EC-13"| DNS["Doctor no-show<br/>patient notified of options"]
    DNS --> CHOICE{"Patient's choice"}
    CHOICE -->|"Rebook"| SLOT
    CHOICE -->|"Full refund"| REF[["Refund task for clinic UF-4"]]

    LIVE --> DROP{"Connection dropped mid-session?"}
    DROP -->|"Yes"| ERR11["ERR-11 Connection problem<br/>messages delivered on reconnect - no extension"]
    ERR11 --> LIVE
    DROP -->|"No"| ENDS["Session ends at configured duration [A]"]
    ENDS --> FU["72h follow-up window [A]"]
    FU --> RX{"Doctor issues a prescription?"}
    RX -->|"Yes"| RXF[["Issue prescription UF-C"]]
    RX -->|"No"| DONE(["Consultation ends"])
    RXF --> DONE

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class START,NOAVX,RESTART,DONE entry
    class CONF,LIVE success
    class ERR10,ERR15,ERR11,DNS,NOAV error
    class HOLD pending
```

### Explanation

1. The patient selects the doctor, then **Live chat**; price and duration are set by the doctor (BR-051, FR-032).
2. If there are no slots, "No availability" is shown; async messaging remains available if enabled (EC-08).
3. A picked slot is held pending payment (FR-033). If another patient got there first — whoever **starts payment** first holds the slot — ERR-10 is shown (EC-21).
4. **Payment is in advance to the clinic's wallet**, and the booking is confirmed only after the clinic confirms payment (BR-050, BR-052). If not confirmed within the window, the slot is released (ERR-15).
5. **At start time:**
   - **Patient absent after 15 min:** the session converts to an async message; the doctor replies within 24h, **no refund** (BR-053).
   - **Doctor absent after 15 min:** the patient chooses **Rebook** or **Full refund** (BR-054). Both absent is treated as a doctor no-show (EC-13 [A]).
   - **Doctor late but within 15 min:** the session runs to the original end time (EC-15 [A]).
6. A dropped connection doesn't end the chat; it continues asynchronously with no extension (EC-14, ERR-11).
7. After the duration ends, the 72h follow-up window applies and a prescription may be issued.

### Related PRD requirements
US-05, BR-050 – BR-056, FR-032 – FR-038, FR-090, AC-18 – AC-21, EC-13 – EC-15, EC-21, ERR-10, ERR-11, ERR-15.

### Open questions
- **Q-07:** Can live chat ever be free? (The diagram assumes always paid — BR-056 [A].)
- **Q-03:** How long a slot is held awaiting payment confirmation.
- **Q-12:** Booking reminder timing.
- **Q-18:** Minimum notice before a slot can be booked.

---

## UF-3 — In-person appointment

```mermaid
flowchart TD
    START(["Patient on doctor profile S-03"]) --> SVC["Select In-person service"]
    SVC --> AUTHQ[["Login and select patient of record<br/>UF-A"]]
    AUTHQ --> AVAIL{"Any slots available?"}
    AVAIL -->|"No"| NOAV(["Exit: No availability EC-08"])
    AVAIL -->|"Yes"| SLOT["Pick a slot S-07"]
    SLOT --> FREE{"Slot still available?"}
    FREE -->|"No"| ERR10["ERR-10 This time is no longer available"]
    ERR10 --> SLOT
    FREE -->|"Yes"| CONF["Booking Confirmed immediately - no prepayment [A]<br/>show address, map, and price payable at clinic"]
    CONF --> REM["Notification and reminder before visit Q-12"]
    REM --> CANQ{"Cancelled by either party?"}
    CANQ -->|"Yes"| CANF[["Cancellation UF-4"]]
    CANQ -->|"No"| VISIT["Patient visits clinic and pays there"]
    VISIT --> MARK{"Doctor or staff sets status [A]"}
    MARK -->|"No-show"| NS["Status: No-show<br/>no prescription allowed EC-16"]
    NS --> NSX(["End"])
    MARK -->|"Completed"| DONEV["Status: Completed"]
    DONEV --> RX{"Doctor issues a prescription?"}
    RX -->|"Yes"| RXF[["Issue prescription UF-C<br/>then Saydaliyati UF-D"]]
    RX -->|"No"| DONE(["Visit ends"])
    RXF --> DONE

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    class START,NOAV,NSX,DONE entry
    class CONF,DONEV success
    class ERR10,NS error
```

### Explanation

1. The patient selects the doctor, then **In-person**, then a slot. No payment goes through the platform; the patient pays at the clinic (BR-060).
2. The booking is confirmed on slot selection (BR-061 [A]), and the clinic address, map pin, and payable price are shown (FR-040).
3. After the visit, the doctor or staff marks it **Completed** or **No-show** (BR-062 [A], FR-041).
4. **Prescriptions are only possible for Completed visits** (FR-042); none for a no-show (EC-16).
5. After a prescription, the patient continues in Saydaliyati (PRD UF-1 steps 8–13 = UF-D here).

### Related PRD requirements
US-06, BR-060 – BR-062, FR-039 – FR-042, AC-22, AC-23, EC-16, ERR-10.

### Open questions
- **Q-12:** Reminder timing.
- **Q-18:** Minimum booking notice.
- The PRD defines no consequence for repeated patient no-shows.

---

## UF-B — Manual payment verification (shared sub-flow)

Called from: paid async message (UF-1), live chat (UF-2), pharmacy order (UF-D).

```mermaid
flowchart TD
    START(["A flow requires payment"]) --> SHOW

    subgraph PATIENT["Patient"]
        SHOW["Payment screen S-08<br/>amount in ILS [A], recipient wallet ID, instructions"]
        SHOW --> EXT["Pay outside the platform via<br/>PalPay, Jawwal Pay, or Bank of Palestine app"]
        EXT --> REFIN["Enter transaction reference and wallet used"]
        REFIN --> USED{"Reference used before? [A]"}
        USED -->|"Yes"| ERR13["ERR-13 This transaction number was already used"]
        ERR13 --> REFIN
        USED -->|"No"| PEND["Payment pending confirmation"]
    end

    subgraph RECIPIENT["Recipient: Doctor or Clinic Staff, or Pharmacy"]
        PEND --> QUEUE["Shown in pending payments<br/>S-18 for clinic, S-24 for pharmacy"]
        QUEUE --> INWIN{"Responded within confirmation window? Q-03"}
        INWIN -->|"No"| ERR15["ERR-15 Reservation expired, payment not confirmed<br/>slot or order released"]
        INWIN -->|"Yes"| CHECK{"Check wallet: amount received?"}
        CHECK -->|"Received"| OK["Payment confirmed, logged who and when"]
        CHECK -->|"Not found"| ERR14["ERR-14 Couldn't find your payment"]
    end

    ERR14 --> FIRST{"First Not found? [A]"}
    FIRST -->|"Yes"| REFIN
    FIRST -->|"No"| ERR15

    OK --> NOTIF["Patient notified: payment confirmed"]
    NOTIF --> BACK(["Return to calling flow: success"])
    ERR15 --> FAILX(["Return to calling flow: failure<br/>EC-20 patient told to contact the clinic"])

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class START,BACK,FAILX entry
    class OK,NOTIF success
    class ERR13,ERR14,ERR15 error
    class PEND,QUEUE pending
```

### Explanation

1. **The platform never receives funds**; payment goes directly from the patient's wallet to the clinic's or pharmacy's wallet (BR-070).
2. The platform shows the amount, recipient wallet ID, and instructions (FR-047). The patient pays in the wallet app, then enters the **transaction reference** and wallet used (BR-072, FR-048). **Receipt images are not used** (BR-074).
3. A reference can be used only once on the platform (BR-075 [A], ERR-13).
4. The **recipient** (doctor or Clinic Staff for clinics; the pharmacy for orders) checks their wallet and marks **Received** or **Not found** (BR-073, FR-050).
5. On "Not found" the patient is notified and may **correct the reference once** before the hold is released (FR-051 [A]).
6. If not confirmed within the **confirmation window**, the slot or order is released (BR-076, FR-052) — window length is an open question.
7. Every confirmation is logged (who and when) (FR-053, NFR-011).

### Related PRD requirements
US-07, US-16, BR-070 – BR-077, FR-047 – FR-053, FR-078, AC-26 – AC-29, EC-18 – EC-21, ERR-13 – ERR-15, INT-1.

### Open questions
- **Q-03:** Confirmation window length, and whether there is escalation if the recipient never responds (EC-20).
- **Q-08:** Dispute resolution — patient says they paid, provider says no, or wrong amount paid (EC-19). No path exists beyond the single correction.
- ILS currency (BR-077) and the single correction (FR-051) are assumptions.

---

## UF-C — Issuing an e-prescription (Doctor)

```mermaid
flowchart TD
    START(["Doctor inside a consultation or visit<br/>S-19 or S-10"]) --> ROLEQ{"User has Doctor role? [A]"}
    ROLEQ -->|"No"| NOROLE(["Exit: doctors only"])
    ROLEQ -->|"Yes"| ELIG{"Eligible service?<br/>async, live chat, or completed visit"}
    ELIG -->|"No"| ERR16["ERR-16 Prescriptions can only follow a completed consultation on the platform"]
    ERR16 --> NOX(["Exit"])
    ELIG -->|"Yes"| EDIT["Open prescription editor S-20<br/>linked to the service's patient of record"]
    EDIT --> SEARCH["Search catalog in Arabic or English"]
    SEARCH --> FOUND{"Medicine in catalog?"}
    FOUND -->|"Yes"| ITEM["Add item: form and strength from catalog<br/>quantity and dosage instructions [A]"]
    FOUND -->|"No"| FREE["Add free-text item<br/>shown to patient: Not available in Saydaliyati"]
    FREE --> REQ["Catalog-addition request auto-created<br/>sent to Admin - see UF-7"]
    REQ --> MORE
    ITEM --> MORE{"Add another item?"}
    MORE -->|"Yes"| SEARCH
    MORE -->|"No"| ISSUE["Issue prescription - not editable after issue [A]"]
    ISSUE --> DELIV["Delivered to patient's Saydaliyati<br/>patient notified"]
    DELIV --> WRONG{"Error in prescription?<br/>e.g. wrong family member EC-23"}
    WRONG -->|"No"| DONE(["Prescription available to patient - see UF-D"])
    WRONG -->|"Yes"| ORD{"Pharmacy order exists for it?"}
    ORD -->|"No"| CANC["Cancel and reissue"]
    CANC --> EDIT
    ORD -->|"Yes"| LOCK(["Cannot be cancelled"])

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    class START,NOROLE,NOX,DONE,LOCK entry
    class ISSUE,DELIV success
    class ERR16 error
```

### Explanation

1. Only doctors can issue prescriptions (BR-083 [A]), and **only after a service performed on the platform**: an async message, a live chat, or a completed in-person visit (BR-080, ERR-16).
2. Items are selected from the **unified catalog**, searchable in Arabic or English (BR-081, FR-055).
3. If a medicine isn't in the catalog, the doctor adds a **free-text item** shown to the patient as "Not available in Saydaliyati", and a **catalog-addition request** is sent to the Admin automatically (BR-082, FR-056).
4. Once issued, the prescription reaches Saydaliyati and the patient is notified (FR-058).
5. A prescription cannot be edited after issue; it can be cancelled and reissued **only if no pharmacy order exists** (BR-085 [A], FR-059, EC-23).

### Related PRD requirements
US-13, US-14, BR-080 – BR-089, FR-054 – FR-063, AC-30 – AC-32, EC-22 – EC-24, ERR-16.

### Open questions
- **Q-19:** Can the same prescription be ordered more than once (refills)? What is its validity period?
- **Q-14:** Regulatory requirements for e-prescriptions and prescription-only medicines.
- Non-editability after issue (BR-085) is an assumption.

---

## UF-D — Saydaliyati: search, order, payment & pickup

```mermaid
flowchart TD
    START(["Prescription issued notification"]) --> LIST["Open Saydaliyati - prescriptions list S-11"]
    LIST --> ONLYFT{"All items free-text?"}
    ONLYFT -->|"Yes"| FTX(["Show prescription, all items: Not available in Saydaliyati EC-22"])
    ONLYFT -->|"No"| GPS{"GPS allowed and successful?"}
    GPS -->|"No"| ERR19["ERR-19 Couldn't get your location<br/>choose area from list"]
    ERR19 --> RES
    GPS -->|"Yes"| RES["Pharmacy results S-12<br/>open and visible only - full availability first, then by distance<br/>price, last updated, outdated badge [A]"]
    RES --> ANY{"Any pharmacy has any item?"}
    ANY -->|"No"| ERR18["ERR-18 No nearby pharmacy currently has these medicines"]
    ERR18 --> NOPH{"Change area or retry later?"}
    NOPH -->|"Change area"| ERR19
    NOPH -->|"Later"| LATER(["Exit: prescription stays pending"])
    ANY -->|"Yes"| FULL{"One pharmacy has all items?"}
    FULL -->|"Yes"| PICK["Pick one pharmacy<br/>pickup window shown before ordering"]
    FULL -->|"No"| HOW{"How does the patient proceed?"}
    HOW -->|"Split"| SPLIT["Split order: one sub-order per pharmacy"]
    HOW -->|"Buy available"| SUB["Order available items<br/>rest stays Pending on prescription"]
    PICK --> SEND["Send order"]
    SPLIT --> SEND
    SUB --> SEND

    SEND --> PHQ["Pharmacy receives order S-24<br/>items only, no diagnosis - countdown"]
    PHQ --> RESP{"Pharmacy response within 30 working minutes?"}
    RESP -->|"No response"| ERR21["ERR-21 The pharmacy didn't respond in time<br/>auto-cancelled"]
    RESP -->|"Reject with reason [A]"| ERR20["ERR-20 The pharmacy couldn't fulfil your order"]
    ERR21 --> BACKP["Items return to Pending<br/>other sub-orders continue EC-28"]
    ERR20 --> BACKP
    BACKP --> RES
    RESP -->|"Accept"| PRICE{"Patient accepts confirmed price? [A]"}
    PRICE -->|"No"| BACKP
    PRICE -->|"Yes"| PAY[["Manual payment verification<br/>UF-B — recipient: pharmacy"]]
    PAY --> PAYOK{"Payment confirmed?"}
    PAYOK -->|"No"| RELEASE["Order released EC-32 - Q-03"]
    RELEASE --> BACKP
    PAYOK -->|"Yes"| CODE["Pickup code issued: numeric and QR<br/>available offline S-13"]
    CODE --> REMIND["Daily reminder until pickup or expiry"]
    REMIND --> INWIN{"Someone with the code arrived within pickup window?"}
    INWIN -->|"No"| EXPIRED["Status: Expired<br/>payment outcome Q-04"]
    EXPIRED --> EXPX(["End: order expired"])
    INWIN -->|"Yes"| VERIFY["Pharmacy enters or scans code"]
    VERIFY --> VALID{"Code valid for this pharmacy?"}
    VALID -->|"No"| ERR22["ERR-22 Code not valid for this pharmacy"]
    ERR22 --> VERIFY
    VALID -->|"Yes"| DELIV["Status: Delivered"]
    DELIV --> DONE(["End: medicine in patient's or relative's hands"])

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class START,FTX,LATER,EXPX,DONE entry
    class CODE,DELIV success
    class ERR18,ERR19,ERR20,ERR21,ERR22,EXPIRED,RELEASE error
    class PHQ,REMIND,BACKP pending
```

### Explanation

1. A notification arrives and the patient opens **Saydaliyati**. Free-text items are never matched to pharmacies (BR-125); if all items are free-text, only the prescription is shown (EC-22).
2. **Location:** GPS first; if denied or failed, the patient picks an area from a list (BR-120, ERR-19, EC-25).
3. **Results:** only open, non-hidden pharmacies (BR-124), ranked with full-availability pharmacies first, then by distance (BR-121, FR-066), with price and "last updated" (BR-122), and an "outdated" badge after 48h (BR-134 [A], EC-27).
4. If no pharmacy has anything, ERR-18 is shown with options to change area or retry later (FR-070).
5. If no single pharmacy has everything, the patient chooses to **split** across pharmacies or **buy what's available** and leave the rest pending (BR-123, FR-067, FR-068). The pickup window is shown before ordering (FR-069).
6. The **pharmacy** sees items only, no diagnosis (FR-077, SEC-02), and must accept or reject within **30 minutes of its working hours**, or the order auto-cancels (BR-131, FR-075, FR-076). Rejection includes a reason (BR-133 [A]).
7. On rejection or auto-cancel, items return to Pending and the patient picks another pharmacy; other sub-orders continue (EC-28).
8. **Payment happens only after acceptance**, and the accepted price is the price to pay (BR-132). The patient may decline before paying if the price changed (EC-30 [A]).
9. Once the pharmacy confirms payment, a **pickup code** (numeric + QR) is issued and viewable offline (BR-095, FR-079, FR-080). **Anyone holding the code can collect** (BR-096). A daily reminder runs until pickup or expiry (BR-098).
10. The pharmacy enters or scans the code; if it belongs to another pharmacy, ERR-22 is shown; otherwise the order is marked **Delivered** (FR-081).
11. If the pickup window ends, the order becomes **Expired** (FR-083); what happens to the payment is open (Q-04).

### Related PRD requirements
US-09 – US-11, US-19 – US-21, BR-095 – BR-099, BR-120 – BR-134, FR-064 – FR-083, AC-35 – AC-47, EC-22, EC-25 – EC-34, ERR-18 – ERR-22, SEC-02, SEC-10, NFR-005.

### Open questions
- **Q-04:** When a paid order expires uncollected, is the patient refunded fully, partially, or not at all?
- **Q-09:** Stock-out after acceptance and payment (EC-31): the pharmacy must refund and cancel, but handling and consequences are undefined — so it isn't drawn as a path.
- **Q-03:** How long the order waits for the patient's payment after acceptance (EC-32).
- **Q-19:** Can the same prescription be reordered? How are long-pending items handled?
- "Notify me when available" is deferred to Phase 3, so it is not drawn under ERR-18.

---

## UF-4 — Cancellation & refund

```mermaid
flowchart TD
    PSTART(["Patient opens booking S-09"]) --> PCAN["Tap Cancel"]
    PCAN --> STARTED{"Session already started?"}
    STARTED -->|"Yes"| ERR23["ERR-23 This booking can no longer be cancelled"]
    ERR23 --> PX(["Exit unchanged"])
    STARTED -->|"No"| TYPE{"Booking type?"}
    TYPE -->|"In-person - unpaid"| INP["Cancel with no financial effect [A]"]
    INP --> PCONF
    TYPE -->|"Prepaid"| TIME{"24h or more before start?"}
    TIME -->|"Yes - including exactly 24h"| SHOWR["Show outcome: full refund"]
    TIME -->|"No"| SHOWN["Show outcome: no refund"]
    SHOWR --> PCONF{"Patient confirms cancellation?"}
    SHOWN --> PCONF
    PCONF -->|"No"| PX
    PCONF -->|"Yes"| CANCELLED["Status: Cancelled<br/>clinic notified"]
    CANCELLED --> NEEDR{"Refund due?"}
    NEEDR -->|"No"| DONE0(["End: cancelled, no refund"])
    NEEDR -->|"Yes"| TASK

    CSTART(["Clinic cancels a booking<br/>doctor, staff, or manager"]) --> CREASON["Enter cancellation reason [A]"]
    SCHED(["Schedule edit S-17"]) --> CONFLICT{"Change affects existing bookings?"}
    CONFLICT -->|"No"| SAVED(["Schedule saved"])
    CONFLICT -->|"Yes"| ERR12["ERR-12 This change affects N bookings"]
    ERR12 --> SCHOICE{"Cancel bookings or abort?"}
    SCHOICE -->|"Abort"| SAVED2(["Exit unchanged"])
    SCHOICE -->|"Cancel bookings"| CREASON
    CREASON --> CCANC["Status: Cancelled by clinic<br/>patient notified"]
    CCANC --> CPAID{"Was the booking paid?"}
    CPAID -->|"No"| DONE1(["End"])
    CPAID -->|"Yes"| TASK

    OTHERSRC(["Other refund sources:<br/>paid async with no reply UF-1<br/>doctor no-show UF-2"]) --> TASK

    TASK["Refund task created for clinic S-18"] --> REFUND["Clinic refunds patient's wallet off-platform"]
    REFUND --> MARKED{"Clinic marked Refunded within 72h? [A]"}
    MARKED -->|"Yes"| NOTIFY["Patient notified of refund"]
    NOTIFY --> DONE2(["End: refunded"])
    MARKED -->|"No"| OVERDUE["Task shown as overdue to Admin S-29"]
    OVERDUE --> ENF(["Enforcement undefined Q-08"])

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class PSTART,PX,DONE0,CSTART,SCHED,SAVED,SAVED2,DONE1,OTHERSRC,DONE2,ENF entry
    class NOTIFY,SHOWR success
    class ERR23,ERR12,OVERDUE,SHOWN error
    class TASK,REFUND pending
```

### Explanation

**Patient cancellation:**
1. The patient opens the booking and taps Cancel. After the session starts, cancellation is not possible (ERR-23).
2. An **in-person** booking can be cancelled any time with no financial effect, since nothing was prepaid (BR-094 [A]).
3. A **prepaid** booking: ≥ 24h before start → **full refund** (BR-090); < 24h → **no refund** (BR-091). Exactly 24h counts as refund (EC-35 [A]).
4. **The outcome is shown before confirming** (FR-084, AC-48, AC-49).

**Clinic cancellation:**
5. The clinic cancels with a reason (FR-085 [A]) → **always a full refund** for a paid booking (BR-092, AC-50).
6. Removing availability that has bookings requires cancelling them, which counts as a clinic cancellation (BR-068 [A], ERR-12).

**Refund execution:**
7. A **refund task** is created for the clinic (FR-086). The clinic refunds the patient's wallet directly, marks "Refunded", and the patient is notified (BR-093).
8. Tasks overdue by more than 72h are shown to the Admin (FR-087 [A], EC-36). What the Admin does next is undefined (Q-08).

### Related PRD requirements
US-08, BR-068, BR-090 – BR-094, BR-043, BR-054, FR-084 – FR-087, AC-48 – AC-50, EC-35, EC-36, ERR-12, ERR-23.

### Open questions
- **Q-08:** How refunds are enforced and the Admin's role in disputes.
- Cancelling a paid async message before the doctor replies is not covered in the PRD (cancellation is defined for bookings only).
- The 72h overdue threshold (FR-087) is an assumption.

---

## UF-5 — Clinic & doctor onboarding, subscription

```mermaid
flowchart TD
    START(["Clinic Manager starts registration S-15"]) --> FORM["Enter clinic name, address, phone<br/>and clinic license"]
    FORM --> PIN{"Map pin set?"}
    PIN -->|"No"| ERR03["ERR-03 Set your location on the map"]
    ERR03 --> PIN
    PIN -->|"Yes"| DOCS["For each doctor: practice license<br/>and dermatology specialty certificate"]
    DOCS --> VALID{"Documents complete, valid type and size?"}
    VALID -->|"No"| ERR01["ERR-01 Please upload the required document"]
    ERR01 --> DOCS
    VALID -->|"Yes"| SUBMIT["Submit<br/>status: Pending Review - hidden from patients"]

    SUBMIT --> REVIEW{"Admin decision"}
    REVIEW -->|"Reject with mandatory reason"| ERR02["ERR-02 Not approved: reason<br/>applicant notified [A]"]
    ERR02 --> FIXIT["Edit and resubmit [A]"]
    FIXIT --> SUBMIT
    REVIEW -->|"Approve"| APPROVED["Clinic approved<br/>applicant notified [A]"]

    APPROVED --> SETUP["Clinic dashboard S-16:<br/>wallet IDs, invite doctors and staff by email"]
    SETUP --> DSET["Each doctor sets services and prices S-21<br/>async mode, live price and duration, in-person price"]
    DSET --> SCH["Doctor or staff sets schedule S-17<br/>live and in-person slots"]
    SCH --> DREV{"Admin approved the doctor's documents?"}
    DREV -->|"No - pending or rejected"| HIDDEN["Doctor hidden from patients"]
    HIDDEN --> DREV
    DREV -->|"Yes"| VISIBLE["Doctor visible to patients"]

    APPROVED --> TRIAL["Free trial starts Q-02 and Q-05"]
    TRIAL --> DUE{"At due date: Admin recorded payment?"}
    DUE -->|"Yes"| ACTIVE["Status: Active"]
    ACTIVE --> DUE
    DUE -->|"No"| GRACE["Status: Grace - 7 days"]
    GRACE --> PAID7{"Paid within 7 days?"}
    PAID7 -->|"Yes"| ACTIVE
    PAID7 -->|"No"| HID["Status: Hidden from new patients<br/>existing bookings and orders continue"]
    HID --> DUE

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class START entry
    class APPROVED,VISIBLE,ACTIVE success
    class ERR01,ERR02,ERR03,HID error
    class SUBMIT,HIDDEN,GRACE,TRIAL pending
```

### Explanation

1. The **Clinic Manager** self-registers the clinic: name, address, map pin, phone, clinic license, and for each doctor a practice license and dermatology specialty certificate (BR-003, FR-001, FR-002). No doctor without a physical clinic (BR-001); a private clinic is a one-doctor clinic (BR-002).
2. A missing document blocks submission (ERR-01, AC-01), as does a missing map pin (ERR-03, AC-05).
3. After submission the status is **Pending Review** and the clinic is **hidden** (BR-004, FR-003, AC-02).
4. The **Admin** approves or rejects with a **mandatory reason** (FR-004, AC-03). A rejected applicant edits and resubmits (BR-007 [A]).
5. After approval, the manager sets wallet IDs and invites doctors and staff by email (FR-007); each doctor configures services and prices; the schedule is set by the doctor or staff (BR-065, FR-043).
6. **Each doctor** stays hidden until the Admin approves their documents (BR-005, FR-008, AC-04). A clinic with zero visible doctors is not listed (EC-01 [A]).
7. **Subscription:** free trial, then monthly (BR-111). If unpaid at the due date, a **7-day grace period** starts, after which the partner is **hidden from new patients** while existing bookings continue (BR-113, FR-093, FR-094, AC-52, AC-53). Payments are recorded manually by the Admin (BR-114).

### Related PRD requirements
US-15, US-17, US-22, US-24, BR-001 – BR-007, BR-065 – BR-068, BR-110 – BR-114, FR-001 – FR-009, FR-043 – FR-046, FR-091 – FR-094, AC-01 – AC-05, AC-24, AC-25, AC-52, AC-53, EC-01 – EC-04, EC-38, ERR-01 – ERR-03.

### Open questions
- **Q-02 / Q-05 / Q-06:** When the trial starts, its length, and subscription prices.
- **Q-15:** Can a Clinic Manager also be a doctor in the same account?
- **EC-04:** An address change requires re-review while the clinic stays visible — an assumption, not drawn as a separate path.
- The PRD doesn't say whether a Hidden partner becomes visible immediately on payment (the diagram simply returns it to the due-date cycle).

---

## UF-6 — Pharmacy onboarding & daily operations

```mermaid
flowchart TD
    START(["Pharmacy starts registration S-22"]) --> FORM["Name, address, map pin, phone<br/>pharmacy license and pharmacist license"]
    FORM --> REQ{"Working hours and at least one wallet ID entered? [A]"}
    REQ -->|"No"| BLOCK["Submission blocked AC-06"]
    BLOCK --> FORM
    REQ -->|"Yes"| DOCQ{"Documents and pin valid?"}
    DOCQ -->|"No"| ERR01["ERR-01 or ERR-03"]
    ERR01 --> FORM
    DOCQ -->|"Yes"| SUBMIT["Status: Pending Review - receives no orders"]
    SUBMIT --> REVIEW{"Admin decision"}
    REVIEW -->|"Reject with reason"| ERR02["ERR-02 with reason - edit and resubmit"]
    ERR02 --> FORM
    REVIEW -->|"Approve"| APPROVED["Pharmacy approved"]
    APPROVED --> SETTINGS["Settings S-25: working hours<br/>pickup window, wallet IDs"]
    SETTINGS --> INV["Set price and stock for catalog items S-23<br/>pharmacies don't add medicines"]

    INV --> DAY(["Start of working day"])
    DAY --> OPENQ{"Pharmacy open?"}
    OPENQ -->|"No - temporary closure"| CLOSED["Toggle Closed: hidden from results"]
    CLOSED --> DAY
    OPENQ -->|"Yes"| UPD{"Prices or stock changed?"}
    UPD -->|"Yes"| EDIT["Update changed items"]
    UPD -->|"No"| BULK["Bulk confirm: no change [A]"]
    EDIT --> STAMP["Last-updated timestamp recorded"]
    BULK --> STAMP
    STAMP --> ORD[["Receive orders, confirm payments, deliver<br/>see UF-D"]]

    DAY --> STALE{"No update for over 48h? [A]"}
    STALE -->|"Yes"| OUTD["Outdated badge shown to patients<br/>still orderable"]

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    classDef pending fill:#fef9c3,stroke:#a16207,color:#713f12
    class START,DAY entry
    class APPROVED,STAMP success
    class BLOCK,ERR01,ERR02,OUTD error
    class SUBMIT,CLOSED pending
```

### Explanation

1. **Registration** works like clinics: self-registration, document upload, Admin approval (BR-010, FR-012). Required documents are derived from "same as clinics" (BR-011 [A]).
2. Working hours and at least one wallet ID are mandatory (BR-012, BR-013, FR-011 [A], AC-06), since working hours drive the 30-minute rule and hiding closed pharmacies.
3. After approval the pharmacy sets its pickup window (BR-097) and price and stock **for catalog items only** (BR-089, FR-071).
4. **Daily:** update prices and stock (BR-130), with a quick "no change" confirm screen (FR-072 [A]). Every update is timestamped and shown (FR-073).
5. The pharmacy can toggle **Closed** temporarily (e.g. during a power cut) and disappears from results (FR-013 [A], EC-05, AC-07).
6. No update for more than 48h shows an "outdated" badge (BR-134 [A]).
7. The subscription follows the same cycle as clinics in UF-5.

### Related PRD requirements
US-18, US-19, US-22, BR-010 – BR-013, BR-089, BR-097, BR-130, BR-134, FR-010 – FR-013, FR-071 – FR-073, AC-06, AC-07, EC-05, ERR-01 – ERR-03.

### Open questions
- **Q-16:** Does a pharmacy need multiple staff logins?
- **Q-02 / Q-05 / Q-06:** Trial start, length, and prices.
- The document list (BR-011) and mandatory working hours and wallet (FR-011) are assumptions.

---

## UF-7 — Admin console

```mermaid
flowchart TD
    START(["Admin logs in"]) --> HOME{"What to handle?"}

    HOME -->|"Verification"| VQ["Verification queue S-26<br/>clinics, doctors, pharmacies"]
    VQ --> VDOC["View uploaded documents"]
    VDOC --> VDEC{"Decision"}
    VDEC -->|"Approve"| VOK["Approve and notify applicant [A]"]
    VDEC -->|"Reject"| VREASON{"Reason entered?"}
    VREASON -->|"No"| VBLOCK["Action blocked until reason entered AC-03"]
    VBLOCK --> VREASON
    VREASON -->|"Yes"| VREJ["Reject and notify applicant with reason"]

    HOME -->|"Catalog"| CQ["Catalog and addition requests S-27"]
    CQ --> CTYPE{"Task type"}
    CTYPE -->|"Doctor's addition request"| CREQ{"Request decision"}
    CREQ -->|"Reject"| CREJ["Reject request"]
    CREQ -->|"Approve"| CNEW
    CTYPE -->|"Create or edit item"| CNEW["AR name, EN name, form, strength"]
    CNEW --> CDUP{"Item with same EN name,<br/>form and strength exists? [A]"}
    CDUP -->|"Yes"| ERR17["ERR-17 This medicine already exists<br/>edit existing"]
    ERR17 --> CNEW
    CDUP -->|"No"| CSAVE["Save item"]
    CTYPE -->|"Deactivate item"| CDEACT["Deactivate item [A]<br/>kept on existing prescriptions and orders<br/>hidden from new prescriptions"]

    HOME -->|"Subscriptions"| SQ["Partners and subscriptions S-28"]
    SQ --> SPAY["Record subscription payment manually"]
    SPAY --> SSTAT["Update status: Trial, Active, Grace, Hidden"]

    HOME -->|"Monitoring"| OQ["Overdue refunds and suspensions S-29"]
    OQ --> OLATE["View refund tasks overdue by more than 72h [A]"]
    OLATE --> OACT{"Suspend an account?"}
    OACT -->|"Yes"| OSUS["Suspend partner or patient account with reason [A]"]
    OACT -->|"No"| OQ8(["Handling undefined Q-08"])

    VOK --> BACK(["Back to console"])
    VREJ --> BACK
    CREJ --> BACK
    CSAVE --> BACK
    CDEACT --> BACK
    SSTAT --> BACK
    OSUS --> BACK

    classDef entry fill:#dbeafe,stroke:#1d4ed8,color:#1e3a8a
    classDef success fill:#dcfce7,stroke:#15803d,color:#14532d
    classDef error fill:#fee2e2,stroke:#b91c1c,color:#7f1d1d
    class START,BACK,OQ8 entry
    class VOK,CSAVE,SSTAT success
    class VBLOCK,ERR17,OLATE error
```

### Explanation

1. **Verification queue:** the Admin reviews documents of clinics, doctors, and pharmacies and approves or rejects; **rejection requires a reason** (FR-004, FR-095, AC-03).
2. **Catalog:** only the Admin creates, edits, and deactivates items (BR-087, FR-060); each item has an Arabic and an English name (BR-088). The Admin handles doctors' addition requests (FR-061). Duplicates are blocked (FR-062 [A], ERR-17). A deactivated item stays on existing prescriptions and orders (FR-063 [A], EC-24).
3. **Subscriptions:** the Admin records payments manually and tracks statuses (BR-114, FR-091, FR-092).
4. **Monitoring:** the Admin sees overdue refund tasks (FR-087 [A]) and can suspend any account with a reason (FR-098 [A]).
5. **The Admin has no access to medical content** in the MVP (FR-099 [A]).

### Related PRD requirements
US-22 – US-24, BR-004, BR-005, BR-087, BR-088, BR-113, BR-114, FR-004, FR-060 – FR-063, FR-087, FR-091 – FR-099, AC-03, AC-33, AC-34, AC-52, EC-24, ERR-17.

### Open questions
- **Q-08:** What action is expected from the Admin on overdue refunds or disputes?
- **Q-10:** Can the Admin access medical content for disputes or abuse reports?
- **Q-20:** Who compiles the initial catalog and areas list?
- The suspension power (FR-098) and notifying the applicant of the decision (FR-005) are assumptions.

---

## Summary of open questions affecting the flows

| Question | Affected flows | Design impact |
|---|---|---|
| Q-03 Payment-confirmation window and escalation | UF-1, UF-2, UF-B, UF-D | Countdown on payment screen; ERR-15 copy |
| Q-04 Payment outcome of an expired order | UF-D | What the patient sees in "Expired" state |
| Q-07 Can live chat be free? | UF-2 | Skip payment step if free is allowed |
| Q-08 Disputes and refund enforcement | UF-B, UF-4, UF-7 | Path after the single correction; overdue-refund path |
| Q-09 Stock-out after payment | UF-D | Pharmacy refund path not drawn |
| Q-10 Admin access to medical content | UF-0, UF-7 | Extra Admin screens if allowed |
| Q-12 Reminder timing | UF-2, UF-3 | Notification scheduling |
| Q-15 Clinic Manager also a doctor? | UF-0, UF-5 | Role switching in navigation |
| Q-16 Multiple pharmacy logins | UF-0, UF-6 | Pharmacy staff management |
| Q-18 Minimum booking notice | UF-2, UF-3 | Hiding near-term slots |
| Q-19 Prescription refills and validity | UF-C, UF-D | Prescription state after ordering |
| Q-02 / Q-05 / Q-06 Subscription | UF-5, UF-6 | Trial start and length |
