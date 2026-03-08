# Morning Standard

Premium coffee sachets and cold brew cans by **Simiran Jha**.

**Live site:** https://aadlakha1.github.io/morning-standard/
**Repo:** https://github.com/aadlakha1/morning-standard
**Admin dashboard:** https://aadlakha1.github.io/morning-standard/?admin

---

## Tech Stack

- Single self-contained `index.html` (no build tools, no dependencies)
- Apple design language (SF Pro fonts, frosted glass nav, pill buttons, scroll reveals)
- Firebase Firestore for real-time order + waitlist storage
- Firebase Auth (email/password) for admin access
- Hosted on GitHub Pages

---

## Project Structure

```
/
├── index.html          # Entire app (HTML + CSS + JS)
├── firestore.rules     # Firestore security rules (deploy manually)
└── README.md
```

---

## Products & Pricing

### Sachets (Box of 12)
| Flavor | Price | ID |
|---|---|---|
| Original Black | $18 | `sachet-original` |
| Oat Latte | $20 | `sachet-oat` |
| Vanilla | $20 | `sachet-vanilla` |
| Salted Caramel | $20 | `sachet-caramel` |

### Cold Brew Cans (Pack of 6)
| Flavor | Price | ID |
|---|---|---|
| Black Cold Brew | $24 | `can-black` |
| Oat Milk Latte | $26 | `can-oat` |
| Mocha | $26 | `can-mocha` |

---

## Firebase Setup (Required)

The site is deployed but **Firebase must be configured** for orders and admin to work. The `firebaseConfig` block in `index.html` currently has placeholder values.

### Step 1: Create Firebase Project
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. **Add Project** → name it `morning-standard` → Create
3. Go to **Project Settings** (gear icon) → **Your apps** → click Web (`</>`)
4. Register app name: `morning-standard`
5. Copy the `firebaseConfig` object

### Step 2: Paste Config
In `index.html`, find and replace this block (~line 470):
```js
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

### Step 3: Enable Firestore
1. Firebase Console → **Build** → **Firestore Database**
2. **Create database** → **Production mode** → Pick region (e.g. `us-central1`) → Done

### Step 4: Deploy Security Rules
1. Firestore → **Rules** tab
2. Paste the contents of `firestore.rules` from this repo → **Publish**

### Step 5: Enable Authentication
1. Firebase Console → **Build** → **Authentication** → **Get started**
2. Enable **Email/Password** sign-in provider
3. Go to **Users** tab → **Add user**
4. Enter admin email + password (used to log into the `?admin` dashboard)

### Step 6: Commit & Push
```bash
git add index.html && git commit -m "Add Firebase config" && git push origin main
```

---

## Security

| Layer | Detail |
|---|---|
| **Firestore rules** | Orders: public write-only (create), admin read/update/delete. Waitlist: public create, admin read. Everything else denied. |
| **Field validation (rules)** | Required fields enforced, status must be `pending` on creation, total must be > 0 and <= $10,000, max 20 line items |
| **Firebase Auth** | Admin dashboard requires email/password sign-in |
| **Rate limiting** | Client-side: max 5 orders per hour per browser (localStorage) |
| **Input sanitization** | All user input HTML-escaped via `textContent` before storage and rendering |
| **XSS prevention** | No `innerHTML` with raw user data — all escaped |

---

## Admin Dashboard

Access: append `?admin` to the site URL.

### Features
- **Real-time orders** — new orders appear instantly via Firestore `onSnapshot`
- **Stats** — total orders, total revenue, items ordered, avg order value
- **Search** — filter by customer name, email, or order ID
- **Status filter** — view by pending / confirmed / shipped / cancelled
- **Status management** — click status badge to cycle: `pending → confirmed → shipped → cancelled`
- **Delete orders** — remove individual orders with confirmation
- **Export CSV** — download all orders as a spreadsheet
- **Sign out** — ends Firebase Auth session

### Order Statuses
| Status | Meaning |
|---|---|
| `pending` | New order, not yet reviewed |
| `confirmed` | Order accepted, preparing |
| `shipped` | Order shipped |
| `cancelled` | Order cancelled |

---

## Customer Pre-Order Flow

1. Click **Pre-Order Now** (hero or nav)
2. Select products and quantities (sachets + cans)
3. Live order summary with running total
4. Click **Continue to Details**
5. Fill in name, email, phone, shipping address, optional notes
6. Click **Place Pre-Order** → saved to Firestore
7. Confirmation screen with unique order ID (e.g. `MS-M4X8K2AB`)

---

## Site Sections

| Section | Description |
|---|---|
| **Hero** | Headline, founder credit, CTA, CSS product showcase with 3D hover effects |
| **Marquee** | Scrolling feature highlights |
| **Products** | Sachets + Cans cards with mini product renders and flavor pills |
| **Why Us** | 4-feature grid (Specialty Grade, 30-Second Ritual, Clean Ingredients, Sustainable) |
| **Big Quote** | Full-width founder quote — Simiran Jha |
| **How It Works** | 3-step cards with connector arrows |
| **Numbers** | Stats bar (12k+ waitlist, 4.9 rating, 30s brew, 0g sugar) |
| **Reviews** | 3 testimonial cards with avatars and star ratings |
| **Waitlist** | Email signup saved to Firestore |
| **Footer** | Links + social icons (Instagram, TikTok, Twitter) |

---

## Design Details

- **Color palette:** Gold accent `#c49b2f`, espresso `#3c2415`, cream `#f0ead6`, dark `#1d1d1f`
- **Dark mode:** Full automatic support via `prefers-color-scheme`
- **Typography:** SF Pro Display system font stack
- **Animations:** Scroll reveal (IntersectionObserver), parallax hero, floating coffee bean particles, pulsing ambient glow, foil shine on sachets, marquee scroll
- **Products:** Pure CSS illustrations — sachets with tear notch, dashed tear line, gold seal, weight labels. Cans with aluminum top, pull tab, bottom rim, metallic shimmer
- **Responsive:** Fully mobile-friendly, nav collapses, grid layouts adapt

---

## Local Development

```bash
# Just open the file
open index.html

# Or use a local server
python3 -m http.server 8000
# then visit http://localhost:8000
```

---

## Deployment

The site auto-deploys via GitHub Pages on push to `main`.

```bash
git add -A && git commit -m "your message" && git push origin main
```

Changes go live within 1-2 minutes.

---

## Future Improvements

- [ ] Connect Firebase config (required for orders to work)
- [ ] Add real product photography to replace CSS illustrations
- [ ] Connect payment processor (Stripe) for real transactions
- [ ] Email notifications on new orders (Firebase Cloud Functions)
- [ ] Custom domain (e.g. morningstandard.com)
- [ ] Add Instagram/TikTok links to footer social icons
- [ ] Google Analytics or Plausible for traffic tracking
- [ ] SEO: Open Graph tags, structured data
