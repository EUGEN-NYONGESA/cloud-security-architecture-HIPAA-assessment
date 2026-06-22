# Cloud Security Architecture — Shared Responsibility & HIPAA Compliance Assessment

![Focus](https://img.shields.io/badge/Focus-Cloud%20Security-1F4E79)
![Compliance](https://img.shields.io/badge/Compliance-HIPAA-2E7D32)
![Model](https://img.shields.io/badge/Framework-Shared%20Responsibility%20Model-555)
![Environment](https://img.shields.io/badge/Environment-Hybrid%20Cloud-orange)

A security assessment of a healthcare organization's **hybrid cloud environment** (on-premises + cloud provider) that processes **Protected Health Information (PHI)**. The work evaluates current controls against cloud security best practices, maps them to the **HIPAA Security Rule**, and delivers prioritized, actionable recommendations.

> **Role:** Member of the IT security team performing the evaluation.
> **Deliverable:** A shared-responsibility analysis, a best-practices gap evaluation, a HIPAA compliance comparison, and a remediation roadmap.

---

## Table of Contents

- [Overview](#overview)
- [Scenario / Environment](#scenario--environment)
- [Frameworks & Concepts Applied](#frameworks--concepts-applied)
- [1. Shared Responsibility Analysis](#1-shared-responsibility-analysis)
- [2. Cloud Security Architecture Evaluation](#2-cloud-security-architecture-evaluation)
- [3. HIPAA Compliance Comparison](#3-hipaa-compliance-comparison)
- [4. Recommendations for Improvement](#4-recommendations-for-improvement)
- [Conclusion](#conclusion)
- [Skills Demonstrated](#skills-demonstrated)
- [Repository Structure](#repository-structure)
- [Disclaimer](#disclaimer)

---

## Overview

The organization has a reasonable security baseline but carries several gaps that affect HIPAA compliance. The most pressing are **unencrypted PHI backups**, **fragmented encryption key management**, and **access control built on manual role management with no reviews**. This assessment documents who owns what across the hybrid estate, scores each control, ties the gaps to specific HIPAA safeguards, and lays out a prioritized fix list.

---

## Scenario / Environment

| Area | Current State |
|------|---------------|
| **Infrastructure** | Hybrid model — sensitive data on-premises, non-critical workloads in the cloud. Provider manages physical security and underlying infrastructure. |
| **Data Security** | Cloud data encrypted at rest and in transit. Keys for on-prem and cloud are in **separate systems with no central control**. **Cloud backups are not encrypted.** |
| **Access Control** | Cloud roles managed manually with **no deactivation lifecycle**; **no periodic access reviews** on-prem or cloud. |
| **Monitoring & IR** | Provider monitoring tools available, but hybrid logs are **not centralized**; incident response plan does not cover hybrid-specific risks. |
| **Network Security** | Cloud VMs exposed to the internet for admin access (limited to a specific IP range). On-prem firewalls are **outdated** and lack deep packet inspection. |

---

## Frameworks & Concepts Applied

- **Shared Responsibility Model** — dividing security duties between the cloud provider and the customer (more falls to the customer in a hybrid setup).
- **HIPAA Security Rule** — Administrative, Physical, and Technical Safeguards.
- **CIA Triad** — Confidentiality, Integrity, Availability of PHI.
- **Defense in Depth**, **Principle of Least Privilege (PoLP)**, **Role-Based Access Control (RBAC)**, **Centralized Key Management**, and **SIEM-based monitoring**.

---

## 1. Shared Responsibility Analysis

Responsibilities split between the cloud provider and the healthcare organization. Because part of the estate is on-premises, several duties that would sit only with the customer in a pure-cloud setup are even more clearly the organization's to own.

| Component | Cloud Provider Responsibility | Healthcare Organization Responsibility |
|-----------|-------------------------------|----------------------------------------|
| **Data Encryption** | Encrypt data at rest on storage devices and provide encryption tools for customer use. | Manage encryption keys securely, ensure consistent encryption policies across the hybrid environment, and encrypt cloud backups. |
| **Access Control** | Provide IAM tools, identity federation, and maintain the underlying identity infrastructure. | Enforce MFA for all users, implement RBAC, conduct periodic access reviews, and manage user lifecycle (onboarding/offboarding). |
| **Monitoring & Incident Response** | Maintain cloud-native logging tools, provide infrastructure-level monitoring, and notify customers of platform-wide incidents. | Integrate hybrid logs into a centralized system, configure automated alerting, and update IR plans for hybrid cloud risks. |
| **Network Security** | Secure physical network infrastructure, provide DDoS protection, and maintain the cloud network fabric. | Configure security group rules, implement network segmentation, and update on-prem firewalls for modern security features. |
| **Backup & Recovery** | Enable automated backup services and provide infrastructure redundancy. | Encrypt cloud backups, test disaster recovery procedures, and ensure backups meet compliance requirements. |
| **Patch Management** | Patch the underlying hypervisor, physical infrastructure, and cloud platform components. | Patch guest operating systems and applications running on cloud VMs. |
| **Physical Security** | Secure data centers with access controls, surveillance, and environmental protections. | N/A — provider handles all physical security for cloud infrastructure. |
| **Compliance & Auditing** | Maintain certifications (e.g., SOC 2, HIPAA BAA) and provide compliance documentation. | Sign a Business Associate Agreement (BAA), conduct regular risk assessments, and maintain HIPAA compliance for PHI. |

---

## 2. Cloud Security Architecture Evaluation

Each best practice is scored as implemented, missing, or incomplete, with a note on the gap and why it matters.

| Best Practice | Status | Notes (Gap / Importance) |
|---------------|:------:|--------------------------|
| **Data encryption at rest and in transit** | ⚠️ Partially | Cloud data is encrypted at rest and in transit. However, **cloud backups are not encrypted**, creating a significant exposure. Encrypting backups prevents data exposure if backup storage is compromised. |
| **Centralized key management** | ❌ No | On-prem and cloud keys are managed in **different systems with no central control**, adding overhead, raising the risk of key mismanagement, and complicating rotation. Centralized key management is essential for consistent security. |
| **Multi-factor authentication (MFA)** | ❌ No | MFA is not enforced. Without it, accounts are vulnerable to credential theft and phishing — a critical gap for systems touching PHI. |
| **Principle of least privilege** | ⚠️ Partially | Roles exist but are managed manually with no lifecycle and no periodic reviews. Permissions can persist after role changes or departures, so least privilege isn't effectively enforced. |
| **Automated monitoring, logging, alerting** | ⚠️ Partially | Provider monitoring tools are available, but logs are **not centralized** across the hybrid environment, limiting threat detection and response. |
| **Network segmentation** | ⚠️ Partially | On-prem/cloud separation exists, but segmentation could be stronger. Cloud VMs are internet-exposed for admin access, and outdated on-prem firewalls lack deep packet inspection. |
| **Regular vulnerability assessments** | ❌ No | No regular vulnerability assessments indicated. Without ongoing scanning, new vulnerabilities go unaddressed. |
| **Documented and tested disaster recovery plans** | ⚠️ Partially | Backups exist but are **not encrypted**, and the scenario does not confirm DR plans are documented or tested. Both create compliance and recovery risk. |

**Legend:** ✅ Implemented · ⚠️ Partially · ❌ Missing

---

## 3. HIPAA Compliance Comparison

Current practices mapped onto HIPAA's safeguard categories, flagging gaps and areas where responsibility is correctly placed.

| HIPAA Requirement | Current Practice | Gap or Improvement Needed |
|-------------------|------------------|---------------------------|
| **Administrative — Security Management Process** | No formal risk assessment process documented. | Conduct annual security risk assessments; document findings and remediation plans. |
| **Administrative — Assigned Security Responsibility** | Security responsibilities not clearly assigned. | Designate a formal Security Officer covering both on-prem and cloud. |
| **Administrative — Workforce Security** | No account deactivation lifecycle; no periodic access reviews. | Implement formal on/offboarding procedures; conduct quarterly access reviews. |
| **Administrative — Information Access Management** | Cloud roles managed manually with no lifecycle management. | Add approval workflows for access requests; use automated identity lifecycle tools. |
| **Administrative — Security Awareness Training** | No formal training program mentioned. | Mandatory security awareness training for all PHI handlers, including hybrid cloud risks. |
| **Administrative — Security Incident Procedures** | IR plans don't account for hybrid cloud risks. | Update IR plans for hybrid scenarios; test annually. |
| **Physical — Facility Access Controls** | Cloud provider ensures data center security. | No gap — provider handles physical security for cloud infrastructure. |
| **Technical — Access Control** | MFA not enforced; no periodic access reviews. | Enable MFA for all accounts; enforce strict access controls; automate access reviews. |
| **Technical — Audit Controls** | Logs not integrated into a centralized system. | Integrate hybrid logs into a SIEM; capture all access attempts and administrative actions. |
| **Technical — Integrity** | No integrity monitoring mentioned. | Implement integrity monitoring to detect unauthorized changes to PHI. |
| **Technical — Transmission Security** | Data in transit is encrypted. | No gap — encryption is properly configured for data in transit. |
| **Technical — Person or Entity Authentication** | No MFA. | Implement MFA to verify user identity — a critical gap. |

---

## 4. Recommendations for Improvement

Grouped by priority. Each notes what to do, why it matters, and how it helps with HIPAA.

### 🔴 High Priority

1. **Enforce Multi-Factor Authentication (MFA) for all users.** Mitigates credential theft and unauthorized access to PHI.
2. **Centralize key management and encrypt all backups.** Improves key governance and protects backup data from exposure.
3. **Implement periodic access reviews and automated user lifecycle management.** Removes unnecessary privileges and disables inactive accounts promptly.
4. **Establish centralized logging with automated alerting (SIEM).** Improves visibility, event correlation, and incident response.
5. **Verify/execute a signed Business Associate Agreement (BAA) with the cloud provider.** Contractually defines responsibilities for PHI protection.
6. **Maintain ongoing security awareness training.** Reinforces phishing awareness, secure cloud usage, and proper PHI handling.

### 🟠 Medium Priority

7. **Upgrade on-premises firewalls and refine network security controls.** Enables deep packet inspection and stronger segmentation.
8. **Establish a vulnerability management program.** Run regular scans (at least monthly) and remediate findings.
9. **Formalize and test disaster recovery procedures.** Define RTO/RPO, encrypt backups, and perform annual recovery testing.
10. **Enhance monitoring with file integrity monitoring.** Detects unauthorized modification of PHI and critical system files.

---

## Conclusion

Implementing these recommendations strengthens the **confidentiality, integrity, and availability (CIA)** of PHI, reduces cyber risk across the hybrid cloud environment, and improves alignment with the **HIPAA Security Rule's Administrative, Physical, and Technical Safeguards**. Together with the shared-responsibility analysis and HIPAA comparison, these improvements move the organization from "meeting the minimum" toward a resilient, defensible security posture.

---

## Skills Demonstrated

- Cloud security assessment of a hybrid (on-prem + cloud) environment
- Application of the **Shared Responsibility Model**
- **HIPAA Security Rule** mapping (Administrative / Physical / Technical safeguards)
- Gap analysis and control evaluation against industry best practices
- Risk-based, prioritized remediation planning
- Security reporting and technical communication

---

## Repository Structure

```
cloud-security-architecture-hipaa-assessment/
├── README.md                         # This report
├── docs/
│   ├── Cloud_Shared_Responsibility_and_Compliance_Report.pdf
│   └── Cloud_Shared_Responsibility_and_Compliance_Report.docx
└── assets/
    └── (optional diagrams / screenshots)
```

---

## Disclaimer

This assessment was completed as part of a cybersecurity course lab (Cloud Security Architecture). It is based on a **fictional scenario** and is intended to demonstrate analytical and reporting skills. It does not represent a real organization or actual PHI.

---

**Author:** Eugen Nyongesa
**Date:** June 22, 2026
