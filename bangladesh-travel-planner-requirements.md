# 🗺️ Bangladesh Travel Planner — Requirements Analysis & Prompt Guide
**Project:** Bangladesh Bhromon (বাংলাদেশ ভ্রমণ) Travel Planner  
**Model:** Waterfall SDLC  
**Architecture:** MVC (Model–View–Controller)  
**Frontend:** MongoDB Stitch (Atlas App Services)  
**Backend:** Antigravity (following Stitch-generated design)  
**Inspired by:** [VromonGuide](https://vromonguide.com/)

---

## PART 1 — REQUIREMENTS ANALYSIS

### 1.1 Project Overview
A production-level travel planning web application targeted at Bangladeshi domestic travelers. It combines a smart budget tracker and an interactive trip planner with Google Maps integration. The app covers all 8 divisions of Bangladesh, their districts, and popular tourist spots — inspired by the VromonGuide content model.

---

### 1.2 Functional Requirements

#### Module 1: Budget Tracker
| ID | Requirement | Priority |
|----|-------------|----------|
| FR-BT-01 | User selects number of travelers (1–20+) | Must Have |
| FR-BT-02 | User inputs destination city/area | Must Have |
| FR-BT-03 | System suggests estimated costs per person: transport, hotel, food, activities | Must Have |
| FR-BT-04 | User can manually override any cost field | Must Have |
| FR-BT-05 | Total trip cost calculated in real time | Must Have |
| FR-BT-06 | Cost breakdown chart (pie/bar) displayed | Should Have |
| FR-BT-07 | Budget saved to user profile (logged-in users) | Should Have |
| FR-BT-08 | Currency shown in BDT (Bangladeshi Taka ৳) | Must Have |
| FR-BT-09 | Share trip budget as PDF or link | Could Have |
| FR-BT-10 | System suggests budget tiers: Budget / Mid-range / Luxury | Should Have |

#### Module 2: Trip Planner
| ID | Requirement | Priority |
|----|-------------|----------|
| FR-TP-01 | User inputs destination city/area | Must Have |
| FR-TP-02 | Google Maps embedded, centered on chosen city | Must Have |
| FR-TP-03 | Nearby hotels displayed as map markers + list cards | Must Have |
| FR-TP-04 | Nearby restaurants displayed as map markers + list cards | Must Have |
| FR-TP-05 | Nearby bus counters/stations displayed | Must Have |
| FR-TP-06 | Nearby bike/cycle rentals displayed | Must Have |
| FR-TP-07 | User can filter markers by category (hotel/food/transport/rental) | Must Have |
| FR-TP-08 | Clicking marker shows name, address, rating, distance | Must Have |
| FR-TP-09 | Itinerary builder: add stops to a day-wise schedule | Should Have |
| FR-TP-10 | Popular spots suggested per destination (Cox's Bazar, Sajek, Sylhet etc.) | Must Have |
| FR-TP-11 | Travel tips per destination (inspired by VromonGuide articles) | Should Have |
| FR-TP-12 | Directions/routing between selected stops | Should Have |
| FR-TP-13 | Save/export trip plan as PDF or shareable link | Could Have |
| FR-TP-14 | Multi-day trip planning support | Should Have |

#### Module 3: User Management
| ID | Requirement | Priority |
|----|-------------|----------|
| FR-UM-01 | User registration / login via email | Must Have |
| FR-UM-02 | Social login (Google) | Should Have |
| FR-UM-03 | Save and view past trips | Should Have |
| FR-UM-04 | Guest mode (no login required for basic use) | Must Have |

---

### 1.3 Non-Functional Requirements
| ID | Category | Requirement |
|----|----------|-------------|
| NFR-01 | Performance | Page load < 3 seconds on 4G mobile |
| NFR-02 | Scalability | Support 500+ concurrent users at MVP |
| NFR-03 | Availability | 99.5% uptime (Antigravity managed hosting) |
| NFR-04 | Security | HTTPS, JWT-based auth, input validation |
| NFR-05 | Usability | Bilingual UI: English + Bangla (বাংলা) |
| NFR-06 | Compatibility | Mobile-first, responsive for desktop |
| NFR-07 | SEO | Each destination has a crawlable route & meta |
| NFR-08 | Accessibility | WCAG 2.1 AA (color contrast, keyboard nav) |

---

### 1.4 MVC Architecture Mapping

```
┌─────────────────────────────────────────────────────────┐
│                         VIEW                            │
│   Stitch-generated Frontend (React/HTML)                │
│   - BudgetTrackerView                                   │
│   - TripPlannerView (Map + Sidebar)                     │
│   - DestinationDetailView                               │
│   - UserDashboardView                                   │
└────────────────────┬───────────────────────────────────-┘
                     │ HTTP/REST calls
┌────────────────────▼────────────────────────────────────┐
│                      CONTROLLER                         │
│   Antigravity Backend (Node.js / Python)                │
│   - BudgetController   → budget logic, cost estimation  │
│   - TripController     → itinerary CRUD, places lookup  │
│   - PlacesController   → Google Maps API wrapper        │
│   - AuthController     → JWT, session management        │
└────────────────────┬────────────────────────────────────┘
                     │ DB calls
┌────────────────────▼────────────────────────────────────┐
│                        MODEL                            │
│   MongoDB Atlas (via Stitch SDK / Antigravity ORM)      │
│   - User { id, name, email, savedTrips[] }              │
│   - Trip { id, userId, destination, days[], budget }    │
│   - Destination { city, division, spots[], tips[] }     │
│   - BudgetTemplate { city, transport, hotel, food, ৳ }  │
│   - Place { name, type, lat, lng, rating, address }     │
└─────────────────────────────────────────────────────────┘
```

---

### 1.5 Waterfall Phase Plan

| Phase | Activities | Deliverables |
|-------|------------|--------------|
| **1. Requirements** | Stakeholder interviews, competitor analysis (VromonGuide), user stories | This Document |
| **2. System Design** | DB schema, API contracts, wireframes, MVC mapping | Design Doc + Wireframes |
| **3. Implementation** | Stitch frontend + Antigravity backend build | Source Code |
| **4. Testing** | Unit, integration, UAT, load testing | Test Reports |
| **5. Deployment** | Antigravity deploy, domain setup, monitoring | Live URL |
| **6. Maintenance** | Bug fixes, content updates, new districts | Patch Releases |

---

### 1.6 Data Models

#### User
```json
{
  "_id": "ObjectId",
  "name": "string",
  "email": "string",
  "passwordHash": "string",
  "savedTrips": ["TripId"],
  "createdAt": "Date"
}
```

#### Trip
```json
{
  "_id": "ObjectId",
  "userId": "ObjectId",
  "title": "string",
  "destination": "string",
  "division": "string",
  "travelers": "number",
  "days": [
    {
      "dayNumber": 1,
      "date": "Date",
      "stops": [{ "placeId": "string", "note": "string", "time": "string" }]
    }
  ],
  "budget": {
    "transport": "number",
    "hotel": "number",
    "food": "number",
    "activities": "number",
    "total": "number"
  },
  "createdAt": "Date"
}
```

#### Destination
```json
{
  "_id": "ObjectId",
  "slug": "coxsbazar",
  "nameBn": "কক্সবাজার",
  "nameEn": "Cox's Bazar",
  "division": "Chittagong",
  "lat": 21.4272,
  "lng": 92.0058,
  "spots": ["string"],
  "budgetTemplate": {
    "budget": { "transport": 800, "hotel": 500, "food": 400 },
    "midrange": { "transport": 1500, "hotel": 1500, "food": 800 },
    "luxury": { "transport": 3000, "hotel": 5000, "food": 2000 }
  },
  "tips": ["string"],
  "coverImage": "string"
}
```

---

### 1.7 API Endpoints (REST Contract)

#### Auth
```
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/google
GET    /api/auth/me
```

#### Destinations
```
GET    /api/destinations              → list all
GET    /api/destinations/:slug        → single destination data
GET    /api/destinations/:slug/budget → cost estimates
```

#### Places (Google Maps Proxy)
```
GET    /api/places?lat=&lng=&type=hotel|restaurant|bus|bike&radius=3000
GET    /api/places/:placeId
```

#### Trips
```
GET    /api/trips                     → user's saved trips
POST   /api/trips                     → create trip
GET    /api/trips/:id
PUT    /api/trips/:id
DELETE /api/trips/:id
```

#### Budget
```
POST   /api/budget/estimate           → body: { city, travelers, tier }
```

---

## PART 2 — STITCH FRONTEND PROMPT

Paste this into **MongoDB Stitch / Atlas App Builder** UI Generator or its AI prompt interface:

---

```
Build a production-level Bangladesh travel planning web application called "বাংলাদেশ ভ্রমণ" (Bangladesh Bhromon).

DESIGN STYLE:
- Mobile-first, responsive layout
- Warm, vibrant aesthetic: deep teal (#006B5E) primary, saffron/golden (#F4A620) accent, off-white (#FAF7F2) background
- Bangla + English bilingual interface (toggle in navbar)
- Font: 'Hind Siliguri' for Bangla text, 'Plus Jakarta Sans' for English
- Inspired by VromonGuide.com — editorial travel blog meets utility app
- Nature photography hero banners for each destination

PAGES & COMPONENTS TO GENERATE:

1. HOME PAGE
   - Full-width hero with Bangladesh landscape photo, animated search bar
   - Search input: "কোথায় যেতে চান? (Where do you want to go?)" with division dropdown
   - Cards grid: 8 divisions (Dhaka, Chittagong, Sylhet, Rajshahi, Khulna, Barisal, Rangpur, Mymensingh)
   - Popular destinations horizontal scroll: Cox's Bazar, Sajek, Sundarbans, Srimangal, Bandarban, Kuakata
   - CTA section: "Plan Your Trip" + "Calculate Budget"
   - Navbar: Logo | Destinations | Trip Planner | Budget | Login

2. BUDGET TRACKER PAGE (/budget)
   - Step-by-step card form:
     Step 1: "How many people are traveling?" → number input (stepper +/-)
     Step 2: "Select destination" → searchable dropdown of all Bangladesh cities
     Step 3: "Budget tier" → 3 cards: Budget ৳ / Mid-range ৳৳ / Luxury ৳৳৳
   - Real-time cost breakdown table: Transport, Hotel/Night, Food/Day, Activities
   - Total cost displayed large with ৳ symbol
   - Animated pie/donut chart showing cost distribution
   - "Save Budget" and "Download PDF" buttons

3. TRIP PLANNER PAGE (/plan)
   - Split-screen layout: Left sidebar (40%) + Right Google Maps (60%)
   - Search bar at top: destination input
   - Left Sidebar contains:
     - Category filter tabs: 🏨 Hotels | 🍽️ Restaurants | 🚌 Bus Counters | 🚲 Bike Rentals
     - Scrollable list of place cards (name, rating stars, distance, thumbnail)
     - "Add to Itinerary" button on each card
     - Itinerary section below: drag-and-drop day-wise stops
   - Right side: Google Maps iframe/embed
     - Colored markers per category (blue=hotel, orange=restaurant, green=bus, purple=bike)
     - Clicking map marker highlights sidebar card and vice versa
   - Mobile: tabs switch between map view and list view

4. DESTINATION DETAIL PAGE (/destination/:slug)
   - Hero banner with destination cover photo + name in Bangla & English
   - Tabs: Overview | Map | Hotels | Food | Getting There | Tips
   - Overview: description, best time to visit, weather
   - Map tab: embedded Google Map with all POIs
   - Getting There: bus/train/launch routes from Dhaka
   - Tips: travel tips list (কিভাবে যাবেন, কোথায় থাকবেন, কী খাবেন)
   - Sidebar: Quick Budget Calculator widget (compact version)

5. USER DASHBOARD (/dashboard)
   - Saved trips list with thumbnail, destination, date, travelers count
   - "Create New Trip" CTA
   - Saved budgets list

6. AUTH PAGES (/login, /register)
   - Clean minimal card layout
   - Email/password + Google OAuth button
   - Bangla welcome text

COMPONENT LIBRARY:
- PlaceCard: thumbnail, name, category badge, rating, distance, "Add" button
- BudgetRow: label, estimated cost input (editable), per-person cost
- DestinationCard: image, name-bn, name-en, division tag, "Explore" button
- MapMarker: color-coded by category
- ItineraryStop: draggable, day label, place name, time picker, remove button
- DivisionBadge: pill tag for each division with icon

STATE MANAGEMENT:
- budgetState: { travelers, city, tier, costs{}, total }
- tripState: { destination, days[], selectedPlaces[] }
- mapState: { center, zoom, activeFilter, markers[] }
- authState: { user, token, isGuest }

API CONNECTIONS (connect to Antigravity backend):
- Base URL: process.env.REACT_APP_API_URL
- Endpoints: /api/destinations, /api/places, /api/budget/estimate, /api/trips, /api/auth/*
- Use Stitch HTTP service or fetch() with JWT Bearer token in headers

OUTPUT: Generate all pages as React components using Tailwind CSS. Export component structure and routing config.
```

---

## PART 3 — ANTIGRAVITY BACKEND PROMPT

Paste this into **Antigravity** after importing or referencing the Stitch frontend design:

---

```
Build the complete backend API for a Bangladesh travel planner application called "Bangladesh Bhromon". 
This backend must match and serve the Stitch-generated frontend exactly. Follow MVC pattern.

TECH STACK:
- Runtime: Node.js (Express.js) OR Python (FastAPI) — choose based on Antigravity defaults
- Database: MongoDB Atlas
- Auth: JWT (jsonwebtoken) + bcrypt, Google OAuth2
- External APIs: Google Maps Places API, Google Maps Geocoding API
- File storage: for user avatars and exported PDFs (S3-compatible or Antigravity storage)
- Environment: production-ready with .env config

ARCHITECTURE (MVC):

MODELS — MongoDB collections with Mongoose/Motor schemas:

1. UserModel
   Fields: _id, name, email, passwordHash, googleId, savedTrips[], createdAt
   Indexes: email (unique)

2. TripModel
   Fields: _id, userId (ref: User), title, destination (slug), division, travelers (int),
           days[{ dayNumber, date, stops[{ placeId, note, arrivalTime }] }],
           budget{ transport, hotel, food, activities, total }, isPublic, shareToken, createdAt
   Indexes: userId, destination

3. DestinationModel
   Fields: _id, slug (unique), nameEn, nameBn, division, lat, lng,
           budgetTemplates{ budget{}, midrange{}, luxury{} },
           popularSpots[], tips[], coverImageUrl, bestTimeToVisit, description
   Pre-seed with all 64 districts of Bangladesh + popular spots data

4. PlaceCacheModel (cache Google Places API results)
   Fields: _id, googlePlaceId, name, type, lat, lng, address, rating, photoRef,
           destinationSlug, cachedAt (TTL index: 7 days)

CONTROLLERS — one file per domain:

1. AuthController
   POST /api/auth/register → validate email/pass → hash password → create User → return JWT
   POST /api/auth/login → verify credentials → return JWT + user
   POST /api/auth/google → verify Google token → upsert User → return JWT
   GET  /api/auth/me → middleware: verifyJWT → return user profile

2. DestinationController
   GET /api/destinations → return all destinations (slug, nameEn, nameBn, division, coverImageUrl)
   GET /api/destinations/:slug → return full destination document
   GET /api/destinations/:slug/budget → return budgetTemplates for that destination

3. PlacesController (wraps Google Maps Places API)
   GET /api/places?lat=&lng=&type=&radius=
     → Check PlaceCacheModel first
     → If cache miss: call Google Places Nearby Search API
     → type mapping: hotel→lodging, restaurant→restaurant, bus→bus_station|transit_station, bike→bicycle_store
     → Normalize response: { placeId, name, type, lat, lng, address, rating, photoUrl, distance }
     → Cache result in PlaceCacheModel
     → Return normalized array
   GET /api/places/:placeId → Google Place Details (for info panel on map click)

4. BudgetController
   POST /api/budget/estimate
     Body: { destinationSlug, travelers, tier: "budget"|"midrange"|"luxury" }
     → Fetch DestinationModel.budgetTemplates[tier]
     → Multiply per-person costs by travelers count
     → Return: { perPerson{}, total{}, grandTotal }

5. TripController (requires auth)
   GET    /api/trips → return user's trips (userId from JWT)
   POST   /api/trips → create new trip → return created trip
   GET    /api/trips/:id → return trip (verify ownership)
   PUT    /api/trips/:id → update trip (add/remove stops, update budget)
   DELETE /api/trips/:id → soft delete
   POST   /api/trips/:id/share → generate shareToken → return public URL

MIDDLEWARE:
- verifyJWT: extract & verify Bearer token, attach req.user
- rateLimiter: 100 req/min per IP (use express-rate-limit or slowapi)
- requestLogger: log method, path, status, duration
- errorHandler: centralized error responses { success: false, message, code }
- corsConfig: allow Stitch frontend domain + localhost:3000

DATABASE SEEDING:
Seed DestinationModel with all 64 districts including:
- Cox's Bazar: budgetTemplate { budget: {transport:800,hotel:500,food:400}, midrange: {transport:1500,hotel:1500,food:800}, luxury: {transport:3000,hotel:5000,food:2000} }
- Sylhet: budgetTemplate { budget: {transport:600,hotel:400,food:350}, midrange: {transport:1200,hotel:1200,food:700}, luxury: {transport:2500,hotel:4000,food:1800} }
- Bandarban, Sajek, Sundarbans, Kuakata, Srimangal — include lat/lng, tips[], description in both English and Bangla

ENVIRONMENT VARIABLES (.env):
MONGO_URI=
JWT_SECRET=
JWT_EXPIRES_IN=7d
GOOGLE_MAPS_API_KEY=
GOOGLE_CLIENT_ID=
GOOGLE_CLIENT_SECRET=
PORT=8080
CLIENT_URL=https://your-stitch-app-url.com

API RESPONSE FORMAT (must match what Stitch frontend expects):
Success: { "success": true, "data": {...}, "message": "optional" }
Error:   { "success": false, "message": "error description", "code": 400 }
List:    { "success": true, "data": [...], "count": N, "page": 1 }

GENERATE:
1. Full folder structure (MVC layout)
2. All model schemas
3. All controller functions with full logic
4. Route files wiring controllers
5. Middleware files
6. app.js / main entry point
7. seed.js script for destination data
8. README with setup and environment instructions
```

---

## PART 4 — ADDITIONAL IDEAS TO MAKE IT PRODUCTION-LEVEL

### Feature Enhancements
| Idea | Value |
|------|-------|
| **Offline Mode (PWA)** | Budget/itinerary accessible without internet — crucial in hill tracts/coastal areas |
| **BDT Cost Auto-Update** | Admin panel to update city-wise cost estimates periodically |
| **Community Tips** | Users submit travel tips per destination (moderated) |
| **Weather Widget** | Show weather forecast for destination on trip dates (OpenWeatherMap API) |
| **Transportation Routes** | Dhaka → [City] bus/train/launch schedule with fare |
| **Group Trip Splitting** | Divide budget among N travelers, show each person's share |
| **Bangla Voice Search** | Speech-to-text destination search using Web Speech API |
| **SEO-Optimized Pages** | `/destination/coxsbazar`, `/destination/sylhet` as SSG pages for Google indexing |

### Tech Quality Checklist
- [ ] Input sanitization (XSS, injection prevention)
- [ ] API key restrictions on Google Maps (domain + API restriction)
- [ ] MongoDB Atlas backups enabled
- [ ] Sentry/LogRocket for error monitoring
- [ ] Lighthouse score > 85 (performance, SEO, accessibility)
- [ ] Image optimization (WebP, lazy loading for destination photos)
- [ ] CI/CD pipeline (GitHub Actions → Antigravity auto-deploy)

---

## PART 5 — PROJECT FOLDER STRUCTURE

```
bangladesh-bhromon/
├── frontend/                   ← Stitch-generated React app
│   ├── src/
│   │   ├── views/
│   │   │   ├── HomeView.jsx
│   │   │   ├── BudgetView.jsx
│   │   │   ├── TripPlannerView.jsx
│   │   │   ├── DestinationDetailView.jsx
│   │   │   └── DashboardView.jsx
│   │   ├── components/
│   │   │   ├── PlaceCard/
│   │   │   ├── BudgetCalculator/
│   │   │   ├── MapView/
│   │   │   ├── ItineraryBuilder/
│   │   │   └── DestinationCard/
│   │   ├── controllers/        ← frontend controllers (API calls)
│   │   │   ├── budgetController.js
│   │   │   ├── tripController.js
│   │   │   └── authController.js
│   │   ├── models/             ← frontend state models (Zustand/Context)
│   │   └── App.jsx
│   └── package.json
│
└── backend/                    ← Antigravity Node.js/Python app
    ├── models/
    │   ├── User.js
    │   ├── Trip.js
    │   ├── Destination.js
    │   └── PlaceCache.js
    ├── controllers/
    │   ├── authController.js
    │   ├── destinationController.js
    │   ├── placesController.js
    │   ├── budgetController.js
    │   └── tripController.js
    ├── routes/
    │   ├── auth.js
    │   ├── destinations.js
    │   ├── places.js
    │   ├── budget.js
    │   └── trips.js
    ├── middleware/
    │   ├── verifyJWT.js
    │   ├── rateLimiter.js
    │   └── errorHandler.js
    ├── seed/
    │   └── seed.js
    ├── app.js
    └── .env.example
```

---

*Document version 1.0 — Bangladesh Bhromon Travel Planner*  
*Prepared for Waterfall Phase 1: Requirements*
