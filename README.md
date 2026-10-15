# Shape Yellow Points

A phone-first tool for sales staff of **Rishabh Industries (Shape), Ratlam** to track
shop sales, earn Yellow Points, and reach tour rewards (Goa / Bangkok / Vietnam).
Works offline as an installable app (PWA), and – once Firebase is turned on – syncs
every phone's data to a private **admin dashboard**.

Made by **Sachin Kumar** · 7681952023 · Sk6747806@gmail.com

---

## 1. Just use it

Open `index.html` (or the live site). On first open it asks for your name and number,
then you're in. Everything is saved on the phone automatically.

- **Parties** – your shops. Tap **+** to add one and pick its tour target.
- **New bill** – choose a shop, search a product, enter cartons/pieces, save.
- **Report** – leaderboard + CSV download.
- Tap a shop's number to **Call** or **WhatsApp** it. Send any bill/statement on WhatsApp.
- Top bar: **theme** (auto / light / dark), **refresh**, and **⋮ menu** (profile, admin, help).

---

## 2. Turn on the live Admin dashboard (Firebase) — one-time, ~5 minutes, free

This is what lets **you** see everyone's data from their phones.

1. Go to <https://console.firebase.google.com> and sign in → **Add project** → give it a
   name (e.g. `shape-yellow-points`) → you can skip Google Analytics → **Create**.
2. In the project, click the **web icon** `</>` ("Add app to get started") → give it a
   nickname → **Register app**. Firebase shows a `firebaseConfig = { ... }` block. Keep it open.
3. Left menu → **Build → Firestore Database** → **Create database** → start in
   **Production mode** → pick the closest location → **Enable**.
4. In Firestore → **Rules** tab, replace everything with the rules below → **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /reps/{id} {
         allow read, write: if request.auth != null;
       }
     }
   }
   ```

5. Left menu → **Build → Authentication** → **Get started** →
   **Sign-in method** → enable **Anonymous** → **Save**.
6. Open `index.html` in a text editor. Near the very top of the `<script>` you'll see:

   ```js
   const FB_CONFIG = {
     apiKey:"",
     authDomain:"",
     projectId:"",
     appId:""
   };
   ```

   Copy the matching values from the `firebaseConfig` block in step 2 into the quotes.
7. Save, re-deploy (see section 3), and open the app. The badge turns **"Live"**.
   Everyone who opens the app and enters their name now appears in **⋮ menu → Admin dashboard**.

**Admin password:** open the dashboard with the password in `index.html`:
`const ADMIN_PIN = "shape2026";` — change it to your own before sharing the tool.

> Note: the current rules let any signed-in app user read the data (fine for a closed
> team). For stronger lock-down (only your account can read everyone), add email login —
> ask for the next level.

If you never add Firebase keys, the tool still works 100% offline; the dashboard just
shows this one phone.

---

## 3. Deploy / publish the updates

The tool is plain static files, so any push to the GitHub Pages repo publishes it.
A ready-to-run command is given with this delivery (section "Push command").

Files in this app:

| File | Purpose |
|------|---------|
| `index.html` | the whole app (UI + logic + product catalog) |
| `sw.js` | service worker – offline cache |
| `manifest.webmanifest` | installable-app settings |
| `icon-192.png`, `icon-512.png` | app icons |

---

## Level 2 — what changed

1. **Admin login with username + password** — not just a PIN.
   Username **Sachin Kumar**, password **7681952023** (change `ADMIN_USER` / `ADMIN_PIN`
   at the top of `index.html`). The username check ignores capitals.
2. **Log out** button on the admin dashboard.
3. **Tap any number → Call / WhatsApp / Copy** sheet — on shops *and* your footer number.
4. **New landing page** with the 3 steps and a "What you can do" feature list.
5. **Liquid-glass theme (iOS-style)** added to the theme button.
   Tap it to cycle: Auto → Light → Dark → **Liquid glass** → Auto.
6. **Clearer cloud status** — says **"Local only"** (not "Offline"), **"Cloud…"** while
   connecting, and **"● Live"** when synced.
7. **Setup steps now inside the app** — the Admin dashboard shows the Firebase steps
   right there (plus the full version here in the README).
8. **More help** — expanded "How it works" sheet (points, tours, amounts, WhatsApp, backup, admin).
9. **Alignment fixed** — pop-ups are centered cards on desktop and bottom sheets on phones,
   so nothing overflows.

## Level 1 — what changed from the original

1. **Theme switch** – auto / light / dark button in the top bar (remembers your choice).
2. **Tap a number → Call or WhatsApp** – on every shop, plus your own number in the footer.
3. **Refresh button** in the top bar (reloads + re-syncs).
4. **5-second clarity** – welcome screen, "1 add shop → 2 make bill → 3 reach tour" strip,
   a hero banner, a How-it-works sheet, and friendlier empty states.
5. **Admin dashboard (live, Firebase)** – see every phone's data, open a user, edit it,
   and download. Password-protected.
6. **Enhanced logo** – clean built-in Shape logo in the header, welcome, and footer.
7. **One-click deploy command** (delivered alongside this zip).
8. **Fits mobile and desktop** – centered layout, no sideways scrolling, bigger tap targets.
9. **Alignment & design polish** – consistent cards, spacing, chips, and totals.
10. **CSV / JSON download** – per-user and all-users export from the dashboard; CSV on Report.
11. **Your name as a watermark** in the footer on every screen.
12. **Your Gmail** (Sk6747806@gmail.com) shown in the footer and used as the contact.

Everything from the original (parties, bills, product search, points, tours, WhatsApp
messages, backup/restore, offline PWA) is kept and working.
