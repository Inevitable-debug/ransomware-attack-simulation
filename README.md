# Ransomware Attack

## ℹ️ Introduction
In this lab, it will involve installing and configuring Infection Monkey, a penetration testing tool. A ransomware attack will be simulated on a Windows 10
virtual machine, and the logs will be enhanced with Sysmon. These logs will be extracted and analysed from the Windows Event Viewer. An incident response report
will also be written as a playbook to respond to a ransomware attack if it were to happen to an endpoint in a corporate, enterprise or personal network.

## Project Objectives
The Project Objectives for this lab will focus on the simulation of a ransomware attack, the incident response report, log analysis, MITRE ATT&CK mapping, and a synthesis
of the key lessons that were learned from this lab. 

- Successfully simulate a Infection Monkey ransomware attack on a Windows 10 VM within a week of starting the project
- Perform a log analysis of the ransomware attack
- Write a detailed incident response plan using a template from an approved government body in Australia
- Map relevant features of the ransomware attack to Mitre ATT&CK
- Synthesise lessons learned from the ransomware attack

## Lab Setup
A [Windows 10 VM](https://www.microsoft.com/en-ca/software-download/windows10iso) was used as the target machine. [Infection Monkey](https://www.akamai.com/infectionmonkey) is the penetration testing tool that will launch a simulated ransomware attack on the Windows 10 VM device. [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is a virtualisation software that can run the iso file of the Windows 10 VM. The [Windows Event Viewer](https://learn.microsoft.com/en-us/shows/inside/event-viewer) will have enhanced log collection due to running [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) as a service in the background.

## Simulation Outcome
The simulated ransomware attack was a success. The target files were encrypted. However, the granularity of Sysmon's logs did not show every step the attacker took.
This could be because Sysmon had to be configured to watch more closely for write operations, or the current configuration was scope did not capture the particular
methods the attacker was using. Sysmon likely showcased the development of the attacker's resources and part of the exploitation attack pathway, but was not able
to capture the entire process in the current Sysmon configuration.

## Timeline of Attack
`2026-04-25 03:45:06: Malicious Actor accesses the Banking Details text file containing sensitive information.`

`2026-04-25 03:45:15: Various DLLs are installed into a temp folder on the target computer as tooling to facilitate the attack.`

`2026-04-25 03:45:16: All DLLs are installed into temp folder.`

`2026-04-25 03:45:16: Creates connection back to server.`

`2026-04-25 03:45:25: Firewall configurations are checked; presumably to identify vulnerabilities for the attacker to exploit`

`2026-04-25 03:45:24: Establishes a connection via TCP between target and host machine`

`2026-04-25 03:45:27: Attacker duplicates the Infection Monkey process with the --multiprocessing-fork command, which suggests the paralellisation of worker subprocesses in orchestrating the attack`

`2026-04-25 03:45:44: Attacker runs a loop to check whether an infection monkey process (PID 8612) is still running: if it's not, delete the monkey infection exe.`

`2026-04-25 03:45:46.791: Process ID 8612 terminates.`

`2026-04-25 03:45:52.162: The loop commands run, but is briefly terminated after it discovers process ID 8612 is no longer running.`

## Mitre ATT&CK Mapping
| Action                       | Evidence                                         | Mitre ATT&CK Tactic                             | Mitre ATT&CK (Sub-)Technique                     |
|------------------------------|--------------------------------------------------|-------------------------------------------------|--------------------------------------------------|
| Query Firewall Information   | `netsh advfirewall show currentprofile`          | Reconaissance                                   | Network Security Appliances (T1590.006)          |
| File Creation of DLLs        | File created: `\Temp\_MEI77402\VCRUNTIME140.dll` | Resource Development                            | Upload Tool (T1608.002)                          | 
| Command Prompt Execution     | `/c tasklist /fi "PID eq 8612"`                  | Execution                                       | Windows Command Shell (T1059.003)                |
| Sensitive Data Encryption    | `Banking Details.txt` encrypted in target folder | Impact                                          | Data Encrypted For Impact (T1471)                |

## Incident Response Report
The Incident Response Report follows a template supplied by the Victorian Government. It outlines the context of the report, the purpose, who oversees incident response efforts, and a defined table of terms to ensure the audience has a common dialect to understand the report. It also outlines common threat vectors, and a comprehensive incident response plan covering the detection phase all the way to lessons learned. This report can be found [here](/incident%20response%20report)

## Lessons Learned
