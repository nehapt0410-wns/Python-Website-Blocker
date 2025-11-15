Python Website Blocker

A Python script to block, unblock, and auto-block websites on your system by modifying the hosts file. The script also allows automatic blocking based on detected keywords from website content.

Features

Block a website: Adds an entry in your hosts file to prevent access.

Unblock a website: Removes the entry from your hosts file.

Auto-block: Scrapes a website and blocks it if certain keywords (e.g., "gaming", "casino", "social media", "adult", "betting") are detected.

Cross-platform support: Works on Windows and Linux/Mac (modify HOSTS_PATH as needed).

User-friendly CLI menu for easy management.

Requirements

Python 3.6+

Packages:

pip install requests beautifulsoup4


Administrator privileges (required to modify the hosts file)

Installation

Clone or download this repository.

Install the required Python packages:

pip install requests beautifulsoup4


Make sure to run the script as Administrator:

Windows: Right-click Command Prompt → Run as administrator, then run python script_name.py

Linux/Mac: Use sudo python3 script_name.py

Usage

Run the script:

python script_name.py


You will see a menu:

--- Python Website Blocker ---
1. Block Website
2. Unblock Website
3. Auto Block (Pattern Detection + Scraping)
4. Exit


Block Website: Enter the domain (e.g., facebook.com) to block.

Unblock Website: Enter the domain to remove from hosts.

Auto Block: Enter a website URL to scan for harmful keywords and block automatically if detected.

Exit: Exit the program.

Important Notes

Administrator Rights: Required to edit the hosts file. Without them, you will see:

❌ PermissionError: Access denied to the hosts file.
   → Run this script as Administrator.


Hosts file paths:

Windows: C:\Windows\System32\drivers\etc\hosts

Linux/Mac: /etc/hosts (uncomment in the code if needed)

The script only blocks by redirecting to 127.0.0.1. It does not permanently prevent internet access if users manually edit the hosts file.

Keywords Detection

By default, the script uses these keywords for auto-blocking:

keywords = ["gaming", "casino", "social media", "adult", "betting"]


You can modify this list in the main() function as needed.


