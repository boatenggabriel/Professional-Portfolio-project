Enterprise Cybersecurity Portfolio & Advisory Platform

A high-performance, single-page application (SPA) engineered to showcase offensive and defensive cybersecurity capabilities, interactive metrics, and an integrated consultation scheduling system backed by Google Cloud Firestore and Gemini AI.

Table of Contents

Executive Summary

System Architecture

Core Competencies Showcased

Key Features

Security Architecture & Posture

Threat Model (STRIDE)

Recommended Firestore Security Rules

API Key Management & Least Privilege

Directory Structure

Local Development & Setup

Configuration & Environment Variables

Deployment Pipeline

Roadmap

Vulnerability Disclosure Policy

Author & Contact

Executive Summary

This platform serves as both a portfolio showcasing specialized technical competencies in computer systems security and an active portal for professional consultations. Engineered using lightweight, dependency-minimized browser technologies, the system pairs client-side visualizations (Chart.js) with serverless persistence (Firestore) and dynamic AI analysis (Gemini API with Google Search grounding).

System Architecture

The solution uses a zero-build, cloud-native architecture optimized for zero latency, minimal attack surface, and seamless browser compatibility:

[ Web Client / Browser ]
       |
       +---> UI Engine: Tailwind CDN + Pure Vanilla JS (ES Modules)
       |
       +---> Data Visualizations: Chart.js Canvas Rendering
       |
       +---> Authentication: Firebase Auth (Anonymous / Custom Claims)
       |
       +---> Storage Layer: Google Cloud Firestore (User Scoped Data)
       |
       +---> Intelligence Layer: Google Gemini 2.5 API (Search Grounding Enabled)


Core Competencies Showcased

Domain

Focus Area

Real-World Application

Defensive Security

HoneyPot Deployment

Setting low-interaction traps (e.g., Cowrie, Dionaea) for threat reconnaissance and telemetry collection.

Offensive Security

Vulnerability Assessment

Network and target mapping using tools such as Nmap, Nessus, OpenVAS, and Metasploit.

Forensics & Analysis

Malware Analysis

Static binary inspection (PE headers, hashes, strings) and dynamic sandboxing.

Governance & Compliance

Policy Development & Docs

Structuring AUPs, ISO/IEC 27001, and NIST CSF-aligned security documentation.

Systems & Platforms

OS Hardening (Kali / Windows)

Secure baseline deployment, kernel parameter tuning, and audit log configuration.

Human Element

Awareness Programs

Phishing simulation campaigns and user training against social engineering attacks.

Key Features

Interactive Metric Visualizations: Visual telemetry for skill distribution and milestone completion rates using native Canvas rendering via Chart.js.

AI-Powered Grounded Deep Dives: Dynamic modal integrations querying Google Gemini 2.5 to provide cited, real-time security breakdowns for each competency.

Authenticated Scheduling: Automated appointment scheduling gated behind user authentication and isolated Firestore collections.

Responsive Dark-Themed Interface: Low-contrast visual design optimized for technical readability and reduced eye strain.

Security Architecture & Posture

Because this repository represents a cybersecurity professional's work, security-by-design principles are applied directly to the application architecture.

Threat Model (STRIDE)

Threat

Vector

Mitigation Implemented / Recommended

Spoofing

Forged booking submissions

Authentication requirement (userId verification) before database writes.

Tampering

Modifying booking records in transit

Enforced HTTPS / TLS 1.3 across all external API endpoints.

Repudiation

Denying an appointment request

Server-assigned timestamps and immutable record structures.

Information Disclosure

Unauthorized access to booking records

Granular Firestore Security Rules restricting reads to the authenticated document owner.

Denial of Service

Exhausting API quotas

Exponential backoff algorithm (fetchWithBackoff) handling HTTP 429 and rate-limiting responses.

Elevation of Privilege

Compromising client environment

Elimination of inline eval() statements and reliance on sandboxed iframe/CSP parameters.

Recommended Firestore Security Rules

To enforce strict tenant isolation, deploy the following rules in the Firebase Console:

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Artifact root path
    match /artifacts/{appId}/users/{userId}/appointments/{appointmentId} {
      // Allow creation only if the user is authenticated and matches the path UID
      allow create: if request.auth != null 
                    && request.auth.uid == userId
                    && request.resource.data.bookedByUid == userId;
                    
      // Allow read/update/delete strictly to the owner or designated admin
      allow read, update, delete: if request.auth != null && request.auth.uid == userId;
    }
  }
}


API Key Management & Least Privilege

Gemini API Key: Restrict the key in the Google Cloud Console to only allow calls to the generativelanguage.googleapis.com service.

HTTP Referrer Restrictions: Lock your production API key to your specific domain (e.g., https://yourdomain.github.io/*).

Directory Structure

├── index.html              # Core single-page application (Markup, Tailwind, Scripts)
├── README.md               # Enterprise-grade project documentation
├── LICENSE                 # Open-source license (MIT)
└── .github/
    └── workflows/
        └── deploy.yml      # CI/CD pipeline for GitHub Pages (Optional)


Local Development & Setup

Prerequisites

A modern Chromium- or Gecko-based browser (Brave, Chrome, Firefox, Edge).

A code editor (e.g., VS Code).

Recommended: Live Server extension for VS Code.

Installation

Clone the repository:

git clone https://github.com/your-username/cybersecurity-portfolio.git
cd cybersecurity-portfolio


Open the project in VS Code:

code .


Launch the Local Server:

Right-click index.html and select "Open with Live Server".

Alternatively, use Python's built-in HTTP server:

python3 -m http.server 8080


Navigate to http://localhost:8080 in your browser.

Configuration & Environment Variables

For local and testing environments, verify the configuration parameters in the <script type="module"> section of index.html:

Variable

Description

Default / Source

GEMINI_API_URL

Endpoint for generative AI queries

https://generativelanguage.googleapis.com/v1beta/models/...

API_KEY

Google AI Studio access key

Configured in client or runtime variable

firebaseConfig

Firebase project credentials object

Injected via runtime or defined in code

appId

Application identifier for Firestore root

Extracted from __app_id or defaults to 'default-app-id'

Deployment Pipeline

Deploying via GitHub Pages

Push your repository to GitHub:

git add .
git commit -m "feat: initial enterprise portfolio release"
git branch -M main
git push -u origin main


Navigate to your repository on GitHub:

Go to Settings > Pages.

Under Build and deployment > Source, select Deploy from a branch.

Select main branch and / (root), then click Save.

Your site will be live at https://<your-username>.github.io/<repo-name>/.

Roadmap

[x] Client-side charting with dynamic dataset sizing.

[x] Grounded generative AI skill breakdowns.

[x] Firestore-backed appointment scheduling.

[ ] Integration with Webhook / Discord alerts for newly booked sessions.

[ ] PGP public key signature block in contact section for encrypted correspondence.

[ ] Automated SAST scanning integration using GitHub Actions (CodeQL).

Vulnerability Disclosure Policy

If you discover a security vulnerability within this project, please send an advisory directly to [your-secure-email@domain.com] or submit an issue flagged as security. Do not open public issues for zero-day vulnerabilities.

Author & Contact

BOATENG GABRIEL

BSc Candidate in Cybersecurity

LinkedIn:https://www.linkedin.com/in/boateng-gabriel

GitHub:https://github.com/boatenggabriel

Email:boatenggabriel129@gmail.com
