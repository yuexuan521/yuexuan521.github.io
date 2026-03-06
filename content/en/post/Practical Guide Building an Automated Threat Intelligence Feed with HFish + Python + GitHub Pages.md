---
title: "Practical Guide: Building an Automated Threat Intelligence Feed with HFish + Python + GitHub Pages"
date: 2026-03-05 12:00:00
category: ""
categories: 
  - ""
tags:
- "Threat Intelligence"
- "HFish"
- "Honeypot"
- "Server"
- "Cybersecurity"
- "Github"
---

[HFish API Configuration Documentation](https://github.com/hacklcx/HFish/blob/master/docs/6-4-api.md)

[Open-Source Threat Intelligence Example: ip_list](https://yuexuan521.github.io/honeypot-blocklist/ip_list.txt)

[honeypot-blocklist Project Repository](https://github.com/yuexuan521/honeypot-blocklist)

## Planning

The most essential characteristic of a honeypot is this: **no legitimate business traffic should ever access it**. Therefore, any data entering a honeypot is essentially “suspicious” or “malicious” by nature. This gives honeypot-collected data an **extremely high signal-to-noise ratio (high fidelity)**.

A honeypot can capture basic attacker information and convert it into **Indicators of Compromise (IOCs)**:

- **Attacker source IP addresses**: Identify where attackers come from (country, ASN, proxy pool).
- **Malicious file hashes (File Hash)**: MD5/SHA256 of uploaded malware samples.
- **Malicious domains/URLs**: Addresses of C2 (Command and Control) servers contacted by malware.
- **Purpose**: Synchronize this data in real time to firewalls (FW), WAFs, or intrusion detection systems (IDS), enabling **“attacked once, blocked everywhere.”**

This article demonstrates how to automatically extract attack information obtained from the HFish honeypot through its built-in API, and distribute it via GitHub/Gitee Pages. (Using simple attacker source IP extraction as an example.)

### Architecture Design

1. **Data source**: An HFish honeypot deployed on an internal or public network.

   Deployment tutorial: [Full Guide to Deploying an HFish Honeypot on a Cloud Server](https://www.freebuf.com/articles/sectool/457499.html)

2. **Processing center**: An intermediate server running a Python script (this can be the HFish host itself).

3. **Publishing platform**: GitHub or Gitee (using their Pages service to host static text files). ( [GitHub](https://github.com/) )

4. **Final output**: A publicly accessible URL (for example: https://yuexuan521.github.io/honeypot-blocklist/ip_list.txt).

## Step 1: Prepare the HFish API

HFish provides an API for retrieving attack data.

1. Log in to the HFish admin panel.
2. Go to **“System Settings” -> “API Settings”**.
3. Obtain the **API Key** and **admin panel address**.
   - *Note: If your HFish is deployed on an internal network, make sure the machine running the script can access the HFish admin port (default: 4433).*

![image-20251227102507885](https://raw.githubusercontent.com/yuexuan521/image/main/20260305220719188.png)

## Step 2: Write the Automated Extraction Script (Python)

We need to write a Python script to perform the workflow: “fetch data -> filter whitelist -> format -> write to file”.

Create `/root/generate_feed.py` on the HFish server or a machine that can access HFish: (you need to modify the values of `HFISH_HOST`, `API_KEY`, and `OUTPUT_TXT` on line 10)

```python
import requests
import json
import ipaddress
import urllib3
import time
import sys
from datetime import datetime, timedelta

# ================= Configuration =================
HFISH_HOST = "https://IP:4433"                       #!!Fill in your HFish URL here!!
API_KEY = ""                                         #!!Fill in your HFish API Key here!!
OUTPUT_TXT = "/root/threat-feed/ip_list.txt"         #!!Fill in the path where you want to save the file!!
TIME_WINDOW_HOURS = 24 

LOCAL_WHITELIST = [
    "127.0.0.1", "192.168.0.0/16", "10.0.0.0/8", "172.16.0.0/12",
    "8.8.8.8", "1.1.1.1", "60.204.200.232"
]
WHITELIST_URLS = {
    "bing": "https://www.bing.com/toolbox/bingbot.json",
    "github": "https://api.github.com/meta"
}
# =========================================

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

class WhitelistManager:
    def __init__(self):
        self.whitelist_cidrs = []
        for ip in LOCAL_WHITELIST:
            try:
                self.whitelist_cidrs.append(ipaddress.ip_network(ip, strict=False))
            except: pass

    def fetch_remote_whitelists(self):
        print("[-] Fetching remote whitelists...")
        for name, url in WHITELIST_URLS.items():
            try:
                resp = requests.get(url, timeout=10)
                if resp.status_code == 200:
                    data = resp.json()
                    prefixes = []
                    if "prefixes" in data: prefixes = [p.get("ipv4Prefix") for p in data["prefixes"]]
                    elif "web" in data: prefixes = data.get("web", [])
                    for p in prefixes:
                        if p and "." in p:
                            self.whitelist_cidrs.append(ipaddress.ip_network(p))
            except: pass

    def is_whitelisted(self, ip_str):
        try:
            target = ipaddress.ip_address(ip_str)
            for network in self.whitelist_cidrs:
                if target in network: return True
        except: pass
        return False

def get_data():
    url = f"{HFISH_HOST}/api/v1/attack/ip?api_key={API_KEY}"
    end_time = int(time.time())
    start_time = 0 if TIME_WINDOW_HOURS == 0 else int(end_time - (TIME_WINDOW_HOURS * 3600))
    
    payload = {
        "start_time": start_time,
        "end_time": end_time,
        "intranet": 0,
        "threat_label": []
    }
    
    try:
        resp = requests.post(url, json=payload, headers={'Content-Type': 'application/json'}, verify=False, timeout=20)
        return resp.json()
    except Exception as e:
        print(f"[!] Request Error: {e}")
        return None

def main():
    wl = WhitelistManager()
    wl.fetch_remote_whitelists()
    
    result = get_data()
    if not result: return

    raw_ips = []
    
    if 'data' in result:
        data_content = result['data']
        print(f"[-] API Response Keys: {data_content.keys() if isinstance(data_content, dict) else 'List Type'}")
        
        if isinstance(data_content, list):

            raw_ips = data_content
        elif isinstance(data_content, dict):

            if 'attack_ip' in data_content:
                raw_ips = data_content['attack_ip']
            elif 'list' in data_content:
                raw_ips = data_content['list']
            else:
                print("[!] Error: Unknown dict structure in 'data'")
                print(data_content) # Print it out for inspection
    else:
        print(f"[!] Error: No 'data' field. keys: {result.keys()}")

    print(f"[-] Raw IPs found: {len(raw_ips)}")


    clean_ips = set()
    for item in raw_ips:
        ip = None

        if isinstance(item, str):
            ip = item

        elif isinstance(item, dict):
            ip = item.get('source_ip') or item.get('ip') or item.get('attack_ip')
            

        if ip and "." in ip and "attack_ip" not in ip:
            if not wl.is_whitelisted(ip):
                clean_ips.add(ip)

    print(f"[-] Final Unique IPs: {len(clean_ips)}")


    with open(OUTPUT_TXT, 'w') as f:
        f.write(f"# HFish Threat Feed\n")
        f.write(f"# Updated: {datetime.now()}\n")
        for ip in clean_ips:
            f.write(f"{ip}\n")
    print(f"[-] Saved to {OUTPUT_TXT}")

if __name__ == "__main__":
    main()
~~~

## Step 3: Create an Open-Source Repository (GitHub/Gitee)

1. Create a new repository on GitHub, for example `honeypot-blocklist`.
2. Install Git on your server and clone the repository. (replace `yourusername` with your actual username)


# Run on the server
cd /root/
git clone https://github.com/yourusername/honeypot-blocklist.git threat-feed
```

Modify the Python script configuration above so that the output path points to this Git directory.

## Step 4: Automate Updates and Pushes (Shell + Crontab)

### 1. Write the Automation Shell Script

Write a Shell script named `update_feed.sh` to combine “generate” and “push” into one workflow:

1. Create the script file:

   ```
   vim /root/update_feed.sh
   ```

2. Add the following content: (you need to modify `git user.name` and `user.email`; ✅ using the privacy email provided by GitHub is recommended)

   **Benefits of GitHub privacy email**: It protects your real email address from being exposed, while still allowing GitHub to recognize that this is your account and award “green squares” on your GitHub Contributions Graph.

   1. Log in to GitHub and go to **Settings** -> **Emails**.
   2. Check **"Keep my email addresses private"**.
   3. You will see an email like this: `12345678+yourusername@users.noreply.github.com`.

   ![image-20251228221426661](https://raw.githubusercontent.com/yuexuan521/image/main/20260305220719189.png)

   **Configuration method:** (modify step 5, Configure Git identity)

   ```
   git config user.name "Your GitHub username"
   git config user.email "12345678+yourusername@users.noreply.github.com"
   ```

   ```shell
   #!/bin/bash
   
   # ================= Path Configuration =================
   PY_SCRIPT="/root/generate_feed.py"
   GIT_REPO="/root/threat-feed"
   LOG_FILE="/var/log/hfish_feed.log"
   # ======================================================
   
   echo "-----------------------------------------------------" >> $LOG_FILE
   echo "[$(date)] Starting update process..." >> $LOG_FILE
   
   # 1. Enter the Git repository directory (this must be done first)
   cd $GIT_REPO || { echo "[Error] Cannot cd into $GIT_REPO" >> $LOG_FILE; exit 1; }
   
   # 2. [New] Pull remote updates first (to avoid push conflicts)
   # This will sync changes such as README edits made on the GitHub web page to the local repo
   echo "[-] Pulling remote changes..." >> $LOG_FILE
   if git pull origin main >> $LOG_FILE 2>&1; then
       echo "[Info] Git pull successful." >> $LOG_FILE
   else
       # If pull fails (rare), it is usually due to conflicts; log it but do not exit, and still try to push
       echo "[Warn] Git pull failed (Conflict?). Will try to push anyway." >> $LOG_FILE
   fi
   
   # 3. Run Python to extract IPs
   # Note: even if git pull fails, we still need to generate new data because the data is the core
   /usr/bin/python3 $PY_SCRIPT >> $LOG_FILE 2>&1
   
   # 4. Check whether the file was generated
   if [ ! -f "ip_list.txt" ]; then
       echo "[Error] ip_list.txt missing. Python script failed?" >> $LOG_FILE
       exit 1
   fi
   
   # 5. Configure Git identity
   git config user.name ""                          //!!Fill in your name and email here!!
   git config user.email ""
   
   # 6. Commit and push
   git add .
   
   if git commit -m "Auto update: $(date "+%Y-%m-%d %H:%M")" >> $LOG_FILE 2>&1; then
       echo "[Info] Changes committed." >> $LOG_FILE
       
       # Try to push
       if git push origin main >> $LOG_FILE 2>&1; then
            echo "[Success] Pushed to GitHub." >> $LOG_FILE
       else
            echo "[Error] Git Push failed. Retrying with --force..." >> $LOG_FILE
            # If a normal push fails, try force push (use with caution, but it is feasible in this append-only threat feed scenario)
            # git push -f origin main >> $LOG_FILE 2>&1
       fi
   else
       echo "[Info] No changes detected. Nothing to push." >> $LOG_FILE
   fi
   ```

3. Grant execute permission:

   ```
   chmod +x /root/update_feed.sh
   ```

------

### 2. Configure Passwordless SSH Push (Critical!)

When the automation script runs in the background, it cannot manually enter a GitHub username or password. You must configure an **SSH Key**.

1. **Check whether a key already exists**:

   ```
   ls ~/.ssh/id_rsa.pub
   ```

   - If the file exists, skip step 2.
   - If not (you get an error), proceed with step 2.

2. **Generate a key** (just press Enter all the way through):

   ```
   ssh-keygen -t rsa -b 4096 -C "hfish-feed"
   ```

3. **Get the public key**:

   ```
   cat ~/.ssh/id_rsa.pub
   ```

   - Copy the output content (the long string starting with `ssh-rsa`).

4. **Upload it to GitHub**:

   - Open the GitHub repository -> **Settings** -> **Deploy keys** -> **Add deploy key**.

     ![image-20251230120117688](https://raw.githubusercontent.com/yuexuan521/image/main/20260305220719190.png)

   - **Title**: HFish Server

   - **Key**: Paste the content you copied just now.

   - **Important**: Check **Allow write access**, otherwise pushing will fail!

     ![image-20251230120223456](https://raw.githubusercontent.com/yuexuan521/image/main/20260305220719191.png)

5. **Manually test the connection** (this must be done once!):
   Run on the server:

   ```
   ssh -T git@github.com
   ```

   - Type `yes` to confirm the fingerprint.
   - If you see `Hi <username>/<repo>! You've successfully authenticated...`, it means the connection works.

6. **Change the repository remote to SSH** (if you cloned using HTTPS before):
   Enter the directory and check:

   ```
   cd /root/threat-feed
   git remote -v
   ```

   - If it shows `https://github.com/...`, run:

     ```
     git remote set-url origin git@github.com:yourusername/your-repository-name.git
     ```

------

### 3. Manually Test the Full Workflow

Now let’s run the Shell script manually once and see whether it can push successfully.

```
/root/update_feed.sh
```

**Check the results:**

1. Check the log: `tail -f /var/log/hfish_feed.log`
2. Check the GitHub web page: refresh your repository and see whether the update time of `ip_list.txt` becomes `"Just now"`.

------

### 4. Set Up a Scheduled Task (Crontab)

After confirming that the manual run works correctly, the final step is to let it run automatically. We will set it to **update once every 2 hours** (to keep the feed fresh without wasting resources).

1. Edit the scheduled tasks:

   ```
   crontab -e
   ```

2. Add the following line at the end of the file:

   ```
   # Run once at minute 5 every 2 hours (staggered execution)
   5 */2 * * * /bin/bash /root/update_feed.sh
   ```

3. Save and exit (if using `vim`, press Esc, type `:wq`, and press Enter).

------

## Step 5: Open It Up for Others to Use

Now, your GitHub repository will contain `ip_list.txt`. You need to enable **GitHub Pages** (turn it on in the repository’s Settings -> Pages).

1. Go to the repository **Settings**.
2. Find **Pages** in the left sidebar.
3. Under **Build and deployment**, choose **Source** as `Deploy from a branch`.
4. Under **Branch**, choose the `main` (or `master`) branch, and select the folder `/ (root)`.
5. Click **Save**.

Once enabled, you will get a globally accessible permanent direct link, for example:
https://yourusername.github.io/honeypot-blocklist/ip_list.txt

After waiting 1–2 minutes, GitHub will generate the page, and others only need to subscribe to this URL ending in `.txt`.

Others can use our data like this:

1. **Palo Alto / Fortinet firewalls**: Create an "External Dynamic List" and fill in your URL.
2. **Linux servers**: Write a script to `wget` your file and import it into `ipset`.

**Effect demonstration:**

![image-20251230120659771](https://raw.githubusercontent.com/yuexuan521/image/main/20260305220719192.png)
