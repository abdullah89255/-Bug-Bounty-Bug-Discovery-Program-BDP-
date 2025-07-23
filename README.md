# -Bug-Bounty-Bug-Discovery-Program-BDP-
To start working on a **Bug Bounty / Bug Discovery Program (BDP)** — often referred to as **bug hunting** — you'll need a structured approach. Here's a step-by-step **beginner-to-advanced roadmap** to help you get started and become successful in the field.

---

## 🔰 STEP 1: **Understand Bug Bounty Basics**

### ✅ What is Bug Hunting?

Bug hunting is finding and responsibly reporting security vulnerabilities in websites, applications, or systems in exchange for **rewards** or **recognition**.

### ✅ Platforms Offering Bug Bounties

* [HackerOne](https://hackerone.com/)
* [Bugcrowd](https://bugcrowd.com/)
* [Synack](https://synack.com/)
* [Intigriti](https://intigriti.com/)
* [YesWeHack](https://yeswehack.com/)
* [Open Bug Bounty](https://openbugbounty.org/)
* [Zerocopter](https://zerocopter.com/)
* Private programs (via responsible disclosure policies)

---

## 🧰 STEP 2: **Build Your Toolkit**

### ✅ Essential Tools:

| Category              | Tools Example                                              |
| --------------------- | ---------------------------------------------------------- |
| Reconnaissance        | `Amass`, `Subfinder`, `Assetfinder`, `Waybackurls`         |
| Scanning              | `Nmap`, `httpx`, `Naabu`, `Shodan`                         |
| Vulnerability Testing | `Nuclei`, `Burp Suite`, `FFUF`, `SQLmap`, `Xray`, `Dalfox` |
| Source Inspection     | `JSParser`, `LinkFinder`, `SecretFinder`                   |
| Automation            | `GF`, `Hakrawler`, `Qsreplace`, `KXSS`                     |

Install them in Kali Linux or a VM to keep your setup isolated.

---

## 🧠 STEP 3: **Learn Common Vulnerabilities**

Start with these OWASP Top 10:

1. **XSS (Cross-Site Scripting)**
2. **SQL Injection**
3. **IDOR (Insecure Direct Object Reference)**
4. **SSRF (Server-Side Request Forgery)**
5. **LFI/RFI (File Inclusions)**
6. **Open Redirect**
7. **Authentication/Authorization Issues**
8. **Information Disclosure**
9. **Clickjacking**
10. **Command Injection**

🛠 Practice on:

* [Hack The Box](https://www.hackthebox.com/)
* [PortSwigger Web Security Academy](https://portswigger.net/web-security)
* [TryHackMe](https://tryhackme.com/)
* [VulnHub](https://vulnhub.com/)
* DVWA, Juice Shop, bWAPP (offline vulnerable apps)

---

## 🛰️ STEP 4: **Target Reconnaissance**

Use automation and manual hunting to gather intel:

```bash
subfinder -d target.com -o subs.txt
httpx -l subs.txt -o live.txt
nuclei -l live.txt -t cves/
```

More advanced:

* Enumerate JS files, hidden params
* Use `gau`, `waybackurls` for old endpoints
* Use `Burp Suite` for intercepting requests

---

## 📝 STEP 5: **Reporting Bugs Properly**

1. **Include**: Summary, steps to reproduce, impact, and PoC (Proof of Concept)
2. **Be Professional**: Write clearly and responsibly.
3. **Don’t Spam**: Submitting duplicates or low-quality reports can get you banned.
4. **Respect Scope**: Only test systems listed in the program scope.

Example (simplified):

```
Summary: Reflected XSS in /search?q= parameter
Steps:
1. Go to https://example.com/search?q=<script>alert(1)</script>
2. See alert box

Impact: Attacker can execute JS in user’s browser.

PoC: [insert screenshot or video]
```

---

## 🧭 STEP 6: **Stay Updated & Learn**

* Follow top bug hunters on Twitter/X
* Read write-ups: [HackerOne Hacktivity](https://hackerone.com/hacktivity), Medium blogs
* Join communities: Discords, Telegram groups, Reddit `r/bugbounty`
* Watch:

  * NahamSec (YouTube/Twitch)
  * InsiderPhD
  * Live hacking videos

---

## 💸 Bonus: Earning Potential

* Beginner: \$0–\$500/month
* Intermediate: \$500–\$2,000/month
* Pro: \$5,000+/month or full-time career

---

## 🚀 Tips to Level Up Faster

* Automate repetitive tasks with bash/python
* Use `tmux` or `screen` for session management
* Build your own recon script
* Develop strong Google Dorking and OSINT skills
* Be consistent: Hunt daily or weekly

---

Would you like:

* A beginner bash script for automated bug bounty recon?
* A roadmap infographic?
* A checklist for each bounty target?

Let me know — happy to provide more resources.
Enumerating **JavaScript (JS) files** and **hidden parameters** is crucial in bug bounty hunting — especially for discovering hidden APIs, sensitive endpoints, and parameters that can lead to XSS, IDOR, auth bypass, etc.

Here’s how you can do that **step-by-step** using tools + manual tricks.

---

## 🎯 1. **Find JavaScript Files (JS Enumeration)**

### ✅ From URLs:

```bash
waybackurls target.com | grep "\.js$"
gau target.com | grep "\.js$"
```

### ✅ With `hakrawler`:

```bash
hakrawler -url https://target.com -js -depth 2
```

### ✅ With `subjs` (extract JS from subdomains):

```bash
subjs -i live.txt -o jsfiles.txt
```

---

## 📥 2. **Download & Inspect JS Files**

Use `wget`, `curl`, or `getjs` to grab JS files for analysis:

```bash
wget https://target.com/assets/app.js
```

Now search for:

* Endpoints (`/api/user`, `/admin`)
* Secrets or keys
* Parameters
* Hidden functions

### 🔍 Grep useful things:

```bash
cat app.js | grep -E "fetch|axios|XMLHttpRequest|token|auth|api|key|client_id"
```

---

## 🧰 Tools to Extract Info from JS Files

### ✅ **LinkFinder** (Find endpoints in JS)

```bash
git clone https://github.com/GerbenJavado/LinkFinder.git
cd LinkFinder
python3 linkfinder.py -i https://target.com/assets/app.js -o cli
```

### ✅ **JSParser** (Extract endpoints & URLs)

```bash
git clone https://github.com/nahamsec/JSParser.git
cd JSParser
python3 parser.py --url https://target.com/script.js
```

### ✅ **SecretFinder** (Find API keys/secrets in JS)

```bash
git clone https://github.com/m4ll0k/SecretFinder.git
cd SecretFinder
python3 SecretFinder.py -i https://target.com/app.js -o cli
```

---

## 🧪 3. **Enumerate Hidden Parameters**

These parameters may not show up in the UI but are used internally via JS or backend.

### ✅ Use `ParamSpider`:

```bash
git clone https://github.com/devanshbatham/ParamSpider
cd ParamSpider
python3 paramspider.py -d target.com -o output
```

### ✅ Use `GF` + `waybackurls`:

```bash
waybackurls target.com | gf xss
waybackurls target.com | gf sqli
```

You can also extract all parameters:

```bash
waybackurls target.com | grep '?'
```

---

## 🧠 4. **Manually Inspect in DevTools**

1. Open browser → F12 → Network tab
2. Reload page and watch JS & XHR requests
3. Inspect JS files and look for:

   * Functions
   * API calls
   * Dynamic parameter names (e.g., `user_id`, `auth_token`)
   * Source maps (`.map` files)

---

## 📦 Bonus: Automation Script Example

```bash
#!/bin/bash
domain=$1

echo "[*] Getting JS files from $domain ..."
gau $domain | grep "\.js$" | tee jsurls.txt

echo "[*] Finding endpoints in JS files ..."
for js in $(cat jsurls.txt); do
    python3 linkfinder.py -i $js -o cli
done

echo "[*] Extracting hidden parameters ..."
paramspider -d $domain -o output
```

---

## ✅ Summary

| Task                   | Tool/Method                                |
| ---------------------- | ------------------------------------------ |
| Get JS URLs            | `gau`, `waybackurls`, `hakrawler`, `subjs` |
| Extract endpoints      | `LinkFinder`, `JSParser`                   |
| Find secrets/tokens    | `SecretFinder`                             |
| Find hidden parameters | `ParamSpider`, `GF`, manual JS review      |
| Manual inspection      | Browser DevTools (Network + Sources tab)   |

---

Would you like:

* A Bash script to automate all of this?
* Example JS files and what to look for?
* Training labs to practice JS analysis?

Let me know how deep you want to go.
Here’s a **Bash script** that automates the full process of:

* Getting JS files
* Extracting endpoints and secrets
* Enumerating hidden parameters

Perfect for **bug bounty recon** or red teaming.

---

### ✅ **Automated JS Recon & Param Enumeration Script**

```bash
#!/bin/bash

# === CONFIG ===
domain=$1
outdir="recon-${domain}"
mkdir -p "$outdir"

echo "[*] Starting JS & Parameter Enumeration for: $domain"
sleep 1

# === Step 1: JS File Collection ===
echo "[*] Collecting JavaScript file URLs..."
gau $domain | grep '\.js$' | sort -u > $outdir/js_urls.txt
waybackurls $domain | grep '\.js$' | sort -u >> $outdir/js_urls.txt
subjs -i <(echo $domain) >> $outdir/js_urls.txt
sort -u $outdir/js_urls.txt -o $outdir/js_urls.txt

echo "[+] Found $(wc -l < $outdir/js_urls.txt) unique JS files."

# === Step 2: Extract Endpoints from JS ===
echo "[*] Extracting endpoints from JS files using LinkFinder..."
mkdir -p "$outdir/endpoints"
while read -r jsurl; do
    filename=$(echo $jsurl | sed 's/[^a-zA-Z0-9]/_/g')
    python3 linkfinder.py -i "$jsurl" -o cli > "$outdir/endpoints/$filename.txt"
done < "$outdir/js_urls.txt"

# === Step 3: Extract Secrets from JS ===
echo "[*] Searching for secrets in JS using SecretFinder..."
mkdir -p "$outdir/secrets"
while read -r jsurl; do
    filename=$(echo $jsurl | sed 's/[^a-zA-Z0-9]/_/g')
    python3 SecretFinder.py -i "$jsurl" -o cli > "$outdir/secrets/$filename.txt"
done < "$outdir/js_urls.txt"

# === Step 4: Parameter Enumeration ===
echo "[*] Extracting parameters using ParamSpider..."
paramspider -d $domain -o "$outdir/paramspider.txt" --exclude woff,ttf,svg,eot,png,jpg

# === Step 5: Extra: Arjun Test on Found URLs (Optional) ===
echo "[*] Optional: Use Arjun manually to test dynamic pages."

echo "🎯 Recon complete. Output saved in: $outdir/"
```

---

### 📦 Requirements (install if not done):

```bash
# Gau
go install github.com/lc/gau@latest

# Waybackurls
go install github.com/tomnomnom/waybackurls@latest

# Subjs
go install github.com/lc/subjs@latest

# LinkFinder
git clone https://github.com/GerbenJavado/LinkFinder.git
cd LinkFinder && pip install -r requirements.txt

# SecretFinder
git clone https://github.com/m4ll0k/SecretFinder.git
cd SecretFinder && pip install -r requirements.txt

# ParamSpider
git clone https://github.com/devanshbatham/paramspider.git
cd paramspider && pip install -r requirements.txt
```

---

### ✅ Usage:

```bash
chmod +x js_enum.sh
./js_enum.sh target.com
```

---

Let me know if you want to:

* Add `Arjun` automation
* Save all JS files locally
* Export everything to one HTML or JSON report

I can expand the script for you.


