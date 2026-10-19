# GRC105 Incident Response Plan (IRP) Assignment

- **Student name:** Esther Adeleke
- **Student ID:** 2025/GCR/12667
- **Official email:** gcr2512667@students.icdfa.edu.ng
- **Course / module:** GRC105 — Incident Management and Business Continuity
- **Word count:** 8059

---

# Executive summary

At 08:45 WAT on Day 0, Northbridge Skills Institute should declare a Critical cybersecurity incident involving confirmed ransomware indicators on FIN-PC07 and FS01, with material disruption to the learning portal through its FS01 dependency (E06–E08). The preceding suspicious cloud upload (E04), unusual privileged logons by svc_backup (E03), and unfamiliar Microsoft 365 sign-in (E02) create a credible possibility of account compromise and data exfiltration, but do not prove either. The incident must therefore be managed as ransomware with a suspected data-security incident until evidence narrows the scope.

Immediate priorities are to stop encryption and lateral movement, protect BAK01 from further access, preserve volatile and audit evidence, secure Microsoft 365 and privileged credentials, and give learners a truthful examination update before the scheduled 11:00 examination. The latest network backup is not a safe recovery point merely because a job reported success (E05): backups are not immutable, the backup account has administrator access to FS01 and BAK01, and no restoration test has occurred in 90 days. The disconnected backup is older, from Friday Day −3 at 17:00, and also requires validation.

This report uses the course teaching model of preparation; detection and analysis; containment, eradication and recovery; and post-incident activity. It uses NIST SP 800-61 Rev. 3 in its CSF 2.0 framing: Govern, Identify and Protect support preparation, while Detect, Respond and Recover form incident response; improvement feeds back into all functions (Nelson et al., 2025). Proposed evidence collection and exercise outcomes are explicitly labelled as planned or simulated.

# Task 1 Incident classification and initial assessment

## 1.1 Evidence-linked timeline

| Time (WAT) | Evidence | Confirmed observation | Working hypothesis/limitation | Validation needed |
|---|---|---|---|---|
| Day −1, 16:40 | E01 | Finance officer reported opening an unexpected payroll attachment on FIN-PC07; mail record exists. | Possible phishing initial access. Malicious code execution is not verified; opening an attachment alone does not prove infection. | Email headers and attachment, endpoint process tree, browser/download history, user interview, EDR telemetry. |
| Day 0, 01:15 | E02 | Microsoft 365 audit records finance officer account sign-in from unfamiliar IP and new device. | Possible credential theft, token theft or unauthorized session. | MFA result, conditional access, sign-in risk, device registration, token/session activity, mailbox audit and user confirmation. |
| Day 0, 01:35 | E03 | svc_backup made remote administrator logons to FS01 and BAK01 outside normal schedule. | Possible attacker use of a privileged service account or legitimate but undocumented maintenance. | Windows Security logs, logon type/source host, task scheduler, service logs, vault records, administrator interview, account activity across systems. |
| Day 0, 01:50 | E04 | Proxy shows 1.2 GB HTTPS upload from FIN-PC07 to unfamiliar cloud storage. | Possible data exfiltration, cloud sync, legitimate transfer or unrelated application traffic; content unknown. | Proxy/DNS/TLS logs, destination ownership, process/network correlation, endpoint artifacts, cloud-provider logs if obtainable, file access audit. |
| Day 0, 02:00 | E05 | Backup console reports successful FS01 and LMS01 jobs. | Job completion is observed, not recoverability or cleanliness. | Backup catalog and snapshot metadata, integrity checks, restore in isolated lab, backup access/audit logs and comparison with known-good records. |
| Day 0, 08:30 | E06 | EDR/helpdesk reports mass file changes on FIN-PC07; .locked files and ransom note on FS01 shares. | Strong ransomware evidence; family and spread mechanism unknown. | Preserve ransom note, sample encrypted files, process/memory and disk images where feasible, EDR timeline, file server logs, malware analysis. |
| Day 0, 08:40 | E07 | LMS login page loads but materials and assessment submissions fail. | Availability failure likely caused by FS01 dependency, but direct LMS compromise is unresolved. | LMS application logs, FS01 mounts/dependency checks, database and web logs, health checks, integrity validation. |
| Day 0, 08:45 | E08 | Incident contact list on FS01 inaccessible; staff phones available; corporate email trust unverified. | Incident is underway and normal coordination tools may be unsafe or unavailable. | Use phone/out-of-band channel; verify alternate contact copy and corporate identity/security status independently. |

## 1.11 Plausible explanations for ambiguous evidence

E02 and E03 may reflect a coordinated intrusion: an attacker steals the finance officer’s credentials or session, uses the workstation as a foothold, then accesses the privileged svc_backup account to reach file and backup systems. The chronology is compatible with this theory but does not prove that the same actor controlled these events. Alternative explanation 1: the finance officer legitimately signed in from a new device or VPN, while svc_backup activity came from scheduled maintenance or an administrator task that was not documented. This must be tested against staff schedules, source hosts, MFA records and change tickets. Alternative explanation 2: the 1.2 GB E04 transfer may be a legitimate cloud backup/synchronization or another application upload rather than exfiltration; destination, process and file-access correlation are needed before calling it stolen data. A further possibility is that ransomware and the upload are related but the upload contained non-personal files, encrypted material or routine data. No evidence supplied establishes attacker identity or proves personal-data theft.

## 1.2 Incident type and severity

| Tier | Operational criteria | Example trigger |
|---|---|---|
| Critical | Active compromise with widespread or mission-critical outage; ransomware or destructive activity affecting core services; privileged/backup compromise plausible; potential serious personal-data exposure; urgent executive command. | Confirmed ransomware plus FS01/LMS disruption and backup trust uncertainty. |
| High | Confirmed security incident with material impact to a business service or sensitive system, but scope is contained and recovery path is reasonably trusted. | One department’s system encrypted; critical services remain available; no evidence of privileged/backup access. |
| Medium | Limited and contained compromise with manageable impact, no material critical-service outage and a clear remediation path. | Single endpoint malware blocked before lateral movement. |
| Low | Unconfirmed alert or minor policy event with no evidence of compromise or meaningful business impact. | Benign attachment or isolated blocked event after validation. |

**Selected level**: CRITICAL (provisional). E06 confirms ransomware indicators on a workstation and shared file service; E07 confirms loss of learner-facing functions before an 11:00 examination for 100 learners. E03 places a highly privileged account on both FS01 and BAK01 outside its normal schedule, and E05 cannot establish backup integrity. E04 introduces a possible confidentiality impact involving 1.2 GB transferred externally. Critical severity is justified by simultaneous availability impact, integrity loss, and material uncertainty around confidentiality and recoverability. Reassess at 10:00 WAT and whenever new evidence materially changes scope. Do not downgrade solely because the portal login page responds.

## 1.3 Asset scope and CIA assessment

| Asset | Status | Confidentiality | Integrity | Availability | Priority |
|---|---|---|---|---|---|
| FIN-PC07 | Confirmed affected: mass file changes; attachment opened; 1.2 GB outbound transfer (E01, E04, E06). | Potentially high risk: user data, credentials or files may have been exposed; content unknown. | High concern: mass changes and ransomware indicators. | High impact to finance workstation. | P1: isolate, preserve, investigate entry and transfer. |
| FS01 | Confirmed affected: .locked files and ransom note; svc_backup admin logon (E03, E06). | Potential exposure through broad file access; not proven. | Confirmed loss of file integrity/availability for affected shares. | High; portal depends on files stored here. | P1: contain, preserve server evidence, protect shares. |
| LMS01 | Portal front page loads; learning materials/submissions unavailable (E07); backup job reported success (E05). | Unknown; direct compromise not established. | Unknown; dependency failures may mimic corruption. | Confirmed learner-facing functional outage. | P1 business continuity; isolate if evidence indicates direct compromise. |
| BAK01 | Potentially exposed: svc_backup admin logon (E03); network backups non-immutable; restore not tested. | Backup data may be accessible to attacker. | Backup deletion/alteration/poisoning possible, unconfirmed. | Recoverability unknown; disconnected copy exists at Day −3 17:00. | P1: isolate backup plane, disable suspect access, preserve audit logs. |
| Microsoft 365 identity/email | Unfamiliar sign-in for finance officer (E02); email trust unverified (E08). | Potential account/mailbox/token exposure. | Mailbox rules, OAuth grants or identity settings may be altered; unknown. | Identity/email continuity and trusted communication at risk. | P1: review sign-in, revoke sessions/tokens and reset credentials safely. |
| Learner/finance data | Stored across institute systems; 1.2 GB upload has unknown content (E04). | Potential personal-data breach; not confirmed. | Assessment and finance records may be altered/encrypted. | Access disrupted where dependent on FS01/LMS01. | P1 legal/privacy assessment and integrity checks. |

## 1.4 Investigation priorities

| Priority question | Validation check | Findings that changes scope |
|---|---|---|
| 1\. Did FIN-PC07 execute malicious code, and what initiated it? | Acquire email with headers/attachment; EDR timeline, process tree, persistence artefacts, disk and memory capture if feasible; interview finance officer. | Malicious process or credential theft confirms phishing/endpoint entry and guides containment; benign attachment shifts likely entry elsewhere but does not remove confirmed ransomware. |
| 2\. What was uploaded in E04 and was it personal or confidential data? | Correlate proxy destination, DNS, TLS metadata, endpoint process, file access and cloud sync logs; preserve relevant M365 and proxy logs. | Evidence of learner/finance records leaving the environment increases confidentiality severity and may trigger NDPA notification analysis; legitimate sync or non-sensitive content narrows the breach hypothesis. |
| 3\. Was svc_backup abused and were backups altered or accessed? | Review Windows logon source, account authentication, backup audit logs, snapshot metadata, deletion events, privilege changes and administrator change tickets. | Unauthorized use or backup tampering confirms broader compromise and removes network backups from consideration until validated; authorized change with corroboration lowers this hypothesis. |
| 4\. Is there lateral movement or identity compromise beyond listed systems? | Search EDR, Windows event logs, firewall/proxy/DNS, M365 audit, mailbox rules, OAuth app consent, new users, scheduled tasks and remote tools across all assets. | Additional hosts, admin identities, or cloud persistence expands scope and may require wider network isolation and more external incident-response support. |
| 5\. Can any backup be restored cleanly and is the LMS itself intact? | Use a segregated recovery lab; verify snapshot timestamps, malware scan, file-system/database integrity, application dependency tests and known-good baselines. | Unreadable, encrypted or attacker-modified backup extends outage and forces rebuild/manual continuity; verified clean restore allows phased recovery but does not prove no data theft. |

# Task 2: Incident response plan and team responsibilities

## 2.1 Policy and operating plan

**Mission**: protect learners, staff, information and essential education services by detecting, containing, investigating and recovering from cyber incidents in a controlled, evidence-led manner. Objectives are to limit harm, preserve reliable evidence, maintain essential operations, meet legal/contractual duties, restore trusted systems, and turn lessons into improvements.

**Scope**: all Northbridge staff, contractors, endpoints, servers, networks, Microsoft 365 identities, cloud services, backups, learner and finance data, and third parties supporting these assets. The plan is activated for credible ransomware, unauthorized privileged access, suspected data exfiltration, critical service outage, compromise of backup or identity systems, or any event whose impact is uncertain but potentially severe.

**Declaration and activation**: the Incident Response Lead (IR Lead) declares and logs the incident immediately on credible evidence. E06–E08 meet the Critical threshold; declare Critical at 08:45 WAT. The IR Lead starts a bridge by staff phone/out-of-band channel, appoints a scribe, assigns incident ID NBSI-IR-2026-001 (fictional tracking ID), and notifies the executive sponsor, IT/security, business continuity, legal/privacy and communications leads. Corporate email is treated as untrusted until reviewed.

**Authority**: the IR Lead may order immediate isolation of an actively encrypting or spreading host, suspend a suspected compromised account, block a malicious destination, and disconnect a threatened segment where delay would increase harm. For broad outages, destructive changes, external notifications, ransom decisions, or service restoration, obtain executive approval with legal/privacy and business-owner consultation when time permits. No approval is required before emergency isolation to stop ongoing encryption; record the decision, reason, timestamp, evidence preserved or lost, and operational effect. No ransom payment is authorized by this plan.

**Documentation**: maintain a time-stamped incident log; record source and confidence for every claim; preserve original evidence read-only; use unique evidence IDs, SHA-256 hashes after collection, access logs and chain-of-custody signatures; record time zone as WAT; maintain decision, communication, system-change and recovery logs. Store incident records in restricted offline or independently secured storage, not FS01 or the potentially compromised corporate mailbox.

**Escalation**: Critical incidents require immediate executive notification and continuous coordination; High incidents notify management within 30 minutes of validation; Medium/Low incidents follow normal ticketing but escalate if scope expands. Engage external IR/forensics, managed security provider, insurer (if applicable), cloud/M365 provider, legal counsel and law enforcement as advised. Review the plan after any major incident/exercise and at least annually.

**Business continuity connection**: IR controls the threat; business continuity maintains critical functions while disruption continues; disaster recovery restores technology. Activate BC/DR alongside IR because LMS01 depends on FS01. Use an alternate examination arrangement approved by the academic owner rather than restoring an unsafe system to meet the deadline. RTO/RPO are proposed targets pending formal BIA and funding approval.

## 2.2 CSIRT structure

| Role | Core responsibility | Coverage |
|---|---|---|
| Incident Commander / IR Lead | Own incident objectives, severity, decision log, coordination, escalation, and executive situation reports. | Security Analyst 1 acts as IR Lead initially; executive sponsor remains accountable for business risk. |
| Security Analyst / Threat Hunter | Correlate EDR, endpoint, network, Windows, and M365 evidence; map scope and IOCs; recommend containment. | Security Analyst 2 / available analyst; all work documented. |
| Digital Forensics Lead | Plan acquisition, preserve volatile data, image systems, hash and maintain custody; coordinate malware analysis. | External IR/forensics provider on call; internal analyst supports collection. |
| IT Operations Lead | Execute network isolation, account controls, rebuilds, patching, backups and technical validation. | IT Lead; two other IT staff assigned by workload. |
| Business Service Owner / Academic Continuity Lead | Assess learner impact, approve alternate examination process and validate business service readiness. | Head of Academic Operations (fictional role). |
| Legal Counsel / Data Protection Officer | Assess privilege, contractual and statutory duties; direct breach-risk analysis and regulator/data-subject advice. | External counsel/DPO service until formal role appointed. |
| Communications Lead | Prepare approved learner, staff, media and partner updates; monitor rumours and maintain message log. | Communications Manager. |
| Executive Sponsor | Approve risk acceptance, significant expenditure, service return and external notifications where required by governance. | Chief Operating Officer (fictional role). |
| Scribe / Incident Coordinator | Maintain chronology, decisions, action owners, meeting cadence, and evidence index. | Operations administrator using offline template. |

The core team is small, so roles may be combined operationally, but accountability must remain explicit. The forensics and legal roles should be contracted in advance with 24/7 escalation contacts. During this incident, a scribe should be appointed from operations and request temporary support from the M365 provider and a qualified external incident-response firm. Do not let the same person perform a sensitive evidence acquisition and independently approve its integrity where avoidable.

## 2.3 RACI matrix

R = Responsible (does the work); A = Accountable (one role answerable for outcome); C = Consulted (provides input before decision); I = Informed (kept updated). Each activity has exactly one A and at least one R.

| Activity | Responsible (R) | Accountable (A) | Consulted (C) | Informed (I) |
|---|---|---|---|---|
| Detect and declare incident | Security Analyst | IR Lead | IT Lead; service owner | Executive sponsor; CSIRT |
| Analyze scope and severity | Security Analyst; Forensics Lead | IR Lead | IT Lead; Legal/Privacy | Executive sponsor; service owner |
| Contain endpoints, network and accounts | IT Lead; Security Analyst | IR Lead | Forensics Lead; M365 provider | Executive sponsor; service owner |
| Collect and preserve evidence | Forensics Lead | IR Lead | Legal Counsel; Security Analyst | Executive sponsor |
| Stakeholder communications | Communications Lead | Executive sponsor | Legal/Privacy; service owner; IR Lead | Learners; staff; partners as approved |
| Assess reporting obligations | Legal/Privacy | Executive sponsor | IR Lead; Communications Lead | Relevant regulators/data subjects as advised |
| Select and validate backup / restore | IT Lead; Forensics Lead | IT Lead | Security Analyst; service owner; IR Lead | Executive sponsor; Communications Lead |
| Approve service return | IT Lead; Security Analyst | Executive sponsor | Service owner; Legal/Privacy; IR Lead | Learners; staff |
| Close incident and review | Scribe; Security Analyst | IR Lead | All CSIRT roles; Legal/Privacy; service owner | Executive sponsor; staff |

## 2.4 Readiness improvement checklist

| Improvement | Owner | Resources | Readiness Check |
|---|---|---|---|
| Maintain offline incident contacts and printed quick-start plan | Operations Manager | Encrypted offline copy, printed sealed copy, phone tree, quarterly owner review. | Quarterly call-tree test reaches 90% of primary/alternate contacts within 15 minutes; test evidence retained. |
| Create immutable/offline backup tier with separate credentials | IT Lead | Budget for immutable object lock or offline media, separate admin identity, MFA, network segmentation. | Monthly audit shows production credentials cannot delete protected copies; quarterly restore report passes. |
| Test FS01/LMS01 restore and define RTO/RPO | BC/DR Coordinator + service owner | Isolated test environment, application owner, representative datasets, time budget. | Quarterly restoration test verifies application dependencies, file integrity and actual RTO/RPO. |
| Complete server EDR/logging coverage and centralize logs | Security Lead | Server EDR licences, SIEM/log storage, retention, alert rules. | 100% critical servers enrolled; alerts tested for mass file change, privileged logon and backup deletion. |
| Harden privileged/service accounts | Identity Administrator | Vault, gMSA or equivalent managed service identity, least privilege, MFA where supported, rotation plan. | No shared admin credentials; svc_backup cannot interactively log on; quarterly access review signed. |
| Prepare out-of-band communications and templates | Communications Lead | Phone bridge, SMS tree, alternate collaboration channel, pre-approved holding statements. | Quarterly exercise launches bridge without corporate email in under 10 minutes. |
| Run role-specific tabletop and phishing exercises | HR + IR Lead | Two hours quarterly, facilitator, scenarios, attendance records. | At least 90% role attendance; action log assigns owners and due dates; overdue actions escalated. |

# Task 3 — Response playbook and evidence preservation

## 3.1 Ordered response playbook

The first 60 minutes run from 08:45 to 09:45 WAT. The playbook is deliberately parallelized: one team member contains while another preserves evidence and the business owner arranges continuity. Approval means the role authorized under the policy; emergency isolation is pre-authorized.

| Time (WAT) | Action | Owner | Approval | Evidence to capture | Success check |
|---|---|---|---|---|---|
| 08:45–08:50 | Declare Critical incident; assign ID; open offline log; phone executive sponsor and core CSIRT; treat email as untrusted. | IR Lead + scribe | IR Lead; no delay for emergency declaration | E01–E08 references; initial situation report; call log; incident log. | Critical status and owners recorded; bridge confirmed. |
| 08:50–08:55 | Isolate FIN-PC07 from wired/Wi-Fi network using switch/EDR quarantine; do not power off if network isolation works. | IT Lead | IR Lead pre-authorized | EDR alert/export, host name, isolation time, operator and effect. | Network telemetry shows host disconnected; user notified by phone. |
| 08:50–09:00 | Isolate FS01 or affected shares/network segment to stop active encryption; preserve business dependency map; do not leave writable shares exposed. | IT Lead | IR Lead; executive informed promptly | Server logs, share list, console screenshots/export, change record. | No further mass changes in monitored window; any outage documented. |
| 08:55–09:05 | Protect BAK01: remove from production network or block inbound admin access; suspend suspect svc_backup use after preserving available authentication logs; use separate clean admin identity. | IT Lead + Security Analyst | IR Lead; urgent containment authority | Backup audit logs, account state, firewall changes, service dependencies. | No production host can write to backup repository; break-glass access verified. |
| 08:55–09:10 | Review M365 sign-in; revoke finance officer sessions/tokens if compromise is credible; reset credentials from clean device; check MFA, mailbox rules, OAuth grants, new devices and admin role changes. | Identity/M365 admin | IR Lead; legal consulted if needed | Sign-in and audit exports before changes where feasible; account action log. | Sessions revoked, credentials reset, MFA validated, suspicious rules/apps triaged. |
| 09:00–09:15 | Preserve volatile and high-value evidence from FIN-PC07 and FS01 where safe: memory capture first if feasible, then forensic images/log exports; retain ransom note and encrypted samples. | Forensics Lead / external provider | IR Lead; emergency containment takes precedence | Memory/disk acquisition logs, original files, hashes after collection, tool/version, operator and times. | Evidence IDs assigned; originals read-only; any missed volatile evidence recorded. |
| 09:05–09:20 | Block confirmed malicious destination/IP/domain only after validating indicators; inspect proxy, DNS, firewall and endpoint connections for related transfer activity. | Security Analyst + network admin | IR Lead | Proxy/firewall/DNS exports, rule changes, indicator confidence and source. | Block tested; monitoring shows no repeated connection; avoid broad block without evidence. |
| 09:10–09:25 | Open an offline scope worksheet; hunt for mass file changes, remote logons, new admin accounts, scheduled tasks, remote tools, backup deletions and unusual M365 events across all assets. | Security Analyst | IR Lead | Search queries, log export ranges, host list, negative/positive findings. | All known assets checked or explicitly marked not yet checked; scope gaps assigned. |
| 09:15–09:30 | Activate business continuity: decide exam contingency (postpone, reschedule or secure alternate delivery) with academic owner; issue holding notice before 10:00. | Academic Service Owner + Comms Lead | Executive sponsor approves public wording; academic owner approves exam decision | Decision log, approval, recipient list, sent copy and timestamp. | Learners receive actionable notice; no promise of 11:00 access until validated. |
| 09:20–09:35 | Engage external IR/forensics, M365 provider and backup vendor; request preservation of relevant cloud/service logs and support case IDs. | IR Lead | Executive sponsor for expenditure if required | Vendor ticket IDs, requests, responses, contact log. | Acknowledgements and ETA logged; missing access escalated. |
| 09:30–09:45 | Issue first executive situation report; establish update cadence every 30 minutes; set next severity review and legal/privacy triage meeting. | IR Lead + scribe | Executive sponsor receives; legal/privacy consulted | Situation report version, decision/actions register. | Open risks, unknowns, decisions and owners visible to all responders. |
| 09:45–10:30 | Continue threat hunting and assess scope across Microsoft 365, endpoint fleet, server logs and network telemetry; collect evidence before destructive cleanup where feasible. | Security Analyst + Forensics | IR Lead | IOC sweep results, affected-host inventory, log retention requests. | Scope report distinguishes confirmed, suspected and not observed. |
| 10:00–11:00 | Assess backup candidates in isolated lab; do not restore to production. Start with disconnected Friday copy only after malware, integrity, access-history and catalog checks; test latest network backup separately as untrusted. | IT Lead + Forensics | IR Lead; business owner consulted | Backup metadata, read-only copy, test logs, validation report. | Candidate is accepted/rejected with documented criteria; no unverified backup reaches production. |
| 11:00–14:00 | Perform longer-term containment: segment finance, server, identity/admin and recovery networks; disable unused remote access; rotate privileged credentials from clean systems; remove compromised sessions and review service-account scope. | IT Lead + Identity admin | IR Lead; executive sponsor informed | Network diagrams, config diff, access review, rotation/change records. | Attack paths blocked; svc_backup least privilege restored only after compromise assessed. |
| 14:00–18:00 | Determine likely entry point and persistence; analyze attachment, process chain, scheduled tasks, services, registry autoruns, WMI, remote tools, mailbox rules, OAuth grants and new identities; patch exploited weaknesses. | Forensics + Security Analyst | IR Lead | Forensic report, malware indicators, patch records, persistence checklist. | Root-cause confidence stated; persistence hunt covers endpoints, servers and cloud identity. |
| 18:00–22:00 | Build clean recovery environment; rebuild compromised systems from known-good images rather than trusting in-place cleanup; restore validated data to isolated network; test LMS dependency on FS01. | IT Lead | IR Lead and business owner; executive sponsor approves major spend | Build logs, image provenance, restore logs, data validation results. | Security checks pass; no direct production connection until acceptance gate. |
| 22:00–08:45 Day +1 | Run enhanced monitoring; check EDR/Windows/M365/proxy/backup logs for renewed encryption, suspicious logons, exfiltration or persistence; complete initial privacy risk assessment and notification decision. | Security Analyst + Legal/Privacy | Executive sponsor | Monitoring snapshots, incident log, privacy assessment, regulator decision record. | No unexplained recurrence during defined observation window; legal deadlines tracked. |
| Before service return | Service owner validates critical workflows, access controls, records and learner submission path; IR Lead confirms containment; executive sponsor authorizes phased return. | Service owner + IT Lead | Executive sponsor; IR Lead and Legal/Privacy consulted | Acceptance checklist, test results, rollback plan, approval timestamp. | Written approval; communications lead issues confirmed status. |
| Within 14 days of closure | Hold blameless review, reconstruct timeline, quantify impact, assess root cause and assign corrective actions; review plan and BC/DR dependencies. | IR Lead + scribe | Executive sponsor | Final report, lessons learned, action register, updated plan. | Every action has owner, due date, metric and closure evidence. |

## 3.2 Difficult decisions and trade-offs

Containment versus evidence: active encryption and lateral movement can destroy more data than a rapid containment action. The default is network isolation rather than shutdown because isolation may preserve volatile memory and running-process evidence. Capture memory and key logs first only if this can be done immediately and safely; do not delay isolation while ransomware continues. If the host cannot be disconnected or spread is imminent, power it down as a last resort, recording who authorized it, the time, why isolation failed, likely loss of volatile evidence, and resulting operational impact. This follows CISA’s guidance to isolate impacted systems immediately and recognizes that powering off can destroy volatile artifacts (CISA et al., 2023).

Examination access versus backup safety: do not restore the latest network backup directly to production merely to meet 11:00. E03 makes BAK01 potentially exposed; E05 reports job success only, while the backups are non-immutable and untested for 90 days. A short exam postponement is preferable to reintroducing malware or overwriting evidence. The academic owner should decide an alternative exam arrangement and communicate it promptly. A controlled, read-only copy of a candidate backup can be tested in an isolated recovery VLAN; only a validated clean build may progress through a service-return gate.

## 3.3 Evidence register and custody

All rows below are proposed collections, not completed acquisitions. Date/time is recorded relative to scenario Day 0 because no calendar date is provided. The collector records the actual timestamp in WAT at collection and never fills in a hash until it has been computed.

| Record ID | Source | Proposed collection | Collector role | Date & Time (WAT) | Integrity check | Secure storage | Custody |
|---|---|---|---|---|---|---|---|
| EV-01 | FIN-PC07 endpoint, email/attachment, EDR telemetry; linked to E01/E04/E06 | Planned: preserve original email with headers and attachment; capture EDR export, volatile memory if feasible, forensic disk image; isolate first if encryption active. | Forensics Lead; external specialist if available | Day 0, planned after 08:45 WAT; exact collection time to be entered at acquisition. | SHA-256 of acquired image/export and original attachment; hash not yet available. | Restricted evidence vault on clean encrypted media, separate from FS01/BAK01; read-only master plus working copy. | Collector → evidence custodian; each transfer signed/time-stamped; not yet collected. |
| EV-02 | FS01 file server, .locked files, ransom note, Windows/share logs; E03/E06 | Planned: export security/system logs, preserve ransom note and representative encrypted files, memory/disk image if safe; record share paths and file timestamps. | Forensics Lead + IT Lead | Day 0, planned; exact WAT time pending. | Hash each collected file/image using SHA-256; no hash claimed yet. | Restricted evidence vault; encrypted storage; access by IR Lead, forensics and legal-approved staff only. | IT operator to forensic collector, then custodian; signatures and transfer times pending. |
| EV-03 | BAK01 backup console/catalog, snapshots and access logs; E03/E05 | Planned: export audit/config/catalog before changing retention; preserve snapshot metadata; acquire read-only candidate copy for isolated validation. | IT Lead with Forensics Lead oversight | Day 0, planned after isolation; exact WAT time pending. | Hash export and copied backup manifest; verify copy consistency; results pending. | Separate clean recovery repository; never place only copy on suspect BAK01. | Backup admin to forensic custodian; all transfers logged; not yet collected. |
| EV-04 | Microsoft 365 sign-in, audit, mailbox rules, OAuth and device records; E02/E08 | Planned: export sign-in and audit logs, preserve mailbox metadata and relevant messages, record session/token revocation and account changes. | Identity Administrator / Security Analyst | Day 0, planned; exact WAT time pending. | Hash exported files after download; capture export query, tenant, time range and tool details. | Encrypted evidence vault with restricted identity/privacy access; do not circulate sensitive logs by email. | Identity admin to evidence custodian; transfer log pending. |
| EV-05 | Proxy, DNS, firewall and destination metadata for 1.2 GB transfer; E04 | Planned: export relevant flow/proxy records, timestamps, destination hostname/IP, process correlation and any cloud provider request/response. | Security Analyst / network admin | Day 0, planned; exact WAT time pending. | SHA-256 of exports; document clock offsets and retention gaps; values pending. | Restricted evidence vault; redacted working copies for broader team. | Network admin to evidence custodian; pending. |

Access restrictions: use least privilege, multifactor authentication and an access register; preserve a read-only master and analyze working copies. The evidence custodian logs evidence ID, description, collector, source system, acquisition tool/version, timestamps, hash, storage location, access, transfer reason, sender/receiver signatures and seal status. Retain evidence under legal hold where advised. Never put evidence in public cloud storage or the possibly compromised corporate email. If a log is unavailable, document the source, reason, retention gap and effect on confidence; do not imply absence of evidence proves absence of activity.

## 3.4 Eradication, credentials and broader compromise

- **Entry point**: inspect the payroll attachment and message headers; validate execution and exploit path; remove malicious email from mailboxes if safe, block sender/domain/attachment indicators, harden attachment sandboxing and macro/script controls, and patch the exploited application or OS. Do not conclude phishing was the root cause until artifacts support it.

- **Persistence**: examine startup folders, registry autoruns, scheduled tasks, services, WMI subscriptions, remote management tools, new accounts, local/domain admin group changes, web shells, cloud app grants, mailbox rules and identity federation changes. Rebuild compromised endpoints and servers from known-good images where possible rather than relying on uncertain cleanup.

- **Credentials**: from a clean admin workstation, disable or rotate compromised user and administrator credentials, revoke M365 refresh tokens/sessions, inspect MFA methods and app consents, remove unauthorized devices, enforce phishing-resistant MFA where supported, and reset privileged credentials after containment. Replace svc_backup with a managed identity or unique vaulted credential, restrict it to necessary backup operations, deny interactive logon and separate backup administration from production administration.

- **Broader compromise**: correlate EDR, Windows, firewall, DNS/proxy, VPN/remote-access, M365 audit, backup console and identity logs from before E01 through containment. Hunt for shared indicators, lateral movement, privilege escalation, data staging, unusual archive tools, outbound volume spikes, deletion attempts and new persistence. Because server EDR coverage is incomplete, absence of an alert is not proof of a clean server.

## 3.5 Recovery order and acceptance criteria

| Tier | Proposed target | Recovery approach |
|---|---|---|
| 0 — Control plane / trusted administration | Begin immediately; no production RTO until identity trust is restored. | Create clean admin devices and recovery network; secure M365 admin identities; confirm DNS, network, time and logging; independent credentials and MFA. |
| 1 — Backup integrity and evidence | Validate candidates before any restore; do not use a fixed deadline as a substitute for proof. | Protect disconnected Day −3 17:00 copy; inspect chain/access history, catalog, malware and integrity; test restore in isolated lab. Treat latest network backup as untrusted until proven clean. |
| 2 — FS01 data and core file service | Proposed RTO 24–48 hours after clean environment is ready; proposed RPO up to 3 days only if business owner accepts it. | Rebuild from clean image; restore files from validated point; scan; compare hashes/manifests where available; validate permissions, timestamps and representative business records. |
| 3 — LMS01 / examination function | Target decision on exam contingency before 10:00; restore target 24–48 hours if safe, otherwise reschedule/alternate delivery. | Restore application only after FS01 dependency is stable; test login, course materials, submissions, grade/assessment integrity and access from test learner accounts. |
| 4 — Finance workstation and finance data | Target 48–72 hours, depending on forensic preservation and data validation. | Reimage FIN-PC07, patch, enroll in EDR, validate finance records against known-good ledger/exports and review any suspected transfer. |

These are proposed operational targets, not established institutional commitments. RTO is maximum tolerable downtime for a service; RPO is acceptable data loss measured in time. A disconnected backup from Day −3 implies up to roughly three days of data loss relative to Day 0 if it is the only clean copy, but the actual recoverable point must be confirmed. The service owner and executive sponsor must explicitly accept or reject that loss after reconciling FS01/LMS01 data with other trusted sources.

Backup selection and validation: preserve the original backup; review backup administrator logs and snapshot metadata; select the newest candidate with a credible pre-compromise point and clean access history; copy it to a segregated recovery environment; verify catalog/manifest integrity and scan for known indicators; restore representative files and the full application dependency chain; validate file counts, hashes where known, database consistency, permissions, LMS submission and finance-record samples; run EDR and vulnerability scans; monitor for recurrence for at least 24 hours before broad service return. If the latest backup fails or is suspect, reject it and test the disconnected copy. Business owner signs acceptance, IR Lead confirms containment, IT Lead signs technical validation, and executive sponsor approves return. A rollback path remains available.

# Task 4 — Communication and escalation

## 4.1 Communication matrix

| Stake holder | Trigger | Message owner | Approver | Channel | Frequency |
|---|---|---|---|---|---|
| Executive sponsor / board | Critical declaration; every material scope change; restoration decision; legal deadline. | IR Lead | Executive sponsor for external statements; IR Lead for internal situation report. | Phone bridge, SMS, sealed offline brief; not corporate email until trusted. | Every 30 minutes for first 4 hours, then hourly or on material change. |
| IT and security staff | Immediate incident activation, isolation orders, scope and change approvals. | IR Lead | IR Lead | Phone/SMS and approved out-of-band bridge. | Continuous bridge; written shift handover every 2 hours. |
| Learners / affected cohort | Portal/materials unavailable; examination decision; safety and account advice if needed. | Communications Lead | Academic Service Owner + Legal/Privacy; executive sponsor for public message. | SMS to registered contacts, official website status page from clean hosting, phone tree; email only if trusted. | Initial notice before 10:00; updates at least hourly until exam decision, then on material change. |
| Teaching staff / learner support | Exam continuity instructions, support script, do-not-speculate guidance. | Academic Service Owner | Communications Lead | Phone/SMS, staff phone tree, alternate collaboration tool. | Every 30–60 minutes during exam window. |
| Microsoft 365 / cloud provider | Suspicious sign-in, request log preservation, identity containment assistance. | IT Lead | IR Lead | Provider support portal from clean device or phone support; preserve ticket ID. | Initial request immediately; every 2 hours until logs secured. |
| Backup / infrastructure provider | BAK01 access anomaly, snapshot preservation, safe recovery support. | IT Lead | IR Lead | Verified support number and provider portal from clean device. | Immediate; updates every 2 hours. |
| Legal counsel / DPO / privacy adviser | Potential personal-data exposure, E04 transfer, evidence preservation, deadlines. | IR Lead | Executive sponsor instructs engagement; counsel directs legal assessment. | Phone call and encrypted channel; avoid untrusted email. | Immediate triage; at least every 2 hours until reporting decision. |
| Regulator / law enforcement (if advised/required) | Legal threshold met or preservation/investigation support needed. | Legal/Privacy | Executive sponsor, based on counsel advice. | Official secure reporting channel / verified phone contact. | Within applicable deadline; record submission and receipt. |

## 4.2 Executive briefing

At 08:45 WAT, Northbridge Skills Institute has declared a Critical cyber incident. The evidence confirms mass file changes on FIN-PC07 and .locked files with a ransom note on FS01; learners cannot access materials or submit assessments through the portal, which depends on FS01 (E06–E08). A finance account had an unfamiliar Microsoft 365 sign-in (E02), svc_backup made unusual administrator logons to FS01 and BAK01 (E03), and FIN-PC07 uploaded 1.2 GB to unfamiliar cloud storage (E04). These events raise a serious concern about account compromise, backup integrity and possible data exfiltration. They do not yet prove attacker identity or that personal data was stolen. A successful backup job report does not establish a clean or recoverable backup (E05). Immediate actions are to isolate affected systems, protect BAK01, secure identity sessions, preserve logs and engage external forensics. The 11:00 examination must not be promised on an unverified system; approval is requested for an alternate examination arrangement and urgent external response support. Legal/privacy advisers are assessing notification duties. The next executive update is scheduled for 09:15 WAT, then every 30 minutes.

## 4.3 Learner notice

Northbridge Skills Institute is investigating a serious IT incident that is affecting access to learning materials and assessment submissions. The learning portal may load, but some functions are currently unavailable. We are taking steps to protect systems and learner information while we assess the impact. The 11:00 examination is **not confirmed to proceed as scheduled**. Please do not repeatedly submit answers or upload sensitive documents while the portal is unstable. Keep your course materials and any completed work saved locally where possible, and monitor SMS messages and the institute’s official status page for the confirmed examination arrangement. Do not respond to unexpected messages asking for passwords, payment or personal information, and do not forward unverified claims about the incident. If you need urgent assistance, contact the learner support line at +234 800 000 0101 (fictional). Our next update will be sent by 10:00 WAT.

## 4.4 Legal/privacy assessment and escalation

Legal/privacy advisers should establish the facts without treating the 1.2 GB upload as proof of exfiltration. They should identify the data controller and any processors, data categories and approximate numbers of people/records potentially involved, whether the data was accessed or disclosed, likely consequences, safeguards such as encryption, and mitigation already taken. They should preserve evidence and document the rationale for notifying or not notifying. Under section 40(2) of Nigeria’s Data Protection Act 2023, a controller must notify the Nigeria Data Protection Commission within 72 hours of becoming aware of a personal-data breach likely to result in risk to individuals’ rights and freedoms; section 40(3) addresses immediate communication to data subjects where the breach is likely to create high risk (Federal Republic of Nigeria, 2023). Counsel must confirm applicability, when awareness occurred, the relevant threshold, content, recipient and deadline. The 72-hour period is not a blanket deadline for every operational incident.

An operational service notice explains outage, examination arrangements and practical steps; it is not a formal breach notification. Formal regulator/data-subject communications should be based on a documented legal assessment and include known facts, uncertainties, likely consequences, contact point and mitigation. If a deadline could expire before all facts are known, counsel should determine whether an initial notification based on available information is required and how to supplement it. Any other jurisdictions, education-sector rules, contracts, insurer terms or law-enforcement considerations must be checked rather than assumed.

# Task 5 — Tabletop exercise and improvement plan

## 5.1 Exercise design

Simulation label: the following is a tabletop walkthrough, not a live incident and not evidence that any technical action was executed. Duration is 60 minutes. The facilitator provides injects; each participant responds in role; the scribe records decisions and gaps. No production changes are made.

- Objectives: (1) activate the plan and communicate without corporate email; (2) contain active ransomware while preserving evidence; (3) make a defensible examination/recovery decision with uncertain backups; (4) assess possible data exfiltration and privacy reporting; (5) maintain consistent public messaging and track corrective actions.

- Participants: Incident Commander/IR Lead; Security Analyst; IT Operations Lead; Forensics Lead; Academic Service Owner; Legal/Privacy Adviser; Communications Lead; Executive Sponsor; scribe/facilitator.

- Agenda: minutes 0–10 initial report and declaration; 10–20 exam pressure; 20–35 extortion claim and evidence review; 35–45 backup failure and recovery choice; 45–55 misinformation/media response; 55–60 hotwash and action assignment.

- Success criteria: (1) declare severity and notify executive sponsor within 5 simulated minutes of E06–E08; (2) agree isolation of FIN-PC07/FS01 and protection of BAK01 within 10 minutes, recording evidence trade-offs; (3) do not restore an unverified backup to production; (4) produce an approved learner status update within 10 minutes of the examination decision; (5) document at least five decisions with owner, rationale and communications; (6) legal/privacy owner records a threshold-based reporting decision and deadline owner.

## 5.2 Simulated injects and decision log

| Inject | Simulated decision | Decision owner | Rationale | Communication | Effect |
|---|---|---|---|---|---|
| Minute 0 — initial facts E01–E08 | Declare Critical; isolate FIN-PC07 and affected FS01 shares; protect BAK01; open offline phone bridge and incident log. | IR Lead | Ransomware and portal impact are confirmed; privileged access and backup compromise are plausible. Containment cannot wait, but preserve logs and memory where feasible. | Phone bridge to CSIRT; executive alert; staff told not to use email for sensitive incident details. | Stops likely spread; portal remains unavailable and examination is at risk. |
| Minute 10 — management demands portal reopening before 11:00 even if latest backup unverified | Decline direct production restoration. Ask academic owner to approve postponement/alternate assessment and authorize isolated restore testing only. | IR Lead recommends; Academic Service Owner decides exam; Executive Sponsor approves risk and resources | E03/E05 mean the network backup is untrusted; reintroducing malware could expand outage and corrupt learner records. Business continuity is not the same as unsafe system restoration. | Immediate executive call; learner holding notice by SMS/status page; next update time stated. | May delay exam but avoids trading a short disruption for reinfection and loss of evidence. |
| Minute 20 — attacker claims all 1,800 learner records and threatens publication in 12 hours; screenshot authenticity unknown | Treat as an unverified extortion claim; preserve screenshot/message metadata, avoid public confirmation, expand exfiltration and data-access hunt, alert legal/privacy and executive sponsor. | IR Lead; Security Analyst investigates; Legal/Privacy assesses | Threat actors may exaggerate; screenshot could be fabricated, old, partial or genuine. E04 requires investigation but does not prove content. | Out-of-band legal briefing; communications holding line says investigation is ongoing and facts are not confirmed. | Scope remains potential breach; reporting assessment starts without repeating attacker’s claim as fact. |
| Minute 35 — test restore of latest network backup fails: unreadable snapshot error | Reject that snapshot; preserve error logs and metadata; keep backup repository isolated; test disconnected Friday copy in a segregated environment and consider clean rebuild/manual recovery. | IT Lead accountable for technical restore; IR Lead coordinates | A failed restore disproves readiness of that candidate, not necessarily all backups. Latest copy may be damaged or tampered with; no production connection. | Update executive and academic owner; explain revised recovery estimate; record provider case. | Recovery becomes slower; restore target revised and contingency plan maintained. |
| Minute 45 — social media says exam cancelled; journalist asks whether learner data exposed | Publish a verified status correction: exam status will be confirmed by a specified time; do not assert cancellation or deny exposure until approved. Route media to Communications Lead; counsel reviews privacy wording. | Communications Lead drafts; Executive Sponsor approves; Academic Service Owner confirms exam status | Uncontrolled speculation damages trust. Avoid definitive claims unsupported by evidence; provide useful learner action and next update. | Clean official status page and SMS; one spokesperson; staff phone tree. | Reduces misinformation and inconsistent statements while preserving uncertainty. |
| Additional inject, Minute 50 — M365 audit reveals a new OAuth app consent and mailbox forwarding rule after the unfamiliar sign-in | Treat identity scope as expanded; preserve audit details, revoke suspicious grant/rule and sessions after evidence export; hunt for additional app grants and privileged role changes. | Identity Administrator executes; Security Analyst scopes; IR Lead accountable | New cloud persistence is plausible; it may enable continued access independent of endpoint isolation. Recheck credentials from clean device. | Urgent phone update to IR Lead and Legal/Privacy; provider support ticket; staff advised of trusted contact route. | Expands investigation to identity plane and delays return until cloud persistence and sessions are validated. |

## 5.3 Simulated evaluation

| Success criterion | Simulated result | Assessment |
|---|---|---|
| Critical declaration and executive notification within 5 minutes | Achieved in walkthrough: Critical declared at Minute 0 and sponsor called. | Pass; operationalize with call-tree timestamps. |
| Containment and BAK01 protection within 10 minutes | Decision made at Minute 0–10; actual technical completion is not tested in tabletop. | Partial; require a technical walkthrough to verify switch/EDR and backup-plane access. |
| No unverified restore to production | Achieved; latest backup rejected after unreadable snapshot inject. | Pass; formalize mandatory restore gate and documented exception prohibition. |
| Approved learner update within 10 minutes of exam decision | Draft approved message and SMS/status-page route specified; exercise does not test delivery. | Partial; test contact list, delivery receipts and alternate website access. |
| At least five documented decisions | Six decisions recorded in this log. | Pass for exercise documentation. |
| Legal/privacy threshold and deadline owner recorded | Legal/privacy assessment initiated; exact factual threshold remains unresolved pending evidence. | Partial; create a breach decision worksheet and named deputy. |

## 5.4 Improvement action plan

| Priority | Improvement | Owner | Target completion | Measurable completion |
|---|---|---|---|---|
| P1 | Create and test an offline contact tree and incident bridge; verify non-email escalation. | Operations Manager | 7 days | 90% of primary/alternate contacts acknowledge test call within 15 minutes; signed test report. |
| P1 | Isolate and validate backup recovery; implement separate protected copy and restore testing. | IT Lead | 30 days | One successful isolated FS01/LMS01 restore; report records RTO/RPO, integrity checks and account separation. |
| P1 | Expand server EDR and central log coverage to FS01, LMS01 and BAK01. | Security Lead | 45 days | 100% critical server coverage; test alerts for mass file change, admin logon and backup deletion. |
| P1 | Implement privileged account hardening for svc_backup and separate backup admin plane. | Identity Administrator | 14 days | Interactive logon denied; least-privilege access review approved; credential rotation and audit evidence. |
| P2 | Create exam continuity procedure with postponement authority, alternate assessment options and learner templates. | Academic Service Owner | 21 days | Approved runbook; tabletop demonstrates learner notice within 10 minutes and no unverified restore. |
| P2 | Prepare a data-breach assessment form and legal escalation roster. | Legal/Privacy Adviser | 14 days | Template includes data categories, affected individuals, risk, awareness time, statutory deadlines and decision sign-off. |
| P2 | Run technical walkthrough of isolation, evidence capture, and restore gate. | IR Lead + Forensics Lead | 30 days | Exercise report shows isolation within 10 minutes, evidence log completeness above 95%, no production restore before validation. |
| P3 | Run quarterly cross-functional tabletop and track overdue actions to executive review. | IR Lead | First follow-up within 60 days; quarterly thereafter | Attendance at least 90%; action register reviewed monthly; no overdue P1 actions without executive exception. |

Post-incident review deadline: within 14 calendar days of incident closure, consistent with the teaching deck’s lessons-learned guidance (ICDFA, Week 20, slide 12). The IR Lead owns the review and evidence-backed final report; the executive sponsor closes each action only after the owner supplies measurable proof. Legal holds and regulatory investigations may require a longer evidence-retention period, but should not prevent prompt operational learning.

# Task 6 — Healthcare case study analysis and critical reflection

The teaching case describes a 2,500-employee healthcare provider hit by a Ryuk ransomware variant after a phishing attachment. Malware reportedly persisted and moved laterally for 72 hours before encryption. The response activated on Day 4, isolated network segments, quarantined affected systems and verified offline backups. Forensic analysis and eradication followed on Days 5–7, systems were rebuilt from clean backups, controls were strengthened, and a Day 14 review updated the plan and training (ICDFA, Week 20, slide 16).

Three strengths stand out. First, the team used segmentation and quarantine to limit spread rather than treating the incident as a single infected device. Second, verified offline backups enabled recovery without paying ransom. Third, a defined response team, phased restoration, timely regulatory notifications, and a post-incident review connected operational recovery to governance and improvement. These practices align with CISA’s recommendation to isolate affected systems, preserve relevant evidence, restore from offline backups, and document lessons learned (CISA et al., 2023).

Two limitations remain. The 72-hour dwell time suggests detection and/or identity monitoring did not identify lateral movement early enough; the slide does not describe what telemetry or alerting would shorten that delay. Also, “no patient data exfiltration confirmed” is an outcome claim without the supporting evidence described. The slide does not specify which egress, endpoint, identity, storage-access and cloud logs were reviewed, their retention/completeness, or how the team ruled out staging and encrypted outbound transfers. A conclusion should be bounded by evidence and confidence, not phrased as absolute proof if telemetry is incomplete.

I would transfer two changes to Northbridge. First, maintain a protected offline/immutable backup and routinely perform isolated restore tests. Northbridge’s network backup is non-immutable, svc_backup can administer both FS01 and BAK01, and no restoration test occurred in 90 days (E03, E05); a successful job report is insufficient. Second, make segmentation and phased recovery explicit, including an isolated recovery network and a signed acceptance gate for LMS01/FS01. Northbridge must not reopen the portal merely to meet the 11:00 examination if the backup is unsafe. Restoring critical systems within 72 hours means restoring a minimum viable, validated service tier; full restoration over seven days can include lower-priority services, wider validation and enhanced monitoring. Before claiming no learner data was stolen, responders would need sufficiently complete proxy/DNS/firewall and endpoint telemetry, file-access records, identity and cloud audit logs, destination analysis, checks for staging/archive tools, and a documented assessment of log gaps. After the tabletop, I would strengthen my original decision by requiring a specific named recovery approver and an explicit “no production restore while unverified” gate.

Word count for Task 6: approximately 370 words (verify using the word-count tool in your word processor).

# References

CISA, NSA, FBI, MS-ISAC and Joint Ransomware Task Force. (2023). \#StopRansomware Guide. Cybersecurity and Infrastructure Security Agency. https://www.cisa.gov/stopransomware/ransomware-guide

> *Use in report: Supports immediate isolation, evidence preservation, credential and backup protection, offline backup testing, recovery and post-incident improvement.*

Federal Republic of Nigeria. (2023). Nigeria Data Protection Act, 2023, section 40. Nigeria Data Protection Commission. https://ndpc.gov.ng/wp-content/uploads/2024/03/Nigeria_Data_Protection_Act_2023.pdf

> *Use in report: Supports the qualified discussion of regulator and data-subject notification duties, including the 72-hour controller notification rule where the statutory risk threshold is met.*

International Cybersecurity and Digital Forensics Academy (ICDFA). (n.d.). Incident Response Planning. GRC105, Week 20 teaching deck, slides 4–16.

> *Use in report: Supports the teaching lifecycle, incident classification, CSIRT roles, RACI concepts, containment and recovery stages, lessons-learned review, and the illustrative healthcare case study on slide 16.*

International Cybersecurity and Digital Forensics Academy (ICDFA). (n.d.). Business Continuity and Disaster Recovery. GRC205 teaching deck, slides 3–6 and 14–15.

> *Use in report: Supports the distinction between incident response, business continuity and disaster recovery, BIA, RTO/RPO, and the need for tested, phased recovery.*
