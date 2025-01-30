import requests
import dns.resolver
import aiohttp
import asyncio
import random
import string

# Sertifika şeffaflık loglarından subdomain çıkarımı (crt.sh)
def passive_scan(domain):
    print("[*] Passive Scan: Retrieving subdomains from crt.sh...")
    url = f"https://crt.sh/?q=%.{domain}&output=json"
    try:
        response = requests.get(url, timeout=10)
        if response.status_code == 200:
            subdomains = set()
            data = response.json()
            for entry in data:
                name = entry.get("name_value", "")
                subdomains.update(name.split("\n"))
            return subdomains
    except Exception as e:
        print(f"[!] Error in passive scan: {e}")
    return set()

# Kelime listesi tabanlı brute force tarama
async def brute_force(domain, wordlist, results):
    async with aiohttp.ClientSession() as session:
        tasks = []
        for word in wordlist:
            subdomain = f"{word}.{domain}"
            tasks.append(check_dns(session, subdomain, results))
        await asyncio.gather(*tasks)

async def check_dns(session, subdomain, results):
    try:
        resolver = dns.resolver.Resolver()
        answers = resolver.resolve(subdomain, 'A')
        results[subdomain] = [str(ip) for ip in answers]
        print(f"[+] Found: {subdomain}")
    except dns.resolver.NXDOMAIN:
        pass
    except Exception as e:
        print(f"[!] Error resolving {subdomain}: {e}")

# Wildcard DNS yapılandırması tespiti
def detect_wildcard(domain):
    print("[*] Detecting Wildcard DNS...")
    random_subdomain = f"{''.join(random.choices(string.ascii_lowercase, k=10))}.{domain}"
    try:
        resolver = dns.resolver.Resolver()
        answers = resolver.resolve(random_subdomain, 'A')
        ips = [str(ip) for ip in answers]
        print(f"[!] Wildcard DNS Detected: {ips}")
        return ips
    except dns.resolver.NXDOMAIN:
        print("[*] No Wildcard DNS Detected.")
        return None
    except Exception as e:
        print(f"[!] Error detecting wildcard DNS: {e}")
        return None

# Ana tarama fonksiyonu
def scan_domain(domain, wordlist_file):
    print(f"[*] Starting scan for {domain}...")
    
    # Pasif tarama
    passive_results = passive_scan(domain)
    print(f"[*] Passive scan found {len(passive_results)} subdomains.")
    
    # Wildcard DNS kontrolü
    wildcard_ips = detect_wildcard(domain)

    # Aktif brute force tarama
    print("[*] Starting brute force scan...")
    with open(wordlist_file, "r") as file:
        wordlist = [line.strip() for line in file]
    results = {}
    asyncio.run(brute_force(domain, wordlist, results))
    
    # Sonuçların doğrulanması ve wildcard filtreleme
    if wildcard_ips:
        print("[*] Filtering wildcard DNS results...")
        results = {sub: ips for sub, ips in results.items() if not any(ip in wildcard_ips for ip in ips)}
    
    # Sonuçların birleştirilmesi
    all_results = passive_results.union(set(results.keys()))
    print(f"[*] Scan completed. Total subdomains found: {len(all_results)}")
    
    # Sonuçların kaydedilmesi
    with open("results.txt", "w") as output_file:
        for subdomain in all_results:
            output_file.write(f"{subdomain}\n")
    print("[*] Results saved to results.txt")

# Kullanıcıdan giriş alınması
if __name__ == "__main__":
    target_domain = input("Enter the target domain: ").strip()
    wordlist_path = input("Enter the path to the wordlist: ").strip()
    scan_domain(target_domain, wordlist_path)
