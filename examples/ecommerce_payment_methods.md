# Example Decomposition: E-Commerce Multi-Provider Payment Checkout

## Input Epic / User Story
> **Original Epic:** "As an e-commerce parcel locker checkout team, we need to implement online digital payment at checkout (Credit/Debit Cards, Apple Pay, Google Pay, and Klarna Pay-Later) so that shoppers can pay seamlessly and have their packages automatically routed to nearby pickup lockers."

---

## 🔍 Backlog Item Analysis
- **Target Persona:** E-Commerce mobile and web shoppers completing checkout.
- **Core Motivation & Outcome:** Frictionless, secure digital payment to guarantee instant parcel locker allocation.
- **Identified Smells / Risk:** 
  - Massive multi-vendor payment scope bundled together.
  - Hidden architectural weight: PCI-DSS compliance, 3D-Secure 2 (SCA) authentication challenges, asynchronous webhook listeners, idempotent transactional order state machines, and refund handling.
  - High risk of a multi-sprint stall if the team tries to launch all payment methods at the same time.
- **Recommended Splitting Strategy:**
  - **Pattern 7 (Major Effort First / The Architectural Spike):** Put 80% of the foundational weight into Item 1 (Core Card gateway via Stripe/Adyen). Once the secure token vault, webhook listener pipeline, and database transaction models are battle-tested in production, adding Apple Pay and Klarna becomes lightweight incremental configuration.

---

## ✂️ Decomposed Stories

### Story 1: Core Credit/Debit Card Gateway & 3D-Secure Compliance (The Heavy Lift)
- **User Story:** As an online shopper, I want to pay for my parcel locker order using my standard Visa or Mastercard with secure 3D-Secure authentication, so that my order is immediately confirmed and reserved at the locker.
- **Pattern Applied:** Pattern 7 (Major Effort First — Foundational Spike)
- **In Scope:** Payment gateway SDK integration (e.g., Stripe Payment Element); PCI-compliant tokenization vault; 3DS2 / SCA challenge modal handling; backend webhook listener for `payment_intent.succeeded` and `payment_intent.payment_failed`; order status state machine updates (`Pending` -> `Paid`).
- **Out of Scope (Deferred):** Apple Pay, Google Pay, Klarna BNPL, saved card profiles for future one-click checkout.
- **Acceptance Criteria:**
  - **Scenario 1:** Successful credit card payment with 3DS2 authentication
    - **Given** I have a cart total of €45.00 ready for checkout
    - **When** I enter valid card details, trigger the 3DS2 bank verification challenge, and authenticate
    - **Then** my payment is authorized, the order status changes to "Paid", and I see the order confirmation with locker pickup details.
  - **Scenario 2:** Card decline handling
    - **Given** a card with insufficient funds or expired validity
    - **When** I click "Pay €45.00"
    - **Then** the payment gateway returns a human-readable decline reason ("Card declined: Insufficient funds"), the order remains in "Pending Payment" state, and I can retry without losing my cart items.
- **Estimated Size:** Medium (M) — ~4 days *(absorbs 80% of the entire epic's engineering lift)*

---

### Story 2: Native Express Wallets (Apple Pay & Google Pay)
- **User Story:** As a mobile shopper, I want to complete checkout with a single tap using Apple Pay or Google Pay, so that I don't have to manually type 16-digit card numbers and billing addresses on my phone.
- **Pattern Applied:** Pattern 7 (Major Effort First — Incremental Provider)
- **In Scope:** Register Apple merchant identifier certificate and domain verification; configure native PaymentSheet express button component; pass authorized wallet token into the existing PaymentIntent API established in Story 1.
- **Out of Scope (Deferred):** Klarna BNPL, alternative regional bank rails.
- **Acceptance Criteria:**
  - **Scenario 1:** 1-Tap Apple Pay checkout on supported iOS device
    - **Given** I am browsing checkout on an iPhone with Apple Pay configured
    - **When** I tap the "Apple Pay" button and authenticate with Face ID
    - **Then** the order is authorized through the existing payment webhook, order status updates to "Paid", and confirmation is displayed in under 3 seconds.
- **Estimated Size:** Small (S) — ~1 day *(leverages established Story 1 webhook & DB architecture)*

---

### Story 3: Regional Alternative Rails (Klarna Buy Now, Pay Later)
- **User Story:** As an e-commerce shopper with a larger basket size, I want to split my purchase into 4 interest-free installments with Klarna, so that I can afford higher-ticket items comfortably.
- **Pattern Applied:** Pattern 7 (Major Effort First — Alternative Regional Rail)
- **In Scope:** Activate Klarna payment method type in the existing Stripe/Adyen session; configure client return URL redirect handler; map asynchronous settlement status webhooks (`payment_intent.requires_action`, `charge.pending`).
- **Out of Scope (Deferred):** Additional local payment rails (iDEAL, MobilePay, Sofort).
- **Acceptance Criteria:**
  - **Scenario 1:** Klarna redirect authorization
    - **Given** an order total exceeding the minimum Klarna threshold (€35.00)
    - **When** I select Klarna and click "Continue to Klarna"
    - **Then** I am redirected to Klarna's secure portal to choose payment schedule, and upon approval, redirected back to the order success screen.
- **Estimated Size:** Small (S) — ~1–2 days *(isolated rollout with zero regression risk to credit card or Apple Pay rails)*

---

## 🚀 Recommended Delivery Order
1. **Sprint 1 (Foundational Milestone):** Story 1 (Core Card Gateway) -> Unblocks 90%+ of paying shoppers and absorbs all security, PCI compliance, and transactional database plumbing.
2. **Sprint 2 (Frictionless Mobile UX):** Story 2 (Apple Pay / Google Pay) -> Ships in 1–2 days with massive conversion upside.
3. **Sprint 3 (Basket Expansion):** Story 3 (Klarna BNPL) -> Captures higher-value orders without risking baseline revenue rails.
