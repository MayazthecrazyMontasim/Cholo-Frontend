# Antigravity Backend Prompt: Cholo Travel & Budget Planner

Build a robust REST API backend for "Cholo", a Bangladesh-focused travel trip planner and budget calculator.

## core Technologies
- Language: Node.js (Express) or Python (FastAPI)
- Database: PostgreSQL or MongoDB (for storing trip plans and user data)
- Integrations: Google Places API, WhatsApp Business API (for sharing)

## Data Models

### 1. Destination
- id: UUID
- name: String (e.g., "Cox's Bazar")
- division: String (e.g., "Chittagong")
- tagline: String
- emoji: String
- coordinates: { lat: Float, lng: Float }
- description: Text

### 2. Place/Service
- id: UUID
- destinationId: UUID
- type: Enum (hotel, bike_rental, bus_counter, restaurant)
- name: String
- detailText: String
- address: String
- price: Decimal (in ৳)
- badge: Enum (Popular, Budget, Luxury, Eco)

### 3. Trip Plan
- id: UUID
- userId: UUID (optional for guest)
- numPeople: Integer
- numDays: Integer
- tripType: Enum (budget, mid, luxury)
- startCity: String
- destinationId: UUID
- selectedServiceIds: Array[UUID]
- totalBudget: Decimal
- createdAt: DateTime

### 4. Expense (for Bill Splitter)
- id: UUID
- tripId: UUID
- paidBy: String (User Name)
- amount: Decimal
- category: Enum (accommodation, food, transport, activities, misc)
- note: String

## Required API Endpoints

### Destinations
- `GET /destinations`: Returns all 64 districts with division info.
- `GET /destinations/:id`: Returns specific district details.

### Places & Services
- `GET /destinations/:id/places?type={type}`: Returns filtered services (hotels, rentals, etc.) for a specific destination.

### Budgeting Engine
- `POST /budget/estimate`: 
  - Input: `{ numPeople, numDays, tripType, destinationId, selectedServiceIds }`
  - Logic: Calculate estimated costs based on historical data and selected services. 
  - Output: `{ totalCost, perPerson, breakdown: { accommodation, food, transport, activities, misc }, smartTip: "String" }`

### Trip Management
- `POST /trips`: Saves a final itinerary and returns a `tripId` and shareable link.
- `GET /trips/:id`: Retrieves saved trip details.

### Bill Splitter Logic
- `POST /trips/:id/expenses`: Add a new expense.
- `GET /trips/:id/settlements`: 
  - Logic: Calculate (Total Spent / NumPeople) and determine who owes whom.
  - Output: List of settlement actions (e.g., "Sarah owes Alex ৳8,333").

## Travis AI Logic (Simulation)
- `POST /chat/travis`:
  - Input: `{ message, context: { currentStep, tripData } }`
  - Logic: Return context-aware travel advice or formatted budget text.

## Constraints
- All currency values must be handled as Decimals to ensure precision.
- Implement basic error handling for 404 (Not Found) and 400 (Bad Request).
- Ensure the API is CORS-enabled for our mobile-first web frontend.