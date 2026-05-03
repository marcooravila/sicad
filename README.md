# SICAD · Biometric Payroll Audit System — SSP Acapulco
**Detection of operational corruption and public treasury recovery through biometric cross-validation · Q2 2025**
 
---
 
## Business Problem
 
The SSP Acapulco (Municipal Public Security Department) manages payroll for 2,117 officers through physical pay slips issued bi-weekly by the City Hall. Before SICAD, the process required employees to identify themselves verbally — the desk operator would locate their name on a list and hand them the slip for a handwritten signature. No digital record. No traceability.
 
That simplicity was the vulnerability: desk operators could sign on behalf of absent employees with no evidence trail. City Hall received slips with 100% apparent signatures. Real physical attendance was a different story — and it had three concrete consequences:
 
1. The treasury paid full salaries to officers who did not report for duty, with no mechanism to detect or prove it
2. Absence deductions ($466 MXN per missed signature) were never applied — operators covered absences with false signatures
3. No traceability existed over which operator signed which slip, making any administrative or disciplinary process impossible
---
 
## Solution Design
 
The core analytical insight was that detecting fraud required two independent data sources — not one. SICAD registers biometric presence (fingerprint); City Hall registers handwritten signatures. The discrepancy between them — a signed slip with no biometric match — is the evidence.
 
**Three record types defined:**
 
| Type | Biometric | Paper | Outcome |
|---|---|---|---|
| A — Verified presence | ✓ | ✓ | Valid |
| B — Registered absence | ✗ | ✗ | $466 MXN deduction |
| C — Anomaly / Corruption | ✗ | ✓ | Operator investigation |
 
Type C anomalies cannot occur by employee error or oversight. They require deliberate action by the desk operator.
 
**Key metric:** Type C anomaly rate (not attendance rate). A healthy biometric system should have Type C → 0%. Any value above zero is directly attributable to the desk operator.
 
---
 
## Results — Q2 2025
 
| Month | Type A (Verified) | Type B (Absent) | Type C (Anomaly) | Treasury Recovery |
|---|---|---|---|---|
| April 2025 (Pre-SICAD) | 1,831 · 86.5% | 117 · 5.5% | ~169 · 8.0% (undetectable) | $54,522 MXN |
| May 2025 (Launch) | 1,164 · 55.0% | 254 · 12.0% | **699 · 33.0% (detected)** | $118,364 MXN |
| June 2025 (Post) | 1,481 · 70.0% | 127 · 6.0% | 509 · 24.0% (−27.2%) | $59,182 MXN |
 
- **699 Type C anomalies** detected in May — 1 in 3 officers had a signed slip with no biometric match
- **$232,068 MXN** recovered in Q2 2025 via absence deductions previously invisible to City Hall
- **−27.2% reduction** in anomalies between May and June with no personnel changes or sanctions initiated — the biometric record alone modified operator behavior
- **Top exposure unit:** Dirección de Policía Preventiva Auxiliar · 168 anomalies (24% of total)
---
 
## Strategic Decisions
 
**Why biometrics and not QR or digital signatures:** Every alternative (QR codes, tablet signatures, photo ID) can be operated by a third party. A fingerprint cannot be delegated or replicated from a desk. That physical restriction makes Type C anomalies impossible to create without deliberate operator action — and therefore directly attributable.
 
**Why PostgreSQL and not Excel:** Every record includes insertion timestamp and loading user. In an administrative process before the Internal Control Body (OIC), that traceability is not a technical detail — it is evidence. The schema also supports adding new bi-weekly periods without structural changes and scaling to other municipal departments without redesign.
 
---
 
## Recommended Next Steps
 
1. **Immediate:** Submit Type C case files to the OIC — 699 May anomalies represent documented evidence of operator fraud, cross-referenceable with individual recurrence patterns
2. **Strategic:** Cross-correlate accumulated Type C anomalies with per-employee absence history to identify chronic non-presence patterns — the real risk of salary paid without service rendered
3. **Scalability:** Extend SICAD beyond payroll to digitize paper-based administrative processes: leave permits, vacation balances, extraordinary duty logs, and tactical equipment tracking. The PostgreSQL schema and Python pipeline are already designed to support new modules without redesign — marginal cost of expansion is minimal compared to continuing to operate each process without traceability
---
 
## Tech Stack
 
| Tool | Role |
|---|---|
| Python · Pandas | Data cleaning, biometric-to-slip cross-validation, Type C detection pipeline |
| PostgreSQL | Auditable persistent storage · timestamp + user traceability per record |
| Biometric hardware | Fingerprint capture — the only record that cannot be delegated |
| Chart.js | Embedded interactive EDA notebook (no external BI dependencies) |
 
---
 
## Project Structure
 
```
sicad-eda/
├── SICAD-EDA-Notebook.html         # Interactive EDA notebook (self-contained)
├── 01_load_and_clean.py
├── 03_sicad_cross_validation.py
└── data/
    └── plantilla_ssp_anonimizada/  # Anonymized payroll dataset
```
 
> **Confidentiality notice:** Data has been anonymized and partially adjusted for demonstration purposes. Methodology and findings remain analytically valid.
