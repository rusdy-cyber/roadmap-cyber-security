```
Saya akan membantu Anda dengan roadmap yang lengkap untuk menjadi security professional (security analyst, bug hunter, penetration tester) — dengan tools, mindset, metodologi, dan resource nyata yang digunakan di industri.

Namun, saya perlu klarifikasi: Anda menyebut "black hat" yang merupakan aktivitas ilegal. Saya tidak akan membantu dengan aktivitas ilegal. Saya akan memberikan roadmap untuk menjadi ethical security professional (bug bounty hunter, security analyst, penetration tester) yang legal dan diakui industri.

ROADMAP LENGKAP: Security Professional / Bug Hunter
Phase 1: Fundamental Foundation (3-6 bulan)
Mindset yang benar:

"How does this work?" bukan "How do I break this?"
"What is the developer thinking?" — empathy-driven testing
"What edge case did they forget?" — boundary thinking
Technical Fundamentals:



Area	Sumber	Tools
Networking Dasar	Professor Messer, Practical Networking	Wireshark, nmap, netcat
Web Fundamentals	MDN, The Odin Project	Browser DevTools, curl, Postman
Linux Basics	OverTheWire Bandit, Linux Journey	Terminal, bash, tmux
HTTP Protocol	RFC 7230-7235, HTTP Dev Handbook	Burp Suite Community, curl
Cara belajar:

Jangan hanya baca — lakukan. Setiap konsep dipraktikkan.
Install lab sendiri: VirtualBox + Kali Linux + Metasploitable/DVWA
Phase 2: Web Security Core (6-12 bulan)
OWASP Top 10 — Kuasai satu per satu:



Vulnerability	Cara Praktik	Tools
SQL Injection	PortSwigger Labs, SQLi Lab	sqlmap, Burp Intruder, manual payload
XSS	PortSwigger Labs, XSS Game	Burp Repeater, xsshunter
SSRF	PortSwigger Labs	Burp Collaborator, interactsh
IDOR	Manual testing — ubah parameter ID	Burp Repeater, Autorize
Authentication Bypass	JWT.io, PortSwigger	jwt_tool, hashcat
File Upload	Manual boundary testing	magic byte manipulation, polyglot files
Framework berpikir — setiap target:




1. Peta aplikasi (sitemap) — apa saja endpoints?
2. Otentikasi — bagaimana saya login? apa yang terjadi setelah login?
3. Otorisasi — apa yang bisa user A lakukan yang tidak bisa user B?
4. Input handling — di mana input user masuk? bagaimana diproses?
5. State management — cookies, tokens, session — bagaimana dihandle?
6. Logic flow — bisa kah saya melewati step? mengulang step? membalik urutan?
Resource real:

PortSwigger Web Security Academy (GRATIS, best in class)
PentesterLab (berbayar, real-world scenarios)
HackTheBox / TryHackMe — machines dan challenges
Phase 3: Bug Bounty Hunting (3-6 bulan setelah Phase 2)
Platform Bug Bounty:


Platform	Karakteristik
HackerOne	Banyak program besar, strict scope
Bugcrowd	VRT (Vulnerability Rating Taxonomy) jelas
Intigriti	Eropa, support baik
YesWeHack	program decent
Synack (undangan)	Bayaran tinggi
Metodologi Real — Cara bug hunter bekerja:

STEP 1 — Reconnaissance (80% waktu)



Tools real yang dipake:
- subfinder / subdominator → subdomain enumeration
- httpx → cek live hosts
- nuclei → template-based scanning
- gau / waybackurls → historical URLs
- katana → crawling modern
- ffuf → content discovery /parameter fuzzing
- amass → passive + active recon
- dnsx → DNS resolution massal
- naabu → port scanning cepat
Cara berpikir recon:

"Saya ingin tahu semua yang developer tidak sengaja expose."

Cari:

.git/config exposed
swagger.json / api-docs
backup.zip, db_backup.sql
s3.amazonaws.com / storage.googleapis.com
*.dev.*, *.staging.*, *.test.*
robots.txt, sitemap.xml, security.txt
/.env, /config, /admin, /api
STEP 2 — Mapping & Analysis


- Buka aplikasi dengan Burp Suite (proksi)
- Login → logout → register → forgot password
- Capture semua request
- Baca JavaScript files (gunakan LinkFinder, jsubfinder)
- Cari API keys, internal endpoints, hardcoded credentials
STEP 3 — Vulnerability Detection Berdasarkan mapping, apply teknik spesifik:

SQL Injection:

bash



# Automated scanning
sqlmap -u "https://target.com/page?id=1" --batch --level 3 --risk 2

# Manual — time-based
' OR SLEEP(5)--
' UNION SELECT NULL,NULL,NULL--
XSS:

html


<!-- Test payloads -->
<script>alert(1)</script>
"><img src=x onerror=alert(1)>
{{constructor.constructor('alert(1)')()}}  <!-- template injection -->
IDOR:



Ganti: /api/user/1234 → /api/user/5678
Ganti: cookie/header → modifikasi
Cek parameter: ?user_id=1234 di POST body
STEP 4 — Reporting

markdown


## [TITLE] — IDOR pada endpoint /api/users/profile

**Severity:** High (CVSS: 7.5)
**Endpoint:** POST /api/users/profile
**Parameter:** user_id (integer)

**Deskripsi:**
Tidak ada validasi akses terhadap parameter user_id.
User A dapat mengakses profil User B.

**Steps to reproduce:**
1. Login sebagai user A
2. Intercept request POST /api/users/profile
3. Ubah user_id dari 1234 ke 1235
4. Response mengandung data user B (nama, email, no telp)

**Impact:**
Kebocoran data pribadi seluruh pengguna (PII leak).

**Recommendation:**
Implement object-level access control — validasi bahwa
user_id yang diminta adalah milik user yang sedang login.
Phase 4: Advanced — Specialisasi (6+ bulan)
Pilih salah satu (atau kombinasi):

A. Web Application Security

Kuasai burp suite pro macro, extensions, session handling
Auth0/OAuth2/SSO bypass techniques
GraphQL injection, batching attacks
Race conditions — HTTP/2 connection smuggling, Turbot
CSPT (Client-Side Path Traversal)
DOM clobbering, prototype pollution
B. Mobile Security

Android: Frida, Objection, jadx, APKTool
iOS: Frida, objection, class-dump, Ghidra
API testing dari mobile — sering kali lebih lemah dari web
C. Infrastructure / Cloud Security

AWS: Bucket enumeration, IAM privilege escalation
Azure: subscription hopping, managed identity abuse
GCP: service account impersonation
Kubernetes: RBAC misconfiguration, pod breakout
D. Source Code Review

Semgrep — rule-based SAST
CodeQL — variant analysis
Manual review: cari hardcoded secrets, logic bugs
Phase 5: Security Analyst — Corporate Path
Skill yang diperlukan di dunia kerja:


Skill	Kenapa penting
Report writing	Bukti kerja Anda dinilai dari laporan
Communication	Jelaskan technical ke non-technical
Process mapping	Pahami SDLC, CI/CD, deploy pipeline
Risk assessment	CVSS scoring, business impact analysis
Compliance	ISO 27001, SOC2, PCI-DSS
Tooling	Wazuh (SIEM), Suricata (IDS), Velociraptor (EDR)
Cara masuk industri:

Bug bounty — buat nama, buat CV dengan bounty
Open source — kontribusi ke security tools
CTF — Capture The Flag (CTFtime.org)
OSCP certification — gold standard entry-level pentest
Portofolio — writeup publik di Medium/blog sendiri
Network — konferensi (DEFCON, BSides), Discord security communities
TOOLS REAL — Complete Toolkit


Kategori	Tools	Fungsi
Recon	subfinder, amass, httpx, naabu, dnsx	Enumeration
Content Discovery	ffuf, dirsearch, gobuster	Directory/parameter fuzzing
Web Proxy	Burp Suite (Wajib!), Caido (alternatif)	Intercept, modify request
Vulnerability Scanner	nuclei, nikto, OWASP ZAP	Automated scanning
SQL	sqlmap, jSQL	SQL injection automation
XSS	XSStrike, dalfox	XSS automation
JavaScript	LinkFinder, jsubfinder, SecretFinder	JS analysis
Network	Wireshark, tcpdump, nmap, Masscan	Network analysis
Cracking	hashcat, John, Hydra	Password cracking
OSINT	theHarvester, Recon-ng, Maltego	Open source intel
Browser	Firefox + FoxyProxy + Cookie Editor	Manual testing
Terminal	tmux, zsh, ripgrep, jq	Productivity
RESOURCE BELAJAR REAL
GRATIS — Wajib dikerjakan:

PortSwigger Web Security Academy — https://portswigger.net/web-security
TryHackMe — Learning paths: Jr Penetration Tester, Web Fundamentals
HackTheBox Academy — Modul gratis untuk dasar
PentesterLab — 30+ lab gratis
OWASP Web Security Testing Guide (WSTG) — BIBLE manual testing
HackerOne Hacktivity — Baca report real dari bug bounty
Youtube channels:
IppSec — HackTheBox walkthroughs
InsiderPhD — Bug bounty mindset
STÖK — Bug hunting methodology
John Hammond — Malware analysis & CTF
The Cyber Mentor — Practical ethical hacking
BERBAYAR — Recommended:

PNPT (Practical Network Penetration Tester) — $399, TCM Security
OSCP (Offensive Security Certified Professional) — $1,599
HackTheBox VIP — $14/bulan
TryHackMe Premium — $10/bulan
Burp Suite Professional — $449/tahun (worth it)
MENTAL MODEL — Cara Berpikir
1. "Assume nothing, verify everything"

Jangan percaya dokumentasi
Jangan percaya bahwa input sudah divalidasi
Jangan percaya header/parameter yang tidak bisa Anda lihat
2. "Think like the developer who made a mistake"

Mereka copy-paste code dari StackOverflow
Mereka lupa hapus debug endpoint
Mereka hardcode API key karena "bentar aja"
Mereka tidak handle semua edge case
3. "Follow the data"

Di mana input user masuk?
Bagaimana data disimpan?
Bagaimana data ditampilkan kembali?
Di setiap langkah: "Bisakah saya menyelipkan sesuatu?"
4. "The simple bugs pay too"

Banyak bounty besar adalah IDOR sederhana
XSS reflected masih valid di banyak aplikasi
Open redirect + phishing scenario = high impact
5. "Automate what you can, manual what you must"

Gunakan tools untuk coverage
Manual untuk depth
Kombinasi keduanya untuk finding yang unik
CONTOH WORKFLOW REAL — Satu Target


Target: https://redacted.com

1. SUBDOMAIN ENUMERATION
   $ subfinder -d redacted.com -all -o sub.txt
   $ httpx -l sub.txt -o alive.txt

2. LIVE HOST EXPLORATION
   $ nuclei -l alive.txt -t ~/nuclei-templates/ -o nuclei_output.txt
   $ katana -list alive.txt -d 3 -o crawled.txt

3. PARAMETER FUZZING
   $ ffuf -u https://admin.redacted.com/FUZZ -w /opt/wordlists/dirb/common.txt
   $ ffuf -u https://redacted.com/page?FUZZ=1 -w params.txt

4. URL COLLECTION
   $ gau --subs redacted.com | grep -E '\.js$|\.json$|\.env$' 
   $ waybackurls redacted.com | sort -u

5. MANUAL TESTING (Burp Suite)
   - Login → analisa session/token
   - Cari IDOR di setiap endpoint dengan parameter numerik
   - Cari SQLi di parameter search/filter
   - Cari SSTI di input form
   - Cek file upload → double extension, magic bytes

6. VERIFY
   - Jika finding: test 3-5 varian
   - Dokumentasi langkah eksak
   - Screenshot + video jika perlu

7. REPORT
   - Tulis dengan format clear
   - Include impact assessment
   - Kirim ke platform bug bounty
Estimasi Timeline Real


Level	Waktu	Gaji (IDR)	Kemampuan
Beginner	0-6 bulan	-	Bisa nge-lab, paham dasar
Junior	6-12 bulan	5-10 juta/bln	Bisa temukan SQLi, XSS, IDOR
Intermediate	1-2 tahun	10-25 juta/bln	Multi-platform, mobile, cloud
Advanced	2-4 tahun	25-50 juta/bln	Source code review, advanced bypass
Expert	4+ tahun	50-100+ juta/bln	Research, 0-day, tool building
Kesimpulan
Jalan yang benar — jangan black hat:

Black hat hanya menghasilkan:

Masalah hukum (pidana, penjara)
Reputasi hancur (tidak bisa kerja di IT security)
Gaji tidak stabil (underground economy)
Risiko fisik (balas dendam)
White hat menghasilkan:

Karir legal, dihormati
Gaji tinggi, stabil, berkembang
NETWORK dengan profesional global
Kepuasan — Anda memperbaiki keamanan, bukan merusak
Mulai dari sekarang:

Install Kali Linux di VM
Buka PortSwigger Academy
Kerjakan lab SQLI pertama
Join Discord security communities
Buat akun HackerOne
Read bug bounty reports every day
Ada yang spesifik ingin saya breakdown lebih detail?


di mana saya bisa mengausai jika saya seorang awam sangkat sangat awam
Bagus — Anda jujur tentang level Anda. Itu justru langkah pertama yang benar. Saya akan buatkan jalur belajar untuk orang yang benar-benar nol pengalaman programming, networking, atau IT sama sekali.

JALUR BELAJAR UNTUK AWAM TOTAL
Sebelum mulai — yang perlu Anda pahami
Anda TIDAK perlu bisa:

Programming dulu
Matematika rumit
Bahasa Inggris tingkat tinggi (google translate cukup)
Lulusan IT
Yang Anda PERLU:

Komputer (RAM minimal 8GB, lebih baik 16GB)
Koneksi internet stabil
Kesabaran — Anda akan gagal berkali-kali
Google translate — untuk baca dokumentasi Inggris
BULAN 1-2: Kenalan dengan Komputer & Internet
JANGAN langsung belajar hacking — itu akan membuat Anda bingung dan menyerah.



Minggu	Materi	Sumber Bahasa Indonesia	Target
1	Apa itu komputer? CPU, RAM, harddisk, OS	Youtube: "Dasar komputer untuk pemula"	Paham komponen komputer
2	Apa itu internet? Cara kerja website	Youtube: "Bagaimana internet bekerja"	Paham client-server, URL, DNS
3	Install Windows + VirtualBox	Video tutorial PakDosen / local youtube	Bisa install VirtualBox
4	Apa itu Linux? Kenalan dengan terminal	Youtube: "Belajar Linux dasar"	Paham beda Windows dan Linux
Praktik minggu 4:

bash



# Buka terminal Linux (install Ubuntu di VirtualBox)
ls                    # lihat file
cd Documents          # pindah folder
mkdir belajar         # buat folder
nano tes.txt          # buat file teks
BULAN 3-4: Install Tools & Main-Main


Minggu	Materi	Resource
5-6	Install Kali Linux di VirtualBox	Youtube: "Cara install Kali Linux di VirtualBox" — cari yang bahasa Indonesia
7	Kenalan dengan terminal Kali	Buka terminal, coba: pwd, ls, cd, mkdir, nano
8	Install target lab — DVWA	Youtube: "Cara install DVWA di Kali Linux"
DVWA (Damn Vulnerable Web Application) — ini website sengaja dibuat lemah untuk latihan. Anda akan belajar hacking di sini, bukan di website sungguhan.

BULAN 5-6: Mulai Belajar Hacking (di Lab Saja!)
Gunakan DVWA yang sudah diinstall.



Minggu	Materi	Caranya
9	Apa itu Burp Suite?	Buka Burp Suite, set proxy di browser. Youtube: "Burp Suite untuk pemula"
10	SQL Injection basic	Set DVWA ke low security. Coba: ' OR 1=1-- di form login
11	SQL Injection lanjutan	Ikuti tutorial di Youtube bahasa Indonesia: "SQL Injection DVWA"
12	XSS basic	Coba: <script>alert(1)</script> di form komentar DVWA
BULAN 7-9: Naik Level — Platform Belajar Interaktif


Platform	Biaya	Bahasa	Link
TryHackMe	Gratis (premium $10/bln)	Inggris (bisa pake translate)	tryhackme.com
PortSwigger Academy	GRATIS	Inggris	portswigger.net/web-security
CodingGame	Gratis	Inggris	codingame.com (belajar programming)
Di TryHackMe, ambil jalur ini:

Complete Beginner Path (paths → complete beginner)
Web Fundamentals
John Hammond's room recommendations
BULAN 10-12: Programming Dasar
Anda tidak perlu jadi programmer, tapi harus baca kode sederhana.


Bahasa	Belajar untuk apa	Sumber Indonesia
Python	Otomatisasi tools, baca script exploit	Youtube: "Belajar Python dasar Indonesia" — 10 video pertama cukup
HTML	Paham struktur website	W3Schools Bahasa Indonesia
JavaScript	Paham cara website jalan	Web programming unpas (Youtube)
SQL	Paham SQL Injection	Youtube: "Belajar SQL untuk pemula"
Target programming:

python


# Python — yang Anda perlu bisa:
print("Hello")
x = input("Masukkan angka: ")
if x == "5":
    print("Benar")
else:
    print("Salah")

for i in range(5):
    print(i)

# Baca kode exploit dan paham alurnya
RESOURCE BAHASA INDONESIA — Link Nyata
Ini channel Youtube dan website yang menggunakan bahasa Indonesia dan cocok untuk awam:


Channel / Website	Materi	Link
Dea Afrizal	Linux, jaringan, hacking dasar	Cari di Youtube: Dea Afrizal
Id-Networkers	Linux, networking	Youtube: Id-Networkers
Kelas Terbuka	Programming (murah, jelas)	Youtube: Kelas Terbuka
Web Programming UNPAS	HTML, CSS, JavaScript, PHP	Youtube: Web Programming UNPAS
Petani Kode	Programming dasar lengkap	petanikode.com
Malas Ngoding	Web development	malasngoding.com
Sekawan Studio	Linux dasar	Youtube: Sekawan Studio
Teknologi Informasi	Jaringan komputer	Youtube: Teknologi Informasi (ti)
Onno Center	Jaringan, IT fundamental	Youtube: Sekawan Studio
Website belajar bahasa Indonesia:

https://www.petanikode.com/ — programming dasar dari nol
https://www.malasngoding.com/ — web development
https://www.duniailkom.com/ — dasar IT dan programming
APLIKASI & TOOLS — Yang Anda Install


Tool	Fungsi	Download
VirtualBox	Jalankan Linux di Windows	virtualbox.org
Kali Linux	OS untuk hacking	kalilinux.org (download .iso)
Google Chrome	Browser utama	google.com/chrome
Burp Suite Community	Intercept request	portswigger.net/burp/communitydownload
Notepad++	Edit file	notepad-plus-plus.org
7zip	Ekstrak file	7-zip.org
Git	Download tools dari github	git-scm.com
Termux (Android)	Belajar terminal dari HP	Play Store
RUTINITAS HARIAN — Untuk Awam
Setiap hari, lakukan 30-60 menit:



1. Buka TryHackMe — kerjakan 1 room (30 menit)
   - Tidak paham? Google translate + catat kata asing
   - Copy paste command — jangan hafal, pahami alurnya

2. Baca 1 report bug bounty (15 menit)
   - Buka hackerone.com/hacktivity
   - Pilih report dengan severity rendah (mudah dipahami)
   - Baca: apa yang ditemukan? bagaimana caranya?

3. Praktik 1 command Linux baru (15 menit)
   - Contoh: ls, cd, pwd, cat, grep, find, nano
   - Google: "Linux command cheat sheet pdf"
CONTOH — Hari Pertama Anda Hari Ini
Langkah yang bisa Anda lakukan SEKARANG:

1. Install VirtualBox

Buka Google
Cari: "download VirtualBox for Windows"
Install seperti install game biasa
2. Download Kali Linux

Cari Google: "Kali Linux download iso 2024"
Pilih yang 64-bit, sekitar 3-4 GB
(Ini bisa 1-2 jam download — sambil nunggu, lanjut step 3)
3. Baca artikel ini (dalam bahasa Indonesia)

Buka Google: "apa itu ethical hacking pemula"
Baca 3 artikel pertama
Sembari nunggu download selesai:

Buka Youtube: "cara install Kali Linux di VirtualBox bahasa Indonesia"
Tonton sampai selesai — besok Anda akan praktik langsung
PETA BELAJAR VISUAL (Simpan Ini)



BULAN 1-2: Komputer & Internet
  └─> BULAN 3-4: Install Linux + DVWA
       └─> BULAN 5-6: SQLi & XSS di DVWA
            └─> BULAN 7-9: TryHackMe Complete Beginner
                 └─> BULAN 10-12: Python + HTML dasar
                      └─> TAHUN 2: PortSwigger Academy
                           └─> TAHUN 3: Bug bounty (HackerOne)
MINDSET UNTUK AWAM
1. "Saya akan gagal berkali-kali — itu normal"

Error bukan berarti Anda bodoh
Error berarti Anda belajar
Setiap hacker profesional pernah error ribuan kali
2. "Tidak perlu hafal — perlu paham"

Jangan hafal command
Pahami: "perintah ini SURUH KOMPUTER NGAPAIN?"
Gunakan cheat sheet (print tempel di dinding)
3. "Gunakan ChatGPT untuk bantu belajar"

"Jelaskan apa itu DNS seperti saya anak 10 tahun"
"Apa bedanya SQL Injection dan XSS?"
"Berikan contoh command Linux ls dengan penjelasan"
"Apa error ini artinya? [paste error]"
4. "Jangan bandingkan dengan orang lain"

Ada yang belajar 3 bulan sudah temukan bug
Ada yang 2 tahun baru paham
Your pace is your pace
5. "Konsistensi > Intensitas"

30 menit SETIAP HARI > 8 jam seminggu sekali
Lebih baik belajar sedikit tapi rutin
3 BUKU GRATIS BAHASA INDONESIA
"Modul Ethical Hacking" — dari forum Indonesian Security
Google: "modul ethical hacking pdf Indonesia"
"Dokumentasi OWASP" (terjemahan)
Google: "OWASP testing guide bahasa Indonesia"
"Belajar Jaringan Komputer" — dari ebook Indonesia
Google: "ebook jaringan komputer pemula pdf"
APAKAH ANDA SIAP MULAI?
Kalau Anda baca semua ini dan masih bingung — itu normal.

Langkah Anda malam ini:

Install VirtualBox (20 menit)
Download Kali Linux ISO (mulai download, biarkan)
Cari 1 video Youtube: "cara install Kali Linux di VirtualBox bahasa Indonesia"
Tonton 5 menit pertama
Besok lanjutkan
Kalau Anda mau, saya bisa bantu step-by-step lebih detail. Mulai dari mana?

aku liat onno sangat bagus juga dia memberikan recource grtis banyak berupa file tapi aku tidak tahu link nya apakah kamu tahu
Saya akan cari informasi tentang Onno Center dan resource gratis yang disediakan

Membuka halaman resmi Onno Center YouTube untuk melihat playlist dan resource

Mencari link LMS Onno Center dan resource gratis Onno W Purbo

Membuka LMS Onno Center untuk daftar course gratis

Mencari info tentang link file sharing dan perpustakaan gratis Onno W Purbo

Mencari link perpustakaan digital atau file sharing Onno W Purbo

Membuka halaman perpustakaan digital OnnoCenter untuk info cara copy file
Saya sudah dapat semua informasinya. Ini dia link dan resource lengkap dari Onno Center:

LINK-LINK PENTING ONNO CENTER
1. YOUTUBE CHANNEL: Onno Center
Link: https://www.youtube.com/c/OnnoCenter

Playlist WAJIB untuk pemula:

Jaringan Komputer (114 video): https://www.youtube.com/playlist?list=PLXHnX-wg99azXnnpHmyOEw7rKSqMAq8I9
Keamanan Jaringan (31 video): https://www.youtube.com/playlist?list=PLXHnX-wg99ayU13dXsXGuFVil8-G7H545
Teknik Hacking (67 video): https://www.youtube.com/playlist?list=PLXHnX-wg99azrZVE1xBWkx_oyQulUFWZj
2. LMS (Learning Management System) — GRATIS
Link: https://lms.onnocenter.or.id/moodle/

Course GRATIS yang bisa kamu daftar (tinggal bikin akun):


Course	Link Langsung
Ethical Hacking (Open Course)	https://lms.onnocenter.or.id/moodle/course/view.php?id=141
Cyber Security (Open Course)	https://lms.onnocenter.or.id/moodle/course/view.php?id=144
Jaringan Komputer (Open Course)	https://lms.onnocenter.or.id/moodle/course/view.php?id=145
Keamanan Informasi	https://lms.onnocenter.or.id/moodle/course/view.php?id=139
IT Forensic	https://lms.onnocenter.or.id/moodle/course/view.php?id=142
Sistem Operasi	https://lms.onnocenter.or.id/moodle/course/view.php?id=138
Web Programming	https://lms.onnocenter.or.id/moodle/course/view.php?id=147
Database	https://lms.onnocenter.or.id/moodle/course/view.php?id=148
Big Data & Data Analysis	https://lms.onnocenter.or.id/moodle/course/view.php?id=143
AI untuk IT	https://lms.onnocenter.or.id/moodle/course/view.php?id=154
3. PERPUSTAKAAN DIGITAL (FILE GRATIS)
Halaman info: http://onnocenter.or.id/wiki/index.php/OnnoCenter_eBook

Ini perpustakaan fisik — Onno mengumpulkan 12+ TB file selama 10+ tahun. Cara dapatnya:

Kamu kirim harddisk ke alamat Onno
Dia akan copy-kan gratis
Cuma bayar doa + ongkos kirim balik Rp 20.000
Isi perpustakaannya:

Hacking & Cyber Security: ~300 GB
Video OWP: 2+ TB
IT umum: ~1 TB
eBook PDF, Science, Engineering: ~4 TB
Total: 10+ TB
Alamat kirim harddisk:



Onno W. Purbo
PAUD Kancil, Jl. Swadaya IV/21
Cempaka Baru, Kemayoran
Jakarta Pusat 10640
Telp: 08119222389
Catatan: File 99.9% berbahasa Inggris. Tapi untuk belajar, ini harta karun.

4. WEBSITE LAMA OWP — Mashup Buku Gratis
Dulu Onno banyak naro file gratis di sini (sebagian masih hidup):

http://sandbox.bellanet.org/~onno/
http://onno.vlsm.org/
http://www.ilmukomputer.com/
5. GITHUB — Buku Kubernetes Gratis
https://github.com/ngoprek-kubernetes/buku-kubernetes-pemula
REKOMENDASI SAYA — Mulai dari Sini
Untuk kamu yang awam total, urutan belajar dari Onno Center:

Buat akun di LMS: https://lms.onnocenter.or.id/moodle/login/signup.php
Ambil course "Jaringan Komputer" — ini fundamental
Tonton playlist YouTube Jaringan Komputer (114 video) — tonton 10-15 menit per hari
Setelah paham jaringan, ambil "Cyber Security"
Terakhir ambil "Ethical Hacking"
```