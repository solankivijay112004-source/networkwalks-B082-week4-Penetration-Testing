
<h1 align="center">🏥 Mediroza General Hospital — Penetration Testing Report</h1>

<p align="center">
  🔐Security Assessment &amp; Vulnerability Analysis
</p>

## Overview
This folder contains the final penetration testing report produced for the **Networkwalks Batch B082, Week 4** training engagement against the simulated target **Mediroza General Hospital** (`medirozahospital.com`).

> This is a training-lab exercise conducted under written authorisation from Networkwalks. All techniques described were applied only against the designated training target and must never be used against systems without explicit written permission.

## Report Structure
| Section | Contents |
|---|---|
| Title Page | Engagement details, target, author, confidentiality notice |
| 1. Executive Summary | High-level overview of risk and key findings |
| 2. Scope and Methodology | Client, target, rules of engagement, authorisation, tools used |
| 3. Findings and Proof of Exploitation | Detailed write-up per milestone, with screenshots |
| 4. Risk Rating | Each finding rated Critical / High / Medium with justification |
| 5. Recommendations and Remediation | Actionable fixes grouped by finding area |
| 6. Conclusion | Summary and priority remediation guidance |

---

## M1 — Initial Access: SQL Injection (Critical)
Submitting a crafted value on the Patient Portal login form returned a raw MySQL syntax error, confirming unsanitised input reaching the backend query.

<img width="1278" height="797" alt="image" src="https://github.com/user-attachments/assets/c11f5c95-e3ea-4e14-94eb-0e3d0c01dfdb" />

*Figure 1. SQL syntax error returned by the Patient Portal login form.*

Further testing with Burp Suite against the login request ultimately bypassed authentication, granting access to the restricted patient area and 3 confidential PDF lab reports.

<img width="1276" height="802" alt="image" src="https://github.com/user-attachments/assets/5d239998-9b9f-44c4-92e2-1004d5fe3812" />

*Figure 2. Burp Suite intercepting a login request during SQL injection testing.*

---

## M2 — Data Extraction: Weak PDF Encryption (High)
Each retrieved PDF was password-protected using standard PDF encryption (PDF R3, 128-bit, MD5 + RC4). Password hashes were extracted and cracked via dictionary attack.

<img width="1887" height="1002" alt="image" src="https://github.com/user-attachments/assets/0d63e6d8-fcd7-4208-9bcc-5f73481afd5c" />


*Figure 3. Decrypted patient_report_1.pdf — Sipho Dlamini.*
<img width="1887" height="952" alt="image" src="https://github.com/user-attachments/assets/a4777bf9-7aba-4000-ae0c-2ff195d3bc24" />

*Figure 4. Dictionary attack recovering the password "123456".*

<img width="1912" height="1011" alt="Screenshot 2026-09-13 084723" src="https://github.com/user-attachments/assets/312b0a27-4825-45a4-bc7a-8e124751bd3b" />

*Figure 5.Decrypted patient_report_2.pdf — John van der merewe.*

<img width="1885" height="841" alt="Screenshot 2026-09-12 215839" src="https://github.com/user-attachments/assets/c42c1a63-3fe1-43f7-8121-8177a05348e9" />

*Figure 6. Dictionary attack recovering the password "password".*

<img width="1917" height="991" alt="image" src="https://github.com/user-attachments/assets/bf1b7ebe-6da2-4085-828a-1c9d2f7dc685" />

*Figure 7. Decrypted patient_report_3.pdf — Emily Thompson.*

<img width="1887" height="862" alt="image" src="https://github.com/user-attachments/assets/b9264171-4e9f-49dc-89d6-a5b5d4a3fe65" />

*Figure 8. Extended attack recovering the stronger password "!@#$%^&".*

---

## M3 — Attack: Exposed Database Backup (Critical)
A legacy, unauthenticated backup file (`mediroza_db_backup_2019.sql`) was discovered exposed on the server, containing 30 staff records and 10 shareholder records.

<img width="1890" height="861" alt="image" src="https://github.com/user-attachments/assets/bd44f750-b948-4956-8c13-0a7c60c1d6dc" />

*Figure 9. Header and staff table schema from the exposed backup file.*

<img width="1892" height="860" alt="image" src="https://github.com/user-attachments/assets/69e532ac-dffc-4fbf-98ba-d7ed4122d192" />

*Figure 10. Staff record data and shareholders table schema.*

<img width="1896" height="857" alt="image" src="https://github.com/user-attachments/assets/de05de6c-cb19-4eeb-b6c4-fe1a65502e97" />

*Figure 11. Shareholder records and end of the exposed backup file.*

The full recovered data is tabulated in the report as **Table 1 (30 staff records)** and **Table 2 (10 shareholder records)**.

---

## Milestone Summary
| Milestone | Finding | Risk |
|---|---|---|
| **M1** — Initial Access | SQL Injection on the Patient Portal login, bypassing authentication | Critical |
| **M2** — Data Extraction | Weak/predictable PDF passwords ( `123456`,`password`, `!@#$%^&`) | High |
| **M3** — Attack (Data Exposure) | Exposed DB backup disclosing staff salaries, national IDs, and shareholder equity | Critical |
| **M4** — Reporting | Full professional pentest report (this deliverable) | — |

## Handling Notes
- This report is marked **CONFIDENTIAL — AUTHORISED PERSONNEL ONLY** and contains simulated PII (names, national ID–style numbers, salaries) generated for the training exercise. Treat it accordingly and do not distribute outside the intended audience (instructor/reviewer).
- Keep the `images/` folder alongside this README so the screenshots render correctly.
- Recommended next step per the assessment brief: submit `Mediroza_Pentest_Report.docx` as the Milestone 4 deliverable.

---
## 👤 Author
**Vijay Solanki**
Cybersecurity Intern, Batch B082

LinkedIn: https://www.linkedin.com/in/solanki-vijay/
