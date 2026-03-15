# ⚓ FuelEU WtW Compliance Pool Manager

> A professional, browser-based compliance management tool for the **FuelEU Maritime Regulation (EU) 2023/1805** — covering Well-to-Wake (WtW) greenhouse gas intensity pooling, ship-level compliance balance calculation, and multi-party pool administration.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Regulatory Background — EU 2023/1805](#regulatory-background--eu-20231805)
- [What Is a Compliance Pool?](#what-is-a-compliance-pool)
- [Features](#features)
- [Getting Started](#getting-started)
- [How to Use](#how-to-use)
- [Fuel Library & WtW Values](#fuel-library--wtw-values)
- [Calculations Explained](#calculations-explained)
- [Data & Privacy](#data--privacy)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [Disclaimer](#disclaimer)

---

## Overview

The **FuelEU WtW Compliance Pool Manager** is a single-file HTML application designed for shipping companies, compliance managers, and maritime verifiers to:

- Calculate vessel-level **GHG Intensity (GHGIE_actual)** using Well-to-Wake emission factors
- Determine each vessel's **Compliance Balance (CB)** in tCO₂e
- Manage **internal fleet pooling** (surplus vessels offsetting deficit vessels within the same company)
- Administer **cross-company pooling** (Pool Owner distributing surplus to customer fleets)
- Track **verifier-indicated CB** vs **calculated CB** per vessel
- Estimate penalty exposure for non-compliant vessels
- Export results to PDF, CSV, and JSON

Built to align with the requirements of **Regulation (EU) 2023/1805 of the European Parliament and of the Council** on the use of renewable and low-carbon fuels in maritime transport.

---

## Regulatory Background — EU 2023/1805

**Regulation (EU) 2023/1805**, also known as **FuelEU Maritime**, entered into force on 13 October 2023 and applies from **1 January 2025**.

### Key Objectives

The regulation aims to:
- Increase the use of renewable and low-carbon fuels in the maritime sector
- Reduce the GHG intensity of energy used on board ships by a stepwise trajectory
- Support the EU's target of climate neutrality by 2050

### Scope

The regulation applies to ships of **5,000 GT or above** calling at EU ports, covering:
- **Voyages between EU ports** (100% scope)
- **Voyages between an EU port and a non-EU port** (50% scope)
- **Energy used at berth in EU ports** (100% scope)

### GHG Intensity Reduction Targets

The regulation sets mandatory reduction targets against a **2020 baseline of 91.16 gCO₂e/MJ**:

| Period      | Reduction | GHG Target (gCO₂e/MJ) |
|-------------|-----------|------------------------|
| 2025–2029   | −2%       | 89.34                  |
| 2030–2034   | −6%       | 85.69                  |
| 2035–2039   | −14.5%    | 77.94                  |
| 2040–2044   | −31%      | 62.90                  |
| 2045–2049   | −62%      | 34.64                  |
| 2050+       | −80%      | 18.23                  |

### Well-to-Wake (WtW) Methodology

The regulation requires that **GHG emissions are accounted on a Well-to-Wake basis**, meaning the full lifecycle of the fuel is considered — from production and processing, through transportation, to combustion on board the vessel.

The WtW GHG Intensity is calculated as:

```
GHGIE_actual = f_wind × ( Σ(E_i × WtW_i) / Σ(E_i × RWD_i) )
```

Where:
- `E_i` = Energy content of fuel i [MJ]
- `WtW_i` = Well-to-Wake emission factor of fuel i [gCO₂e/MJ]
- `RWD_i` = Reward factor for renewable fuels (2.0 for RFNBOs, 1.0 for others)
- `f_wind` = Wind propulsion reward factor (≤ 1.0)

### Compliance Balance

Each ship receives a **Compliance Balance (CB)** in tCO₂e:

```
CB = (GHGIE_target − GHGIE_actual) × Σ(E) / 1,000,000
```

- **CB > 0** → Surplus (ship outperforms the target)
- **CB < 0** → Deficit (ship falls short of the target)

### Penalties

Non-compliant ships face a penalty calculated as:

```
Penalty = (|CB in gCO₂e|) / (GHGIE_actual × 41,000) × €2,400
```

Penalties are paid to the administering Member State and ring-fenced for maritime decarbonisation projects.

### Pooling Mechanism (Article 23)

Companies may form **compliance pools** to collectively meet GHG intensity targets. A pool must:
- Have a designated **Pool Organiser** (Pool Owner)
- Be registered with the administering authority **before 1 February** of the reporting year
- Cover a minimum of two ships
- Transfer compliance surplus from outperforming ships to underperforming ships within the pool

> This tool automates the entire pooling calculation process.

---

## What Is a Compliance Pool?

A compliance pool allows shipping companies to **net out surpluses and deficits** across a fleet or across multiple companies.

```
Pool Owner (Surplus Fleet)
        │
        │  transfers surplus CB [tCO₂e]
        ▼
┌───────────────────────────────┐
│        COMPLIANCE POOL        │
│   Global Transfer Mechanism   │
└───────────────┬───────────────┘
                │
        ┌───────┴────────┐
        ▼                ▼
  Customer A          Customer B
  (Deficit)           (Deficit)
```

**Types of pooling in this tool:**

| Level | Description |
|---|---|
| **Internal (fleet-level)** | Surplus vessels within one company's fleet offset that company's deficit vessels |
| **Global (cross-company)** | Pool Owner's remaining surplus is distributed to customer companies with deficits |

**Allocation modes available:**
- **Proportional** — surplus distributed proportionally to each customer's deficit size
- **Sequential** — largest deficits are filled first (priority-based)

---

## Features

### 📊 Overview Tab
- Real-time pool KPIs: Owner Surplus, Customer Deficits, Transferable amount, Remaining balance
- Interactive participant table with click-to-expand fleet view
- Configurable CB source: Indicated CB (verifier-reported) or Calculated CB
- Global transfer cap setting

### 🏢 Pool Owner Tab
- Owner profile with logo, documents, and fleet management
- Internal fleet pooling with mode and cap controls
- Per-vessel: IMO number, flag state, verifier, ship class, energy, GHGIE, CB

### 👥 Customers Tab
- Multi-customer management (unlimited customers)
- Per-customer fleet with full vessel data entry
- Real-time compliance summary with green/red status indicator
- Customer-level document attachments

### 🧮 Ship Calc Tab
- Full WtW calculation from fuel consumption data
- Supports mixed fuel types in a single voyage calculation
- Configurable f_wind factor
- Penalty estimate output
- One-click result transfer to Owner or Customer fleet

### ⛽ Consumption Tab
- Multi-fuel row entry with unit auto-conversion (t → MJ, kg → MJ, kWh → MJ)
- Scope factor for partial EU voyage coverage
- Real-time energy and emissions totals

### 📚 Fuel Library Tab
- Pre-loaded with FuelEU Annex II default values (MGO, VLSFO, LNG, HVO, RFNBO, OPS)
- Fully editable LCV, WtW, and RWD values
- Custom fuel types supported (biofuels, blends, e-fuels)

### 💾 Data Management
- **Auto-save** via browser localStorage
- **JSON export/import** for full state backup and sharing
- **Excel/CSV fleet import** using template
- **PDF export** (current tab or full report)
- **CSV export** of consumption data

---

## Getting Started

### Option 1 — Open Directly (No Setup)

1. Download `index.html`
2. Double-click to open in any modern browser (Chrome, Edge, Firefox, Safari)
3. The app runs entirely in your browser — no server, no installation

> ⚠️ Excel `.xlsx` import requires a local server (see Option 2). CSV import always works.

### Option 2 — Local Server (Full Features)

```bash
# Navigate to the folder containing index.html
cd path/to/folder

# Start Python server
python3 -m http.server 8080

# Open in browser
# http://localhost:8080/index.html
```

### Option 3 — GitHub Pages (Live Hosted)

This repository is configured for GitHub Pages. Access the live app at:

```
https://YOUR_USERNAME.github.io/fueleu-pool/
```

See [Deployment](#deployment) for setup instructions.

---

## How to Use

### Step 1 — Configure the Pool
1. Open the **Overview** tab
2. Select the **CB source** (Indicated CB recommended for verified data)
3. Choose the **global allocation mode** (Proportional or Sequential)
4. Optionally set a **global transfer cap**

### Step 2 — Set Up the Pool Owner
1. Go to the **Pool Owner** tab
2. Enter the owner name and upload a logo (optional)
3. Add vessels manually (`+ Add vessel`) or import from Excel/CSV
4. For each vessel, enter: IMO, flag, verifier, ship class, energy [MJ], GHGIE_actual, and CB values

### Step 3 — Add Customers
1. Go to the **Customers** tab
2. Click **Create / Select** and enter a customer name
3. Add vessels to the customer fleet
4. Repeat for additional customers

### Step 4 — Calculate from Consumption (Optional)
1. Go to the **Consumption** tab and add fuel rows
2. Enter fuel amounts for the reporting period
3. Switch to the **Ship Calc** tab — GHGIE_actual and CB are calculated automatically
4. Click **Add result** to push the vessel into the Owner or Customer fleet

### Step 5 — Review Pool Results
1. Return to the **Overview** tab
2. Review KPIs: surplus, deficit, transferable, and remaining balance
3. Click any participant row to expand and view vessel-level detail
4. Double-click a row to jump directly to that participant's fleet

### Step 6 — Export
- **Export PDF** — current tab or full report (via browser print dialog → Save as PDF)
- **Export JSON** — full configuration backup
- **Export CSV** — consumption data for the active vessel

---

## Fuel Library & WtW Values

Default values are based on **FuelEU Maritime Annex II** and **EU Delegated Regulation** on WtW emission factors:

| Fuel | LCV [MJ/kg] | WtW [gCO₂e/MJ] | RWD |
|---|---|---|---|
| MGO/MDO (fossil) | 42.7 | 90.63 | 1.0 |
| VLSFO/HFO (fossil) | 40.5 | 91.60 | 1.0 |
| LNG (fossil) | 49.3 | *enter value* | 1.0 |
| Bio-LNG (biomethane) | 49.9 | *certified PoS/PoC* | 1.0 |
| Biofuel / HVO / FAME | 42.0 | *certified PoS/PoC* | 1.0 |
| RFNBO (e-fuel) | 50.0 | *certified PoS/PoC* | **2.0** |
| Electricity (OPS) | — | *grid-dependent* | 1.0 |

> For **biofuels**, **Bio-LNG**, and **RFNBOs**: always enter the **certified WtW value** from the Proof of Sustainability (PoS) or Proof of Compliance (PoC) certificate.

---

## Calculations Explained

### GHG Intensity (GHGIE_actual)

```
GHGIE_actual = f_wind × ( Σ(E_i × WtW_i) / Σ(E_i × RWD_i) )
```

### Compliance Balance (CB)

```
CB [tCO₂e] = (GHGIE_target − GHGIE_actual) × ΣE / 1,000,000
```

### Penalty Estimate

```
Penalty [€] = ( |CB_g| / (GHGIE_actual × 41,000) ) × 2,400
```

Where `CB_g = CB × 1,000,000` (converted back to gCO₂e).

> ⚠️ The penalty estimate is indicative only. Official penalties are determined by the administering authority.

### Internal Pooling

```
Transferable = min(Σ surplus vessels, Σ |deficit vessels|, cap)
```

Proportional mode:
```
Transfer to vessel_i = Transferable × (|CB_i| / Σ|deficits|)
```

### Global Pooling

```
Global Transferable = min(Owner surplus, Σ customer deficits, global cap)
```

---

## Data & Privacy

- **All data is stored locally** in your browser's `localStorage`
- **No data is transmitted** to any server — the app works entirely offline after first load
- The only external requests are:
  - Google Fonts (typography)
  - SheetJS CDN (Excel import only, loaded once)
- Use **Export JSON** regularly to back up your data
- Use **Embed Base64** mode to persist logos and documents across browser sessions

---

## Deployment

### GitHub Pages

1. Fork or clone this repository
2. Rename the HTML file to `index.html`
3. Push to the `main` branch
4. Go to **Settings → Pages → Source → main / (root)**
5. Your app will be live at `https://YOUR_USERNAME.github.io/REPO_NAME/`

### Local Network Sharing

```bash
# Find your local IP address
ipconfig  # Windows
ifconfig  # Mac/Linux

# Start server accessible on your network
python3 -m http.server 8080 --bind 0.0.0.0

# Others on same network can access via:
# http://YOUR_IP:8080/index.html
```

---

## Troubleshooting

| Issue | Solution |
|---|---|
| Excel import not working | Use Python local server or VS Code Live Server; or use CSV import instead |
| Data not saving between sessions | Ensure localStorage is not blocked (private/incognito mode disables it) |
| Calculations show `–` | Check that WtW values are filled in the Fuel Library for all fuel types used |
| PDF export cuts off columns | Use A3 landscape orientation in the print dialog |
| Indicated CB not used in pooling | Set CB Source dropdown to "Indicated CB (if present) else Calculated CB" |
| Customer shows wrong compliance | Verify the CB source selection and check that vessel CB values are entered |

---

## Disclaimer

> This tool is provided for **informational and planning purposes only**. Compliance determinations under Regulation (EU) 2023/1805 must be made by an **accredited verifier** in accordance with the Implementing Regulations issued by the European Commission. The penalty estimates are approximate and do not constitute legal or financial advice. Always refer to the official regulatory text and consult your verifier for binding compliance assessments.

---

*README version 2026 · FuelEU WtW Compliance Pool Manager · Normec Group*
