Python Website Blocker with Pattern Detection

A simple Python-based website blocker that:

Blocks or unblocks websites by editing the system hosts file

Can auto-scan a website’s content (via web scraping) and block it if it matches certain keywords/patterns (e.g., gaming, casino, adult, betting)

Runs in the terminal with a simple menu-based interface

⚠️ Warning: This script modifies your system hosts file. Use it carefully and only if you understand what it does.

1. Features

✅ Block specific websites (e.g., facebook.com, instagram.com)

✅ Unblock previously blocked websites

✅ Auto-detect “harmful” content using pattern matching on page text

✅ Customizable keyword list for pattern detection

✅ Cross-platform logic (hosts path for Windows / Linux / Mac)

✅ Error handling for:

Permission issues

Missing hosts file

Network / request failures

2. How It Works (High-Level)

Blocking websites

The script adds a line to your hosts file:

127.0.0.1  facebook.com


This redirects traffic to localhost, effectively blocking the website on that machine.

Unblocking websites

The script reads all lines of the hosts file and rewrites it without the lines containing the target website.

Auto-block via pattern detection

The script:

Sends an HTTP GET request to the website

Extracts visible page text using BeautifulSoup

Converts text to lowercase

Checks for the presence of any keyword from a list (e.g. "gaming", "casino", "adult")

If a match is found → website is automatically blocked

3. Project Structure

For a simple version, your project can look like this:

website_blocker/
│
├── website_blocker.py     # Your main Python script (contains main(), block, unblock, auto-block)
└── README.md              # This documentation

4. Prerequisites
4.1. Python

Python 3.8+ recommended

Check version:

python --version
# or
python3 --version

4.2. Required Python Packages

The script uses:

requests – for HTTP requests

beautifulsoup4 – for parsing HTML

Install them via:

pip install requests beautifulsoup4
# or
pip3 install requests beautifulsoup4

5. Hosts File Configuration

In your script, you have a constant:

HOSTS_PATH = r"C:\Windows\System32\drivers\etc\hosts"  # For Windows
# HOSTS_PATH = "/etc/hosts"  # For Linux/Mac (uncomment when needed)

5.1. For Windows

Leave this line active:

HOSTS_PATH = r"C:\Windows\System32\drivers\etc\hosts"


Run the script as Administrator:

Right-click Command Prompt / PowerShell / VS Code

Select “Run as administrator”

5.2. For Linux/Mac

Comment Windows path and uncomment Unix path:

# HOSTS_PATH = r"C:\Windows\System32\drivers\etc\hosts"
HOSTS_PATH = "/etc/hosts"


Run script with sudo:

sudo python3 website_blocker.py

6. Running the Script

From the project folder:

python website_blocker.py
# or
python3 website_blocker.py


You will see a menu like:

--- Python Website Blocker ---
1. Block Website
2. Unblock Website
3. Auto Block (Pattern Detection + Scraping)
4. Exit
Enter choice:

7. Usage Guide
7.1. Option 1 – Block Website

Choose option 1

Enter website, e.g.:

Enter website (example: facebook.com): facebook.com


Script will:

Open hosts file

Append a line: 127.0.0.1 facebook.com

Print:

[+] Blocked: facebook.com


If already blocked, prints:

[!] Website 'facebook.com' is already blocked.

7.2. Option 2 – Unblock Website

Choose option 2

Enter website to unblock:

Enter website to unblock: facebook.com


Script will:

Read all lines from hosts

Write back only those lines that do not contain facebook.com

Print:

[-] Unblocked: facebook.com


If not found, prints:

[!] Website 'facebook.com' was not found in hosts file.

7.3. Option 3 – Auto Block (Pattern Detection + Scraping)

Choose option 3

Enter website to scan, e.g.:

Enter website to scan: example.com


Internally:

Calls scrape_website(url)

Extracts page text using BeautifulSoup

Checks for keywords from:

keywords = ["gaming", "casino", "social media", "adult", "betting"]


If a keyword is found:

[+] Scanning website: example.com
[AUTO] Website 'example.com' blocked based on detected pattern.


If nothing harmful detected:

[+] Scanning website: example.com
[AUTO] No harmful pattern detected.

8. Code Overview (Functions)
8.1. block_website(website)

Opens HOSTS_PATH in read+write mode

Checks if the website entry already exists

If not, appends:
127.0.0.1 <website>

8.2. unblock_website(website)

Reads all lines from HOSTS_PATH

Rewrites the file without any lines containing the given website

8.3. scrape_website(url)

Sends GET request to http://<url>

Parses HTML with BeautifulSoup

Returns all visible text in lowercase

8.4. detect_patterns(text, keywords)

Returns True if any keyword is found in the text

8.5. auto_block(url, keywords)

Scrapes website → gets text

Uses detect_patterns to check if the content is “harmful”

If harmful → calls block_website(url)

8.6. main()

Displays menu in a loop

Takes user input and routes to appropriate function

Exits when the user selects option 4

9. Error Handling & Troubleshooting
9.1. PermissionError: Access denied to the hosts file.

Cause: Script doesn’t have permission to edit hosts.

Windows:

Run terminal or VS Code as Administrator

Linux/Mac:

Run with sudo:

sudo python3 website_blocker.py

9.2. FileNotFoundError: hosts file not found at: ...

Check your HOSTS_PATH

Make sure the correct path for your OS is uncommented

9.3. Failed to fetch website: <url>

Network issue OR website down OR blocked:

Check your internet connection

Try opening the site in a browser

For internal networks, there may be firewalls

10. Customization
10.1. Change Keywords for Pattern Detection

In main():

keywords = ["gaming", "casino", "social media", "adult", "betting"]


You can modify or extend this list, for example:

keywords = [
    "casino", "betting", "poker", "lottery",
    "18+", "xxx", "adult", "porn", "gambling",
    "crypto trading", "binary options"
]

10.2. Log Actions to a File

You can extend the project to log each action (block/unblock/auto-block) to a file like blocker.log.

11. Security & Ethical Notes

This tool affects only the local machine where it’s run.

Do not use it to control or restrict others’ internet usage without their consent.

Be transparent if using on a shared system.

Always keep a backup of your original hosts file before experimenting.

Example backup:

# Windows (PowerShell)
copy C:\Windows\System32\drivers\etc\hosts C:\Windows\System32\drivers\etc\hosts.backup

# Linux/Mac
sudo cp /etc/hosts /etc/hosts.backup

12. Future Enhancements (Ideas)

GUI version using Tkinter or Streamlit

Add category-wise blocking (social, gambling, adult, etc.)

Schedule-based blocking (e.g., block social media during work hours)

Export/import blocked website list from a file

Add logs & analytics (how many sites blocked, when, etc.)
