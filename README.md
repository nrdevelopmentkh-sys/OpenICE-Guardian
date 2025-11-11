# OpenICE-Guardian
A cybersecurity tool for scanning Telegram message logs, detecting suspicious APKs, scam links, and generating structured abuse reports for Trust &amp; Safety review.
# 🧊 OpenICE Guardian

**OpenICE Guardian** is a lightweight Python-based simulation tool designed to help researchers and developers analyze and monitor messages for suspicious or harmful content — such as phishing links, malware `.apk` files, or adult material — in a safe and controlled environment.  

⚠️ **Disclaimer:**  
This project is built strictly for **educational and cybersecurity research purposes**.  
It does **not** interact with Telegram servers, APIs, or any real accounts.  
Users must comply with all applicable **laws and platform terms of service**.

---

## 🚀 Features

- 🧩 Detect suspicious keywords and URLs (e.g. `t.me/`, `.apk`, `bit.ly`, `porn`, `hack`, `malware`)
- 🧱 Process messages from `.txt` or `.json` files
- 📋 Generate full scan reports (`.txt` and `.md`)
- 🕵️ Real-time flagging of harmful or malicious text
- 🧰 Simple and customizable keyword blacklist
- 🧊 Lightweight — no external dependencies required

---

## 🪜 Setup Guide

### 1️⃣ Create a project folder
```bash
mkdir OpenICE
cd OpenICE
####2️⃣  Create a virtual environment
python -m venv venv
# Activate it
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate
#####3️⃣ Create the Python file
openice_guardian.py
#!/usr/bin/env python3
"""
OpenICE Guardian – Simulation Security Monitor
Author: <Your Name>
License: MIT
"""

import re
import os
import json
import datetime
from pathlib import Path

INPUT_FILE = "messages.txt"
REPORT_DIR = Path("reports")
REPORT_DIR.mkdir(exist_ok=True)

BLACKLIST = [
    "t.me/", ".apk", "porn", "sex", "nude", "bit.ly", "hack", "phishing", "malware", "trojan"
]

def load_messages(input_path):
    path = Path(input_path)
    if not path.exists():
        print(f"[⚠] File not found: {input_path}")
        return []
    messages = []
    if path.suffix == ".json":
        try:
            messages = json.loads(path.read_text(encoding="utf-8"))
        except Exception as e:
            print("Error loading JSON:", e)
    else:
        for line in path.read_text(encoding="utf-8").splitlines():
            if ":" in line:
                user, text = line.split(":", 1)
                messages.append({"user": user.strip(), "text": text.strip()})
    return messages

def scan_messages(messages):
    suspicious, safe = [], []
    for msg in messages:
        text = msg["text"].lower()
        if any(word in text for word in BLACKLIST):
            suspicious.append(msg)
        else:
            safe.append(msg)
    return suspicious, safe

def generate_reports(suspicious, safe):
    now = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    log_path = REPORT_DIR / f"suspicious_log_{now}.txt"
    md_path = REPORT_DIR / f"report_{now}.md"

    with open(log_path, "w", encoding="utf-8") as log:
        for msg in suspicious:
            log.write(f"[{now}] {msg['user']}: {msg['text']}\n")

    with open(md_path, "w", encoding="utf-8") as md:
        md.write("# 🧊 OpenICE Guardian Report\n")
        md.write(f"**Generated:** {now}\n\n")
        md.write("## ⚠ Suspicious Messages\n")
        if suspicious:
            for msg in suspicious:
                md.write(f"- **{msg['user']}** → {msg['text']}\n")
        else:
            md.write("✅ No suspicious content detected.\n")

        md.write("\n## 🟢 Safe Messages\n")
        for msg in safe:
            md.write(f"- {msg['user']}: {msg['text']}\n")

    print(f"\n✅ Reports generated:")
    print(f" - Text log: {log_path}")
    print(f" - Markdown: {md_path}")

def main():
    print("=== 🧊 OpenICE Guardian v1.0 ===")
    messages = load_messages(INPUT_FILE)
    if not messages:
        print("No messages found. Please add messages.txt or messages.json in this folder.")
        return

    suspicious, safe = scan_messages(messages)

    print(f"Total messages: {len(messages)}")
    print(f"Suspicious: {len(suspicious)}")
    print(f"Safe: {len(safe)}")

    for msg in suspicious:
        print(f"⚠ {msg['user']} → {msg['text']}")
    generate_reports(suspicious, safe)
    print("\nScan complete. Stay protected with OpenICE Guardian.")

if __name__ == "__main__":
    main()
4️⃣ Add a test message file

Create a file:

messages.txt

Add sample messages:

Alice: Check this link https://bit.ly/fakeapp
Bob: Hello, how are you?
Charlie: Download this cool.apk
Dara: join https://t.me/spamchannel

5️⃣ Run the script

python openice_guardian.py
---
📁 Project Structure
OpenICE/
├── openice_guardian.py
├── messages.txt
├── reports/
│   ├── suspicious_log_2025-11-11_20-10-55.txt
│   └── report_2025-11-11_20-10-55.md
└── README.md


---

⚙️ Configuration

To modify suspicious keywords, edit the BLACKLIST list in openice_guardian.py:

BLACKLIST = [
    "t.me/", ".apk", "porn", "sex", "nude", "bit.ly",
    "hack", "phishing", "malware", "trojan"
]

---

🧾 Example Report (Markdown Output)

# 🧊 OpenICE Guardian Report
**Generated:** 2025-11-11_20-10-55

## ⚠ Suspicious Messages
- **Alice** → Check this link https://bit.ly/fakeapp
- **Charlie** → Download this cool.apk
- **Dara** → join https://t.me/spamchannel

## 🟢 Safe Messages
- **Bob**: Hello, how are you?


---

🧑‍💻 Author

OpenICE Team
Created and maintained by Cybersecurity Research Developers for open educational research.


---

📜 License

This project is licensed under the MIT License — free to use, modify, and distribute with attribution.
See LICENSE for details.


---

💬 Feedback & Issues

If you find a bug or want to request a feature, please open an issue here:
👉 GitHub Issues


---

🔒 Stay Safe — Stay Informed

> OpenICE Guardian helps you learn and defend against digital threats safely.
Practice ethical cybersecurity, protect your data, and keep your community secure.


