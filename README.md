# AI Defense - GCP Connector Setup Prototype

Interactive prototype for the simplified Google Cloud Platform connector onboarding flow in Cisco AI Defense.

## What This Is

A clickable HTML prototype showing a **2-step** GCP connector setup that achieves parity with the existing Azure connector flow. The original GCP design had 5 steps; this version consolidates them down to Configure and Deploy.

## How to View

Open `index.html` in any browser. No build tools or dependencies required.

## Interactive Elements

### Step 1: Configure
- **Connector scope**: Click Organization / Folder / Project cards (the ID field changes dynamically)
- **Authentication method**: Switch between Workload Identity Federation and Service Account Key (fields and Step 2 content adapt)
- **Advanced options**: Click to expand region selector and exclusion fields
- **Region picker**: Toggle between "All Vertex AI regions" and "Specific regions only" to see the region grid

### Step 2: Deploy
- **Background polling**: After clicking Next, a live timer and pending checks appear. AI Defense polls for the deployment automatically
- **Simulate deployment**: Click the demo button to watch checks pass one by one with staggered animation
- **Complete button**: Disabled until all checks pass, then enables automatically
- **SA Key path**: Switch auth method in Step 1 to see the file upload variant

## Design Decisions

| Decision | Rationale |
|----------|-----------|
| **2 steps (not 5)** | Matches Azure connector parity |
| **Regions default to "All"** | GCP Vertex AI APIs are regional (unlike Azure ARM which is global), but scanning all 10 regions costs ~$0.01 and takes ~30s. Hidden under Advanced for compliance use cases |
| **WIF + SA Key auth** | Mirrors Azure's choice of Federated Identity / Certificate / Client Secret. GCP has no certificate equivalent, so 2 options instead of 3 |
| **Background polling** | Replaces the paste-back pattern (Azure requires pasting Managed Identity Client ID). GCP service account email is deterministic from the deployment project ID, so no manual input is needed |
| **No inventory summary** | Azure doesn't show post-setup inventory. Kept at parity; can be added to both connectors later |

## Azure vs GCP Connector Comparison

| Element | Azure | GCP (Old) | GCP (This Prototype) |
|---------|-------|-----------|----------------------|
| Steps | 2 | 5 | 2 |
| Fields in Step 1 | 6 | 9 (across 2 screens) | 6 (Advanced collapsed) |
| Region picker | N/A (global API) | Required field | Advanced option (hidden) |
| Auth methods | 3 | 1 (WIF only) | 2 (WIF + SA Key) |
| Post-script action | Paste Client ID | None (auto-verify) | None (background polling) |
| Verification | Implicit | Separate step | Inline with polling |

## File Structure

```
index.html    # Self-contained prototype (HTML + CSS + JS, no dependencies)
README.md     # This file
```

## Context

- **Product**: Cisco AI Defense
- **Charter**: Asset Governance (Integrations)
- **Design system**: Cisco Magnetic (light theme)
- **Status**: Prototype for design review
