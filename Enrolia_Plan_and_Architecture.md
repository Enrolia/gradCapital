# Enrolia

Enrolia is a post-graduate application workflow that pulls admission data, deadlines, scholarships, faculty rosters, peer groups, reddit threads and VISA information into one live hub for students. 

## Product Flow and UI (5 tabs and sidebar)

```mermaid
%% Layout + spacing + title margins
%%{init: {
  "flowchart": {
    "rankSpacing": 170,
    "nodeSpacing": 90,
    "diagramPadding": 15,
    "subGraphTitleMargin": { "top": 20, "bottom": 16 },
    "curve": "basis"
  },
  "themeVariables": {
    "fontFamily": "Inter, Segoe UI, sans-serif"
  }
}}%%
flowchart TB
classDef hub fill:#e5e7eb,stroke:#1f2937,color:#111827,stroke-width:2px;
classDef tabs fill:#dbeafe,stroke:#2563eb,color:#1d4ed8,stroke-width:2px;
classDef side fill:#fee2e2,stroke:#dc2626,color:#b91c1c,stroke-width:2px;

  App[Enrolia]

  subgraph Tabs
    direction TB
    T1[Tab 1: Discovery & Onboarding]
    T2[Tab 2: Calendar & Reminders]
    T3[Tab 3: Applications Dashboard]
    T4[Tab 4: Finance Planner]
    T5[Tab 5: Comparison]
  end

  subgraph Sidebar
    direction TB
    S1[Post-Acceptance Kits]
    S2["Utilities (Low Priority)"]
    S3[Settings]
  end

  App --> Tabs
  App --> Sidebar

  class App hub;
  class T1,T2,T3,T4,T5 tabs;
  class S1,S2,S3 side;
```

### Tab 1 · Discovery & Onboarding
- Three fields: University, program, intake duration, Optional field(add spouse/family)
- Results shows (5-7 minutes): (applicants' country specific)
  - Deadlines
  - Tuition and insurance fees
  - Scholarships available
  - Relevant Faculty list with email and research areas
  - Current students in the program with linked-in profiles
  - Cost of living around the University Area
  - Visa processing times (if applicable)
  - Reddit threads related to the program
  - Local recent news relevant for the candidate (frequent storms, tsunami prone, theft prone, anti-immigrant riots etc.)
  - Working laws for full/part time students
  - Recent graduates from the university (< 2 years) and what they are doing
  - Optional (laws regarding family immigration, and extra funding from university for family support)
- Call to action: <span style="color:#16a34a"><strong>Add to Dashboard</strong></span>

### Tab 2 · Calender and Reminders
- Calendar view with color-coded event types (application, scholarship, SOP, LOR, interview)
- One-click sync to Google Calendar
- Early reminders via mail and notifications to user to fill their SOPs, get LORs, pay application fees way before time (reminders can be edited)
- Basic application and scholarship deadlines are added when clicked on CTA in tab1

### Tab 3 · Applications Dashboard (Center Tab)
- Grid/list of programs with status progression `Not Started → Drafting → Submitted → Accepted/Rejected`
- Expanding panel provides:
  - Checklists (required documents, GRE/IELTS, custom tasks) and percent completion
  - Essay studio: embedded google docs that is deep linked (open the google docs app if available or an embedded web view)
  - Drive linked documents
  - Sharing link of SOPs for people to review (and add suggestions)
  - Option to delete application
- Basic checklist already added when clicked on CTA in tab1

### Tab 4 · Finance Planner
- Tuition, insurance and living cost(prefilled) with scholarship offsets (can change the different amounts)
- Loan planner which allows user to add various funding sources (family savings, part-time work, internships during vacations, professors' grants), with EMI slider
- Not planning anything like this right now (but there is always an option to recommend local banks and insurance)
- Option to export this info as pdf

### Tab 5 · Comparison 
- Side-by-side cards (up to 3 programs) with fees, location, ranking, research opportunities, visa timelines, funding options, basically all the info we scraped earlier for choosing and deciding their options via a scoring system where the candidate can select how much weight they want to put towards each criteria. (there would be a default weight applied)

### Sidebar · Post-Acceptance Kits
- Visa checklist per destination, pre-departure packing, packing checklist, arrival paperwork (I have expertise in making overkill packing checklists!!!)
- Also gives user a document for common laws that he/she could be unaware coming from a different country
- When user updates that they accepted an offer, a checklist for that university is generated for them. 
- Option to export to pdf


### Sidebar · Utilities (Low priority)
- Very straight to the point, pdf resizer/size reducer and jpeg size reducer/resizer - Low priority 
- Gmail monitoring for university and professor replies (from official domains) - default opt out (but user can opt in for this) - Low priority feature
- Chrome extension for prefilling profile info during applications (higher priority)
- Daily (pre curated) ling of one thread/blog posts/video about story of a student getting into the program of their choice (higher priority)
- Additional language support (lower priority)
- Screen reader support (higher priority)

### Sidebar · Settings
- Profile & Identity: Name, photo, nationality, education level, GRE/GMAT scores, GPA scale, research interests (for prefilling in applications)
- Data & Privacy: GPDR compliancy, option to request deletion of data from server during account deletion, revoking drive/gmail/google account access (can't use most features without it)
- Delete account
- Subscriptions (not planned the cost model yet)
- Feature requests/ feedback section

## Planned Architecture and development flow

```mermaid
%%{init: {
  "flowchart": {
    "rankSpacing": 300,
    "nodeSpacing": 200,
    "diagramPadding": 30,
    "subGraphTitleMargin": { "top": 10, "bottom": 10 },
    "curve": "basis"
  },
  "themeVariables": {
    "fontFamily": "Inter, Segoe UI, sans-serif"
  }
}}%%
flowchart TD
  classDef onprem fill:#fde2e1,stroke:#dc2626,color:#111,stroke-width:1.5px;
  classDef onprem_prem fill:#fff7ed,stroke:#f97316,color:#111,stroke-width:1.5px;
  classDef free   fill:#dbeafe,stroke:#2563eb,color:#111,stroke-width:1.5px;
  classDef prem   fill:#dcfce7,stroke:#16a34a,color:#111,stroke-width:1.5px;
  classDef infra  fill:#f3f4f6,stroke:#9ca3af,color:#111,stroke-width:1px;
  classDef ext    fill:#f8fafc,stroke:#94a3b8,color:#111,stroke-width:1px;
  classDef onprem_infra fill:#fee2e2,stroke:#ef4444,color:#111,stroke-width:1px;

  subgraph VSC ["Version Control & CI/CD"]
    direction LR
    DEV[Developer]:::ext
    GIT[Git Repository]:::ext
    CICD_BACKEND["Backend CI/CD"]:::ext
    CICD_CLIENTS["Client CI/CD"]:::ext
  end

  subgraph DevClients ["Development Clients"]
      direction LR
      C_DEV["Local Dev"]:::ext
  end

  subgraph ProdClients ["Production Clients"]
    direction LR
    C1["iOS (SwiftUI)"]:::infra
    C2["Android (Compose)"]:::infra
    C3["Web (SvelteKit)"]:::infra
  end

  subgraph Gateway
    direction TB
    GW["API Gateway (Envoy)"]:::infra
  end

  subgraph Clusters
    direction LR
    subgraph GKE["GKE Cluster (Production)"]
      direction TB
      subgraph NP_F["Node Pool (Standard Machines)"]
        direction LR
        subgraph NS_F["Namespace: free-tier"]
            direction TB
            F_API[Django API]:::free
            F_DB[(PostgreSQL)]:::free
        end
        subgraph NS_A["Namespace: automation"]
            direction TB
            N8N[n8n Queue Workers]:::infra
            MIG["Migration Jobs"]:::infra
        end
      end
      subgraph NP_P["Node Pool (High-Performance Machines)"]
        direction LR
        subgraph NS_P["Namespace: premium-tier"]
            direction TB
            P_API[Django API]:::prem
            P_DB[(PostgreSQL)]:::prem
        end
      end
    end

    subgraph Proxmox["Proxmox Cluster (Dev/Test)"]
      direction TB
       subgraph NP_F_DEV["Node Pool (Dev Standard)"]
        direction LR
        subgraph NS_F_DEV["Namespace: free-tier (Dev)"]
            direction TB
            O_API_F[Django API]:::onprem
            O_DB_F[(PostgreSQL)]:::onprem
        end
        subgraph NS_A_DEV["Namespace: automation (Dev)"]
            direction TB
            N8N_DEV["n8n (Dev)"]:::onprem_infra
            MIG_DEV["Migration Jobs (Dev)"]:::onprem_infra
        end
      end
      subgraph NP_P_DEV["Node Pool (Dev Performance)"]
        direction LR
        subgraph NS_P_DEV["Namespace: premium-tier (Dev)"]
            direction TB
            O_API_P[Django API]:::onprem_prem
            O_DB_P[(PostgreSQL)]:::onprem_prem
        end
      end
    end
  end

  subgraph Externals [External Services]
    direction TB
    subgraph Backups
        BACKUP["Backup Storage (3-2-1)"]:::infra
    end
    subgraph Integrations
      direction TB
      RCAT[RevenueCat]:::ext
      RCAT_DEV["RevenueCat (Dev)"]:::ext
      GAPI[Google APIs]:::ext
      PUSH[Push Services]:::ext
      PUSH_DEV["Push Services (Dev)"]:::ext
      subgraph AI_Models["AI Model Providers"]
        direction TB
        AIAPI_F["Free Tier Models"]:::ext
        AIAPI_P["Premium Tier Models"]:::ext
      end
      OBS[Observability Stack]:::ext
    end
    subgraph Storage["GCS Buckets"]
        direction TB
        GCS_FREE[(GCS – Free)]:::infra
        GCS_PREM[(GCS – Premium)]:::infra
        GCS_DEV_F["GCS – Dev (Free)"]:::infra
        GCS_DEV_P["GCS – Dev (Premium)"]:::infra
    end
  end


  %% CI/CD Workflow
  DEV -->|1 Commit code| GIT
  
  %% Backend Pipeline
  GIT -->|2 Trigger pipeline| CICD_BACKEND
  CICD_BACKEND -->|3 Test & Migrate Dev| MIG_DEV
  CICD_BACKEND --->|4 Deploy App Code| F_API
  CICD_BACKEND --->|4 Deploy App Code| P_API
  CICD_BACKEND -->|5 Migrate Prod DB| MIG

  %% Client Pipeline
  GIT -->|2 Trigger pipeline| CICD_CLIENTS
  CICD_CLIENTS -->|Deploy| C1
  CICD_CLIENTS -->|Deploy| C2
  CICD_CLIENTS -->|Deploy| C3

  %% Main Application Flow
  ProdClients --> GW
  GW --> F_API
  GW --> P_API

  %% Internal Production Links
  RCAT --> GW; RCAT --> N8N
  F_API --> F_DB; P_API --> P_DB
  F_API --> GCS_FREE; P_API --> GCS_PREM
  F_API <--> N8N; P_API <--> N8N
  N8N <--> GAPI; N8N --> PUSH; N8N <--> AIAPI_F; N8N <--> AIAPI_P
  N8N --> MIG
  MIG --> F_DB; MIG --> P_DB; MIG --> GCS_FREE; MIG --> GCS_PREM
  F_API --> OBS; P_API --> OBS; N8N --> OBS

  %% Internal Development Links
  C_DEV --> O_API_F; C_DEV --> O_API_P
  O_API_F --> O_DB_F; O_API_P --> O_DB_P
  O_API_F --> GCS_DEV_F; O_API_P --> GCS_DEV_P
  O_API_F <--> N8N_DEV; O_API_P <--> N8N_DEV
  N8N_DEV <--> GAPI; N8N_DEV <--> AIAPI_F; N8N_DEV <--> AIAPI_P
  N8N_DEV --> MIG_DEV
  MIG_DEV --> O_DB_F; MIG_DEV --> O_DB_P; MIG_DEV --> GCS_DEV_F; MIG_DEV --> GCS_DEV_P
  O_API_F --> OBS; O_API_P --> OBS; N8N_DEV --> OBS
  RCAT_DEV --> N8N_DEV
  N8N_DEV --> PUSH_DEV


  %% Backup Links
  GIT --> BACKUP
  F_DB --> BACKUP
  P_DB --> BACKUP
  GCS_FREE --> BACKUP
  GCS_PREM --> BACKUP
  GCS_DEV_F --> BACKUP
  GCS_DEV_P --> BACKUP
  O_DB_P --> BACKUP
  O_DB_F --> BACKUP

  %% LINK STYLES
  %% Total links: 63 (Indices 0-62)

  %% CI/CD Pipeline
  linkStyle 0,1,2,3,4,5,6,7,8,9 stroke:#8b5cf6,stroke-dasharray:5 5,stroke-width:2.5px;
  %% Main Application Flow
  linkStyle 10,11,12 stroke:#64748b,stroke-width:2.5px;
  %% Entitlement Signals (Prod)
  linkStyle 13,14 stroke:#f59e0b,stroke-width:2px;

  %% Production - Free Tier
  linkStyle 11,15,17,23,30 stroke:#2563eb,stroke-width:2.2px;
  %% Production - Premium Tier
  linkStyle 12,16,18,24,31 stroke:#16a34a,stroke-width:2.2px;
  %% Production - Workflow & Migrations
  linkStyle 19,20,21,22,25,26,27,28,29,32 stroke:#6b7280,stroke-dasharray:4 4,stroke-width:2px;

  %% Dev Client -> On-Prem Backend
  linkStyle 33,34 stroke:#8b5cf6,stroke-dasharray:4 4,stroke-width:2px;
  %% Development - Free Tier (On-Prem)
  linkStyle 35,37,39,45,46,49 stroke:#dc2626,stroke-width:2px;
  %% Development - Premium Tier (On-Prem)
  linkStyle 36,38,40,50 stroke:#f97316,stroke-width:2px;
  %% Development - Workflow & Migrations (Dashed Purple)
  linkStyle 41,42,43,44,47,48,51,53 stroke:#8b5cf6,stroke-dasharray:4 4,stroke-width:2px;
  %% Entitlement Signals (Dev)
  linkStyle 52 stroke:#f59e0b,stroke-dasharray:4 4,stroke-width:2px;

  %% Backup Links
  linkStyle 54,55,56,57,58,59,60,61,62 stroke:#0891b2,stroke-dasharray:3 3,stroke-width:2px;
```


## Current progress
- Researched and designed the UI flow, have started creating some mocks in figma
- Researched about the backend architecture, current plan is to have one development environment in my homelab and two production server on GKE (one for free, one for premium servers). During the alpha would not be going for HA and would be just following 3-2-1 backups. 
- Have started developing 2 of the n8n workflows, only testing with local models right now (whatever currently my llama.cpp server can handle on 24gb VRAM)
- After workflow finalization, I am tilting towards groq (if the open-weights models work as expected)
- The plan is to use the lowest possible performant model as slave to get the workflow to appropriate results, for the master model, I am testing with GPT OSS ones, but would go for models GPT 5 medium reasoning one when I have finalised. 


## Plan
- Prototype: I plan to have a alpha version of Enrolia by December 15th. I would have ample time in December to solve user queries after invite only release. I won't have any premium tier during alpha release.  

- During alpha phase, would only be marketing through reddit threads, linked-in and personal circle (no ads yet)

- Alpha (Dec 2025): I am not going to charge users during the alpha stage so would be bleeding a little in API costs and server costs duing this phase. I ran a small experiment in my homelab (4 node PVE cluser with my own networking), a kubernetes cluster similar to my experiment would cost me around 100 dollar per month for hardware and 400-600 dollars in API for 4000-5000 users. (Assuming users are not abusing the app, I would be rate limitng for that)
I would also be limiting my alpha release to 10000 users so that I can contain the initial costs. 

- Pricing/Subscription cost: I am not deciding the subscription cost right now and also brainstorming if I should have a one time BYOK option. I would have much clarity about my expense v/s pricing strategy during the alpha release. 

- Beta target: Beta stage should be around March-April, however I want to polish the UX and onboarding before that and will extend that if not ready to my expectations. Will think about ads and marketing strategies. 

- Developer tools: Jira and mattermost is my goto, however I am inclining towards using linear too (haven't decided that yet)

## Grant usage

- Primarily would be using the grant for API credits and GCP costs (after extinguishing the free GCP credits)

## Future plan

- Will be applying for GCP start tier credits after Jan. 
- Depending upon the initial respone during alpha phase, will think about scaling the infra and marketing strategies




