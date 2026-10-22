# Business Continuity and Disaster Recovery Plan

## Northbridge Skills Institute — GRC205 Incident Management and Business Continuity, Week 21

**Student name:** Esther Adeleke  
**Student ID:** 2025/GCR/12667
**Official email:** gcr2512667@students.icdfa.edu.ng
**Role assumed:** Business Continuity and Disaster Recovery Coordinator  
**Scenario:** Main site unavailable after flooding and power failure  
**Word count:** Approximately 3,300 words excluding tables, references and appendices  


## Executive summary

At 08:00 WAT on Monday (Day 0), flooding and a power failure make Northbridge Skills Institute’s main site and local systems unavailable. Facilities expects the building to be inaccessible for 72 hours. Microsoft 365 identity and email remain available over mobile internet, while the local directory AUTH01 and applications are unavailable. Northbridge has 1,800 learners, 90 staff, two disaster-recovery technicians and an alternate facility that can supply 25 workstations from 10:00. The immediate operational pressure is an 11:00 examination, followed by fee confirmation at 16:00 and admissions closure at 18:00.

The Business Impact Analysis (BIA) places assessment first because the deadline is immediate and learner welfare and assessment integrity matter, followed by fees, admissions, payroll, and records/certificates. The supplied hourly disruption rates total NGN 135,000. Without continuity measures, estimated cost is NGN 1,200,000 over eight hours and NGN 3,360,000 over 24 hours, including the one-off NGN 120,000 examination rescheduling trigger. These figures are exercise estimates, not observed losses, and do not price reputational harm, learner distress or data-integrity failures.

I recommend the Hot package for the next-year strategy. With the required independent protected backups and training/testing, its annual cost is exactly NGN 3,000,000, within the limit but leaving no contingency. Its stated one-hour critical-service estimate and up-to-15-minute data-loss window best match the proposed assessment objectives, although supplier claims must be validated. The current warm-site drill does not restore the LMS until 11:45 and all five functions until 13:00. Therefore, an approved assessment workaround is essential; technology recovery alone cannot meet the 11:00 business deadline.

## Assumptions and method

The scenario’s times are local West Africa Time (WAT). Cost rates are treated as separate, non-overlapping incremental costs, as instructed, and applied for the first 24 hours. The examination charge is added once if acceptable assessment service is not available at 11:00. For checkpoint age, the reference point is the 08:00 disruption. The warm-site timings include local validation, each action occupies one technician for its full duration, actions cannot be split, and parallel work is permitted only after prerequisites are complete. No actual supplier service-level agreement, backup log, learner accessibility register, insurance recovery, or facilities report is supplied; any related plan detail is a proposed control, not a verified fact. The package recovery windows are planning estimates and are not substituted for the measured warm-site schedule in Task 5.

# Task 1 — Business Impact Analysis

A BIA connects disruption to the services learners and staff need, rather than ranking servers only by technical convenience. This follows the teaching deck’s distinction between business continuity, which keeps organisational services operating, and disaster recovery, which restores technology (ICDFA, *Business Continuity and Disaster Recovery*, slides 3–4). Northbridge must therefore start continuity work while technicians restore the shared infrastructure.

### 1.1 BIA matrix

|Function / owner role|Users affected and critical resources|Dependencies and external needs|Impact over time / deadline|Minimum acceptable service and controls|Priority|
|-|-|-|-|-|-|
|**F1 Teaching and assessment** — Academic Registrar / Assessment Lead|120 learners sitting the 11:00 examination; assessors, invigilators, LMS01, DB01, devices and secure exam content|Network, AUTH01, DB01, LMS01; accessible room, power, invigilation, learner communications and approved assessment materials|Immediate high impact at 11:00; one-off NGN 120,000 rescheduling cost if no acceptable service; learner confidence and integrity are also at risk|Run an approved, time-controlled assessment for all learners. Use authenticated access where possible; otherwise use approved sealed/paper or offline materials, numbered scripts, two-person custody, attendance, invigilator logs and later dual-approved entry. Provide an accessible option and a documented alternative for learners unable to attend the centre.|**1 — urgent**|
|**F2 Fees and finance** — Finance Manager|Learners making payments, finance staff, accounts and reconciliation; FIN01, DB01, receipts and wallet records|Network, AUTH01, DB01, FIN01 and external payment provider; secure bank/provider reports|NGN 40,000/hour; fee confirmation deadline 16:00 Day 0. Payments may succeed externally while local records remain stale.|Accept payments through the provider but do not claim local confirmation until reconciled. Keep a controlled register with transaction reference, amount, time, payer identifier and operator; two-person review and daily provider statement matching.|**2 — same day**|
|**F3 Admissions and registration** — Admissions Manager|Prospective learners, admissions team; REG01, DB01, application forms and decision records|Network, AUTH01, DB01, REG01; admissions channels and privacy controls|NGN 20,000/hour; admissions close 18:00 Day 0. Lost or duplicated applications and missed deadlines are possible.|Time-stamp applications in a restricted register, issue a reference, retain the original submission and provide a clear receipt. A second staff member reviews changes; reconcile entries into REG01 with duplicate checks.|**3 — deadline-driven**|
|**F4 Staff payroll** — HR/Payroll Lead|90 staff and payroll approvers; HR01, DB01, payroll file and bank submission credentials|Network, AUTH01, DB01, HR01, bank portal and verified employee/payee data|NGN 10,000/hour; bank submission due 12:00 Day 1. Errors or duplicate payment instructions may harm staff and require manual correction.|Prepare from the last approved payroll extract; independently verify changes and totals; require dual authorisation and bank acknowledgement. Do not create an unreviewed replacement file.|**4 — next-day deadline**|
|**F5 Records and certificates** — Registrar / Records Manager|Learners and alumni requesting records; FS01, files and certificate templates|Network, AUTH01, FS01; secure file access, records staff and identity verification|NGN 5,000/hour; 48-hour internal service commitment. Confidentiality, integrity and access restrictions are essential.|Accept and time-stamp requests, verify identity, log access, prioritise urgent cases and issue only from a verified copy. No certificate or record is released if its source or authorisation is uncertain.|**5 — within 48 hours**|

All five functions also require available and briefed staff, secure access, reliable communications, power, safe premises and clear delegated authority. Learner welfare, disability accommodations, language/accessibility needs, data minimisation and the ability to contact people who rely on institutional channels must be considered explicitly. Suppliers include the payment provider, alternate-site operator, telecom/mobile internet provider, offsite backup provider and any printing or transport supplier.

### 1.2 Impact calculations

Combined hourly rate:

**NGN 60,000 + 40,000 + 20,000 + 10,000 + 5,000 = NGN 135,000 per hour.**

|Function|Rate/hour|8-hour calculation|8-hour total|24-hour calculation|24-hour total|
|-|-:|-:|-:|-:|-:|
|F1 assessment|NGN 60,000|60,000 × 8 + 120,000 trigger|NGN 600,000|60,000 × 24 + 120,000 trigger|NGN 1,560,000|
|F2 fees|NGN 40,000|40,000 × 8|NGN 320,000|40,000 × 24|NGN 960,000|
|F3 admissions|NGN 20,000|20,000 × 8|NGN 160,000|20,000 × 24|NGN 480,000|
|F4 payroll|NGN 10,000|10,000 × 8|NGN 80,000|10,000 × 24|NGN 240,000|
|F5 records|NGN 5,000|5,000 × 8|NGN 40,000|5,000 × 24|NGN 120,000|
|**Combined**|**NGN 135,000**|**1,080,000 + 120,000**|**NGN 1,200,000**|**3,240,000 + 120,000**|**NGN 3,360,000**|

These calculations assume no continuity or recovery measure makes assessment acceptable by 11:00, so the one-off trigger applies at both horizons. They are not forecasts of actual cash losses. First, the hourly values are supplied estimates with no supporting methodology and assume impacts are linear, even though real impacts often spike around deadlines. Second, the calculation omits indirect and non-financial consequences, including learner anxiety, accessibility harm, reputation, contractual remedies, regulatory exposure and the cost of corrupted or duplicated records. Financial impact must not be treated as a complete measure of harm.

### 1.3 Business priority versus technical order

The business priority is F1, F2, F3, F4 and F5 because of the 11:00 examination and the same-day finance/admissions deadlines. The technical order is different: the network must be restored before AUTH01; AUTH01 before security and backup checks; checks before DB01; DB01 before LMS01, FIN01, REG01 and HR01. FS01 also depends on AUTH01 and security/backup checks. Shared dependencies create at least four risks: (1) a delayed or failed AUTH01 restore blocks all local functions; (2) DB01 corruption or a broken incremental chain blocks four applications; (3) one compromised administrator identity could affect both production and backups; and (4) the alternate-site capacity and power/connectivity may be insufficient for all learners. A fifth risk is inconsistent finance records while the external provider continues accepting payments. The continuity lead must manage these business risks in parallel with the technical queue.

# Task 2 — Recovery objectives and backup design

### 2.1 Definitions and targets

The Recovery Time Objective (RTO) is the target elapsed time from disruption to restore a specified service. The Recovery Point Objective (RPO) is the maximum acceptable age of data that may be lost, measured back from disruption. Maximum Tolerable Downtime (MTD) is the maximum time a function can be disrupted before the consequences become unacceptable. They are related but not interchangeable. Northbridge should declare a function recovered only after the application works, required users can access it, business transactions pass validation, security controls are active, and the business owner signs acceptance. A running server alone is not service recovery.

|Function|RTO|RPO|MTD|Justification / recovery declaration|
|-|-:|-:|-:|-|
|F1 assessment|2 hours for an approved assessment service, including the workaround|15 minutes|3 hours|The 11:00 deadline requires an approved continuity service before the deadline. A 2-hour RTO leaves a 1-hour buffer before the 3-hour MTD; the LMS may take longer, so the workaround must be ready. The Hot package estimate must be validated.|
|F2 fees|4 hours|15 minutes|8 hours|Same-day 16:00 confirmation; provider references and controlled interim records support reconciliation.|
|F3 admissions|6 hours|15 minutes|12 hours|Admissions closes at 18:00; controlled intake can preserve submissions while REG01 is restored.|
|F4 payroll|8 hours|1 hour|24 hours|Bank deadline is 12:00 Day 1; protect payment integrity through dual review.|
|F5 records/certificates|12 hours|4 hours|48 hours|The service commitment is 48 hours; data confidentiality and source integrity outweigh releasing an unverified record.|

The RTOs leave a buffer before the MTD for business validation and escalation, but the special assessment deadline is earlier than its technical RTO. The workaround is therefore mandatory, not optional. The 15-minute RPOs for F1–F3 are aspirational targets supported by a hot strategy and more frequent backups; the existing checkpoint does not meet them. Payroll’s one-hour RPO is also not met by the supplied checkpoint exactly? The DB checkpoint is one hour old, so it meets the one-hour boundary only if all increments through 07:00 are valid. There is no buffer for a failed increment. F5’s four-hour RPO is not met by the nine-hour-old file backup.

### 2.2 Checkpoint age and exposure

|Item|Checkpoint|Age at 08:00|Affected functions|Assessment against target|
|-|-|-:|-|-|
|B01 DB01 full|23:00 Day −1|9 hours|Base for F1–F4|Cannot be restored alone to reach the latest point; needs all required increments.|
|B02 DB01 incremental chain|07:00 Day 0|1 hour|F1–F4|Potentially recoverable to 07:00 if B01 and every required increment are present, readable and consistent. Meets F4’s 1-hour target at the boundary, but misses 15-minute targets for F1–F3.|
|B03 AUTH01 snapshot|00:00 Day 0|8 hours|F1–F5 authentication and configuration|Exceeds the 15-minute targets and creates risk that account/configuration changes since midnight need reconstruction.|
|B04 FS01 full|23:00 Day −1|9 hours|F5|Exceeds F5’s 4-hour RPO by 5 hours.|

At the 08:00 disruption, the possible data-loss windows are therefore one hour for business data, eight hours for directory/configuration changes, and nine hours for files, assuming successful restores. Provider statements, approved forms, email, bank records and application logs may help reconstruct transactions after the checkpoint. They do not prove that every database transaction, file version, access-control change or audit event is recoverable. Reconciliation must preserve original evidence and distinguish confirmed facts from inferred entries.

### 2.3 Proposed backup and restoration design

Adopt a 3-2-1 approach: at least three copies of important data, on two different storage types or administrative domains, with at least one offsite copy. Add an offline or immutable copy because the supplied production and backup administration share an administrator identity and the offsite provider’s access-independence controls are undocumented. The disconnected full backup from 17:00 Day −3 is useful as an additional recovery point, but it is not sufficiently recent to be the only fallback.

Proposed schedule: take a database full backup weekly; take transaction-log backups every 15 minutes where supported and tested; take incremental backups hourly as an additional restore aid; take a configuration-aware AUTH01 backup every 15 minutes after material changes and at least hourly; and back up FS01 changes every hour with daily full backups. Keep daily restore points for 35 days, weekly full copies for 12 weeks and monthly protected copies for 12 months, subject to records-retention and privacy review. Keep one copy in a separate cloud/offsite account and one encrypted immutable or offline copy in a separately administered location. These frequencies and retention periods are proposed controls, not existing capabilities, and must be costed and confirmed with vendors.

The exact DB01 restore chain is B01 full at 23:00 Day −1, followed by every required B02 incremental in sequence through 07:00 Day 0. If any increment fails validation, stop; do not skip it and claim the 07:00 point. Restore B01 plus the latest complete verified chain, or use an alternate validated log/backup chain. Record the selected restore point and resulting RPO gap. Use separate named backup identities, multifactor authentication, least privilege, time-limited elevation, approval for deletion, and immutable retention. Encrypt data in transit and at rest, store recovery keys separately, and alert on unusual administrator logons or backup-policy changes.

Offsite storage alone does not prove recoverability: copies can be incomplete, inaccessible when credentials fail, or dependent on the same compromised identity. A live replica can rapidly copy accidental deletion, corruption or malicious changes. Verification should include cryptographic hashes or vendor integrity results, completeness and sequence checks, a clean restore environment, malware/security checks, application-level row counts and transaction tests, access-control tests, file opening and version checks, and business-owner sampling. Test access using a break-glass identity that is independent of production administration. Conduct monthly sample restores and quarterly end-to-end recovery tests. The present brief does not confirm immutability, independent credentials, encryption-key recovery or successful restore testing; these remain unverified until evidenced.

# Task 3 — Recovery strategy and budget

All package costs must include the two required additions: NGN 400,000 annually for independent protected backups and NGN 200,000 for training and tests.

|Package|Base annual cost|Protected backup|Training/tests|Total annual cost|Stated first critical service / data loss|Fit and gaps|
|-|-:|-:|-:|-:|-|-|
|Cold|NGN 600,000|NGN 400,000|NGN 200,000|**NGN 1,200,000**|24–48 hours / up to 24 hours|Fits budget but misses all urgent RTO/RPO targets; unsuitable as primary strategy.|
|Warm|NGN 1,500,000|NGN 400,000|NGN 200,000|**NGN 2,100,000**|3–5 hours / 1–6 hours|Fits and leaves NGN 900,000, but fails F1’s 1-hour continuity objective and 15-minute RPOs; measured drill has later service times.|
|Hot|NGN 2,400,000|NGN 400,000|NGN 200,000|**NGN 3,000,000**|About 1 hour / up to 15 minutes|Fits exactly and best matches critical objectives, but has no budget headroom and estimate needs proof.|

I recommend the Hot package, conditional on supplier evidence and an approved continuity workaround. The reason is not simply that hot is fastest: the assessment deadline and the proposed 15-minute RPO for F1–F3 are business requirements that the other packages do not meet. The annual total is NGN 2,400,000 + NGN 400,000 + NGN 200,000 = NGN 3,000,000, exactly the cap. The Finance Director and executive sponsor must accept the lack of financial contingency and the residual risk that actual recovery takes longer than the quoted estimate. If procurement cannot demonstrate the promised recovery and independent backup access, the board should not treat “hot” as proof; it should renegotiate scope, obtain a tested service commitment or formally accept a lower objective with compensating continuity controls.

The eight-hour loss estimate is NGN 1.2 million and the 24-hour estimate NGN 3.36 million. Both exceed the NGN 1.2 million cold-package annual cost, while the 24-hour estimate exceeds even the Hot package’s annual cost. This comparison indicates material exposure, but it does not establish ROI. One outage estimate is not an annual loss expectancy: frequency, probability, correlated flood/power risk, insurance, vendor failure, recovery investment life, implementation costs, and non-financial impact are missing. Before approval, obtain a tested supplier RTO/RPO report, service credits and exclusions, geographic separation evidence, recovery-site access and power details, subcontractor dependencies, backup immutability proof, penetration/security assurance, restore-test results, data residency/retention terms, and a complete cost schedule including connectivity and renewal increases.

Cold offers lower cost but prolonged service interruption and high data loss. Warm provides a sensible compromise for many organisations, but its quoted 3–5 hour service and 1–6 hour loss window conflict with Northbridge’s time-critical assessment and 15-minute targets. Hot is the best strategic fit within the stated budget, but no budget buffer remains for extra licences, travel, power, or unplanned supplier fees. The executive sponsor owns acceptance of that financial residual risk; business owners own the consequence of any formally relaxed RTO/RPO.

# Task 4 — Coordinated continuity and disaster recovery plan

### 4.1 Activation, authority and coordination

**Activation threshold:** activate the plan when the main site is unsafe or unavailable for more than 30 minutes, a critical local system is unavailable with a deadline at risk, or the expected interruption exceeds the affected function’s RTO. The flood/power event meets the threshold immediately. The authorised decision maker is the Institute Director (Incident Executive); the deputy is the Operations Director. If neither is reachable within 15 minutes, the most senior available member of the designated continuity team may activate the plan and record the decision for later ratification.

The coordinator maintains one incident log, time-stamped situation reports every 30 minutes during the first four hours and hourly thereafter, a decision register, a contact tree and a list of open risks. Microsoft 365 email and mobile internet are available initially; use a pre-agreed group chat or conference bridge as secondary channels. If mobile data fails, use voice/SMS and a designated call tree. Do not put learner personal data or backup credentials in broad group messages. Staff must verify unusual instructions through a second channel.

The continuity team owns learner-facing workarounds, staffing, alternate-site operations and service prioritisation. The technical recovery team restores systems and documents evidence. Facilities confirms building safety, power and environmental conditions. The incident response lead joins if there is evidence or reasonable suspicion of compromise, such as an unexpected privileged logon, failed integrity checks, unexplained data changes, malware indicators or suspicious backup-policy changes. Flooding alone is a continuity/disaster event, not proof of a cyberattack. If compromise is suspected, preserve logs and snapshots, restrict access, isolate affected systems where safe, maintain chain of custody and coordinate containment before restoring into a clean environment.

### 4.2 RACI matrix

R = responsible for doing the work; A = one accountable decision owner; C = consulted; I = informed. Each row has exactly one A.

|Activity|Incident Executive|BC/DR Coordinator|Technical Lead|Business Owner|Facilities Lead|Communications Lead|Incident Response Lead|
|-|-|-|-|-|-|-|-|
|Activate plan and set incident level|A|R|C|C|C|I|I|
|Approve temporary assessment delivery|I|R|C|A|C|C|C|
|Validate backup and restore point|I|C|A/R|C|I|I|C|
|Restore infrastructure and applications|I|C|A/R|C|C|I|C|
|Issue learner/staff/supplier communications|I|C|C|C|I|A/R|I|
|Approve finance/admissions manual controls|I|C|C|A/R|I|I|C|
|Authorise return to primary site|A|R|C|C|R|I|C|
|Close incident and approve lessons/actions|A|R|C|C|C|C|C|

### 4.3 Temporary procedures for the 72-hour closure

**Assessment:** the alternate site has 25 workstations, while 40 learners require centre workstations and 80 can use their own devices. The site cannot seat all 40 centre-dependent learners simultaneously. The Assessment Lead must decide, before the exam begins, whether an approved paper/offline format, accessible loan devices, a second safe venue, or a formally approved staggered sitting preserves fairness and security. Do not assume an unapproved change of format is acceptable. Use two-person control of question papers, sealed envelopes or encrypted files, learner identity checks, attendance and incident logs, controlled start/finish times, and sealed return of scripts. Keep the 80 BYOD learners on their devices only if a secure approved platform, authenticated access, connectivity and assessment-integrity controls are available. If an acceptable service cannot be approved and delivered by 11:00, invoke the rescheduling procedure and communicate the revised arrangements; the cost model then applies the NGN 120,000 trigger.

**Fees:** finance staff record provider transaction references, amount, time and a minimal payer identifier in a restricted spreadsheet or numbered paper log. One person enters and another checks totals. Issue a receipt or status message that clearly says “payment received by provider; institute record pending reconciliation” where that is accurate. Reconcile the provider’s report to the temporary register before updating FIN01; never infer success from a learner screenshot alone.

**Admissions:** keep an access-controlled intake register with submission time, unique reference, contact details only where necessary, and a receipt to the applicant. Preserve the original email/form and a change history. The Admissions Manager approves exceptional deadline treatment and ensures no application is silently lost or entered twice. Only authorised staff can view personal information; do not use personal accounts or open shared spreadsheets.

**Payroll:** freeze unapproved changes and use the last approved payroll baseline. HR and Finance compare any proposed changes against authorised employee records, verify bank details through a trusted channel, calculate control totals, and require two authorised approvers before bank submission. Retain the bank acknowledgement and record who approved the file. Never resubmit merely because an acknowledgement is delayed; first confirm whether the bank accepted the original.

**Records/certificates:** receive and queue requests, verify identity using the normal standard, record purpose and access, and prioritise requests approaching the 48-hour commitment. Only release information from a verified backup or confirmed authoritative copy. A second authorised staff member checks each release. Do not create a replacement certificate from memory or from unverified data.

Minimum staffing is the two technicians plus a coordinator, Assessment Lead, invigilators, Finance representative, Admissions representative, HR/Payroll approver, Records representative, Facilities contact and communications owner. Business coordinators manage manual services while technicians recover systems. Minimum equipment includes 25 working workstations, secure network and power, accessible seating, locked storage for scripts, printed control forms, phones, chargers, secure shredding/return arrangements and a private space for sensitive records. Supplier support is required from the alternate-site operator, telecom provider, payment provider, bank and offsite backup provider. If the site drops to 15 workstations, re-plan assessment cohorts and do not quietly exceed safe capacity.

### 4.4 DR runbook

|Order / owner|Action and prerequisite|Completion evidence|If the step fails|
|-|-|-|-|
|1. BC/DR Coordinator + Facilities|Declare incident, confirm people safety and site restriction; start incident log and notify roles|Time-stamped activation, contact acknowledgements, facilities restriction|Use remote coordination; do not send staff into unsafe premises.|
|2. Technical Lead|Establish isolated recovery network and confirm power, connectivity and admin access|Network tests, access log, clean recovery boundary|Use alternate connectivity or escalate supplier; keep services on manual procedures.|
|3. Technical Lead|Restore AUTH01 from B03 only after checking provenance, timestamp and integrity|Directory service test, privileged-account review, successful test login|Use independent recovery credentials; investigate suspicious access and engage incident response if unexplained.|
|4. Technical Lead + IR Lead consulted|Complete security and backup checks; review admin logs, backup catalogue and restore chain|Signed checklist, hashes/integrity report, chosen restore point|Quarantine suspect copy; do not restore a failed increment. Select last complete verified chain and record RPO breach.|
|5. Technical Lead|Restore DB01: B01 full plus every valid increment through selected point; validate schema and transaction totals|Restore logs, integrity results, row/control totals, timestamp and RPO calculation|Fall back to last complete chain; protect evidence and document lost interval.|
|6. Technical Lead|Restore LMS01, FIN01, REG01 and HR01 after DB01 acceptance; restore FS01 after AUTH01/security checks|App login, key workflow tests, file sampling and access checks|Keep relevant function in controlled manual mode; no unsupported data edits.|
|7. Business Owners|Test real business workflows and reconcile priority transactions|Signed acceptance, sample transactions, exception register|Reject release, rollback to last known-good state or continue manual service.|
|8. BC/DR Coordinator|Communicate service status and approved limitations; monitor error and backlog rates|Time-stamped update, stakeholder receipt and dashboard|Correct misleading notice quickly and escalate any missed deadline.|
|9. Facilities + Incident Executive|Approve return only after safety, utilities and environment are cleared|Written facilities clearance and executive decision|Continue alternate-site operation and re-check conditions.|
|10. Technical Lead + Business Owners|Transfer new records in controlled batches, compare counts, reconcile, obtain sign-off and close|Batch manifest, hashes/control totals, dual approval and closure report|Stop transfer on mismatch; preserve both copies and resolve before decommissioning temporary records.|

Restoring technology is not equivalent to resuming an acceptable service. The business owner must test a real transaction and confirm that users, data, security and operational procedures work. Rollback means stopping access to a failed or inconsistent recovery, preserving it for investigation, returning to a verified recovery point and keeping the approved manual process active.

### 4.5 Stakeholder communication matrix

|Audience|Owner / channel|Timing and content|
|-|-|-|
|Learners|Communications Lead; M365 email, SMS or official portal|Initial notice as soon as activation is approved; next update before the exam decision deadline; state what is known, what learners should do, accessibility route and next update time.|
|Staff and managers|BC/DR Coordinator; verified staff email and call tree|Initial activation and every material change; roles, safe-site restrictions, approved manual controls and reporting route.|
|Parents/sponsors where authorised|Communications Lead|Only where applicable and permitted; avoid disclosing personal learner information.|
|Payment provider and bank|Finance Manager through verified contact details|Confirm service status, transaction-report access and payroll submission state; record reference numbers.|
|Backup/alternate-site suppliers|Technical Lead / Facilities Lead|Request incident support, access and recovery estimates; obtain written confirmation of service status and dependencies.|
|Executive/board|BC/DR Coordinator|Situation reports every 30 minutes initially, including decisions, deadlines, risks, cost estimates and required approvals.|

**Initial learner notice (simulated, 145 words):**

> \\\*\\\*Subject: Important update: Northbridge site closure and today’s examination\\\*\\\*
>
> Northbridge Skills Institute’s main site is temporarily unavailable following flooding and a power failure. The building is being treated as inaccessible while Facilities confirms that it is safe. Our email service remains available, and the continuity team is working to maintain teaching and learner support from an alternate location.
>
> If you are scheduled for the 11:00 examination today, please check your official Northbridge email and messages before travelling. We are confirming the approved arrangements, including options for learners who need a centre workstation or an accessibility adjustment. Do not rely on unofficial social media posts or send sensitive personal information in group chats.
>
> We will send the next update by 10:15 WAT, or sooner if a decision is made. If you have an urgent access or accessibility concern, reply to this message using your official learner account. We apologise for the uncertainty and will provide clear instructions as soon as they are confirmed.

### 4.6 Return to site

The primary site must not reopen merely because the 72-hour estimate has elapsed. Facilities must document structural, electrical, water, sanitation, fire and environmental clearance. IT must inspect equipment for water or power damage and confirm safe power, network and environmental controls. The Incident Executive authorises return after consulting Facilities, Technical Lead and business owners. New records created at the alternate site are transferred in numbered batches with operator, timestamp, source and destination, record counts and hashes where applicable. A second person verifies counts and exceptions before the temporary register is locked as read-only. Business owners sign off reconciliations, the Technical Lead confirms stable monitoring, and the Coordinator issues a final notice. Temporary records are retained under the records schedule until the reconciliation and audit period is complete.

# Task 5 — Recovery schedule and exercise

### 5.1 Modelled warm-site baseline schedule

The following is a **modelled baseline**, not a live recovery test. Each task uses one technician for its full duration. Technician 1 follows the critical database path; Technician 2 starts FS01 only after its prerequisites are met.

|Action|Technician|Start|Finish|Duration|Prerequisite|
|-|-|-|-|-:|-|
|Recovery network|Tech 1|08:00|08:30|30 min|None|
|AUTH01 restoration|Tech 1|08:30|09:00|30 min|Recovery network|
|Security and backup checks|Tech 1|09:00|09:30|30 min|AUTH01 restoration|
|DB01 restoration|Tech 1|09:30|11:00|90 min|Security and backup checks|
|FS01 restoration|Tech 2|09:30|10:30|60 min|AUTH01 and security/backup checks|
|LMS01 restoration|Tech 1|11:00|11:45|45 min|DB01 restoration|
|FIN01 restoration|Tech 2|11:00|11:30|30 min|DB01 restoration|
|REG01 restoration|Tech 2|11:30|12:00|30 min|DB01 restoration|
|HR01 restoration|Tech 1|11:45|12:30|45 min|DB01 restoration|

The critical path to assessment is network (30) + AUTH01 (30) + security/backup checks (30) + DB01 (90) + LMS01 (45) = **225 minutes = 3 hours 45 minutes**, so assessment technology is available at 11:45. Tech 2 can restore FS01 from 09:30 to 10:30 once AUTH01 and the checks are complete, then restore FIN01 from 11:00 to 11:30 and REG01 from 11:30 to 12:00 after DB01 is ready. Tech 1 restores HR01 after LMS01, from 11:45 to 12:30. This schedule respects the two-technician limit and does not split actions. The **earliest feasible all-function completion is 12:30**, while assessment technology remains available at 11:45.

### 5.2 Baseline results against objectives

|Function|Modelled availability|Elapsed from 08:00|RTO|MTD|Baseline result|
|-|-|-:|-:|-:|-|
|F1 assessment/LMS|11:45|3 h 45 min|2 h assessment service|3 h|RTO and 11:00 deadline missed without workaround; MTD exceeded for business service.|
|F2 fees/FIN01|11:30|3 h 30 min|4 h|8 h|RTO met; still use provider reconciliation before confirmation.|
|F3 admissions/REG01|12:00|4 h|6 h|12 h|RTO met.|
|F4 payroll/HR01|12:30|4 h 30 min|8 h|24 h|RTO met; bank deadline remains Day 1 at 12:00.|
|F5 records/FS01|10:30|2 h 30 min|12 h|48 h|RTO met; RPO is not met by the 9-hour-old backup.|

The baseline schedule proves that an acceptable service needs more than a successful server restore. Without a separately approved assessment workaround, F1 misses the 11:00 deadline and the NGN 120,000 rescheduling trigger applies. The fee and admissions workarounds should remain active until transactions are reconciled. The baseline data point for DB01 is 07:00, an age of one hour: this misses F1–F3’s 15-minute RPOs, meets F4’s one-hour target only at the boundary, and depends on the entire chain passing. AUTH01’s eight-hour-old snapshot and FS01’s nine-hour-old full backup miss their RPOs. These are modelled results based on the brief’s assumption that baseline backup integrity checks pass; they are not claims that a live drill occurred.

### 5.3 Forty-five-minute tabletop exercise log

**Exercise purpose:** test decision-making when the baseline assumptions fail. **Participants:** Coordinator, Incident Executive, Technical Lead, Assessment Lead, Finance Manager, Admissions Manager, HR/Payroll Lead, Records Manager, Facilities Lead, Communications Lead and Incident Response Lead. A single student may simulate all roles. **Scoring:** decision recorded, owner assigned, rationale stated, learner/business impact considered, and a measurable follow-up action logged.

|Exercise minute / inject|Simulated decision and owner|Rationale and service effect|
|-|-|-|
|00–10: start and baseline|Coordinator confirms activation, safety, contact tree, 11:00 assessment deadline and 25-seat limit; Incident Executive approves workaround planning.|Establishes shared facts and separates business continuity from technical restoration. Learner notice promises an update, not an unverified recovery time.|
|**10: site closure extends to five days**|Facilities Lead updates site estimate; Incident Executive extends alternate-site operations to five days, asks procurement to confirm venue, power, internet and accessible capacity, and activates staff rota and welfare checks.|A 72-hour plan is not enough. Longer closure requires supplies, transport, privacy and staff fatigue controls; the primary site remains closed pending written clearance.|
|**20: alternate facility has only 15 workstations**|Assessment Lead and Coordinator revise seating and delivery plan; reserve workstations for learners who genuinely require centre equipment, assess an approved second venue or accessible loan equipment, and seek formal approval for staggered/paper delivery.|Do not assume all 40 centre-dependent learners can be accommodated. If equitable and secure assessment cannot be delivered by 11:00, reschedule and apply the trigger. Communications Lead sends corrected instructions.|
|**30: one DB01 incremental fails integrity; unexpected backup administrator logon**|Technical Lead stops the chain at the failed increment, preserves logs and copy metadata, and selects the latest complete verified chain; Incident Response Lead investigates the logon, restricts the account and preserves evidence. Coordinator maintains manual service; Executive approves any material RPO breach.|Skipping a failed increment would create false confidence. Unexpected privileged access plus integrity failure is reasonable suspicion of compromise. Do not delete evidence or reconnect a suspect copy to production. Recalculate RPO and document lost transactions.|
|**40: provider reports successful payments absent from restored finance records**|Finance Manager freezes duplicate collection and resubmission, obtains provider statement directly, matches transaction references to temporary logs, and requires second-person approval before posting corrections. Incident Response Lead checks whether omissions are accidental or malicious.|A successful external payment may not exist in the restored checkpoint. Reconcile evidence rather than asking learners to pay again. Record unmatched items, notify affected learners individually through verified channels, and do not expose payment details broadly.|
|40–45: hot debrief|Coordinator records decisions, open risks, owners and due dates; Executive confirms action priorities and next update.|Every decision becomes an improvement task with a completion test, not just a meeting note.|

**Corrective-action register (simulated):**

|Priority|Action|Owner|Target date from exercise|Measurable completion evidence|
|-|-|-|-|-|
|P1|Separate production and backup administrator identities; enable MFA and alerting for privileged logons|IT Security Lead|Within 14 days|Access review signed; test alert triggered and acknowledged within 15 minutes.|
|P1|Implement immutable/offline backup copy and document independent break-glass access|Technical Lead|Within 30 days|Configuration evidence plus successful restore using independent credentials.|
|P1|Test DB01 chain-failure recovery and document last-known-good selection|Database Lead|Within 21 days|Exercise report proves recovery without skipping failed increment and records actual RPO.|
|P1|Approve accessible exam continuity options and alternate-site capacity plans for 15, 25 and 40 workstation scenarios|Assessment Lead|Within 10 days|Approved procedure and tabletop attendance; all scenarios have named decision owner and learner communication.|
|P2|Build finance reconciliation template with duplicate detection and two-person sign-off|Finance Manager|Within 14 days|Test set with at least 20 transactions; 100% matched or exceptioned, zero duplicate posting.|
|P2|Confirm five-day alternate-site and staff welfare arrangements with suppliers|Facilities Lead|Within 30 days|Written supplier confirmation and documented capacity, power, connectivity and accessibility checks.|
|P2|Conduct full recovery drill and update actual RTO/RPO dashboard|BC/DR Coordinator|Within 60 days|All five functions tested; results recorded, owners accept gaps and 90% of actions have due dates.|

# Task 6 — Governance and performance measures

### 6.1 Ownership, maintenance and framework alignment

The Institute Director owns the continuity policy and accepts residual business risk. The BC/DR Coordinator owns this plan, the contact list, exercise calendar, incident log template, version history and annual review. Business owners approve their minimum-service definitions, RTO/RPO and manual reconciliations. The Technical Lead owns restoration runbooks and backup evidence. The Finance Director approves expenditure and documents acceptance of the Hot package’s zero budget headroom. The Incident Response Lead owns investigation and evidence preservation if compromise is suspected.

Review the plan at least annually and after a site move, application/database change, new payment provider, material staffing change, failed restore, actual disruption, security incident or significant flood warning. Store a controlled master with version number, owner, approval date, next review date and change summary. Distribute a read-only copy to named roles and maintain an offline copy accessible during M365 or network failure. Train new staff on appointment and provide annual refresher training. Run quarterly tabletop exercises, monthly sample restore tests, and an annual end-to-end recovery exercise; test high-risk components more often after material changes. ISO 22301 frames continuity as a management system covering context, leadership, planning, support, operation, performance evaluation and improvement (ISO, 2019; ISO, 2024). This is an educational alignment, not a claim of certification.

|Northbridge output|NIST SP 800-34 Rev. 1 planning step|ISO 22301 theme from teaching slides|
|-|-|-|
|BIA and priority matrix|Step 2: Conduct BIA|Clause 8: Operation — BIA, risk assessment, strategy and plans|
|Recovery package and backup strategy|Step 3: Preventive controls; Step 4: Contingency strategies|Clause 6: Planning; Clause 8: Operation|
|Coordinated plan and DR runbook|Step 5: Develop contingency plan|Clause 5: Leadership and roles; Clause 7: Support; Clause 8: Operation|
|Tabletop and restore test programme|Step 6: Testing, training and exercises|Clause 9: Performance evaluation; Clause 10: Improvement|
|KPI dashboard, corrective actions and version review|Step 7: Plan maintenance|Clause 9: Performance evaluation; Clause 10: Improvement|
|Executive approval and resource allocation|Step 1: Contingency planning policy|Clause 4: Context; Clause 5: Leadership; Clause 6: Planning|

### 6.2 Performance dashboard

|KPI|Formula / method|Data source|Target|Frequency|Accountable owner|
|-|-|-|-|-|-|
|Recovery objective achievement|Number of functions meeting both approved RTO and RPO ÷ functions tested × 100|Restore logs, timestamps, selected checkpoints and business sign-off|≥90% in first 6 months; 100% for F1 by month 12, or documented executive acceptance|Every test and quarterly report|BC/DR Coordinator|
|Restoration-test coverage|Critical systems with successful restore test in last quarter ÷ critical systems × 100|Backup platform reports and test register|100% of AUTH01, DB01, LMS01, FIN01, REG01, HR01 and FS01 each quarter (sampled application workflows where full restore is impractical)|Monthly/quarterly|Technical Lead|
|Corrective-action closure|Actions closed with evidence by due date ÷ actions due × 100|Exercise/action register|≥90% on time; zero overdue P1 actions|Monthly|Risk/Compliance Lead|
|Staff training completion|Named plan roles completing annual training ÷ named plan roles × 100|Attendance and competency records|100% of named roles annually; ≥90% pass a short role-based check|Quarterly tracking; annual completion|HR Lead|
|Backup integrity and access independence|Successful restore tests with verified integrity and independent access ÷ tests performed × 100|Backup logs, hashes, access review and restore evidence|100% successful; every failed test has an owner and due date within 2 business days|Monthly|Technical Lead|
|Learner communication timeliness|Incidents where first approved notice sent within 30 minutes ÷ incidents requiring notice × 100|Incident timeline and message logs|≥95%|After each incident/exercise|Communications Lead|

Targets are deliberately measurable. A dashboard result should include the period, numerator, denominator, exception, owner and corrective action. A successful backup job is not counted as a successful restore test unless the data can be restored, read, authenticated and used by the business owner.

### 6.3 Maturity and climate resilience

Using the teaching deck’s five-level maturity model (slide 13), Northbridge’s current maturity is assessed as **Level 1: Initial / Ad Hoc**, for planning purposes. This is an inference, not an audited rating: the brief supplies backup checkpoints and a warm-site drill, but also identifies shared administrator identity, undocumented offsite access independence, and no verified exercise results. These gaps suggest that recovery depends too much on assumptions and individual knowledge. The realistic 12-month target is **Level 3: Defined / Standardized**: approved plans, named owners, quarterly exercises, documented restore evidence, repeatable runbooks, measured KPIs and a tracked corrective-action process. Level 4 would be premature until there is a year of reliable measurements and risk-based management review.

Flood risk should be treated as a recurring climate and location consideration, not an isolated accident. Northbridge should assess flood maps and historical water ingress, site drainage, placement of electrical and network equipment above likely flood levels, safe shutdown, generator/UPS placement, alternate-site geography, transport routes and supplier concentration in the same hazard area. Avoid placing the primary and recovery sites in the same floodplain or power distribution zone where practicable. ISO’s 2024 climate-action amendment asks organisations to consider whether climate change is relevant to the context of the management system and interested-party requirements (ISO, 2024). Northbridge should record its assessment and update it as local hazard information changes.

## Conclusion

Northbridge’s main lesson is that a recovery plan must protect the service deadline, not just bring servers back online. The BIA prioritises the examination and same-day financial and admissions work. The warm-site schedule cannot restore LMS01 before 11:00, so an approved assessment workaround is required to protect fairness and avoid the rescheduling trigger. The current checkpoints leave an eight-hour directory gap and a nine-hour file gap, while the database chain only meets the one-hour boundary if every increment is valid. The Hot package is the strongest fit within the NGN 3 million limit, but it must be independently tested and has no budget headroom. Clear ownership, controlled manual records, verified backups, realistic exercises and measurable corrective actions turn the plan from a document into a capability.

## References

* International Cybersecurity and Digital Forensics Academy (ICDFA). (2026). *Business Continuity and Disaster Recovery: GRC205 Incident Management and Business Continuity, Week 21* \[Teaching slides], especially slides 3–4, 9 and 13–15.
* International Cybersecurity and Digital Forensics Academy (ICDFA). (2026). *GRC205 Business Continuity and Disaster Recovery Assignment* \[Assessment brief and marking rubric], pp. 1–8.
* International Organization for Standardization (ISO). (2019). *ISO 22301:2019 Security and resilience — Business continuity management systems — Requirements*. https://www.iso.org/standard/75106.html
* International Organization for Standardization (ISO). (2024). *ISO 22301:2019/Amd 1:2024 — Climate action changes*. https://committee.iso.org/cms/live/live/en/sites/isoorg/contents/data/standard/08/84/88412.html
* Swanson, M., Bowen, P., Phillips, A., Gallup, D., \& Lynes, D. (2010). *Contingency Planning Guide for Federal Information Systems (NIST SP 800-34 Rev. 1)*. National Institute of Standards and Technology. https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final

