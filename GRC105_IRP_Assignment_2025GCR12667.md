# Incident Response Planning Assignment 

GRC105  Incident Management and Business Continuity  Week 20 

**Student name:** Esther Adeleke 

**Student ID:** 2025/GCR/12667 

**Official email:** gcr2512667@students.icdfa.edu.ng 

**Course / module:** GRC105 — Incident Management and Business Continuity 

**Word count:** 8059 

## Executive summary 

At 08:45 WAT on Day 0, Northbridge Skills Institute should declare a Critical cybersecurity incident involving confirmed ransomware indicators on FIN-PC07 and FS01, with material disruption to the learning portal through its FS01 dependency (E06–E08). The preceding suspicious cloud upload (E04), unusual privileged logons by svc_backup (E03), and unfamiliar Microsoft 365 sign-in (E02) create a credible possibility of account compromise and data exfiltration, but do not prove either. The incident must therefore be managed as ransomware with a suspected data-security incident until evidence narrows the scope. 

Immediate priorities are to stop encryption and lateral movement, protect BAK01 from further access, preserve volatile and audit evidence, secure Microsoft 365 and privileged credentials, and give learners a truthful examination update before the scheduled 11:00 examination. The latest network backup is not a safe recovery point merely because a job reported success (E05): backups are not immutable, the backup account has administrator access to FS01 and BAK01, and no restoration test has occurred in 90 days. The disconnected backup is older, from Friday Day −3 at 17:00, and also requires validation. 

This report uses the course teaching model of preparation; detection and analysis; containment, eradication and recovery; and post-incident activity. It uses NIST SP 800-61 Rev. 3 in its CSF 2.0 framing: Govern, Identify and Protect support preparation, while Detect, Respond and Recover form incident response; improvement feeds back into all functions (Nelson et al., 2025). Proposed evidence collection and exercise outcomes are explicitly labelled as planned or simulated. 

## Task 1 Incident classification and initial assessment 

### 1.1 Evidence-linked timeline 

|Time (WAT)|Evidence|Confirmed<br>observation|Working<br>hypothesis/limitatio<br>n|Validation<br>needed|
|---|---|---|---|---|
|Day −1, 16:40|E01|Finance officer<br>reported opening|Possible phishing initial<br>access. Malicious code|Email headers and<br>attachment,|



|||an unexpected<br>payroll attachment<br>on FIN-PC07; mail<br>record exists.|execution is not verified;<br>opening an attachment<br>alone does not prove<br>infection.|endpoint process<br>tree,<br>browser/download<br>history, user<br>interview, EDR<br>telemetry.|
|---|---|---|---|---|
|Day 0, 01:15|E02|Microsoft 365<br>audit records<br>finance officer<br>account sign-in<br>from unfamiliar IP<br>and new device.|Possible credential theft,<br>token theft or<br>unauthorized session.|MFA result,<br>conditional access,<br>sign-in risk, device<br>registration,<br>token/session<br>activity, mailbox<br>audit and user<br>confirmation.|
|Day 0, 01:35|E03|svc_backup made<br>remote<br>administrator<br>logons to FS01 and<br>BAK01 outside<br>normal schedule.|Possible attacker use of a<br>privileged service<br>account or legitimate but<br>undocumented<br>maintenance.|ii<br>Windows Security<br>logs, logon<br>type/source host,<br>task scheduler,<br>service logs, vault<br>records,<br>administrator<br>interview, account<br>activity across<br>systems.|
|Day 0, 01:50|E04|Proxy shows 1.2<br>GB HTTPS upload<br>from FIN-PC07 to<br>unfamiliar cloud<br>storage.|Possible data exfiltration,<br>cloud sync, legitimate<br>transfer or unrelated<br>application traffic;<br>content unknown.|Proxy/DNS/TLS logs,<br>destination<br>ownership,<br>process/network<br>correlation,<br>endpoint artifacts,<br>cloud-provider logs<br>if obtainable, file<br>access audit.|
|Day 0, 02:00|E05|Backup console<br>reports successful<br>FS01 and LMS01<br>jobs.|Job completion is<br>observed, not<br>recoverability or<br>cleanliness.|Backup catalog and<br>snapshot metadata,<br>integrity checks,<br>restore in isolated<br>lab, backup<br>access/audit logs<br>and comparison<br>with known-good<br>records.|
|Day 0, 08:30|E06|EDR/helpdesk<br>reports mass file<br>changes on FIN-<br>PC07; .locked files<br>and ransom note<br>on FS01 shares.|Strong ransomware<br>evidence; family and<br>spread mechanism<br>unknown.|Preserve ransom<br>note, sample<br>encrypted files,<br>process/memory<br>and disk images<br>where feasible, EDR<br>timeline, file server<br>logs, malware<br>|
|Day0,08:40|E07|LMS loginpage|Availabilityfailure likely|analysis.<br>LMS application|



|||loads but materials<br>and assessment<br>submissions fail.|caused by FS01<br>dependency, but direct<br>LMS compromise is<br>unresolved.|logs, FS01<br>mounts/dependency<br>checks, database<br>and web logs, health<br>checks, integrity<br>validation.|
|---|---|---|---|---|
|Day 0, 08:45|E08|Incident contact<br>list on FS01<br>inaccessible; staff<br>phones available;<br>corporate email<br>trust unverified.|Incident is underway and<br>normal coordination<br>tools may be unsafe or<br>unavailable.|Use phone/out-of-<br>band channel; verify<br>alternate contact<br>copy and corporate<br>identity/security<br>status<br>independently.|



### 1.11 Plausible explanations for ambiguous evidence 

E02 and E03 may reflect a coordinated intrusion: an attacker steals the finance officer’s credentials or session, uses the workstation as a foothold, then accesses the privileged svc_backup account to reach file and backup systems. The chronology is compatible with this theory but does not prove that the same actor controlled these events. Alternative explanation 1: the finance officer legitimately signed in from a new device or VPN, while svc_backup activity came from scheduled maintenance or an administrator task that was not documented. This must be tested against staff schedules, source hosts, MFA records and change tickets. Alternative explanation 2: the 1.2 GB E04 transfer may be a legitimate cloud backup/synchronization or another application upload rather than exfiltration; destination, process and file-access correlation are needed before calling it stolen data. A further possibility is that ransomware and the upload are related but the upload contained non-personal files, encrypted material or routine data. No evidence supplied establishes attacker identity or proves personal-data theft. 

### 1.2 Incident type and severity 

|Tier|Operational criteria<br>i|Example trigger|
|---|---|---|
|Critical|Active compromise with<br>widespread or mission-critical<br>outage; ransomware or destructive<br>activity affecting core services;<br>privileged/backup compromise<br>plausible; potential serious|Confirmed ransomware plus<br>FS01/LMS disruption and backup<br>trust uncertainty.|



||personal-data exposure; urgent<br>executive command.<br>i||
|---|---|---|
|High|Confirmed security incident with<br>material impact to a business<br>service or sensitive system, but<br>scope is contained and recovery<br>path is reasonablytrusted.|One department’s system<br>encrypted; critical services remain<br>available; no evidence of<br>privileged/backup access.|
|Medium|Limited and contained compromise<br>with manageable impact, no<br>material critical-service outage and<br>a clear remediationpath.<br>i|Single endpoint malware blocked<br>before lateral movement.|
|Low|Unconfirmed alert or minor policy<br>event with no evidence of<br>compromise or meaningful<br>business impact.|Benign attachment or isolated<br>blocked event after validation.|



**Selected level** : CRITICAL (provisional). E06 confirms ransomware indicators on a workstation and shared file service; E07 confirms loss of learner-facing functions before an 11:00 examination for 100 learners. E03 places a highly privileged account on both FS01 and BAK01 outside its normal schedule, and E05 cannot establish backup integrity. E04 introduces a possible confidentiality impact involving 1.2 GB transferred externally. Critical severity is justified by simultaneous availability impact, integrity loss, and material uncertainty around confidentiality and recoverability. Reassess at 10:00 WAT and whenever new evidence materially changes scope. Do not downgrade solely because the portal login page responds. 

### 1.3 Asset scope and CIA assessment 

|Asset|Status<br>i|Confidentiality|Integrity|Availabilit<br>y|Priority|
|---|---|---|---|---|---|
|FIN-PC07|Confirmed<br>affected: mass<br>file changes;<br>attachment<br>opened; 1.2 GB<br>outbound<br>transfer (E01,<br>E04,E06).<br>i|Potentially high<br>risk: user data,<br>credentials or files<br>may have been<br>exposed; content<br>unknown.|High concern: mass<br>changes and<br>ransomware indicators.|High impact<br>to finance<br>workstation.|P1: isolate,<br>preserve,<br>investigate<br>entry and<br>transfer.|
|FS01|Confirmed<br>affected: .locked<br>files and ransom<br>note; svc_backup<br>admin logon<br>(E03,E06).|Potential exposure<br>through broad file<br>access; not<br>proven.|Confirmed loss of file<br>integrity/availability for<br>affected shares.|High; portal<br>depends on<br>files stored<br>here.|P1: contain,<br>preserve<br>server<br>evidence,<br>protect<br>shares.|
|LMS01|Portal front page<br>loads; learning<br>materials/submis|Unknown; direct<br>compromise not<br>established.|Unknown; dependency<br>failures may mimic<br>corruption.|Confirmed<br>learner-<br>facing|P1 business<br>continuity;<br>isolate if|



||sions unavailable<br>(E07); backup job<br>reported success<br>(E05).|||functional<br>outage.|evidence<br>indicates<br>direct<br>compromise<br>.|
|---|---|---|---|---|---|
|BAK01|Potentially<br>exposed:<br>svc_backup<br>admin logon<br>(E03); network<br>backups non-<br>immutable;<br>restore not<br>tested.|Backup data may<br>be accessible to<br>attacker.|Backup<br>deletion/alteration/poi<br>soning possible,<br>unconfirmed.|Recoverabili<br>ty unknown;<br>disconnecte<br>d copy<br>exists at Day<br>−3 17:00.|P1: isolate<br>backup<br>plane,<br>disable<br>suspect<br>access,<br>preserve<br>audit logs.|
|Microsoft<br>365<br>identity/em<br>ail|Unfamiliar sign-in<br>for finance officer<br>(E02); email trust<br>unverified (E08).|Potential<br>account/mailbox/t<br>oken exposure.|Mailbox rules, OAuth<br>grants or identity<br>settings may be altered;<br>unknown.|Identity/<br>email<br>continuity<br>and trusted<br>communica<br>tion at risk.|P1: review<br>sign-in,<br>revoke<br>sessions/tok<br>ens and<br>reset<br>credentials<br>safely.|
|Learner/<br>finance data|Stored across<br>institute systems;<br>1.2 GB upload has<br>unknown content<br>(E04).|Potential personal-<br>data breach; not<br>confirmed.|Assessment and<br>finance records may be<br>altered/encrypted.|Access<br>disrupted<br>where<br>dependent<br>on<br>FS01/LMS01<br>.|P1<br>legal/privacy<br>assessment<br>and integrity<br>checks.|



### 1.4 Investigation priorities 

|Priority question|Validation check|Findings that changes scope<br>i|
|---|---|---|
|1. Did FIN-PC07 execute malicious<br>code, and what initiated it?|Acquire email with<br>headers/attachment; EDR timeline,<br>process tree, persistence artefacts,<br>disk and memory capture if<br>feasible; interview finance officer.|Malicious process or credential<br>theft confirms phishing/endpoint<br>entry and guides containment;<br>benign attachment shifts likely<br>entry elsewhere but does not<br>remove confirmed ransomware.<br>i|
|2. What was uploaded in E04 and<br>was it personal or confidential<br>data?|Correlate proxy destination, DNS,<br>TLS metadata, endpoint process,<br>file access and cloud sync logs;<br>preserve relevant M365 and proxy<br>logs.|Evidence of learner/finance records<br>leaving the environment increases<br>confidentiality severity and may<br>trigger NDPA notification analysis;<br>legitimate sync or non-sensitive<br>content narrows the breach<br>hypothesis.|
|3. Was svc_backup abused and<br>were backups altered or accessed?|Review Windows logon source,<br>account authentication, backup<br>audit logs,snapshot metadata,|Unauthorized use or backup<br>tampering confirms broader<br>compromise and removes network|



||deletion events, privilege changes<br>and administrator change tickets.|backups from consideration until<br>validated; authorized change with<br>corroboration lowers this<br>hypothesis.<br>i   ii|
|---|---|---|
|4. Is there lateral movement or<br>identity compromise beyond listed<br>systems?|Search EDR, Windows event logs,<br>firewall/proxy/DNS, M365 audit,<br>mailbox rules, OAuth app consent,<br>new users, scheduled tasks and<br>remote tools across all assets.|Additional hosts, admin identities,<br>or cloud persistence expands scope<br>and may require wider network<br>isolation and more external<br>incident-response support.<br>t|
|5. Can any backup be restored<br>cleanly and is the LMS itself intact?|Use a segregated recovery lab;<br>verify snapshot timestamps,<br>malware scan, file-system/database<br>integrity, application dependency<br>tests and known-good baselines.|Unreadable, encrypted or attacker-<br>modified backup extends outage<br>and forces rebuild/manual<br>continuity; verified clean restore<br>allows phased recovery but does<br>notprove no data theft.|



## Task 2: Incident response plan and team responsibilities 

### 2.1 Policy and operating plan 

**Mission** : protect learners, staff, information and essential education services by detecting, containing, investigating and recovering from cyber incidents in a controlled, evidence-led manner. Objectives are to limit harm, preserve reliable evidence, maintain essential operations, meet legal/contractual duties, restore trusted systems, and turn lessons into improvements. 

**Scope** : all Northbridge staff, contractors, endpoints, servers, networks, Microsoft 365 identities, cloud services, backups, learner and finance data, and third parties supporting these assets. The plan is activated for credible ransomware, unauthorized privileged access, suspected data exfiltration, critical service outage, compromise of backup or identity systems, or any event whose impact is uncertain but potentially severe. 

**Declaration and activation** : the Incident Response Lead (IR Lead) declares and logs the incident immediately on credible evidence. E06–E08 meet the Critical threshold; declare Critical at 08:45 WAT. The IR Lead starts a bridge by staff phone/out-of-band channel, appoints a scribe, assigns incident ID NBSI-IR-2026-001 (fictional tracking ID), and notifies the executive sponsor, IT/security, business continuity, legal/privacy and communications leads. Corporate email is treated as untrusted until reviewed. 

**Authority** : the IR Lead may order immediate isolation of an actively encrypting or spreading host, suspend a suspected compromised account, block a malicious destination, and disconnect a threatened segment where delay would increase harm. For broad outages, destructive 

changes, external notifications, ransom decisions, or service restoration, obtain executive approval with legal/privacy and business-owner consultation when time permits. No approval is required before emergency isolation to stop ongoing encryption; record the decision, reason, timestamp, evidence preserved or lost, and operational effect. No ransom payment is authorized by this plan. 

**Documentation** : maintain a time-stamped incident log; record source and confidence for every claim; preserve original evidence read-only; use unique evidence IDs, SHA-256 hashes after collection, access logs and chain-of-custody signatures; record time zone as WAT; maintain decision, communication, system-change and recovery logs. Store incident records in restricted offline or independently secured storage, not FS01 or the potentially compromised corporate mailbox. 

**Escalation** : Critical incidents require immediate executive notification and continuous coordination; High incidents notify management within 30 minutes of validation; Medium/Low incidents follow normal ticketing but escalate if scope expands. Engage external IR/forensics, managed security provider, insurer (if applicable), cloud/M365 provider, legal counsel and law enforcement as advised. Review the plan after any major incident/exercise and at least annually. 

**Business continuity connection** : IR controls the threat; business continuity maintains critical functions while disruption continues; disaster recovery restores technology. Activate BC/DR alongside IR because LMS01 depends on FS01. Use an alternate examination arrangement approved by the academic owner rather than restoring an unsafe system to meet the deadline. RTO/RPO are proposed targets pending formal BIA and funding approval. 

### 2.2 CSIRT structure 

|Role|Core responsibility<br>i|Coverage|
|---|---|---|
|Incident Commander / IR Lead|Own incident objectives, severity,<br>decision log, coordination,<br>escalation, and executive situation<br>reports.|Security Analyst 1 acts as IR Lead<br>initially; executive sponsor remains<br>accountable for business risk.|
|Security Analyst / Threat Hunter|Correlate EDR, endpoint, network,<br>Windows, and M365 evidence;<br>map scope and IOCs; recommend<br>containment.<br>i  i|Security Analyst 2 / available<br>analyst; all work documented.|
|Digital Forensics Lead|Plan acquisition, preserve volatile<br>data, image systems, hash and<br>maintain custody; coordinate<br>malware analysis.<br>i|External IR/forensics provider on<br>call; internal analyst supports<br>collection.|
|IT Operations Lead|Execute network isolation, account<br>controls, rebuilds, patching,<br>backups and technical validation.|IT Lead; two other IT staff assigned<br>by workload.|



|Business Service Owner / Academic<br>Continuity Lead|Assess learner impact, approve<br>alternate examination process and<br>validate business service readiness.|Head of Academic Operations<br>(fictional role).|
|---|---|---|
|Legal Counsel / Data Protection<br>Officer|Assess privilege, contractual and<br>statutory duties; direct breach-risk<br>analysis and regulator/data-subject<br>advice.<br>f|External counsel/DPO service until<br>formal role appointed.|
|Communications Lead|Prepare approved learner, staff,<br>media and partner updates;<br>monitor rumours and maintain<br>message log.<br>i|Communications Manager.|
|Executive Sponsor|Approve risk acceptance, significant<br>expenditure, service return and<br>external notifications where<br>required by governance.|Chief Operating Officer (fictional<br>role).|
|Scribe / Incident Coordinator|Maintain chronology, decisions,<br>action owners, meeting cadence,<br>and evidence index.|Operations administrator using<br>offline template.|



The core team is small, so roles may be combined operationally, but accountability must remain explicit. The forensics and legal roles should be contracted in advance with 24/7 escalation contacts. During this incident, a scribe should be appointed from operations and request temporary support from the M365 provider and a qualified external incident-response firm. Do not let the same person perform a sensitive evidence acquisition and independently approve its integrity where avoidable. 

### 2.3 RACI matrix 

R = Responsible (does the work); A = Accountable (one role answerable for outcome); C = Consulted (provides input before decision); I = Informed (kept updated). Each activity has exactly one A and at least one R. 

|Activity|Responsible(R)|Accountable(A)|Consulted(C)|Informed(I)<br>i|
|---|---|---|---|---|
|Detect and declare<br>incident|Security Analyst|IR Lead|IT Lead; service<br>owner|Executive sponsor;<br>CSIRT<br>i|
|Analyze scope and<br>severity|Security Analyst;<br>Forensics Lead|IR Lead|IT Lead;<br>Legal/Privacy|Executive sponsor;<br>service owner|
|Contain endpoints,<br>network and<br>accounts|IT Lead; Security<br>Analyst|IR Lead|Forensics Lead;<br>M365 provider|Executive sponsor;<br>service owner|
|Collect and<br>preserve evidence|Forensics Lead|IR Lead|Legal Counsel;<br>SecurityAnalyst|Executive sponsor<br>f|
|Stakeholder<br>communications<br>i|Communications<br>Lead|Executive sponsor<br>i|Legal/Privacy;<br>service owner; IR<br>Lead|Learners; staff;<br>partners as<br>approved|
|Assess reporting|Legal/Privacy|Executive sponsor|IR Lead;|Relevant|



|obligations|||Communications<br>Lead|regulators/data<br>subjects as advised|
|---|---|---|---|---|
|Select and validate<br>backup / restore|IT Lead; Forensics<br>Lead|IT Lead|Security Analyst;<br>service owner; IR<br>Lead|Executive sponsor;<br>Communications<br>Lead|
|Approve service<br>return|IT Lead; Security<br>Analyst|Executive sponsor|Service owner;<br>Legal/Privacy; IR<br>Lead|Learners; staff|
|Close incident and<br>review|Scribe; Security<br>Analyst|IR Lead|All CSIRT roles;<br>Legal/Privacy;<br>service owner|Executive sponsor;<br>staff|



### 2.4 Readiness improvement checklist 

|Improvement|Owner|Resources|Readiness Check|
|---|---|---|---|
|Maintain offline incident<br>contacts and printed<br>quick-start plan|Operations Manager|Encrypted offline copy,<br>printed sealed copy,<br>phone tree, quarterly<br>owner review.|Quarterly call-tree test<br>reaches 90% of<br>primary/alternate<br>contacts within 15<br>minutes; test evidence<br>retained.|
|Create immutable/offline<br>backup tier with separate<br>credentials|IT Lead|Budget for immutable<br>object lock or offline<br>media, separate admin<br>identity, MFA, network<br>segmentation.|Monthly audit shows<br>production credentials<br>cannot delete protected<br>copies; quarterly restore<br>reportpasses.<br>i|
|Test FS01/LMS01 restore<br>and define RTO/RPO|BC/DR Coordinator +<br>service owner|Isolated test<br>environment, application<br>owner, representative<br>datasets, time budget.|Quarterly restoration test<br>verifies application<br>dependencies, file<br>integrity and actual<br>RTO/RPO.<br>i|
|Complete server<br>EDR/logging coverage and<br>centralize logs|Security Lead|Server EDR licences,<br>SIEM/log storage,<br>retention, alert rules.|100% critical servers<br>enrolled; alerts tested for<br>mass file change,<br>privileged logon and<br>backupdeletion.|
|Harden privileged/service<br>accounts|Identity Administrator|Vault, gMSA or equivalent<br>managed service identity,<br>least privilege, MFA<br>where supported,<br>rotationplan.|No shared admin<br>credentials; svc_backup<br>cannot interactively log<br>on; quarterly access<br>review signed.|
|Prepare out-of-band<br>communications and<br>templates|Communications Lead|Phone bridge, SMS tree,<br>alternate collaboration<br>channel, pre-approved<br>holdingstatements.|Quarterly exercise<br>launches bridge without<br>corporate email in under<br>10 minutes.|
|Run role-specific tabletop<br>and phishing exercises|HR + IR Lead|Two hours quarterly,<br>facilitator, scenarios,<br>attendance records.|At least 90% role<br>attendance; action log<br>assigns owners and due<br>dates; overdue actions<br>escalated.|



## Task 3 — Response playbook and evidence preservation 

### 3.1 Ordered response playbook 

The first 60 minutes run from 08:45 to 09:45 WAT. The playbook is deliberately parallelized: one team member contains while another preserves evidence and the business owner arranges continuity. Approval means the role authorized under the policy; emergency isolation is preauthorized 

|08:45<br>–<br>08:50|Declare Critical incident; assign<br>ID; open offline log; phone<br>executive sponsor and core<br>CSIRT; treat email as untrusted.|IR Lead +<br>scribe|IR Lead; no<br>delay for<br>emergency<br>declaration|E01–E08<br>references; initial<br>situation report;<br>call log; incident<br>log.|Critical status<br>and owners<br>recorded; bridge<br>confirmed.|
|---|---|---|---|---|---|
|08:50<br>–<br>08:55|Isolate FIN-PC07 from<br>wired/Wi-Fi network using<br>switch/EDR quarantine; do not<br>power off if network isolation<br>works.|IT Lead|IR Lead<br>pre-<br>authorized|EDR alert/export,<br>host name,<br>isolation time,<br>operator and<br>effect.|Network<br>telemetry shows<br>host<br>disconnected;<br>user notified by<br>phone.|
|08:50<br>–<br>09:00|Isolate FS01 or affected<br>shares/network segment to<br>stop active encryption; preserve<br>business dependency map; do<br>not leave writable shares<br>exposed.|IT Lead|IR Lead;<br>executive<br>informed<br>promptly|Server logs, share<br>list, console<br>screenshots/expo<br>rt, change record.|No further mass<br>changes in<br>monitored<br>window; any<br>outage<br>documented.|
|08:55<br>–<br>09:05|Protect BAK01: remove from<br>production network or block<br>inbound admin access; suspend<br>suspect svc_backup use after<br>preserving available<br>authentication logs; use<br>separate clean admin identity.|IT Lead +<br>Security<br>Analyst|IR Lead;<br>urgent<br>containme<br>nt<br>authority|Backup audit<br>logs, account<br>state, firewall<br>changes, service<br>dependencies.|No production<br>host can write to<br>backup<br>repository;<br>break-glass<br>access verified.|
|08:55<br>–<br>09:10|Review M365 sign-in; revoke<br>finance officer sessions/tokens<br>if compromise is credible; reset<br>credentials from clean device;<br>check MFA, mailbox rules,<br>OAuth grants, new devices and<br>admin role changes.|Identity/<br>M365 admin|IR Lead;<br>legal<br>consulted if<br>needed|Sign-in and audit<br>exports before<br>changes where<br>feasible; account<br>action log.|Sessions revoked,<br>credentials reset,<br>MFA validated,<br>suspicious<br>rules/apps<br>triaged.|
|09:00<br>–<br>09:15|Preserve volatile and high-value<br>evidence from FIN-PC07 and<br>FS01 where safe: memory<br>capture first if feasible,then|Forensics<br>Lead /<br>external<br>provider|IR Lead;<br>emergency<br>containme<br>nt takes|Memory/disk<br>acquisition logs,<br>original files,<br>hashes after|Evidence IDs<br>assigned;<br>originals read-<br>only;anymissed|



||forensic images/log exports;<br>retain ransom note and<br>encrypted samples.||precedence|collection,<br>tool/version,<br>operator and<br>times.|volatile evidence<br>recorded.|
|---|---|---|---|---|---|
|09:05<br>–<br>09:20|Block confirmed malicious<br>destination/IP/domain only<br>after validating indicators;<br>inspect proxy, DNS, firewall and<br>endpoint connections for<br>related transfer activity.|Security<br>Analyst +<br>network<br>admin|IR Lead|i<br>Proxy/firewall/<br>DNS exports, rule<br>changes,<br>indicator<br>confidence and<br>source.|Block tested;<br>monitoring<br>shows no<br>repeated<br>connection;<br>avoid broad<br>block without<br>evidence.|
|09:10<br>–<br>09:25|Open an offline scope<br>worksheet; hunt for mass file<br>changes, remote logons, new<br>admin accounts, scheduled<br>tasks, remote tools, backup<br>deletions and unusual M365<br>events across all assets.|Security<br>Analyst|IR Lead|Search queries,<br>log export ranges,<br>host list,<br>negative/positive<br>findings.|All known assets<br>checked or<br>explicitly marked<br>not yet checked;<br>scope gaps<br>assigned.|
|09:15<br>–<br>09:30|Activate business continuity:<br>decide exam contingency<br>(postpone, reschedule or<br>secure alternate delivery) with<br>academic owner; issue holding<br>notice before 10:00.|Academic<br>Service<br>Owner +<br>Comms Lead|Executive<br>sponsor<br>approves<br>public<br>wording;<br>academic<br>owner<br>approves<br>exam<br>decision|Decision log,<br>approval,<br>recipient list, sent<br>copy and<br>timestamp.|Learners receive<br>actionable<br>notice; no<br>promise of 11:00<br>access until<br>validated.|
|09:20<br>–<br>09:35|Engage external IR/forensics,<br>M365 provider and backup<br>vendor; request preservation of<br>relevant cloud/service logs and<br>support case IDs.|IR Lead|Executive<br>sponsor for<br>expenditur<br>e if<br>required|Vendor ticket IDs,<br>requests,<br>responses,<br>contact log.|Acknowledgeme<br>nts and ETA<br>logged; missing<br>access escalated.|
|09:30<br>–<br>09:45|Issue first executive situation<br>report; establish update<br>cadence every 30 minutes; set<br>next severity review and<br>legal/privacytriage meeting.<br>i  i|IR Lead +<br>scribe|Executive<br>sponsor<br>receives;<br>legal/privac<br>yconsulted|Situation report<br>version,<br>decision/actions<br>register.|Open risks,<br>unknowns,<br>decisions and<br>owners visible to<br>all responders.|
|09:45<br>–<br>10:30|Continue threat hunting and<br>assess scope across Microsoft<br>365, endpoint fleet, server logs<br>and network telemetry; collect<br>evidence before destructive<br>cleanupwhere feasible.|Security<br>Analyst +<br>Forensics|IR Lead|IOC sweep<br>results, affected-<br>host inventory,<br>log retention<br>requests.|Scope report<br>distinguishes<br>confirmed,<br>suspected and<br>not observed.|
|10:00<br>–<br>11:00|Assess backup candidates in<br>isolated lab; do not restore to<br>production. Start with<br>disconnected Friday copy only<br>after malware, integrity, access-<br>history and catalog checks; test<br>latest network backup|IT Lead +<br>Forensics|IR Lead;<br>business<br>owner<br>consulted|Backup<br>metadata, read-<br>only copy, test<br>logs, validation<br>report.|Candidate is<br>accepted/rejecte<br>d with<br>documented<br>criteria; no<br>unverified<br>backupreaches|



||separatelyas untrusted.||||production.|
|---|---|---|---|---|---|
|11:00<br>–<br>14:00|Perform longer-term<br>containment: segment finance,<br>server, identity/admin and<br>recovery networks; disable<br>unused remote access; rotate<br>privileged credentials from<br>clean systems; remove<br>compromised sessions and<br>review service-account scope.|IT Lead +<br>Identity<br>admin|IR Lead;<br>executive<br>sponsor<br>informed|Network<br>diagrams, config<br>diff, access<br>review,<br>rotation/change<br>records.|Attack paths<br>blocked;<br>svc_backup least<br>privilege restored<br>only after<br>compromise<br>assessed.|
|14:00<br>–<br>18:00|Determine likely entry point<br>and persistence; analyze<br>attachment, process chain,<br>scheduled tasks, services,<br>registry autoruns, WMI, remote<br>tools, mailbox rules, OAuth<br>grants and new identities; patch<br>exploited weaknesses.|Forensics +<br>Security<br>Analyst|IR Lead|Forensic report,<br>malware<br>indicators, patch<br>records,<br>persistence<br>checklist.|Root-cause<br>confidence<br>stated;<br>persistence hunt<br>covers endpoints,<br>servers and cloud<br>identity.|
|18:00<br>–<br>22:00|Build clean recovery<br>environment; rebuild<br>compromised systems from<br>known-good images rather than<br>trusting in-place cleanup;<br>restore validated data to<br>isolated network; test LMS<br>dependencyon FS01.|IT Lead|IR Lead and<br>business<br>owner;<br>executive<br>sponsor<br>approves<br>major<br>spend|Build logs, image<br>provenance,<br>restore logs, data<br>validation results.|Security checks<br>pass; no direct<br>production<br>connection until<br>acceptance gate.|
|22:00<br>–<br>08:45<br>Day<br>+1|Run enhanced monitoring;<br>check<br>EDR/Windows/M365/proxy/bac<br>kup logs for renewed<br>encryption, suspicious logons,<br>exfiltration or persistence;<br>complete initial privacy risk<br>assessment and notification<br>decision.|Security<br>Analyst +<br>Legal/Privac<br>y|Executive<br>sponsor|Monitoring<br>snapshots,<br>incident log,<br>privacy<br>assessment,<br>regulator<br>decision record.|No unexplained<br>recurrence<br>during defined<br>observation<br>window; legal<br>deadlines<br>tracked.|
|Befor<br>e<br>servic<br>e<br>return|Service owner validates critical<br>workflows, access controls,<br>records and learner submission<br>path; IR Lead confirms<br>containment; executive sponsor<br>authorizesphased return.|Service<br>owner + IT<br>Lead|Executive<br>sponsor; IR<br>Lead and<br>Legal/Priva<br>cy<br>consulted|Acceptance<br>checklist, test<br>results, rollback<br>plan, approval<br>timestamp.|Written approval;<br>communications<br>lead issues<br>confirmed status.|
|Withi<br>n 14<br>days<br>of<br>closur<br>e|Hold blameless review,<br>reconstruct timeline, quantify<br>impact, assess root cause and<br>assign corrective actions;<br>review plan and BC/DR<br>dependencies.|IR Lead +<br>scribe|Executive<br>sponsor|Final report,<br>lessons learned,<br>action register,<br>updated plan.|Every action has<br>owner, due date,<br>metric and<br>closure evidence.|
|08:45<br>–<br>08:50|Declare Critical incident; assign<br>ID; open offline log; phone<br>executive sponsor and core<br>CSIRT; treat email as untrusted.|IR Lead +<br>scribe|IR Lead; no<br>delay for<br>emergency<br>declaration|E01–E08<br>references; initial<br>situation report;<br>call log; incident<br>log.|Critical status<br>and owners<br>recorded; bridge<br>confirmed.|



### 3.2 Difficult decisions and trade-offs 

Containment versus evidence: active encryption and lateral movement can destroy more data than a rapid containment action. The default is network isolation rather than shutdown because isolation may preserve volatile memory and running-process evidence. Capture memory and key logs first only if this can be done immediately and safely; do not delay isolation while ransomware continues. If the host cannot be disconnected or spread is imminent, power it down as a last resort, recording who authorized it, the time, why isolation failed, likely loss of volatile evidence, and resulting operational impact. This follows CISA’s guidance to isolate impacted systems immediately and recognizes that powering off can destroy volatile artifacts (CISA et al., 2023). 

Examination access versus backup safety: do not restore the latest network backup directly to production merely to meet 11:00. E03 makes BAK01 potentially exposed; E05 reports job success only, while the backups are non-immutable and untested for 90 days. A short exam postponement is preferable to reintroducing malware or overwriting evidence. The academic owner should decide an alternative exam arrangement and communicate it promptly. A controlled, read-only copy of a candidate backup can be tested in an isolated recovery VLAN; only a validated clean build may progress through a service-return gate. 

### 3.3 Evidence register and custody 

All rows below are proposed collections, not completed acquisitions. Date/time is recorded relative to scenario Day 0 because no calendar date is provided. The collector records the actual timestamp in WAT at collection and never fills in a hash until it has been computed. 

|Recor<br>d ID|Source|Proposed<br>collection|Collector<br>role|Date &<br>Time<br>(WAT)|Integrity<br>check|Secure<br>storage|Custody|
|---|---|---|---|---|---|---|---|
|EV-01|FIN-PC07<br>endpoint,<br>email/attachm<br>ent, EDR<br>telemetry;<br>linked to<br>E01/E04/E06|Planned:<br>preserve<br>original email<br>with headers<br>and attachment;<br>capture EDR<br>export, volatile<br>memory if<br>feasible,<br>forensic disk<br>image; isolate<br>first if<br>encryption<br>active.|Forensics<br>Lead;<br>external<br>specialist if<br>available|Day 0,<br>planned<br>after<br>08:45<br>WAT;<br>exact<br>collectio<br>n time to<br>be<br>entered<br>at<br>acquisiti<br>on.|SHA-256<br>of<br>acquired<br>image/exp<br>ort and<br>original<br>attachmen<br>t; hash not<br>yet<br>available.|Restricted<br>evidence<br>vault on<br>clean<br>encrypted<br>media,<br>separate<br>from<br>FS01/BAK01;<br>read-only<br>master plus<br>working<br>copy.|Collector<br>→<br>evidence<br>custodian;<br>each<br>transfer<br>signed/ti<br>me-<br>stamped;<br>not yet<br>collected.|



|EV-02|FS01 file<br>server, .locked<br>files, ransom<br>note,<br>Windows/shar<br>e logs;<br>E03/E06|Planned: export<br>security/system<br>logs, preserve<br>ransom note<br>and<br>representative<br>encrypted files,<br>memory/disk<br>image if safe;<br>record share<br>paths and file<br>timestamps.|Forensics<br>Lead + IT<br>Lead|Day 0,<br>planned;<br>exact<br>WAT<br>time<br>pending.|Hash each<br>collected<br>file/image<br>using<br>SHA-256;<br>no hash<br>claimed<br>yet.|Restricted<br>evidence<br>vault;<br>encrypted<br>storage;<br>access by IR<br>Lead,<br>forensics<br>and legal-<br>approved<br>staff only.|IT<br>operator<br>to<br>forensic<br>collector,<br>then<br>custodian;<br>signatures<br>and<br>transfer<br>times<br>pending.|
|---|---|---|---|---|---|---|---|
|EV-03|BAK01 backup<br>console/catalo<br>g, snapshots<br>and access<br>logs; E03/E05|i<br>Planned: export<br>audit/config/cat<br>alog before<br>changing<br>retention;<br>preserve<br>snapshot<br>metadata;<br>acquire read-<br>only candidate<br>copy for isolated<br>validation.|IT Lead<br>with<br>Forensics<br>Lead<br>oversight|Day 0,<br>planned<br>after<br>isolation;<br>exact<br>WAT<br>time<br>pending.|Hash<br>export and<br>copied<br>backup<br>manifest;<br>verify copy<br>consistenc<br>y; results<br>pending.|Separate<br>clean<br>recovery<br>repository;<br>never place<br>only copy on<br>suspect<br>BAK01.|Backup<br>admin to<br>forensic<br>custodian;<br>all<br>transfers<br>logged;<br>not yet<br>collected.|
|EV-04|Microsoft 365<br>sign-in, audit,<br>mailbox rules,<br>OAuth and<br>device records;<br>E02/E08|i<br>Planned: export<br>sign-in and audit<br>logs, preserve<br>mailbox<br>metadata and<br>relevant<br>messages,<br>record<br>session/token<br>revocation and<br>account<br>changes.|Identity<br>Administra<br>tor /<br>Security<br>Analyst|Day 0,<br>planned;<br>exact<br>WAT<br>time<br>pending.|Hash<br>exported<br>files after<br>download;<br>capture<br>export<br>query,<br>tenant,<br>time range<br>and tool<br>details.|Encrypted<br>evidence<br>vault with<br>restricted<br>identity/priv<br>acy access;<br>do not<br>circulate<br>sensitive<br>logs by<br>email.|Identity<br>admin to<br>evidence<br>custodian;<br>transfer<br>log<br>pending.|
|EV-05|Proxy, DNS,<br>firewall and<br>destination<br>metadata for<br>1.2 GB<br>transfer; E04|Planned: export<br>relevant<br>flow/proxy<br>records,<br>timestamps,<br>destination<br>hostname/IP,<br>process<br>correlation and<br>any cloud<br>provider<br>request/respons<br>e.|Security<br>Analyst /<br>network<br>admin|Day 0,<br>planned;<br>exact<br>WAT<br>time<br>pending.|SHA-256<br>of exports;<br>document<br>clock<br>offsets and<br>retention<br>gaps;<br>values<br>pending.|Restricted<br>evidence<br>vault;<br>redacted<br>working<br>copies for<br>broader<br>team.|Network<br>admin to<br>evidence<br>custodian;<br>pending.|



Access restrictions: use least privilege, multifactor authentication and an access register; preserve a read-only master and analyze working copies. The evidence custodian logs evidence ID, description, collector, source system, acquisition tool/version, timestamps, hash, storage location, access, transfer reason, sender/receiver signatures and seal status. Retain evidence under legal hold where advised. Never put evidence in public cloud storage or the possibly compromised corporate email. If a log is unavailable, document the source, reason, retention gap and effect on confidence; do not imply absence of evidence proves absence of activity. 

### 3.4 Eradication, credentials and broader compromise 

- **Entry point** : inspect the payroll attachment and message headers; validate execution and exploit path; remove malicious email from mailboxes if safe, block sender/domain/attachment indicators, harden attachment sandboxing and macro/script controls, and patch the exploited application or OS. Do not conclude phishing was the root cause until artifacts support it. 

- **Persistence** : examine startup folders, registry autoruns, scheduled tasks, services, WMI subscriptions, remote management tools, new accounts, local/domain admin group changes, web shells, cloud app grants, mailbox rules and identity federation changes. Rebuild compromised endpoints and servers from known-good images where possible rather than relying on uncertain cleanup. 

- **Credentials** : from a clean admin workstation, disable or rotate compromised user and administrator credentials, revoke M365 refresh tokens/sessions, inspect MFA methods and app consents, remove unauthorized devices, enforce phishing-resistant MFA where supported, and reset privileged credentials after containment. Replace svc_backup with a managed identity or unique vaulted credential, restrict it to necessary backup operations, deny interactive logon and separate backup administration from production administration. 

- **Broader compromise** : correlate EDR, Windows, firewall, DNS/proxy, VPN/remote-access, M365 audit, backup console and identity logs from before E01 through containment. Hunt for shared indicators, lateral movement, privilege escalation, data staging, unusual archive tools, outbound volume spikes, deletion attempts and new persistence. Because server EDR coverage is incomplete, absence of an alert is not proof of a clean server. 

### 3.5 Recovery order and acceptance criteria 

|Tier|Proposed target|Recoveryapproach|
|---|---|---|
|0 — Control plane / trusted<br>administration|Begin immediately; no production<br>RTO until identity trust is restored.|Create clean admin devices and<br>recovery network; secure M365<br>admin identities; confirm DNS,<br>network, time and logging;<br>independent credentials and MFA.|
|1 — Backup integrity and evidence|Validate candidates before any<br>restore;do not use a fixed deadline|Protect disconnected Day −3 17:00<br>copy;inspect chain/access history,|



||as a substitute for proof.|catalog, malware and integrity; test<br>restore in isolated lab. Treat latest<br>network backup as untrusted until<br>proven clean.|
|---|---|---|
|2 — FS01 data and core file service|Proposed RTO 24–48 hours after<br>clean environment is ready;<br>proposed RPO up to 3 days only if<br>business owner accepts it.|Rebuild from clean image; restore<br>files from validated point; scan;<br>compare hashes/manifests where<br>available; validate permissions,<br>timestamps and representative<br>business records.<br>i  t|
|3 — LMS01 / examination function|Target decision on exam<br>contingency before 10:00; restore<br>target 24–48 hours if safe,<br>otherwise reschedule/alternate<br>delivery.|Restore application only after FS01<br>dependency is stable; test login,<br>course materials, submissions,<br>grade/assessment integrity and<br>access from test learner accounts.|
|4 — Finance workstation and<br>finance data|Target 48–72 hours, depending on<br>forensic preservation and data<br>validation.|Reimage FIN-PC07, patch, enroll in<br>EDR, validate finance records<br>against known-good ledger/exports<br>and review anysuspected transfer.|



These are proposed operational targets, not established institutional commitments. RTO is maximum tolerable downtime for a service; RPO is acceptable data loss measured in time. A disconnected backup from Day −3 implies up to roughly three days of data loss relative to Day 0 if it is the only clean copy, but the actual recoverable point must be confirmed. The service owner and executive sponsor must explicitly accept or reject that loss after reconciling FS01/LMS01 data with other trusted sources. 

Backup selection and validation: preserve the original backup; review backup administrator logs and snapshot metadata; select the newest candidate with a credible pre-compromise point and clean access history; copy it to a segregated recovery environment; verify catalog/manifest integrity and scan for known indicators; restore representative files and the full application dependency chain; validate file counts, hashes where known, database consistency, permissions, LMS submission and finance-record samples; run EDR and vulnerability scans; monitor for recurrence for at least 24 hours before broad service return. If the latest backup fails or is suspect, reject it and test the disconnected copy. Business owner signs acceptance, IR Lead confirms containment, IT Lead signs technical validation, and executive sponsor approves return. A rollback path remains available. 

## Task 4 — Communication and escalation 

### 4.1 Communication matrix 

|Stake holder<br>Trigger|Message|Approver|Channel|Frequenc|
|---|---|---|---|---|



|||owner|i||y|
|---|---|---|---|---|---|
|Executive<br>sponsor / board|Critical declaration;<br>every material scope<br>change; restoration<br>decision; legal deadline.|IR Lead|Executive<br>sponsor for<br>external<br>statements; IR<br>Lead for<br>internal<br>situation<br>report.|Phone<br>bridge, SMS,<br>sealed<br>offline brief;<br>not<br>corporate<br>email until<br>trusted.|Every 30<br>minutes for<br>first 4<br>hours, then<br>hourly or on<br>material<br>change.<br>i|
|IT and security<br>staff|Immediate incident<br>activation, isolation<br>orders, scope and<br>change approvals.|IR Lead|IR Lead|Phone/SMS<br>and<br>approved<br>out-of-band<br>bridge.|Continuous<br>bridge;<br>written shift<br>handover<br>every 2<br>hours.|
|Learners /<br>affected cohort|Portal/materials<br>unavailable; examination<br>decision; safety and<br>account advice if<br>needed.|Communication<br>s Lead|Academic<br>Service Owner<br>+ Legal/Privacy;<br>executive<br>sponsor for<br>public message.|SMS to<br>registered<br>contacts,<br>official<br>website<br>status page<br>from clean<br>hosting,<br>phone tree;<br>email only if<br>trusted.|Initial notice<br>before<br>10:00;<br>updates at<br>least hourly<br>until exam<br>decision,<br>then on<br>material<br>change.|
|Teaching staff /<br>learner support|Exam continuity<br>instructions, support<br>script, do-not-speculate<br>guidance.|Academic<br>Service Owner|Communication<br>s Lead|Phone/SMS,<br>staff phone<br>tree,<br>alternate<br>collaboratio<br>n tool.|Every 30–60<br>minutes<br>during exam<br>window.|
|Microsoft 365 /<br>cloud provider|Suspicious sign-in,<br>request log preservation,<br>identity containment<br>assistance.|IT Lead|IR Lead|Provider<br>support<br>portal from<br>clean device<br>or phone<br>support;<br>preserve<br>ticket ID.|Initial<br>request<br>immediately<br>; every 2<br>hours until<br>logs<br>secured.|
|Backup /<br>infrastructure<br>provider|BAK01 access anomaly,<br>snapshot preservation,<br>safe recovery support.|IT Lead|IR Lead|i<br>Verified<br>support<br>number and<br>provider<br>portal from<br>clean<br>device.|Immediate;<br>updates<br>every 2<br>hours.|
|Legal counsel /<br>DPO / privacy<br>adviser|Potential personal-data<br>exposure, E04 transfer,<br>evidence preservation,<br>deadlines.|IR Lead|Executive<br>sponsor<br>instructs<br>engagement;<br>counsel directs|Phone call<br>and<br>encrypted<br>channel;<br>avoid|Immediate<br>triage; at<br>least every<br>2 hours<br>until|



||||legal<br>assessment.|untrusted<br>email.|reporting<br>decision.|
|---|---|---|---|---|---|
|Regulator / law<br>enforcement (if<br>advised/require<br>d)|Legal threshold met or<br>preservation/investigatio<br>n support needed.|Legal/Privacy|Executive<br>sponsor, based<br>on counsel<br>advice.|Official<br>secure<br>reporting<br>channel /<br>verified<br>phone<br>contact.|Within<br>applicable<br>deadline;<br>record<br>submission<br>and receipt.|



### 4.2 Executive briefing 

At 08:45 WAT, Northbridge Skills Institute has declared a Critical cyber incident. The evidence confirms mass file changes on FIN-PC07 and .locked files with a ransom note on FS01; learners cannot access materials or submit assessments through the portal, which depends on FS01 (E06 –E08). A finance account had an unfamiliar Microsoft 365 sign-in (E02), svc_backup made unusual administrator logons to FS01 and BAK01 (E03), and FIN-PC07 uploaded 1.2 GB to unfamiliar cloud storage (E04). These events raise a serious concern about account compromise, backup integrity and possible data exfiltration. They do not yet prove attacker identity or that personal data was stolen. A successful backup job report does not establish a clean or recoverable backup (E05). Immediate actions are to isolate affected systems, protect BAK01, secure identity sessions, preserve logs and engage external forensics. The 11:00 examination must not be promised on an unverified system; approval is requested for an alternate examination arrangement and urgent external response support. Legal/privacy advisers are assessing notification duties. The next executive update is scheduled for 09:15 WAT, then every 30 minutes. 

### 4.3 Learner notice 

Northbridge Skills Institute is investigating a serious IT incident that is affecting access to learning materials and assessment submissions. The learning portal may load, but some functions are currently unavailable. We are taking steps to protect systems and learner information while we assess the impact. The 11:00 examination is **not confirmed to proceed as scheduled**. Please do not repeatedly submit answers or upload sensitive documents while the portal is unstable. Keep your course materials and any completed work saved locally where possible, and monitor SMS messages and the institute’s official status page for the confirmed examination arrangement. Do not respond to unexpected messages asking for passwords, payment or personal information, and do not forward unverified claims about the incident. If you need urgent assistance, contact the learner support line at +234 800 000 0101 (fictional). Our next update will be sent by 10:00 WAT. 

### 4.4 Legal/privacy assessment and escalation 

Legal/privacy advisers should establish the facts without treating the 1.2 GB upload as proof of exfiltration. They should identify the data controller and any processors, data categories and approximate numbers of people/records potentially involved, whether the data was accessed or disclosed, likely consequences, safeguards such as encryption, and mitigation already taken. They should preserve evidence and document the rationale for notifying or not notifying. Under section 40(2) of Nigeria’s Data Protection Act 2023, a controller must notify the Nigeria Data Protection Commission within 72 hours of becoming aware of a personal-data breach likely to result in risk to individuals’ rights and freedoms; section 40(3) addresses immediate communication to data subjects where the breach is likely to create high risk (Federal Republic of Nigeria, 2023). Counsel must confirm applicability, when awareness occurred, the relevant threshold, content, recipient and deadline. The 72-hour period is not a blanket deadline for every operational incident. 

An operational service notice explains outage, examination arrangements and practical steps; it is not a formal breach notification. Formal regulator/data-subject communications should be based on a documented legal assessment and include known facts, uncertainties, likely consequences, contact point and mitigation. If a deadline could expire before all facts are known, counsel should determine whether an initial notification based on available information is required and how to supplement it. Any other jurisdictions, education-sector rules, contracts, insurer terms or law-enforcement considerations must be checked rather than assumed. 

## Task 5 — Tabletop exercise and improvement plan 

### 5.1 Exercise design 

Simulation label: the following is a tabletop walkthrough, not a live incident and not evidence that any technical action was executed. Duration is 60 minutes. The facilitator provides injects; each participant responds in role; the scribe records decisions and gaps. No production changes are made. 

 Objectives: (1) activate the plan and communicate without corporate email; (2) contain active ransomware while preserving evidence; (3) make a defensible examination/recovery decision with uncertain backups; (4) assess possible data exfiltration and privacy reporting; (5) maintain consistent public messaging and track corrective actions. 

 Participants: Incident Commander/IR Lead; Security Analyst; IT Operations Lead; Forensics Lead; Academic Service Owner; Legal/Privacy Adviser; Communications Lead; Executive Sponsor; scribe/facilitator. 

 Agenda: minutes 0–10 initial report and declaration; 10–20 exam pressure; 20–35 extortion claim and evidence review; 35–45 backup failure and recovery choice; 45–55 misinformation/media response; 55–60 hotwash and action assignment. 

 Success criteria: (1) declare severity and notify executive sponsor within 5 simulated minutes of E06–E08; (2) agree isolation of FIN-PC07/FS01 and protection of BAK01 within 10 minutes, recording evidence trade-offs; (3) do not restore an unverified backup to production; (4) produce an approved learner status update within 10 minutes of the examination decision; (5) document at least five decisions with owner, rationale and communications; (6) legal/privacy owner records a threshold-based reporting decision and deadline owner. 

### 5.2 Simulated injects and decision log 

|Inject|Simulated decision|Decision<br>owner|Rationale|Communicatio<br>n|Effect|
|---|---|---|---|---|---|
|Minute 0 —<br>initial facts<br>E01–E08|Declare Critical; isolate<br>FIN-PC07 and affected<br>FS01 shares; protect<br>BAK01; open offline<br>phone bridge and<br>incident log.|IR Lead|Ransomware<br>and portal<br>impact are<br>confirmed;<br>privileged<br>access and<br>backup<br>compromise<br>are<br>plausible.<br>Containment<br>cannot wait,<br>but preserve<br>logs and<br>memory<br>where<br>feasible.|Phone bridge to<br>CSIRT; executive<br>alert; staff told<br>not to use email<br>for sensitive<br>incident details.|Stops likely<br>spread; portal<br>remains<br>unavailable<br>and<br>examination is<br>at risk.|
|Minute 10<br>—<br>managemen<br>t demands<br>portal<br>reopening<br>before 11:00<br>even if latest<br>backup<br>unverified|Decline direct<br>production restoration.<br>Ask academic owner to<br>approve<br>postponement/alternat<br>e assessment and<br>authorize isolated<br>restore testing only.|IR Lead<br>recommends;<br>Academic<br>Service Owner<br>decides exam;<br>Executive<br>Sponsor<br>approves risk<br>and resources|E03/E05<br>mean the<br>network<br>backup is<br>untrusted;<br>reintroducin<br>g malware<br>could<br>expand<br>outage and<br>corrupt<br>learner<br>records.|Immediate<br>executive call;<br>learner holding<br>notice by<br>SMS/status page;<br>next update time<br>stated.|May delay<br>exam but<br>avoids trading<br>a short<br>disruption for<br>reinfection<br>and loss of<br>evidence.|



||||Business<br>continuity is<br>not the<br>same as<br>unsafe<br>system<br>restoration.|||
|---|---|---|---|---|---|
|Minute 20<br>— attacker<br>claims all<br>1,800<br>learner<br>records and<br>threatens<br>publication<br>in 12 hours;<br>screenshot<br>authenticity<br>unknown|Treat as an unverified<br>extortion claim;<br>preserve<br>screenshot/message<br>metadata, avoid public<br>confirmation, expand<br>exfiltration and data-<br>access hunt, alert<br>legal/privacy and<br>executive sponsor.|IR Lead;<br>Security Analyst<br>investigates;<br>Legal/Privacy<br>assesses|Threat<br>actors may<br>exaggerate;<br>screenshot<br>could be<br>fabricated,<br>old, partial<br>or genuine.<br>E04 requires<br>investigation<br>but does not<br>prove<br>content.|Out-of-band legal<br>briefing;<br>communications<br>holding line says<br>investigation is<br>ongoing and facts<br>are not<br>confirmed.|Scope remains<br>potential<br>breach;<br>reporting<br>assessment<br>starts without<br>repeating<br>attacker’s<br>claim as fact.|
|Minute 35<br>— test<br>restore of<br>latest<br>network<br>backup fails:<br>unreadable<br>snapshot<br>error|Reject that snapshot;<br>preserve error logs and<br>metadata; keep backup<br>repository isolated; test<br>disconnected Friday<br>copy in a segregated<br>environment and<br>consider clean<br>rebuild/manual<br>recovery.|IT Lead<br>accountable for<br>technical<br>restore; IR Lead<br>coordinates|A failed<br>restore<br>disproves<br>readiness of<br>that<br>candidate,<br>not<br>necessarily<br>all backups.<br>Latest copy<br>may be<br>damaged or<br>tampered<br>with; no<br>production<br>connection.|Update executive<br>and academic<br>owner; explain<br>revised recovery<br>estimate; record<br>provider case.|Recovery<br>becomes<br>slower;<br>restore target<br>revised and<br>contingency<br>plan<br>maintained.|
|Minute 45<br>— social<br>media says<br>exam<br>cancelled;<br>journalist<br>asks<br>whether<br>learner data<br>exposed|Publish a verified status<br>correction: exam status<br>will be confirmed by a<br>specified time; do not<br>assert cancellation or<br>deny exposure until<br>approved. Route media<br>to Communications<br>Lead; counsel reviews<br>privacy wording.|Communication<br>s Lead drafts;<br>Executive<br>Sponsor<br>approves;<br>Academic<br>Service Owner<br>confirms exam<br>status|Uncontrolle<br>d<br>speculation<br>damages<br>trust. Avoid<br>definitive<br>claims<br>unsupported<br>by evidence;<br>provide<br>useful<br>learner<br>action and<br>next update.|Clean official<br>status page and<br>SMS; one<br>spokesperson;<br>staff phone tree.|Reduces<br>misinformatio<br>n and<br>inconsistent<br>statements<br>while<br>preserving<br>uncertainty.|
|Additional<br>inject,|Treat identity scope as<br>expanded; preserve|Identity<br>Administrator|New cloud<br>persistence|Urgent phone<br>update to IR Lead|Expands<br>investigation|



|Minute 50<br>— M365<br>audit reveals<br>a new<br>OAuth app<br>consent and<br>mailbox<br>forwarding<br>rule after<br>the<br>unfamiliar|audit details, revoke<br>suspicious grant/rule<br>and sessions after<br>evidence export; hunt<br>for additional app<br>grants and privileged<br>role changes.|executes;<br>Security Analyst<br>scopes; IR Lead<br>accountable|is plausible;<br>it may<br>enable<br>continued<br>access<br>independent<br>of endpoint<br>isolation.<br>Recheck<br>credentials<br>from clean|and Legal/Privacy;<br>provider support<br>ticket; staff<br>advised of trusted<br>contact route.|to identity<br>plane and<br>delays return<br>until cloud<br>persistence<br>and sessions<br>are validated.|
|---|---|---|---|---|---|
|sign-in|||device.|||



### 5.3 Simulated evaluation 

|Success criterion|Simulated result<br>i|Assessment|
|---|---|---|
|Critical declaration and executive<br>notification within 5 minutes|Achieved in walkthrough: Critical<br>declared at Minute 0 and sponsor<br>called.|Pass; operationalize with call-tree<br>timestamps.<br>i|
|Containment and BAK01 protection<br>within 10 minutes|Decision made at Minute 0–10;<br>actual technical completion is not<br>tested in tabletop.|Partial; require a technical<br>walkthrough to verify switch/EDR<br>and backup-plane access.|
|No unverified restore to production|Achieved; latest backup rejected<br>after unreadable snapshot inject.<br>t|Pass; formalize mandatory restore<br>gate and documented exception<br>prohibition.<br>i|
|Approved learner update within 10<br>minutes of exam decision<br>i|Draft approved message and<br>SMS/status-page route specified;<br>exercise does not test delivery.|Partial; test contact list, delivery<br>receipts and alternate website<br>access.<br>i|
|At least five documented decisions|Six decisions recorded in this log.<br>i|Pass for exercise documentation.|
|Legal/privacy threshold and<br>deadline owner recorded|Legal/privacy assessment initiated;<br>exact factual threshold remains<br>unresolvedpendingevidence.|Partial; create a breach decision<br>worksheet and named deputy.|



### 5.4 Improvement action plan 

|Priority|Improvement|Owner|Target<br>completion|Measurable<br>completion|
|---|---|---|---|---|
|P1|Create and test an<br>offline contact tree<br>and incident bridge;<br>verify non-email<br>escalation.|Operations<br>Manager|7 days|90% of<br>primary/alternate<br>contacts<br>acknowledge test<br>call within 15<br>minutes; signed test<br>report.|
|P1|Isolate and validate<br>backup recovery;<br>implement separate<br>protected copy and<br>restore testing.|IT Lead|30 days|One successful<br>isolated<br>FS01/LMS01<br>restore; report<br>records RTO/RPO,|



|||||integrity checks and<br>account separation.|
|---|---|---|---|---|
|P1|Expand server EDR<br>and central log<br>coverage to FS01,<br>LMS01 and BAK01.|Security Lead|45 days|100% critical server<br>coverage; test alerts<br>for mass file<br>change, admin<br>logon and backup<br>deletion.<br>i|
|P1|Implement<br>privileged account<br>hardening for<br>svc_backup and<br>separate backup<br>adminplane.|Identity<br>Administrator|14 days|Interactive logon<br>denied; least-<br>privilege access<br>review approved;<br>credential rotation<br>and audit evidence.|
|P2|Create exam<br>continuity<br>procedure with<br>postponement<br>authority, alternate<br>assessment options<br>and learner<br>templates.|Academic Service<br>Owner|21 days|Approved runbook;<br>tabletop<br>demonstrates<br>learner notice<br>within 10 minutes<br>and no unverified<br>restore.|
|P2|Prepare a data-<br>breach assessment<br>form and legal<br>escalation roster.|Legal/Privacy<br>Adviser|14 days|Template includes<br>data categories,<br>affected individuals,<br>risk, awareness<br>time, statutory<br>deadlines and<br>decision sign-off.|
|P2|Run technical<br>walkthrough of<br>isolation, evidence<br>capture, and restore<br>gate.|IR Lead + Forensics<br>Lead|30 days|Exercise report<br>shows isolation<br>within 10 minutes,<br>evidence log<br>completeness<br>above 95%, no<br>production restore<br>before validation.|
|P3|Run quarterly cross-<br>functional tabletop<br>and track overdue<br>actions to executive<br>review.|IR Lead|First follow-up<br>within 60 days;<br>quarterly thereafter|i<br>Attendance at least<br>90%; action register<br>reviewed monthly;<br>no overdue P1<br>actions without<br>executive exception.|



Post-incident review deadline: within 14 calendar days of incident closure, consistent with the teaching deck’s lessons-learned guidance (ICDFA, Week 20, slide 12). The IR Lead owns the review and evidence-backed final report; the executive sponsor closes each action only after the owner supplies measurable proof. Legal holds and regulatory investigations may require a longer evidence-retention period, but should not prevent prompt operational learning. 

## Task 6 — Healthcare case study analysis and critical reflection 

The teaching case describes a 2,500-employee healthcare provider hit by a Ryuk ransomware variant after a phishing attachment. Malware reportedly persisted and moved laterally for 72 hours before encryption. The response activated on Day 4, isolated network segments, quarantined affected systems and verified offline backups. Forensic analysis and eradication followed on Days 5–7, systems were rebuilt from clean backups, controls were strengthened, and a Day 14 review updated the plan and training (ICDFA, Week 20, slide 16). 

Three strengths stand out. First, the team used segmentation and quarantine to limit spread rather than treating the incident as a single infected device. Second, verified offline backups enabled recovery without paying ransom. Third, a defined response team, phased restoration, timely regulatory notifications, and a post-incident review connected operational recovery to governance and improvement. These practices align with CISA’s recommendation to isolate affected systems, preserve relevant evidence, restore from offline backups, and document lessons learned (CISA et al., 2023). 

Two limitations remain. The 72-hour dwell time suggests detection and/or identity monitoring did not identify lateral movement early enough; the slide does not describe what telemetry or alerting would shorten that delay. Also, “no patient data exfiltration confirmed” is an outcome claim without the supporting evidence described. The slide does not specify which egress, endpoint, identity, storage-access and cloud logs were reviewed, their retention/completeness, or how the team ruled out staging and encrypted outbound transfers. A conclusion should be bounded by evidence and confidence, not phrased as absolute proof if telemetry is incomplete. 

I would transfer two changes to Northbridge. First, maintain a protected offline/immutable backup and routinely perform isolated restore tests. Northbridge’s network backup is nonimmutable, svc_backup can administer both FS01 and BAK01, and no restoration test occurred in 90 days (E03, E05); a successful job report is insufficient. Second, make segmentation and phased recovery explicit, including an isolated recovery network and a signed acceptance gate for LMS01/FS01. Northbridge must not reopen the portal merely to meet the 11:00 examination if the backup is unsafe. Restoring critical systems within 72 hours means restoring a minimum viable, validated service tier; full restoration over seven days can include lower-priority services, wider validation and enhanced monitoring. Before claiming no learner data was stolen, responders would need sufficiently complete proxy/DNS/firewall and endpoint telemetry, fileaccess records, identity and cloud audit logs, destination analysis, checks for staging/archive 

tools, and a documented assessment of log gaps. After the tabletop, I would strengthen my original decision by requiring a specific named recovery approver and an explicit “no production restore while unverified” gate. 

Word count for Task 6: approximately 370 words (verify using the word-count tool in your word processor). 

## References 

CISA, NSA, FBI, MS-ISAC and Joint Ransomware Task Force. (2023). #StopRansomware Guide. Cybersecurity and Infrastructure Security Agency. 

https://www.cisa.gov/stopransomware/ransomware-guide 

_Use in report: Supports immediate isolation, evidence preservation, credential and backup protection, offline backup testing, recovery and post-incident improvement._ 

Federal Republic of Nigeria. (2023). Nigeria Data Protection Act, 2023, section 40. Nigeria Data Protection Commission. 

https://ndpc.gov.ng/wp-content/uploads/2024/03/Nigeria_Data_Protection_Act_2023.pdf 

_Use in report: Supports the qualified discussion of regulator and data-subject notification duties, including the 72-hour controller notification rule where the statutory risk threshold is met._ 

International Cybersecurity and Digital Forensics Academy (ICDFA). (n.d.). Incident Response Planning. GRC105, Week 20 teaching deck, slides 4–16. 

_Use in report: Supports the teaching lifecycle, incident classification, CSIRT roles, RACI concepts, containment and recovery stages, lessons-learned review, and the illustrative healthcare case study on slide 16._ 

International Cybersecurity and Digital Forensics Academy (ICDFA). (n.d.). Business Continuity and Disaster Recovery. GRC205 teaching deck, slides 3–6 and 14–15. 

_Use in report: Supports the distinction between incident response, business continuity and disaster recovery, BIA, RTO/RPO, and the need for tested, phased recovery._ 

