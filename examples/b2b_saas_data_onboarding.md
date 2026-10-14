# Example Decomposition: B2B SaaS Vendor Onboarding

## Input Epic / User Story
> **Original Epic:** "As an account administrator in an enterprise organization, I want to onboard our existing vendor catalog and invoice data into the platform so that our procurement department can start processing purchase orders without downtime."

---

## 🔍 Backlog Item Analysis
- **Target Persona:** Enterprise Account Administrator / Procurement Operations Specialist
- **Core Motivation & Outcome:** Transitioning from legacy spreadsheets or ERP into the new SaaS platform without disruption.
- **Identified Smells / Risk:** 
  - Massive scope covering UI, file parsers, schema validation, and potentially background batch workers.
  - High risk of stalling the sprint on edge cases in vendor address formatting, tax IDs, or corrupted CSV rows.
- **Recommended Splitting Strategy:**
  - **Pattern 4 (Data Variations):** Split by ingestion complexity (Manual Web Form -> Bulk CSV File -> REST API).
  - **Pattern 8 (Deferring Performance):** Defer asynchronous background queues for small batch sizes first.

---

## ✂️ Decomposed Stories

### Story 1: Manual Vendor Creation via Interactive Form
- **User Story:** As an enterprise admin, I want to manually create and save a new vendor profile with essential billing information, so that I can immediately start processing orders for our primary supplier.
- **Pattern Applied:** Pattern 4 (Data Variations — Baseline Input)
- **In Scope:** Single vendor creation modal/page; validation for Vendor Name, Tax ID / VAT number, and Country; saves to active vendor database.
- **Out of Scope (Deferred):** CSV upload, batch operations, ERP integration.
- **Acceptance Criteria:**
  - **Scenario 1:** Successful vendor creation with valid EU VAT
    - **Given** I am logged in as an Admin on the Vendors page
    - **When** I click "Add Vendor", enter "Nordic Logistics Oy", a valid VAT "FI12345678", and click Save
    - **Then** the vendor appears in my active list with status "Ready".
  - **Scenario 2:** Duplicate Tax ID prevention
    - **Given** a vendor with Tax ID "FI12345678" already exists
    - **When** I attempt to save another vendor with the same Tax ID
    - **Then** the system shows an inline error: "A vendor with this Tax ID is already registered."
- **Estimated Size:** Small (S) — ~2 days

---

### Story 2: Bulk Vendor Import via CSV with Error Reporting
- **User Story:** As an enterprise admin, I want to upload a CSV file containing our vendor list with downloadable error feedback, so that I can import dozens of vendors at once without manual typing.
- **Pattern Applied:** Pattern 4 (Data Variations — Bulk File) + Pattern 8 (Deferring Performance)
- **In Scope:** File dropzone supporting `.csv` up to 500 rows; client-side/synchronous server validation; downloadable CSV containing invalid rows with error reasons.
- **Out of Scope (Deferred):** Background worker queues for 100k+ rows, automated field auto-mapping via AI.
- **Acceptance Criteria:**
  - **Scenario 1:** Valid CSV upload
    - **Given** I have a formatted CSV file with 50 valid vendor rows
    - **When** I upload the file and confirm import
    - **Then** all 50 vendors are created and a success toast shows "50 vendors imported successfully".
  - **Scenario 2:** Partial validation failure
    - **Given** a CSV with 45 valid rows and 5 rows missing mandatory Tax IDs
    - **When** I upload the file
    - **Then** the 45 valid vendors are imported, and an error banner appears with a button: "Download 5 failed rows (.csv)".
- **Estimated Size:** Medium (M) — ~3-4 days

---

### Story 3 (Deferred): Automated ERP Sync via REST API & Webhooks
- **Job Story:** When our ERP system updates a vendor record, we want the changes to synchronize automatically via API, so our procurement team never works with outdated banking details.
- **Pattern Applied:** Pattern 4 (Data Variations — Automated Pipeline)
- **In Scope:** Inbound REST endpoint `/api/v1/vendors` with OAuth2 Bearer token authentication and webhook event emitting.
- **Estimated Size:** Large (L) — Reserved for Phase 2.
