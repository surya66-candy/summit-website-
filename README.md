# AURA 2026 — Fellowship Registration Web Application

A full-stack Node.js web application recreating the **AURA 2026 Rotaract Fellowship** event registration platform, reverse-engineered from `https://fellowship.aura3206.world/register`.

---

## 🌟 Features & Architecture

### 1. Multi-Step Registration Wizard (`/register`)
- **Step 1: Attendee Details**
  - Rotaractor toggle: *Yes, I'm a Rotaractor* vs *No, I'm a guest*.
  - Group selection (Groups 1–10) dynamically filtering the **104 registered Rotaract clubs** of Rotary International District 3206.
  - Role selection: President, Secretary, Board Member, Club Member, Campus Ambassador, Other.
  - Guest institution / college input for external guests.
  - Multi-attendee management (register 1 to 25 attendees in a single submission).
  - Validation: Full name, 10-digit Indian mobile number (`/^[6-9]\d{9}$/`), email validation, and intra-group duplicate email prevention.
  - Food preference selector: Vegetarian, Non-Vegetarian, Vegan.

- **Step 2: Scan & Pay / Bank Transfer**
  - Dynamic total calculation based on `count × ₹400`.
  - Live dynamic UPI QR code generator (`upi://pay?pa=sury00043111@suryoday...`) with exact payable amount.
  - Official bank transfer details with one-click copy buttons:
    - **Bank Name:** `Suryoday Small Finance Bank`
    - **Account Name:** `Rotaract Club of Coimbatore Texcity`
    - **Account Number:** `262000008343`
    - **IFSC Code:** `SURY0BK0000`
    - **UPI ID:** `sury00043111@suryoday`

- **Step 3: Upload Proof & Submit**
  - Transaction Reference / UTR input (min 3 chars).
  - Drag-and-drop / file upload zone for payment screenshot (PNG, JPG, WEBP under 5MB) with instant preview.
  - Real-time submission handling with duplicate registration conflict checks.

- **Confirmation Screen**
  - Unique registration ID generation (`AURA-XXXXXX-XXXX`).
  - Pass breakdown with individual attendee ticket codes (`AURA-XXXXXX-XXXX-P1`, etc.).
  - 24-hour verification notice and digital pass instructions.

### 2. Registration Status Lookup (`/status`)
- Real-time search by:
  - Registration ID (e.g. `AURA-437892-7666`)
  - Attendee Email address
  - Attendee Mobile number
  - Ticket code
- Displays verification state (`Pending Verification` or `Approved & Confirmed`) and individual attendee passes.

### 3. Mr & Ms Fellowship Contest (`/fellowship`)
- Pageant / contest application form reverse-engineered from the event schema:
  - Name, RI ID, Rotaract Club, Designation, Gender category, Age.
  - "Describe yourself in three words"
  - "Why do you want to participate?" (min 20 chars)
  - "What makes you unique?"
  - Passions and portrait photo upload.

### 4. Event Schedule & Info Hub (`/schedule`, `/info`)
- Detailed timeline from 09:00 AM Registration to 05:00 PM DJ & Awards.
- Venue address: *Outdoor Auditorium, Hindusthan College of Arts and Science, Coimbatore, Tamil Nadu 641028*.
- Direct link to Google Maps.

### 5. Private Admin Control Center (`/admin`)
- **Completely Hidden & Independent**: Zero admin links or buttons anywhere on the public website.
- **Passcode Protected**: Unlocks via session authentication (default passcode: `aura2026`).
- **100% Dynamically Editable**:
  - **Event Details**: Name, Tagline, District, Dates, Venue, Address, Maps link, Host Clubs, Event Chair, Stats Ribbons (2500+ attendees, 10+ acts, 10+ hours).
  - **Fees & Banking**: Toggle Registration Open/Closed gate, Ticket Fee (₹), Deadline, Bank Name, Account Name, Account Number, IFSC Code, UPI ID.
  - **Announcements**: Full CRUD to post, edit, and delete real-time updates.
  - **Highlights**: Add, edit, or remove attraction cards.
  - **Schedule Agenda**: Add, reorder, or edit timeline program items.
  - **FAQs**: Dynamic accordion editor.
  - **104 Rotaract Clubs Catalog**: Search, add new clubs, edit names and group numbers (1–10), and delete clubs.
  - **Registrations Manager**: Live attendee metrics, search & filter by status, UTR & payment screenshot lightbox, delegate breakdown modal, approve / reject status actions, and complete CSV export.
  - **Mr & Ms Fellowship Submissions**: Review contestant entries with uploaded portrait photos and details.
  - **Inquiries Inbox**: Review and manage messages submitted via the contact form.
  - **Email Notification Controls**: Configure SMTP credentials (Host, Port, SSL, User, App Password), toggle automated registration receipts and approval passes, test mail connectivity, and inspect the real-time dispatched outbox log.
  - **Security**: Change admin passcode directly from the dashboard.

### 6. Automated Attendee Email Notifications (`nodemailer`)
- **Instant Registration Receipt**:
  - Immediately triggered when a registration is completed.
  - Every attendee in the submission receives a customized email containing their unique Ticket Code (`AURA-XXXXXX-XXXX-P1`), Registration ID, Club affiliation, Food preference, Payment UTR, Event venue, and a one-click button to track live verification status.
- **Pass Approval Notification**:
  - Automatically dispatched when organizers click **✓ Approve** in the Admin Portal.
  - Releases the verified digital delegate pass with morning gate check-in instructions.
- **Resilient Delivery**:
  - Supports production SMTP servers (Gmail, SendGrid, Resend, Amazon SES, etc.).
  - Graceful dev fallback: if SMTP is not configured yet, messages are formatted and safely logged to the interactive admin outbox without blocking registration.

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+)

### Running the Application

```bash
cd "aura-fellowship-web"
npm start
```

Access Points:
- 🌐 **Public Website**: `http://localhost:3000`
- 🔐 **Private Admin Panel**: `http://localhost:3000/admin` *(Passcode: `aura2026`)*

---

## 📁 Project Structure

```text
aura-fellowship-web/
├── data/
│   ├── clubs.json         # 104 official Rotaract clubs across Groups 1-10
│   ├── settings.json      # Dynamic event info, bank credentials, announcements, FAQs
│   ├── registrations.json # Stores all submitted attendee registrations & UTRs
│   ├── fellowship.json    # Stores Mr & Ms Fellowship contest submissions
│   └── messages.json      # Stores contact form inquiries
├── public/
│   ├── assets/            # AURA brand logo, sponsor badges, icons
│   ├── uploads/           # Uploaded payment screenshots & contestant photos
│   ├── admin.html         # Private standalone admin control center SPA
│   ├── app.js             # Public website dynamic router & registration engine
│   ├── favicon.png        # Favicon
│   ├── index.html         # Main public SPA entry (zero admin links)
│   └── styles.css         # Dark glassmorphic design system
├── src/
│   ├── db.js              # Atomic JSON storage & conflict validation engine
│   └── server.js          # Express backend API, CSV exporter & admin REST routes
└── package.json
```
