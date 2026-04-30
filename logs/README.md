# Step-by-step of the Ransomware Attack
In this README.md, this will showcase a sequential and chronological account of the simulated ransomware attack. There are 89 recorded events in a Windows Event Log file. Although each of these recorded log events are in some way connected with the simulated ransomware attack, only relevant logs will be analysed to determine how they contributed to the cyber attack. Instead, a more
holistic view of these other logs will be taken to develop a broader understanding of how those logs fit in to the bigger picture. As such, they will not be completely excluded, they will simply
not be the focus of this analysis.

## Phase A: Initial Access and Setup Configuration
### Exhibit A
<img width="1356" height="699" alt="123eb92f680f748f9c248ccb4466f957" src="https://github.com/user-attachments/assets/4d25091a-60aa-458b-8a00-19da5744757a" />

Upon gaining initial access to the file directory the ransomware operator wishes to exploit, in the log above they are able to read the contents of the 
`Banking Details.txt` file.


### Exhibit B
<img width="1351" height="677" alt="64f34f49d5a7a2fa2c00627742d33969" src="https://github.com/user-attachments/assets/f2877725-559f-4f1e-9622-0382f8b953ee" />

~50 DLLs were installed into the Temp folder in the target machine. These ranged from `sqlite3.dll`, `python3.dll`, `libcrypto-1_1.dll`, and various Microsoft api dlls such as
`api-ms-win-crt-runtime-l1-1-0.dll`. It is possible the `libcrypto-1_1.dll` will be involved in facilitating the encryption of the file contents inside of `Banking Details.txt`.

## Phase B: Duplicate Processes and Ascertain Network Security
### Exhibit A
<img width="1355" height="677" alt="efcefddf3617e29ab6dacc14dc6d9dda" src="https://github.com/user-attachments/assets/d15f0494-c06a-49c9-b8de-dfad0d939d15" />
In this log, the `monkey-windows-64.exe` application process ID 8612 is being duplicated by `--multiprocessing-fork`. This can leverage multiple processors
to run functions in paralell with different inputs at the same time. It is not completely clear why this process is being duplicated. It could be to encrypt
files faster, to establish multiple network connections to the attacking server, or for some other reason.

### Exhibit B
<img width="1349" height="676" alt="41b8f95895ecabfe2d9d27443d975f33" src="https://github.com/user-attachments/assets/a182c0ab-27da-404b-85de-368bcdd14ac6" />
This log showcases the `netsh advfirewall show currentprofile` command being run. This lets the attacker check the existing Firewall configurations, such as inbound/outbound rules, logging settings of network connections and if the state is configured to on. The utility of an attacker being able to access this is they can ascertain what activities may trigger an alert via a connection, what inbound or outbound actions are allowed or not allowed, and if the Firewall is operating in the first place.

## Phase C: Establish Connections and Cleanly Exit
### Exhibit A
<img width="1363" height="699" alt="f466673d0cd04caddc4bf9aeef9a5157" src="https://github.com/user-attachments/assets/ccaea0f9-037a-4029-9bd0-4012f49017b5" />
A TCP connection request is being sent from the attacking server to the host machine. Source Port 49905 and 49906 are used for destination port 5000. This suggests the attacker wants two potential
attack vectors into the one port. This could be to take advantage of the paralellised processes that were created earlier.

### Exhibit B
<img width="1351" height="693" alt="407b7dbaa4ce275b5e32267885c40f1f" src="https://github.com/user-attachments/assets/a7a80120-a3a7-4cf1-9262-fcbf3ec1d3f9" />
A complex Windows Batch command is shown in this log. A nested for loop has an outer loop and inner loop. The outer loop is designed to run the inner loop 20 times. This nested for loop is necessary, as for loops with the argument /F cannot define how many times it runs by itself. As a result, the outer loop is needed to ensure it iterates at least 20 times.
The inner loop tries to find the process ID equalling 8612 that is returned from a tasklist. If a row with the process ID of 8612 is found, it is passed through a pipe operator (|) to `find`, which sifts through the row that was provided to it by the previous Microsoft batch command. If a match is found, the `%%j` value should be set to 1, and a timeout of 5 seconds will occur. Otherwise, if Process ID 8612 is not found, then the Infection Monkey.exe installed on the target machine will be removed, and the loop will terminate.

