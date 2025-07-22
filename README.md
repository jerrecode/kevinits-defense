# KevinITS-Defense
*Evidence Preservation · Anti-SLAPP · Community-Led Legal Support*

> **This repository is a public, audit-ready evidence archive and task hub created by volunteers to help the German nursing-whistle-blower & YouTuber **Kevin ITS** defend himself against a series of alleged SLAPP lawsuits and cease-and-desist letters (Abmahnungen) issued by Schertz Bergmann on behalf of several hospital staff members and their employer.  
> _No copyrighted video material or personal data is stored here—only cryptographic fingerprints, transcripts, and procedural metadata._  

---

## 1  Background — Why this repo exists
- **Root incident (Apr–May 2025).**  
  Three nurses live-streamed from ICU wards, accidentally exposing patient data.  
  Kevin ITS published short, redacted excerpts to document the privacy breach and alerted the hospitals & nursing authorities.  
- **Legal backlash (May → Jul 2025).**  
  11 cease-and-desist letters + multiple penalty lawsuits were filed, widely viewed as **Strategic Lawsuits Against Public Participation (SLAPPs)** aimed at exhausting Kevin financially.  
- **Community response.**  
  A volunteer team formed to (a) preserve tamper-proof evidence, (b) prepare NGO/press dossiers, and (c) coordinate crowd-sourced legal support—**without re-publishing the raw footage.**

---

## 2  Goals
| # | Objective | Success Metric |
|---|-----------|----------------|
|1 | **Immutability.** Secure every relevant stream clip with SHA-256 hashes, OSS time-stamping, and offline storage | 100 % clip coverage, verifiable hash match |
|2 | **Transparency.** Provide courts & NGOs with ready-to-use transcripts, timelines, and chain-of-custody logs | Dossier v1 accepted by at least one NGO |
|3 | **Cost control.** Cut Kevin’s lawyer hours by delivering clean, well-indexed evidence | < 20 min lookup time per clip |
|4 | **Anti-SLAPP advocacy.** Document the cost-escalation pattern and notify press & legal-freedom groups | Case listed in two international SLAPP trackers |
|5 | **Sustainable workflow.** Run the whole pipeline on free/open tools that any citizen can replicate | Fully reproducible from `git clone` |

---

## 3  Repository layout
```txt
.
├── videos/
│   ├── hashes/                     Json meta–blocks (public)
│   └── raw/                        Encrypted originals (NOT in git)
├── transcripts/                    Whisper SRT+TXT files
├── docs/
│   ├── dossier/                    case-dossier,timeline,SLAPP-log
│   ├── legal/                      draft Schutzschrift,NGO-letters
│   └── chain_of_custody.md
├── scripts/                        Python-Scripts
├── urls.csv
├── requirements.txt
└── README.md
```

### Hash policy

* SHA-256 for every raw file
* Metadata JSON skeleton:

```json
{
  "url": "https://www.tiktok.com/...",
  "sha256": "4fa2…",
  "downloaded_at": "2025-05-12T18:44:07Z",
  "size": 73492311
}
```

* Each hash file is **timestamped** via [OpenTimestamps](https://opentimestamps.org) for provable creation date.

---

## 4  Project plan & Kanban

The live task board lives in **GitHub Projects → *KevinITS-Defense Kanban***.
High-level phase overview:

1. **Phase 0 – Setup**   (complete)
2. **Phase 1 – Evidence capture**   (ongoing)
3. **Phase 2 – Transcription & QC**
4. **Phase 3 – Dossier build**
5. **Phase 4 – NGO / Press outreach**
6. **Phase 5 – Continuous updates & legal hand-off**

Detailed technical To-Do lists are stored in `/docs/project_plan.md` and mirrored on the Kanban board.

---

## 5  Quick-start (Linux/Mac CLI)

| Nr. | Action | Linux Shell Command |
| --- | --- | --- |
| 1 | Clone the repo | `git clone https://codeberg.org`/kevinits-defense.git; cd kevinits-defense` |
| 2 | Install Pip + PipEnv | `python -m pip install --upgrade pip pipenv` |
| 3 | Create isolated env | `pipenv install yt-dlp openai-whisper ffmpeg-python tqdm` |
| 4 | Add URLs to urls.csv | `printf "url\nhttps://www.tiktok.com/…\n" > urls.csv` |

### 5.1 Videos, Hashes and Transcripts

| Nr. | Action | Linux Shell Command |
| --- | --- | --- |
| 1 | Download & hash | `pipenv run python scripts/download_tiktok.py urls.csv` |
| 2 | Auto-transcribe | `pipenv run python scripts/transcribe.py` |
| 3 | Verify integrity | `pipenv run python scripts/verify_hashes.py` |

---

## 6  How to contribute

1. **Evidence curator** – add new URLs to `urls.csv`; run the pipeline; open a PR with new hash JSONs.
2. **Transcript QC** – pick random `.srt` files, compare to video audio, flag errors in Issues.
3. **Outreach helper** – translate key docs to EN, draft NGO e-mails, or maintain the SLAPP timeline.
4. **Legal research** – improve the Schutzschrift or anti-SLAPP argumentation (docs/legal).

See `docs/CONTRIBUTING.md` for coding style and commit rules.

---

## 7  Code of Conduct

We follow the [Contributor Covenant v2.1](https://www.contributor-covenant.org). Harassment, doxxing, or sharing of patient data results in immediate ban.

---

## 8  Legal disclaimer

* This repository **does not** contain patient images, full abmahnung PDFs, or any raw video.
* All scripts are public domain (CC0).
* Nothing herein is legal advice; consult qualified counsel for courtroom use.
* Brand names and trademarks belong to their respective owners.

---

## 9  License

* **Code:** CC0-1.0 *(public domain)*
* **Documentation (\*/docs):** Creative Commons BY-SA 4.0

---

## 10  Contact

* Matrix: **#kevinits-support\:matrix.org**
* Maintainer (PGP-encrypted mail preferred): `kevinits.defense@riseup.net`
* Press & NGO queries: see `/docs/dossier/press_contact.txt`

> *“Sunlight is the best disinfectant — but cryptographic sunlight never blinds the innocent.”*

```