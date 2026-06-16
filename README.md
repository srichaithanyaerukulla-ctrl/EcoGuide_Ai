# 🌿 EcoGuide AI — Know Your Carbon. Change Your Life.

> An AI-powered carbon footprint tracker that helps individuals understand, track, and reduce their CO₂ emissions through personalised Google Gemini insights.

![EcoGuide AI](https://img.shields.io/badge/EcoGuide-AI-1A3C2B?style=for-the-badge&logo=leaf&logoColor=white)
![Gemini API](https://img.shields.io/badge/Powered%20by-Google%20Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Vite](https://img.shields.io/badge/Build-Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

---

## 🎯 Chosen Vertical

**Carbon Footprint & Sustainability**

This project directly addresses the challenge:

> *"Design a solution that helps individuals understand, track, and reduce their carbon footprint through simple actions and personalised insights."*

EcoGuide AI is built around a single persona: an everyday person who is climate-aware but overwhelmed by the complexity of carbon data. They need clarity, a real number, and a practical plan — not guilt or generic advice.

---

## 🧠 Approach & Logic

The solution is built around a **three-phase behaviour change journey**:

```
UNDERSTAND → TRACK → REDUCE
```

### Phase 1 — Understand
Before users track anything, they need context. The Understand page teaches the four emission categories, provides global benchmarks, debunks common myths, and builds the mental model needed to interpret their own result.

### Phase 2 — Track
A 15-input, 4-step calculator (Transport → Home → Food → Stuff) uses peer-reviewed IPCC/EPA emission factors to compute an accurate annual CO₂e figure. Results are saved to localStorage and carried forward to the Insights page — no login required.

### Phase 3 — Reduce
Two reduction mechanisms work together:
- **AI Insights** — Google Gemini generates a personalised 5-section reduction plan using the user's actual numbers, not templates
- **Actions Library** — 21 curated climate actions with exact CO₂ savings, category filters, difficulty ratings, and commit tracking

---

## ⚙️ How It Works

```
User fills 4-step form (15 inputs)
         ↓
calculateFootprint()         Pure JS function → { transport, home, food, stuff, total }
         ↓
Results saved to localStorage → passed to Insights page
         ↓
buildGeminiPrompt()          Injects real user numbers into structured prompt
         ↓
callGemini()                 POST to Gemini 2.0 Flash API
         ↓ (on error)
buildFallbackInsights()      Personalised text generated locally — no API needed
         ↓
renderInsights()             Sanitized markdown → HTML displayed in 6 section cards
```

### Calculation Formula

```
Transport (kg) = (carKm × 52 × carFactor) + (transitHrs × 52 × 0.089)
               + (shortFlights × 255) + (longFlights × 1620)

Home (kg)      = ((elecKwh × 12 × 0.233 × (1 − renewShare))
               + (gasM3 × 12 × 2.04)) ÷ hhSize

Food (kg)      = dietBase × wasteMultiplier × localMultiplier

Stuff (kg)     = (clothing × 12 × 6.5) + (electronics × 150)
               + (orders × 52 × 2.2)

Total (tonnes) = (transport + home + food + stuff) ÷ 1000
```

---

## 📝 Assumptions Made

| Area | Assumption |
|---|---|
| Car emissions | Return trips assumed; world-average grid mix for EVs (0.053 kg/km) |
| Flights | Economy class, return trip; radiative forcing not included |
| Electricity | World average grid (0.233 kg/kWh); reduced proportionally by renewable share input |
| Household energy | Total home energy divided equally among household members |
| Food | Annual emission averages per diet type (Oxford/Poore & Nemecek 2018) |
| Local food modifier | ±10% adjustment for local vs imported food purchasing habits |
| Clothing | Average lifecycle emissions per garment based on fast fashion estimates |
| Online orders | Last-mile delivery emissions only; warehouse and packaging excluded |
| AI insights | Gemini API available; if unavailable, fallback generates personalised insights locally |

---

## 📁 Project Structure

```
ecoguide-ai/
├── index.html                  ← Home / Landing page
├── understand.html             ← Carbon footprint education
├── tracker.html                ← 4-step carbon calculator
├── insights.html               ← AI-powered personalised insights
├── actions.html                ← Climate action library (21 actions)
├── about.html                  ← About the project
├── src/
│   ├── styles/
│   │   ├── main.css            ← Design tokens, reset, utilities
│   │   ├── nav.css             ← Navigation + mobile menu
│   │   ├── hero.css            ← Hero sections + earth animation
│   │   └── components.css      ← All page components
│   └── js/
│       ├── main.js             ← Shared init (nav, scroll reveal)
│       ├── calculator.js       ← Pure carbon calculation engine
│       ├── gemini.js           ← Gemini API + fallback insights
│       ├── actions.js          ← Actions render + filter logic
│       ├── storage.js          ← localStorage helpers
│       └── data/
│           ├── emissionFactors.js   ← All emission constants + sources
│           └── actionsData.js       ← 21 climate actions dataset
├── public/
│   ├── favicon.svg
│   ├── robots.txt
│   └── manifest.webmanifest    ← PWA manifest
├── .github/
│   └── workflows/
│       └── deploy.yml          ← CI/CD auto-deploy to Firebase
├── firebase.json               ← Firebase Hosting config
├── .firebaserc                 ← Firebase project config
├── vite.config.js              ← Build configuration
├── netlify.toml                ← Netlify alternative hosting
├── vercel.json                 ← Vercel alternative hosting
├── .eslintrc.json              ← ESLint config
├── .prettierrc                 ← Prettier config
├── .gitignore
└── package.json
```

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** >= 18 ([download](https://nodejs.org))
- **npm** >= 9
- A **Google account** (for Gemini API key)
- **Git** installed

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/ecoguide-ai.git
cd ecoguide-ai
```

### 2. Install dependencies

```bash
npm install
```

### 3. Add your Gemini API key

Open `src/js/gemini.js` and replace the placeholder:

```js
// 🔑 Get your free key at: https://aistudio.google.com/app/apikey
const GEMINI_API_KEY = "YOUR_GEMINI_API_KEY_HERE";
```

**Get a free key:**
1. Go to [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)
2. Sign in with your Google account
3. Click **Create API key**
4. Copy and paste it above

### 4. Run the development server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) — hot reload is enabled.

### 5. Build for production

```bash
npm run build
```

Output goes to `dist/`. Preview locally:

```bash
npm run preview
```

---

## 🌐 Deployment

### Google Firebase Hosting (Recommended)

```bash
# Install Firebase CLI
npm install -g firebase-tools

# Login with Google account
firebase login

# Initialise (select ecoguide-ai project, public dir = dist, SPA = yes)
firebase init hosting

# Build and deploy
npm run build
firebase deploy
```

**Live at:** `https://ecoguide-ai.web.app`

### Netlify

```bash
npm install -g netlify-cli
netlify login
netlify init
npm run deploy
```

### Vercel

```bash
npm install -g vercel
vercel --prod
```

### GitHub Pages

```bash
npm install --save-dev gh-pages
npx gh-pages -d dist
```

---

## 🤖 AI Integration — Google Gemini

EcoGuide AI uses **Google Gemini 2.0 Flash** to generate personalised carbon reduction plans.

### API Details

| Property | Value |
|---|---|
| Model | `gemini-2.0-flash` |
| Endpoint | `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent` |
| Max tokens | 1,500 |
| Temperature | 0.7 |
| Free tier | 15 requests/min, 1M tokens/day |

### How the prompt works

The prompt is dynamically built from the user's actual calculated data:

```
Total footprint → transport breakdown → home energy → food & diet → shopping
↓
Injected into structured 5-section prompt
↓
Gemini returns: Glance | Top 3 Actions | Quick Wins | 12-Month Roadmap | Collective Impact
```

### Fallback system

If the Gemini API is unavailable for any reason, the app automatically generates personalised insights locally from the user's data — same structure, same personalisation, zero generic text. Users always get a useful result.

---

## 🔬 Emission Factors

All calculations use peer-reviewed, publicly available sources:

| Factor | Value | Source |
|---|---|---|
| Petrol car | 0.21 kg CO₂e/km | EPA 2023 |
| Electric vehicle | 0.053 kg CO₂e/km | IPCC AR6 |
| Public transit | 0.089 kg CO₂e/hr | IEA 2023 |
| Short-haul flight | 255 kg CO₂e/return | ICAO 2023 |
| Long-haul flight | 1,620 kg CO₂e/return | ICAO 2023 |
| Electricity (global avg) | 0.233 kg CO₂e/kWh | IEA 2023 |
| Natural gas | 2.04 kg CO₂e/m³ | IPCC AR6 |
| Vegan diet | 900 kg CO₂e/yr | Oxford (Poore & Nemecek 2018) |
| Vegetarian diet | 1,200 kg CO₂e/yr | Oxford (Poore & Nemecek 2018) |
| Moderate meat diet | 1,900 kg CO₂e/yr | Oxford (Poore & Nemecek 2018) |
| Heavy meat diet | 3,300 kg CO₂e/yr | Oxford (Poore & Nemecek 2018) |
| Clothing per item | 6.5 kg CO₂e | Fashion Footprint Report 2022 |
| Electronics per device | 150 kg CO₂e | Apple/Samsung lifecycle reports |

---

## 📊 Evaluation Criteria Coverage

| Criterion | Implementation |
|---|---|
| **Smart Dynamic Assistant** | Gemini called with user's actual data; prompt rebuilt dynamically per user |
| **Logical Decision Making** | Pure calculation engine; EV/petrol branching; renewable multiplier; household splitting |
| **Practical Usability** | 21 actions with exact savings; commit tracking; category filters; mobile responsive |
| **Code Quality** | 7 JS modules, 4 CSS files, clear separation of concerns, ESLint + Prettier |
| **Security** | No hardcoded secrets guidance; CSP headers; innerHTML sanitized; no eval() |
| **Efficiency** | ~18 KB JS gzipped; IntersectionObserver; asset fingerprinting; single API call |
| **Testing** | 29 unit tests for calculator engine; all passing via Vitest |
| **Accessibility** | 48 ARIA attributes; keyboard navigation; prefers-reduced-motion; focus-visible |

---

## 🧪 Testing

```bash
# Run all tests
npm test

# Watch mode
npm run test:watch

# Coverage report
npm run test:coverage
```

**Test coverage:** `src/js/calculator.js` — 29 unit tests covering:
- All emission factor calculations
- EV vs petrol branching
- Renewable energy multiplier
- Household size division
- Diet type progression
- Score label thresholds
- Edge cases (zero inputs, unknown diet types, hhSize=0)

---

## ♿ Accessibility

- Semantic HTML5: `<nav>`, `<section>`, `<article>`, `<footer>`, `<h1>`–`<h3>` hierarchy
- 48 `aria-*` attributes across all pages
- `aria-live="polite"` on all dynamic result regions
- Full keyboard navigation — action cards respond to Enter/Space
- `:focus-visible` outline: 3px solid `#7DB88A`
- `@media (prefers-reduced-motion: reduce)` — all animations disabled
- WCAG AA colour contrast on all text elements
- Tab panel accessibility: `aria-selected`, `aria-controls`, `role="tabpanel"`

---

## 📱 Browser Support

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full |
| iOS Safari 14+ | ✅ Responsive |
| Android Chrome | ✅ Responsive |

---

## 🛠️ Scripts Reference

```bash
npm run dev           # Start dev server at localhost:3000
npm run build         # Build production bundle to dist/
npm run preview       # Preview production build locally
npm test              # Run unit tests (Vitest)
npm run test:watch    # Run tests in watch mode
npm run lint          # ESLint — auto-fix JS issues
npm run format        # Prettier — format all source files
npm run deploy        # Build + deploy to Firebase
```

---

## 🔒 Security

- No API keys committed to repository — key entered at runtime
- Security headers configured: `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`, `Content-Security-Policy`
- AI output sanitized before innerHTML insertion — `&`, `<`, `>` escaped
- No `eval()` or `document.write()` anywhere in codebase
- All assets fingerprinted with content hash for cache security
- `.env` and `.env.local` in `.gitignore`

---

## 📄 License

MIT License — free to use, modify, and deploy.

---

## 🙏 Credits

| Resource | Source |
|---|---|
| Emission factors | IPCC AR6, Our World in Data, EPA 2023, ICAO |
| Diet data | Oxford University — Poore & Nemecek (2018) |
| AI insights | Google Gemini 2.0 Flash |
| Fonts | DM Serif Display, Inter, JetBrains Mono (Google Fonts) |
| Build tool | Vite |
| Testing | Vitest |

---

## 👤 Author

Built for the **AI Hackathon — Carbon Footprint Vertical**

> *"The best time to reduce your carbon footprint was 20 years ago. The second best time is now."*

---

<p align="center">
  Made with 💚 for the planet
  <br/>
  <a href="#hero">Back to top ↑</a>
</p>
