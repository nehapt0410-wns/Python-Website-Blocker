import time
import requests
from bs4 import BeautifulSoup

HOSTS_PATH = r"C:\Windows\System32\drivers\etc\hosts"   # Windows
# HOSTS_PATH = "/etc/hosts"  # Linux/Mac

REDIRECT_IP = "127.0.0.1"

def block_website(website):
    with open(HOSTS_PATH, "r+") as file:
        content = file.read()
        if website not in content:
            file.write(REDIRECT_IP + " " + website + "\n")
            print(f"[+] Blocked: {website}")
        else:
            print(f"[!] Already blocked: {website}")

def unblock_website(website):
    with open(HOSTS_PATH, "r") as file:
        lines = file.readlines()

    with open(HOSTS_PATH, "w") as file:
        for line in lines:
            if website not in line:
                file.write(line)
    print(f"[-] Unblocked: {website}")

def scrape_website(url):
    try:
        response = requests.get("http://" + url, timeout=5)
        soup = BeautifulSoup(response.text, "html.parser")
        return soup.get_text().lower()
    except:
        return ""

def detect_patterns(text, keywords):
    for keyword in keywords:
        if keyword.lower() in text:
            return True
    return False

def auto_block(url, keywords):
    print(f"\n[+] Scanning website: {url}")
    content = scrape_website(url)

    if detect_patterns(content, keywords):
        block_website(url)
        print(f"[AUTO] Website '{url}' blocked based on detected pattern.\n")
    else:
        print("[AUTO] No harmful pattern detected.\n")


if __name__ == "__main__":

    keywords = ["gaming", "casino", "social media", "adult", "betting"]

    while True:
        print("\n--- Python Website Blocker ---")
        print("1. Block Website")
        print("2. Unblock Website")
        print("3. Auto Block (Pattern Detection + Scraping)")
        print("4. Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            site = input("Enter website (example: facebook.com): ")
            block_website(site)

        elif choice == "2":
            site = input("Enter website to unblock: ")
            unblock_website(site)

        elif choice == "3":
            site = input("Enter website to scan: ")
            auto_block(site, keywords)

        elif choice == "4":
            print("Exiting...")
            break

        else:
            print("Invalid choice. Try again.")
