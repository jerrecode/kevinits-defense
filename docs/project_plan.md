Below is a **comprehensive, end-to-end project to-do plan** for **KevinITS-Defense**.
It is organized as phases ⇒ epics ⇒ actionable tasks, with suggested owners, duration estimates (in ideal person-days), dependencies, and “Definition of Done” (DoD). Adapt durations to your real bandwidth.

---

## Phase 0  Repository & Governance Bootstrap  *(\~2 days)*

| Epic         | Task                                                                      | Owner        | Dur. | Depends    | DoD                                    |
| ------------ | ------------------------------------------------------------------------- | ------------ | ---- | ---------- | -------------------------------------- |
| Repo hygiene | Create root `.gitignore` (exclude `videos/raw/`); commit                  | Maintainer   | 0.1  | —          | File present, verified by `git status` |
|              | Add `.gitattributes` (text eol, linguist-docs)                            | Maintainer   | 0.1  | —          | File pushes with no diff noise         |
|              | Push **scripts/** placeholders                                            | Tech-lead    | 0.3  | —          | `download_tiktok.py` etc. importable   |
|              | Publish `docs/chain_of_custody.md` template                               | Tech-lead    | 0.1  | —          | Empty table committed                  |
| Governance   | Enable **GitHub Projects** board (columns: To Do/In Progress/Review/Done) | Maintainer   | 0.1  | —          | Board URL posted in README             |
|              | Add **Issue / PR templates** + CoC link                                   | Maintainer   | 0.2  | —          | `.github/` templates merged            |
|              | Hook Matrix room to repo webhooks (via maubot/GitHub-bot)                 | Moderator    | 0.4  | Board live | Events mirrored to chat                |
| Legal        | Add `LICENSE` (CC0 + BY-SA split) & `NOTICE`                              | Legal helper | 0.2  | —          | Files render on repo home              |

---

## Phase 1  Evidence Capture  *(\~4 days baseline + continuous)*

| Epic       | Task                                                                 | Owner        | Dur. | Depends    | DoD                        |
| ---------- | -------------------------------------------------------------------- | ------------ | ---- | ---------- | -------------------------- |
| URL intake | Create `urls.csv` schema (`url,source,added_by`)                     | Curator      | 0.2  | P0         | File exists                |
|            | Collect first batch of 50 links                                      | Curator team | 1.0  | schema     | CSV committed              |
| Downloader | Implement `download_tiktok.py`  - yt-dlp wrapper, SHA-256, JSON meta | Dev 1        | 0.8  | URLs       | Script runs on sample list |
|            | Unit-test: hash collision & re-run skip                              | Dev 1        | 0.3  | script     | `pytest` green             |
| Integrity  | Implement `verify_hashes.py` (spot/all)                              | Dev 2        | 0.5  | downloader | CLI returns 0 on success   |
| Chain log  | Extend script to append to `docs/chain_of_custody.md`                | Dev 2        | 0.3  | verify     | Log entries appear         |
| Storage    | Encrypt raw videos (`gocryptfs` / AES ZIP) and move to `/videos/raw` | Ops          | 0.4  | downloader | Encrypted dir size visible |
| QA         | Random-sample integrity check (5 clips)                              | QA           | 0.5  | storage    | Hashes match               |

Continuous task: **Weekly URL scrape** (cron job or manual) and re-run pipeline.

---

## Phase 2  Transcription & Data Prep  *(\~3 days + rolling)*

| Epic            | Task                                    | Owner | Dur. | Depends     | DoD                         |
| --------------- | --------------------------------------- | ----- | ---- | ----------- | --------------------------- |
| Whisper wrapper | Implement `transcribe.py` (SRT + TXT)   | Dev 3 | 0.8  | Phase 1     | SRT saved in `/transcripts` |
|                 | ThreadPool / progress bar               | Dev 3 | 0.4  | script      | Concurrency works           |
| QC              | Transcript spot-check (5 %)             | QA    | 0.6  | transcripts | Error rate <3 %             |
| Automation      | Add language override flag (de/en auto) | Dev 3 | 0.3  | script      | CLI `--lang` works          |
| Hash verify     | Append transcript SHA-256 to JSON meta  | Dev 2 | 0.4  | transcripts | JSON enriched               |

---

## Phase 3  Documentation & Dossier  *(\~2 days)*

| Epic             | Task                                                   | Owner     | Dur. | Depends     | DoD                              |
| ---------------- | ------------------------------------------------------ | --------- | ---- | ----------- | -------------------------------- |
| Timeline builder | `build_timeline.py` — parse metas ⇒ `docs/timeline.md` | Dev 4     | 0.8  | Phase 1 & 2 | File lists clips chronologically |
| Dossier          | Create `docs/dossier/outline.md` (EN + DE)             | Docs team | 0.5  | timeline    | Outline approved                 |
|                  | Automate PDF export via Pandoc GitHub Action           | DevOps    | 0.7  | outline     | Artifact in CI run               |
| FAQ              | Draft contributor FAQ (common errors, legal)           | Docs team | 0.4  | outline     | Linked in README                 |

---

## Phase 4  Outreach & Anti-SLAPP  *(\~3 days initial)*

| Epic      | Task                                              | Owner         | Dur. | Depends      | DoD                      |
| --------- | ------------------------------------------------- | ------------- | ---- | ------------ | ------------------------ |
| NGO kit   | Tailor dossier summary for ECPMF & GFF            | Outreach lead | 0.6  | Phase 3      | PDF <5 pages             |
| Submit    | File legal-support forms, attach kit              | Outreach team | 0.4  | kit          | Ticket IDs logged        |
| Press kit | One-page media brief + timeline PNG               | Outreach lead | 0.7  | kit          | Uploaded to `docs/press` |
| Tracking  | Create `/docs/slapp_log.md` cost-escalation table | Legal helper  | 0.3  | Ongoing data | First entry added        |

---

## Phase 5  CI/CD & Quality Gate  *(\~1.5 days)*

| Epic               | Task                                                              | Owner      | Dur. | Depends | DoD                             |
| ------------------ | ----------------------------------------------------------------- | ---------- | ---- | ------- | ------------------------------- |
| Static checks      | GitHub Action: `python -m py_compile` + `ruff`                    | DevOps     | 0.4  | scripts | CI red on syntax error          |
| Artifact retention | Workflow to upload encrypted video bundle to GH Releases (tagged) | DevOps     | 0.6  | storage | Release asset visible (private) |
| Badge              | Add build-status badge to README                                  | Maintainer | 0.1  | CI      | Badge green                     |

---

## Phase 6  Maintenance & Growth  *(continuous)*

* **Weekly triage** of Issues/PRs
* **Monthly dependency bump** (`pipenv update`)
* **Quarterly audit** of encryption keys and off-site backups
* **Track SLAPP costs** and update `slapp_log.md` after every legal invoice
* **Add new languages** (community PRs) for dossier translations

---

### **Timeline at a glance**

| Week | Key Milestone                                    |
| ---- | ------------------------------------------------ |
| 1    | Phase 0 complete, repo ready for contributors    |
| 2    | Phase 1 scripts running, first 50 clips archived |
| 3    | Phase 2 transcripts + integrity check live       |
| 4    | Dossier v0.1, press/NGO packets sent             |
| 5    | CI pipeline & badge green                        |
| 6+   | Rolling evidence intake, outreach, legal updates |

---

### **Role Matrix**

| Role          | Primary Tasks                  | Backup     |
| ------------- | ------------------------------ | ---------- |
| Maintainer    | Repo hygiene, README, releases | Tech-lead  |
| Tech-lead     | Script architecture, pipeline  | Dev 3      |
| Dev 1-4       | Specific script epics          | Each other |
| QA            | Spot checks, transcript QA     | Any Dev    |
| Docs team     | Dossier, FAQ                   | Outreach   |
| Outreach lead | NGOs, press, SLAPP log         | Maintainer |
| Moderator     | Matrix/Jitsi, community health | Docs       |

---

### **Risk Register (top 3)**

| Risk                    | Likelihood | Impact              | Mitigation                                      |
| ----------------------- | ---------- | ------------------- | ----------------------------------------------- |
| TikTok blocks downloads | Med        | High (evidence gap) | Use youtube-dl archive API and fallback proxies |
| SLAPP cost spike        | High       | High                | Early NGO funding + cost log transparency       |
| Contributor burnout     | Med        | Med                 | Rotate tasks, weekly 15-min sync, praise system |

---

**This plan can be copied into `/docs/project_plan.md`** and tracked via GitHub Projects columns or the `KANBAN.md` board. Adjust owners/durations as your volunteer capacity evolves.

