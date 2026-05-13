# Roadmap: Security Professional (Bug Hunter / Pentester / Security Analyst)

**Ringkasan:**
Roadmap ini menyajikan jalur pembelajaran mulai dari dasar (bagi pemula) hingga teknik lanjutan untuk security professional, bug bounty hunter, dan penetration tester. Termasuk mindset, metodologi, toolkit nyata, contoh workflow, dan sumber belajar gratis serta berbayar.

**Sumber asli:** [chat content user-provided] (https://hackerai.co/share/f8aa7358-b7dc-4d44-a8d8-85ed9b5b4da3)
**Tanggal pembuatan:** 2026-05-13

---

## Daftar Isi

- [Pendahuluan](#pendahuluan)
- [Phase 1 — Fundamental Foundation (3–6 bulan)](#phase-1---fundamental-foundation-3–6-bulan)
- [Phase 2 — Web Security Core (6–12 bulan)](#phase-2---web-security-core-6–12-bulan)
- [Phase 3 — Bug Bounty Hunting (3–6 bulan setelah Phase 2)](#phase-3---bug-bounty-hunting-3–6-bulan-setelah-phase-2)
- [Phase 4 — Advanced / Spesialisasi (6+ bulan)](#phase-4---advanced--spesialisasi-6-bulan)
- [Phase 5 — Security Analyst / Karir Korporat](#phase-5---security-analyst--karir-korporat)
- [Toolkit Lengkap (ringkasan)](#toolkit-lengkap-ringkasan)
- [Contoh Workflow Nyata](#contoh-workflow-nyata)
- [Jalur Belajar untuk Pemula Total (bahasa Indonesia)](#jalur-belajar-untuk-pemula-total-bahasa-indonesia)
- [Onno Center — Resource Lokal & Gratis](#onno-center---resource-lokal--gratis)
- [Rekomendasi Langkah Awal & Rutinitas Harian](#rekomendasi-langkah-awal--rutinitas-harian)
- [Catatan Etika & Hukum](#catatan-etika--hukum)

---

## Pendahuluan
Roadmap ini ditujukan untuk siapa saja yang ingin menjadi security professional legal: bug bounty hunter, penetration tester, atau security analyst. Fokus pada praktik yang sah (white-hat), pembangunan portofolio, dan sumber daya nyata yang dipakai di industri.

> Catatan: Jangan gunakan panduan ini untuk kegiatan ilegal. Selalu konfirmasi scope sebelum menguji sistem yang bukan milik Anda.

---

## Phase 1 - Fundamental Foundation (3–6 bulan)

Tujuan: memahami konsep dasar yang menjadi pondasi keamanan aplikasi dan jaringan.

### Mindset yang penting
- Tanyakan: "Bagaimana ini bekerja?" bukan hanya "Bagaimana cara memecahkan?"
- Empati terhadap developer: pahami flow aplikasi.
- Pikirkan edge cases dan boundary conditions.

### Area inti, sumber & tools
| Area | Sumber singkat | Tools / contoh |
|---|---:|---|
| Networking dasar | Professor Messer, Practical Networking | Wireshark, nmap, netcat |
| Web fundamentals | MDN, The Odin Project | Browser DevTools, curl, Postman |
| Linux basics | OverTheWire Bandit, Linux Journey | Terminal, bash, tmux |
| HTTP protocol | RFC 7230–7235, HTTP Dev Handbook | Burp Suite (Community), curl |

Praktik: jangan hanya membaca — bangun lab sendiri (VirtualBox + Kali Linux + target seperti Metasploitable / DVWA).

---

## Phase 2 - Web Security Core (6–12 bulan)

Tujuan: menguasai OWASP Top 10 dan teknik manual/otomatisasi untuk pengujian aplikasi web.

### Vulnerabilities & cara praktis
| Vulnerability | Cara praktik | Tools |
|---|---|---|
| SQL Injection | Latihan di PortSwigger Labs, SQLi Lab | sqlmap, Burp Intruder, payload manual |
| XSS | PortSwigger Labs, XSS Game | Burp Repeater, xsshunter |
| SSRF | PortSwigger Labs | Burp Collaborator, interactsh |
| IDOR | Manual testing (ubah parameter ID) | Burp Repeater, Autorize |
| Authentication Bypass | JWT.io, PortSwigger | jwt_tool, hashcat |
| File upload | Boundary testing, magic bytes | polyglot files, manual checks |

### Framework berpikir saat testing
1. Peta aplikasi — endpoints dan fungsi utama.
2. Otentikasi — bagaimana flow login dan post-login.
3. Otorisasi — batasan antar user/role.
4. Input handling — titik masuk data pengguna.
5. State management — cookies, token, session handling.
6. Logic flow — bisa melewati langkah/urutan?

Sumber praktis: PortSwigger Web Security Academy (gratis), PentesterLab, HackTheBox, TryHackMe.

---

## Phase 3 - Bug Bounty Hunting (3–6 bulan setelah Phase 2)

Tujuan: menerapkan skill pada program bug bounty, membangun reputasi dan portofolio.

### Platform populer
- [HackerOne](https://hackerone.com) — program besar, scope ketat
- [Bugcrowd](https://bugcrowd.com) — VRT jelas
- Intigriti, YesWeHack, Synack (undangan)

### Metodologi singkat (cara kerja bug hunter)
- STEP 1 — Reconnaissance (80% waktu)
  - Tools: subfinder, httpx, nuclei, gau/waybackurls, katana, ffuf, amass, dnsx, naabu
  - Cari: file sensitive (.env, backup), API docs, subdomains, storage exposure (S3), dll.

- STEP 2 — Mapping & Analysis
  - Proxy semua request (Burp), daftar endpoints, baca JS untuk menemukan internal endpoints/API keys.

- STEP 3 — Vulnerability Detection
  - Gunakan kombinasi automated + manual (contoh sqlmap, manual payloads, XSS payloads). See examples:

```bash
# contoh scanning otomatis SQLi
sqlmap -u "https://target.com/page?id=1" --batch --level 3 --risk 2
```

Contoh payload XSS:

```html
<script>alert(1)</script>
"><img src=x onerror=alert(1)>
```

- STEP 4 — Reporting
  - Format laporan yang jelas (title, severity, endpoint, parameter, reproduksi, impact, rekomendasi).

Contoh template laporan (Markdown):

```markdown
## [TITLE] — IDOR pada endpoint /api/users/profile
**Severity:** High (CVSS: 7.5)
**Endpoint:** POST /api/users/profile
**Parameter:** user_id (integer)

**Steps to reproduce:**
1. Login sebagai user A
2. Intercept request POST /api/users/profile
3. Ubah user_id dari 1234 ke 1235
4. Response mengandung data user B

**Recommendation:** Implement object-level access control.
```

---

## Phase 4 - Advanced (Spesialisasi, 6+ bulan)

Pilih jalur spesialisasi:

A) Web Application Security — Burp Suite Pro advanced, GraphQL, OAuth2/SSO bypass, prototype pollution.

B) Mobile Security — Frida, Objection, jadx, APKTool, mobile API testing.

C) Infrastructure / Cloud Security — AWS S3/IAM, Azure managed identity, GCP service account, Kubernetes RBAC.

D) Source Code Review — Semgrep, CodeQL, manual review untuk logic bugs & hardcoded secrets.

---

## Phase 5 - Security Analyst (Path korporat)

Skill penting:
- Report writing, komunikasi non-technical, process mapping (SDLC, CI/CD), risk assessment (CVSS), compliance (ISO 27001, SOC2), tooling SIEM/EDR.

Cara masuk industri: bug bounty + portofolio, kontribusi open-source, CTF, sertifikasi (OSCP/PNPT), jaringan komunitas.

---

## Toolkit Lengkap (Ringkasan)

| Kategori | Tools | Fungsi |
|---|---|---|
| Recon | subfinder, amass, httpx, naabu, dnsx | Enumeration |
| Content discovery | ffuf, dirsearch, gobuster | Directory/parameter fuzzing |
| Web proxy | Burp Suite | Intercept & modify requests |
| Vulnerability scanner | nuclei, nikto, OWASP ZAP | Automated scanning |
| SQL | sqlmap, jSQL | SQL injection automation |
| XSS | XSStrike, dalfox | XSS automation |
| JS analysis | LinkFinder, jsubfinder, SecretFinder | Cari endpoint & secrets di JS |
| Network | Wireshark, tcpdump, nmap, Masscan | Network analysis |
| Cracking | hashcat, John, Hydra | Password cracking |
| OSINT | theHarvester, Recon-ng, Maltego | Open source intel |
| Browser setup | Firefox + FoxyProxy + Cookie Editor | Manual testing |
| Terminal | tmux, zsh, ripgrep, jq | Productivity |

---

## Contoh Workflow Nyata — Satu Target

Target: https://redacted.com

1) Subdomain enumeration

```bash
subfinder -d redacted.com -all -o sub.txt
httpx -l sub.txt -o alive.txt
```

2) Live host exploration

```bash
nuclei -l alive.txt -t ~/nuclei-templates/ -o nuclei_output.txt
katana -list alive.txt -d 3 -o crawled.txt
```

3) Parameter fuzzing

```bash
ffuf -u https://admin.redacted.com/FUZZ -w /opt/wordlists/dirb/common.txt
ffuf -u "https://redacted.com/page?FUZZ=1" -w params.txt
```

4) URL collection

```bash
gau --subs redacted.com | grep -E '\.js$|\.json$|\.env$'
waybackurls redacted.com | sort -u
```

5) Manual testing (Burp Suite)
- Analisa session/token, cari IDOR, SQLi, SSTI, file upload checks.

6) Verify & dokumentasi (varian test, screenshot/video jika perlu).

7) Report sesuai format platform.

---

## Estimasi Timeline & Perkiraan Level

| Level | Waktu | Target / Kapabilitas |
|---|---:|---|
| Beginner | 0–6 bulan | Paham lab, konsep dasar |
| Junior | 6–12 bulan | Temukan SQLi, XSS, IDOR |
| Intermediate | 1–2 tahun | Multi-platform, mobile, cloud |
| Advanced | 2–4 tahun | Source code review, bypass lanjutan |
| Expert | 4+ tahun | Research, 0-day, tool building |

---

## Jalur Belajar untuk Pemula Total (bahasa Indonesia)

Ringkasan program 12 bulan (bulan demi bulan) — fokus pada praktik di lab, bukan target nyata.

- Bulan 1–2: Kenalan dengan komputer & internet; install VirtualBox.
- Bulan 3–4: Install Kali Linux + DVWA; belajar command dasar Linux.
- Bulan 5–6: Mulai belajar Burp Suite, dasar SQLi & XSS di DVWA.
- Bulan 7–9: Pindah ke TryHackMe / PortSwigger Academy.
- Bulan 10–12: Belajar dasar Python & HTML/JS; baca exploit sederhana.

Contoh praktik harian (30–60 menit):
1. Kerjakan 1 room di TryHackMe (30 menit)
2. Baca 1 laporan bug bounty (15 menit)
3. Latihan 1 perintah Linux baru (15 menit)

Contoh perintah Python sederhana:

```python
print("Hello")
x = input("Masukkan angka: ")
if x == "5":
    print("Benar")
else:
    print("Salah")

for i in range(5):
    print(i)
```

---

## Onno Center — Resource Lokal & Gratis

Beberapa sumber Onno Center (lokal, berbahasa Indonesia dan global) yang direkomendasikan:
- YouTube: [Onno Center](https://www.youtube.com/c/OnnoCenter)
  - Playlist: Jaringan Komputer, Keamanan Jaringan, Teknik Hacking
- LMS (gratis): https://lms.onnocenter.or.id/moodle/
  - Course examples:
    - Ethical Hacking (https://lms.onnocenter.or.id/moodle/course/view.php?id=141)
    - Cyber Security (https://lms.onnocenter.or.id/moodle/course/view.php?id=144)
    - Jaringan Komputer (https://lms.onnocenter.or.id/moodle/course/view.php?id=145)
- Perpustakaan digital info: http://onnocenter.or.id/wiki/index.php/OnnoCenter_eBook

Catatan: Onno Center memiliki koleksi file besar dan beberapa sumber lama di domain pribadi. Ikuti petunjuk resmi untuk akses.

---

## Resource Tambahan (ringkasan)

- Gratis: PortSwigger Web Security Academy, TryHackMe, HackTheBox Academy, PentesterLab (beberapa lab), OWASP WSTG.
- Berbayar disarankan: OSCP, PNPT, Burp Suite Pro, TryHackMe/HackTheBox premium.
- YouTube channels: IppSec, InsiderPhD, STÖK, John Hammond, The Cyber Mentor.

---

## Rekomendasi Langkah Awal

1. Siapkan VM: install VirtualBox + Kali Linux.
2. Mulai PortSwigger Academy (section Web Fundamentals).
3. Kerjakan DVWA untuk latihan SQLi & XSS.
4. Baca laporan bug bounty setiap hari untuk memahami format dan pola.

---

## Catatan Etika & Hukum

Panduan ini hanya untuk tujuan pembelajaran dan kegiatan etis (white-hat). Jangan melakukan pengujian tanpa izin. Pelanggaran hukum akan berakibat pidana dan merusak reputasi profesional.

---

Jika Anda ingin, langkah berikut yang dapat dilakukan sekarang:
- Saya bisa: memecah konten ini ke folder `/docs` menjadi file per phase + TOC di README utama.
- Atau: menambahkan badge metadata, table-of-contents otomatis, atau menerbitkan versi berbahasa Inggris.

Pilih aksi lanjutan (contoh: "split-to-docs", "add-badges", "make-english", atau "tidak ada").
