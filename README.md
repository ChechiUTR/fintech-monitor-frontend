# Advanced Financial Market & Crypto Asset Monitor — Frontend

> User-facing portal and live financial dashboards for the Advanced Financial Market & Crypto Asset Monitor.

This is the **frontend / client-side interface** repository. It is one half of a two-repository project. The data ingestion and processing engine lives in the companion repository, [`fintech-monitor-backend`](#related-repositories).

---

## Project Description

The Advanced Financial Market & Crypto Asset Monitor tracks **50 stocks and crypto assets in near real time** and presents their volatility and risk indicators through dynamic, interactive dashboards.

**This repository contains the frontend**, which is responsible for:

- Presenting a user-facing portal that surfaces the project's live dashboards.
- Embedding **Looker Studio** dashboards with dynamic time-series line charts and financial-risk indicators.
- Providing client-side controls (asset selection, time-range filters, refresh).
- Being hosted as a static site on **AWS**.

The frontend does **not** query market APIs directly. It reads from the Looker Studio dashboards, which in turn are connected to a Google Sheet kept up to date by the backend. This keeps the interface fast, simple, and decoupled from data collection.

---

## Related Repositories

This project is split into two coordinated repositories. They are designed to work together and share the Google Sheet as their integration contract.

| Repository | Responsibility | Link |
|---|---|---|
| **`fintech-monitor-frontend`** _(this repo)_ | User-facing portal, Looker Studio dashboards, client-side controls, static hosting on AWS | — |
| **`fintech-monitor-backend`** | Data ingestion, concurrent processing, risk calculation, Google Sheet updates, AWS deployment | `https://github.com/<org>/fintech-monitor-backend` |

**Integration boundary:** the backend is the sole writer of the Google Sheet; this frontend (via Looker Studio) is a read-only consumer of it. Neither repo calls the other directly.

---

## Team Members

The same three-person team owns both repositories. Primary focus areas are noted to clarify ownership, but the team collaborates across the full stack.

| Name | GitHub | Primary Focus |
|---|---|---|
| _[Member 1 — Full Name]_ | `@handle` | Backend & Data Engineering Lead |
| _[Member 2 — Full Name]_ | `@handle` | Cloud Infrastructure & DevOps (AWS) |
| _[Member 3 — Full Name]_ | `@handle` | Frontend & Visualization Engineer |

> Replace the placeholders above with real names and GitHub handles before the first sprint.

---

## Defined Objectives

The frontend is responsible for the following deliverables, centered on the project's **Data Visualization (VDB)** pillar and its **Cloud Services (SEN)** hosting requirement.

### Data Visualization (VDB)
- Build the **Looker Studio** dashboards connected to the backend's Google Sheet.
- Render **dynamic time-series line charts** for the 50 tracked assets.
- Display clear **financial-risk and volatility indicators** that update as the underlying sheet refreshes.
- Provide intuitive client-side controls: asset selection, time-range filtering, and refresh.

### Cloud Services (SEN)
- Host the portal as a static site on **AWS** (e.g., S3 + CloudFront or AWS Amplify).
- Ensure the deployed site loads the latest published dashboards reliably.

### User Experience
- Present a clean, responsive interface that works on desktop and mobile.
- Make the dashboards understandable to a non-technical viewer within seconds.
- Keep the asset list and labels consistent with what the backend publishes.

### Definition of Done
- Looker Studio dashboards are live and bound to the backend's Google Sheet.
- Charts and risk indicators update automatically as the sheet refreshes.
- The portal is deployed and publicly reachable on AWS.
- Client-side filters work and the interface is responsive.

---

## Technology Stack

**Visualization & Data Bridge (the core of this repo)**
- **Looker Studio** — the dashboards themselves, built in Google's **no-code** interface. Dynamic time-series line charts, risk indicators, filters, and time-range controls are all configured here — no application code required.
- **Google Sheets** — the read-only data source, populated by the backend.

**Presentation / Hosting wrapper (optional, AWS requirement only)**
- A single static **HTML** page that embeds the Looker Studio report via an `<iframe>`, plus minimal **CSS** for the page frame (title, layout).
- This page exists *only* so the dashboard can be served from our own AWS URL. It is **not** a separate web application — there is no JavaScript framework, component tree, or build step. If the team prefers to simply share the Looker Studio report link, this page can be skipped entirely.

**Cloud & Infrastructure (AWS)**
- **Amazon S3 + Amazon CloudFront** — host the static embed page / CDN, **or**
- **AWS Amplify** — managed static hosting
- **AWS Route 53** _(optional)_ — custom domain

---

## Repository Structure

```
fintech-monitor-frontend/
├── public/
│   └── index.html          # static page that embeds the Looker Studio dashboard (iframe)
├── assets/
│   └── styles.css          # minimal styling for the wrapper page (optional)
├── dashboards/
│   └── looker-config.md    # dashboard IDs, data-source mapping, chart/indicator definitions
├── infra/                  # AWS hosting config (S3 + CloudFront or Amplify)
├── .gitignore
└── README.md
```

> The real "build work" of this repo happens inside Looker Studio, not in code. `dashboards/looker-config.md` is where the team documents the report URL, the connected Google Sheet, and how each chart/indicator is set up, so the dashboard is reproducible and not just a setting buried in someone's Google account.

---

## Getting Started

Most of the work lives in Looker Studio, so there's no build to run. The only local artifact is the optional embed page.

```bash
# 1. Clone and enter the repo
git clone https://github.com/<org>/fintech-monitor-frontend.git
cd fintech-monitor-frontend

# 2. Preview the embed page locally (optional)
npx serve public          # or simply open public/index.html in a browser

# 3. Deploy the page to AWS (S3 + CloudFront or Amplify) — see infra/
```

To work on the dashboard itself, open the Looker Studio report linked in `dashboards/looker-config.md` and edit it in the browser.

> **Note:** the Looker Studio dashboards read from the **same Google Sheet** the backend writes to. Keep the embedded dashboard IDs and the tracked asset list in sync with `fintech-monitor-backend`.
