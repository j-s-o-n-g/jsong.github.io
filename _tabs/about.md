---
# the default layout is 'page'
icon: fas fa-info-circle
order: 1
---

<style>
.rs { --rs-accent: #d6004c; --rs-text: var(--text-color, #222); --rs-muted: var(--text-muted-color, #6c6c6c); --rs-strong: var(--heading-color, #111); --rs-line: var(--main-border-color, #dcdcdc); color: var(--rs-text); font-size: 0.95rem; line-height: 1.6; }
html[data-mode="dark"] .rs { --rs-accent: #ff4d86; }
@media (prefers-color-scheme: dark) { html:not([data-mode="light"]) .rs { --rs-accent: #ff4d86; } }
.rs a { color: inherit; text-decoration: none; border-bottom: 1px dotted var(--rs-muted); }
.rs .rs-kicker { color: var(--rs-accent); font-weight: 700; font-size: 0.85rem; letter-spacing: 0.02em; margin: 0 0 0.25rem; }
.rs .rs-hero { display: flex; gap: 1.5rem; align-items: flex-start; justify-content: space-between; }
.rs .rs-hero-main { flex: 1 1 auto; min-width: 0; }
.rs .rs-name { font-size: 2.6rem; font-weight: 500; line-height: 1.1; margin: 0 0 0.9rem; color: var(--rs-strong); border: 0; padding: 0; }
.rs .rs-headline { font-size: 1.1rem; font-weight: 700; color: var(--rs-strong); margin: 0 0 0.4rem; }
.rs .rs-sub { color: var(--rs-muted); font-size: 0.85rem; margin: 0 0 0.9rem; }
.rs .rs-photo { flex: 0 0 auto; width: 120px; height: 160px; object-fit: cover; border: 1px solid var(--rs-line); pointer-events: none; cursor: default; }
.rs .rs-info { display: grid; grid-template-columns: max-content 1fr max-content 1fr; gap: 0.35rem 1.2rem; font-size: 0.88rem; }
.rs .rs-info b { color: var(--rs-strong); }
.rs .rs-sec { display: flex; align-items: center; gap: 0.5rem; font-size: 1.2rem; font-weight: 700; color: var(--rs-strong); margin: 2.2rem 0 0.7rem; padding: 0; border: 0; }
.rs .rs-sec::before { content: ""; width: 0.6rem; height: 0.6rem; background: var(--rs-accent); flex: 0 0 auto; }
.rs table.rs-tbl { width: 100%; border-collapse: collapse; border-top: 2px solid var(--rs-strong); border-bottom: 2px solid var(--rs-strong); margin: 0; display: table; }
.rs .table-wrapper { margin: 0; overflow: visible; }
.rs table.rs-tbl tr { background: transparent !important; border-bottom: 0 !important; }
.rs table.rs-tbl td { white-space: normal !important; font-size: inherit !important; border: 0; border-bottom: 1px solid var(--rs-line); padding: 0.55rem 0.6rem; vertical-align: middle; background: transparent !important; }
.rs table.rs-tbl tr:last-child td { border-bottom: 0; }
.rs table.rs-tbl td.rs-lbl { font-weight: 700; color: var(--rs-strong); white-space: nowrap !important; width: 1%; padding-right: 1.4rem; }
.rs table.rs-tbl td.rs-date { white-space: nowrap !important; width: 1%; padding-right: 1.4rem; }
.rs table.rs-tbl ul { margin: 0; padding-left: 1.1rem; }
.rs table.rs-tbl li { margin: 0.1rem 0; }
.rs .rs-job { margin-top: 1.4rem; }
.rs .rs-job-head { display: flex; justify-content: space-between; align-items: baseline; gap: 1rem; flex-wrap: wrap; }
.rs .rs-org { font-size: 1.12rem; font-weight: 700; color: var(--rs-strong); }
.rs .rs-period { color: var(--rs-accent); font-weight: 700; white-space: nowrap; }
.rs .rs-role { color: var(--rs-muted); font-size: 0.85rem; margin: 0.1rem 0 0.5rem; }
.rs .rs-hl { color: var(--rs-accent); font-weight: 700; }
.rs .rs-check { list-style: none; padding-left: 0 !important; }
.rs .rs-check li::before { content: "\2713"; color: var(--rs-accent); font-weight: 700; margin-right: 0.4rem; }
.rs table.rs-certs td { width: auto; }
.rs table.rs-certs td.rs-date { width: 1%; }
.rs .rs-foot { display: flex; gap: 1.5rem; margin-top: 2.5rem; padding-top: 0.8rem; border-top: 1px solid var(--rs-line); font-size: 0.8rem; color: var(--rs-muted); }
.rs .rs-foot b { color: var(--rs-accent); }
@media (max-width: 640px) {
  .rs .rs-hero { flex-direction: column-reverse; }
  .rs .rs-photo { width: 105px; height: 140px; }
  .rs .rs-name { font-size: 2.1rem; }
  .rs .rs-info { grid-template-columns: max-content 1fr; }
  .rs table.rs-tbl, .rs table.rs-tbl tbody { display: block; }
  .rs table.rs-tbl tr { display: block; padding: 0.45rem 0; border-bottom: 1px solid var(--rs-line) !important; }
  .rs table.rs-tbl tr:last-child { border-bottom: 0 !important; }
  .rs table.rs-tbl td, .rs table.rs-tbl td.rs-lbl, .rs table.rs-tbl td.rs-date { display: block; width: auto; border: 0; padding: 0.1rem 0.3rem !important; white-space: normal !important; }
  .rs table.rs-certs tr { display: grid; grid-template-columns: 5.2rem 1fr; }
}
</style>

<div class="rs">
<div class="rs-hero">
<div class="rs-hero-main">
<p class="rs-kicker">IT Infrastructure · Cloud · Security Engineer — Comprehensive Resume</p>
<p class="rs-name">Jay Song</p>
<p class="rs-headline">Hands-on experience building Linux, network, and security infrastructure, responding to incidents, and automating with cloud and AI</p>
<p class="rs-sub">RHCSA · CCNA · Security+ · ISO 27001 Lead Auditor · M.S. in Computer Science candidate, Georgia Tech</p>
<div class="rs-info">
<b>Date of Birth</b><span>N/A</span><b>Military Service</b><span>ROK Army, Sergeant (Honorably Discharged)</span>
<b>Phone</b><span>N/A</span><b>Email</b><span>N/A</span>
<b>Portfolio</b><span><a href="https://jaekuk.com">jaekuk.com</a></span><b>LinkedIn</b><span><a href="https://www.linkedin.com/in/jaekuk">linkedin.com/in/jaekuk</a></span>
</div>
</div>
<img class="rs-photo" src="/assets/img/profile/resume-photo.jpg" alt="Jay Song">
</div>

<p class="rs-sec">Education</p>
<table class="rs-tbl">
<tr><td class="rs-date">2026.01 – Present</td><td class="rs-lbl">Georgia Institute of Technology</td><td>M.S. in Computer Science (Online) · GPA 4.0 / 4.0</td></tr>
<tr><td class="rs-date">2021.08 – 2025.05</td><td class="rs-lbl">The Ohio State University</td><td>B.S. in Data Analytics · GPA 3.5 / 4.0 · Cum Laude</td></tr>
</table>

<p class="rs-sec">Experience</p>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">Volga Partners</span><span class="rs-period">2025.09 – 2026.05 (9 months)</span></div>
<p class="rs-role">AI Content Evaluator · Contract &nbsp;/&nbsp; LLM-Generated Code Evaluation</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">Responsibilities</td><td><ul>
<li>Tested and validated LLM-generated Python and SQL code against guidelines</li>
<li>Documented defects and their root causes as structured written feedback</li>
</ul></td></tr>
<tr><td class="rs-lbl">Results</td><td><ul class="rs-check">
<li>Contributed to a <span class="rs-hl">2.2% improvement</span> in model performance through feedback</li>
</ul></td></tr>
</table>
</div>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">The Ohio State University</span><span class="rs-period">2023.09 – 2024.05 (9 months)</span></div>
<p class="rs-role">Research Assistant · College of Optometry &nbsp;/&nbsp; Data Automation &amp; ML Pipeline</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">Responsibilities</td><td><ul>
<li>Automated manual encoding of 1M+ data records using Python and OCR</li>
<li>Optimized an ML imaging pipeline for retinal disease prediction through frame sampling</li>
<li>Explained complex technical and statistical concepts to non-specialist postdoctoral researchers</li>
</ul></td></tr>
<tr><td class="rs-lbl">Results</td><td><ul class="rs-check">
<li>Task time <span class="rs-hl">reduced from 3 hours to 1 hour</span>, with <span class="rs-hl">100%</span> accuracy</li>
<li>ML pipeline compute cost <span class="rs-hl">reduced by 67%</span></li>
</ul></td></tr>
</table>
</div>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">DOmedia (USA)</span><span class="rs-period">2019.04 – 2019.07 (4 months)</span></div>
<p class="rs-role">IT Intern &nbsp;/&nbsp; Data Processing &amp; IT Request Support</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">Responsibilities</td><td><ul>
<li>Extracted, updated, and loaded data with SQL queries, and integrated geospatial data via the Google Maps API</li>
<li>Tracked, diagnosed, and resolved IT requests through a ticketing system, following up on escalations through closure</li>
</ul></td></tr>
</table>
</div>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">Wedding Photography</span><span class="rs-period">2013.10 – 2016</span></div>
<p class="rs-role">Wedding Photographer</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">Responsibilities</td><td><ul>
<li>Photographed weddings and edited images with Lightroom and Photoshop</li>
<li>Created and shipped photo albums and responded to customer phone calls</li>
</ul></td></tr>
</table>
</div>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">Republic of Korea Army</span><span class="rs-period">2011.08 – 2013.05 (1 year 9 months)</span></div>
<p class="rs-role">IT Helpdesk Technician · Sergeant (Honorably Discharged) &nbsp;/&nbsp; User Support &amp; IT Operations</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">Responsibilities</td><td><ul>
<li>Provided on-site and phone support for system, application, and network issues for 200+ users, escalating per SOP</li>
<li>Installed and updated 200+ Windows and Linux machines; replaced end-of-life ports and switches and monitored routers</li>
<li>Processed 400+ access requests, including account creation and termination, permission changes, and password resets</li>
</ul></td></tr>
<tr><td class="rs-lbl">Results</td><td><ul class="rs-check">
<li>Received <span class="rs-hl">5</span> Army commendations (including 3 from a Brigadier General)</li>
</ul></td></tr>
</table>
</div>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">Lotte Cinema</span><span class="rs-period">2010.01 – 2010.12</span></div>
<p class="rs-role">Ticketing, Admission Control &amp; Concessions Staff</p>
</div>

<div class="rs-job">
<div class="rs-job-head"><span class="rs-org">University Cafeteria</span><span class="rs-period">2010.01 – 2010.12</span></div>
<p class="rs-role">Cafeteria Staff</p>
</div>

<p class="rs-sec">Projects</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">RHEL 10 Home Lab</td><td>Built two RHEL 10 VMs on VMware with a DVD-based offline repository, additional disks, static IPs via nmcli, and SSH key authentication; ran Apache on a non-standard port with SELinux and firewalld; practiced LVM, NFS autofs, and chrony</td></tr>
<tr><td class="rs-lbl">Cisco Network Home Lab</td><td>Configured VLANs, 802.1Q trunks, LACP, OSPF, and IPv4/IPv6 subnets in Packet Tracer, and diagnosed and fixed configuration errors</td></tr>
<tr><td class="rs-lbl">STP Simulation</td><td>Georgia Tech CS 6250: simulated root bridge election and loop-free path selection in Python</td></tr>
<tr><td class="rs-lbl">HTB Holmes CTF 2026</td><td>Team of 4, ranked 176th of ~8,000 (top ~2.2%); solved 8 of 9 DFIR scenarios through log analysis and forensics</td></tr>
<tr><td class="rs-lbl">AI Study App · Automation</td><td>Built a custom study app with AI coding tools and scored 88/99 on the ASVAB in 2 weeks / Set up a scheduled task that filters job postings by criteria and reports them by email / Built 4 Tableau dashboards</td></tr>
</table>

<p class="rs-sec">Certifications</p>
<table class="rs-tbl rs-certs">
<tr><td class="rs-date">2026.10</td><td>Red Hat Certified System Administrator</td></tr>
<tr><td class="rs-date">2026.09</td><td>Cisco Certified Network Associate</td></tr>
<tr><td class="rs-date">2026.05</td><td>CompTIA Security+ CE</td></tr>
<tr><td class="rs-date">2025</td><td>AWS Cloud Essentials</td></tr>
<tr><td class="rs-date">2015.05</td><td>ISO 27001 Lead Auditor (expired)</td></tr>
</table>

<p class="rs-sec">Awards</p>
<table class="rs-tbl rs-certs">
<tr><td class="rs-date">2026.09</td><td>HTB Holmes CTF, top ~2.2% (176th)</td></tr>
<tr><td class="rs-date">2021–2025</td><td>Veeam Scholarship</td></tr>
<tr><td class="rs-date">2021–2025</td><td>OSU Dean's List</td></tr>
<tr><td class="rs-date">2012.07</td><td>ROK Army Brigadier General Commendation (identified a threat during CCTV surveillance)</td></tr>
<tr><td class="rs-date">2013</td><td>Best Poster Award, IT Awareness Campaign</td></tr>
<tr><td class="rs-date">2013</td><td>Award, KFN Video Contest</td></tr>
<tr><td class="rs-date">2013</td><td>Commendation for Apprehending a Sexual Assailant While on Leave</td></tr>
<tr><td class="rs-date">2013</td><td>Award for Outstanding Conduct During Driver Training</td></tr>
</table>

<p class="rs-sec">Technical Skills</p>
<table class="rs-tbl">
<tr><td class="rs-lbl">Linux · Server</td><td>RHEL 10 (systemd, LVM, SELinux, firewalld, NFS·autofs, chrony, sudo·ACL), Windows, Active Directory</td></tr>
<tr><td class="rs-lbl">Virtualization · Cloud</td><td>VMware (VM provisioning, snapshots), offline repository setup, AWS fundamentals (EC2, S3, VPC, IAM), preparing for SAA-C03</td></tr>
<tr><td class="rs-lbl">Networking</td><td>TCP/IP, DNS, DHCP, VLAN, 802.1Q, LACP, OSPF, STP, switch and router replacement and monitoring</td></tr>
<tr><td class="rs-lbl">Security</td><td>Access control and privilege management, server security configuration, log monitoring, DFIR, ISO 27001</td></tr>
<tr><td class="rs-lbl">Scripting · Docs · Languages</td><td>Bash, Python, PowerShell, SQL, Git, Tableau, SOP·README documentation &nbsp;/&nbsp; Korean (native), English</td></tr>
</table>

<div class="rs-foot"><b>JAY SONG</b><span>IT Infrastructure · Cloud · Security Engineer</span></div>
</div>
