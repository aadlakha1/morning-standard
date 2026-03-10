# HYDR8

Protein & electrolyte drinks by **Simiran Jha**.

**Tagline:** Beyond Water.

**Live site:** https://aadlakha1.github.io/morning-standard/
**Admin dashboard:** append `?admin` to the site URL

---

## Tech Stack

- Single self-contained `index.html` (no build tools, no dependencies)
- Dark-first athletic design with neon accent colors
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

### Powder Sticks (Box of 12)
| Flavor | Price | ID | Color |
|---|---|---|---|
| Green Lightning (Lemon-Lime) | $22 | `stick-lemon-lime` | Neon green |
| Berry Burst (Mixed Berry) | $24 | `stick-berry` | Purple |
| Citrus Shock (Orange-Grapefruit) | $22 | `stick-citrus` | Neon yellow |
| Midnight (Unflavored) | $20 | `stick-unflavored` | Dark gray |

### RTD Cans (Pack of 6)
| Flavor | Price | ID | Color |
|---|---|---|---|
| Arctic Blast (Lemon-Lime) | $28 | `can-lemon-lime` | Neon green |
| Voltage (Tropical Punch) | $30 | `can-tropical` | Hot pink |
| Ice Storm (Blue Raspberry) | $28 | `can-blue-rasp` | Electric blue |
| Mango Surge (Mango) | $30 | `can-mango` | Amber |
| Shadow (Unflavored) | $26 | `can-unflavored` | Matte black |

---

## Firebase Setup (Required)

The site is deployed but **Firebase must be configured** for orders and admin to work. The `firebaseConfig` block in `index.html` currently has placeholder values.

### Step 1: Create Firebase Project
1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. **Add Project** → name it `hydr8` → Create
3. Go to **Project Settings** (gear icon) → **Your apps** → click Web (`</>`)
4. Register app name: `hydr8`
5. Copy the `firebaseConfig` object

### Step 2: Paste Config
In `index.html`, find and replace the `firebaseConfig` block with your project's config.

### Step 3: Enable Firestore
1. Firebase Console → **Build** → **Firestore Database**
2. **Create database** → **Production mode** → Pick region → Done

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
2. Select products and quantities (sticks + cans)
3. Live order summary with running total
4. Click **Continue to Details**
5. Fill in name, email, phone, shipping address, optional notes
6. Click **Place Pre-Order** → saved to Firestore
7. Confirmation screen with unique order ID (e.g. `H8-M4X8K2AB`)
8. Social share buttons (X/Twitter, copy to clipboard)

---

## Site Sections

| Section | Description |
|---|---|
| **Hero** | "Beyond Water." headline, countdown timer, glitch text effect, CSS product showcase with 3D mouse-follow tilt |
| **Marquee** | Scrolling feature highlights (25g Protein, Zero Sugar, Lab Tested, etc.) |
| **Products** | Powder Sticks + RTD Cans cards with mini product renders and flavor pills |
| **Ingredient Strip** | Scrolling transparency strip showing exact ingredient amounts |
| **The Science** | 4-feature grid (25g Protein, Electrolytes, Zero Compromise, Instant Mix) |
| **Big Quote** | Full-width founder quote — Simiran Jha |
| **How It Works** | 3-step cards: Pick your fuel → We deliver → Perform |
| **Numbers** | Animated counter stats (8k+ signups, 4.9 rating, 25g protein, 0g sugar) |
| **Athletes** | 3 testimonial cards from CrossFit athlete, marathon runner, personal trainer |
| **Waitlist** | Email signup saved to Firestore |
| **Footer** | Links + social icons (Instagram, TikTok, Twitter) |

---

## Design Details

- **Color palette:** Neon green `#00ff88` primary, electric blue `#00d4ff`, hot pink `#ff2d78`, neon yellow `#e6ff00` on near-black `#0a0a0a`
- **Always dark** — no light mode
- **Typography:** SF Pro Display system font stack
- **Animations:** Scroll reveal, glitch text on hero h1, countdown timer, animated number counters, mouse-follow parallax on hero products, floating diamond particles (cycling neon colors), marquee scroll
- **Products:** Pure CSS illustrations — stick packs with diagonal stripe overlay, cans with neon color bands and glow effects, H8 seal
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
- [ ] Custom domain (e.g. hydr8.co)
- [ ] Add Instagram/TikTok links to footer social icons
- [ ] Google Analytics or Plausible for traffic tracking
- [ ] SEO: Open Graph tags, structured data
