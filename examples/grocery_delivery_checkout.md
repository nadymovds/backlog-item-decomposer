# Example Decomposition: On-Demand Grocery Delivery Checkout

## Input User Story
> **Original Story:** "As an on-demand grocery customer, I want to order my cart in the app with customized delivery slots, courier tipping, and out-of-stock item replacements so that my weekly shopping arrives conveniently without stress."

---

## 🔍 Backlog Item Analysis
- **Target Persona:** Busy Consumer / Grocery Shopper (e.g., Wolt Market, Oda, Gorillas user).
- **Core Motivation & Outcome:** Getting groceries delivered to home without manual errand running.
- **Identified Smells / Risk:** 
  - Overloaded compound story combining checkout payment, delivery logistics scheduling, tipping incentives, and complex inventory substitution workflows.
  - Would easily take 3 sprints if built as a single monolithic block.
- **Recommended Splitting Strategy:**
  - **Pattern 6 (Workflow Steps):** Extract the core happy path (instant checkout with default address/card) first.
  - **Pattern 2 (Business Rule Variations):** Move substitution preferences and tip calculations to subsequent increments.

---

## ✂️ Decomposed Stories

### Story 1: Core 1-Click Instant Delivery Checkout (Happy Path)
- **User Story:** As a grocery shopper, I want to confirm my order using my saved default address and default payment card for ASAP delivery, so that I can place my order in under 10 seconds.
- **Pattern Applied:** Pattern 6 (Workflow Steps — Core Path)
- **In Scope:** Checkout review screen with saved address, saved card (Apple Pay / Stripe), order summary, and "Place Order (ASAP Delivery)" button.
- **Out of Scope (Deferred):** Delivery time window scheduling, courier tipping, item replacement settings.
- **Acceptance Criteria:**
  - **Scenario 1:** Successful instant order placement
    - **Given** I have €35 worth of groceries in my cart and a saved address in Helsinki
    - **When** I click "Place Order" with Apple Pay
    - **Then** my payment is authorized, order status moves to "Received by Store", and estimated arrival shows "30–45 mins".
- **Estimated Size:** Small (S) — ~2 days

---

### Story 2: Scheduled Delivery Time Slots & Delivery Instructions
- **User Story:** As a grocery shopper, I want to choose a future delivery window and provide courier access notes, so that my delivery arrives when I am home from work.
- **Pattern Applied:** Pattern 6 (Workflow Steps — Step Flexibility)
- **In Scope:** Date & 1-hour time window picker; free-text field for building door code and delivery instructions.
- **Out of Scope (Deferred):** Live driver tracking, item substitution logic.
- **Acceptance Criteria:**
  - **Scenario 1:** Selecting a next-day evening slot
    - **Given** I am on the checkout screen
    - **When** I toggle from "ASAP" to "Schedule for later" and select "Tomorrow, 18:00 - 19:00"
    - **Then** the selected delivery slot is reserved for my cart and reflected in the order confirmation.
- **Estimated Size:** Small (S) — ~2 days

---

### Story 3: Out-of-Stock Substitution Preferences & Courier Tip
- **Job Story:** When an item in my cart is unavailable at the store, I want to set my preferred substitution rule (replace with similar brand vs. refund), so that my dinner recipe isn't ruined.
- **Pattern Applied:** Pattern 2 (Business Rule Variations)
- **In Scope:** Per-item or order-level substitution toggle ("Refund item" vs. "Allow store choice"); optional pre-set tip buttons (€1, €2, €5).
- **Estimated Size:** Medium (M) — ~3 days
