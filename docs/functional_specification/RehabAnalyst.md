# Master Prompt: Functional Specification for "RehabAnalyst AI" Application

## **Your Role**

You are a Senior Product Manager and Lead Systems Architect. Your task is to create a detailed functional specification and UI/UX wireframe description for a new web application called **RehabAnalyst AI**. This document will serve as the blueprint for the development team.

The primary goal of this application is to provide real estate investors and renovators with a seamless, two-step tool:
1.  **Analyze & Budget:** Generate an expert-level, "as-is" rehab cost estimate based on property photos and an address.
2.  **Visualize & Upgrade:** Offer interactive, AI-generated design mockups for key rooms at different finish levels, with dynamically updated budgets.

The most critical requirement is **cost consistency**. The initial repair budget must serve as the logical baseline for all subsequent upgrade costs.

---

## **Application Blueprint: RehabAnalyst AI**

### **1. Core User Flow & UI Logic**

The user experience is divided into two distinct, sequential stages.

#### **Stage 1: Property Analysis (Input & Baseline Report)**

1.  **Landing Page:** Clean interface with a single call-to-action: "Analyze a Property."
2.  **Input Form:**
    *   Field 1: Property Address (with Google Maps API autocomplete).
    *   Field 2: Photo Uploader (supports drag-and-drop of multiple interior and exterior images).
    *   A prominent "Analyze Now" button. This button is disabled until both an address is entered and at least one photo is uploaded.
3.  **Processing State:** After the user clicks "Analyze Now," the UI displays a loading animation with messages indicating progress (e.g., "Analyzing property data...", "Inspecting exterior photos...", "Evaluating kitchen condition...", "Calculating local labor costs...").
4.  **Report Generation (The Backend Engine):**
    *   Upon submission, the backend will execute a process **identical to the 'Master Prompt: Real Estate Rehab Cost Estimation Report'** (the detailed report generator we previously designed).
    *   It will perform the necessary web searches for local market costs and property data (year built, sq ft) to inform its estimates.
    *   The output is a structured JSON object containing the full baseline rehab report, broken down by area (Exterior, Interior, Kitchen, etc.), with descriptions, suggested repairs, difficulty ratings, and baseline cost estimates.

#### **Stage 2: Interactive Report & Design Mockups**

1.  **Report Display:** The baseline report is presented not as a wall of text, but as a clean, interactive dashboard. Each major area (e.g., "Kitchen," "Bathroom 1," "Exterior") is displayed as a separate card or expandable section.
2.  **Interactive Cards:**
    *   Each card (e.g., the "Kitchen" card) shows the key findings from the report:
        *   **Image:** A thumbnail of the original, damaged kitchen.
        *   **Condition:** "A complete teardown. Dated, worn cabinets..."
        *   **Baseline Scope:** "Full Gut, New Cabinets (Builder-Grade), New Laminate Countertops..."
        *   **Baseline Cost:** `$12,000 – $18,000`
        *   **Difficulty:** `4/5`
3.  **Unlocking the Design Module:** At the bottom of each *interior* room card (Kitchen, Bathrooms, Living Area), there is a call-to-action: **"Visualize Upgrades."**
4.  **The Design & Budgeting Module (Modal or New View):**
    *   When a user clicks "Visualize Upgrades" for the Kitchen, a new interface appears.
    *   It displays the original photo on one side. On the other side are three buttons:
        *   **Tier 1: Basic (Investor Grade)**
        *   **Tier 2: Mid-Range (Market Standard)**
        *   **Tier 3: Luxury (High-End)**
    *   When the user clicks a tier:
        *   An AI image generator creates a photorealistic mockup of the renovated kitchen in that style.
        *   Below the mockup, an **adjusted cost** is displayed.

### **2. Feature Specification: Costing & Mockup Logic (The Core Innovation)**

This ensures cost consistency.

*   **Baseline Cost:** The cost generated in Stage 1 represents a functional, "rent-ready" or "flipper-basic" repair. This is the **source of truth**.
    *   *Example (Kitchen Baseline):* `$15,000` (midpoint) for builder-grade RTA cabinets, laminate counters, basic stainless appliance package, and new LVP flooring.

*   **Tiered Upgrade Costs (The Modifiers):** The costs for the design tiers are calculated as **deltas from the baseline**, not as entirely new estimates.
    *   **Tier 1: Basic (Investor Grade):** This cost is often the *same as the baseline*. The mockup simply visualizes the baseline scope.
        *   *Cost Display:* "$15,000 (Baseline Cost)"
    *   **Tier 2: Mid-Range (Market Standard):** The AI calculates the cost difference for specific material upgrades.
        *   *Logic:* Baseline ($15k) + (Cost of Quartz Counters - Cost of Laminate) + (Cost of Tiled Backsplash - Cost of Paint) + (Cost of Upgraded Lighting).
        *   *Cost Display:* "$21,500"
    *   **Tier 3: Luxury (High-End):** Calculates the cost for premium upgrades.
        *   *Logic:* Baseline ($15k) + (Cost of Semi-Custom Cabinets - Cost of RTA) + (Cost of High-End Quartz/Marble - Cost of Laminate) + (Cost of Premium Appliances - Cost of Basic Suite).
        *   *Cost Display:* "$32,000"

### **3. Feature Specification: Dynamic Budget Summary**

*   A persistent "Total Project Budget" summary is always visible on the screen.
*   Initially, it displays the total baseline rehab cost from the Stage 1 report.
*   As the user selects different finish tiers for various rooms (e.g., they choose a "Mid-Range" kitchen but leave the bathrooms at "Basic"), this total budget **updates in real-time**.
*   This allows the user to mix and match finishes to meet a specific target renovation budget.

### **4. Final Output**

The user can export a final "Project Summary" PDF which includes:
1.  The full baseline rehab report.
2.  Their chosen design mockups for each room.
3.  A final, itemized budget reflecting their tier selections.
4.  The overall project difficulty rating and final recommendations.
