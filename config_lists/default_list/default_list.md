# Default_list

An example of a priority list that can be used to generate ordered config files.

Notable mentions from this list:

- Programs that may/may not be in the environment tend to be removed (Ex. Dropbox)

## Testing

Testing was done using atomic-redteam. Testing is not complete, I'm working on a better way to test.

| Technique ID | Tested | Detected | Notes |
| :--- | :--- | :--- | :--- |
| T1059.003 | T | T | Detected as Event ID 1 |
| T1059.001 | T | T | Detected as Event ID 1 |
| T1047 | T | T | Detected as Event ID 7 - Image Loaded. Tested 1,2,3 |
| T1027 | T | T | Detected as EID 1 |
| T1218.011 | T | T | Detected as EiD 1, seems to have extensive coverage, tested 1-13 |
| T1105 | T | T | Tested 2,3,4,5,6,7 |
| T1055 | T | T | Tested 1,3 |
| T1569.002 | T | T | Tested 1, need to return for psexec tests |
| T1036.003 | T | F | 1 should be detected, need a better method though. |
| T1003.001 | T | T | 2 created 3 events, 8 created 1 |

### T1562.001

Atomic Testing with this EID showed that PowerShell commands for registry modification seem to call CONHOST.exe with the ParentCommandLine of whatever modifications are being made. See below. Omitted "Message" and "EventData" fields. The ProcessIDs were found from `grep "HKLM" ./T1562.001/13/logs_T1562.001_13_.json | jq`. **For an unknown reason, despite modifying the registry, no Event ID 12-14 were seen while using the research config.**

```sh
jq -s '.[] | select(.ProcessId != null) | select(.ProcessId == 4784 or .ParentProcessId == 4784 or .ParentProcessId == 4712 or .ProcessId == 4712)' ./T1562.001/13/logs_T1562.001_13_.json
...
{
  "EventSourceName": "Microsoft-Windows-Sysmon",
  "Provider": "Microsoft-Windows-Sysmon",
  "ProviderGuid": "{5770385f-c22a-43e0-bf4c-06f5698ffbd9}",
  "Level": "4",
  "Keywords": "0x8000000000000000",
  "Channel": "Microsoft-Windows-Sysmon/Operational",
  "Computer": "WIN-MUOPDL92HGD",
  "TimeCreated": "2023-05-10T03:41:21.738Z",
  "TimeGenerated": "2023-05-10T07:41:21.738Z",
  "EventRecordID": "25668",
  "EventID": 1,
  "Task": 1,
  "RuleName": "-",
  "UtcTime": "2023-05-10 19:41:21.738",
  "ProcessGuid": "a9c6006c-f361-645b-2902-000000000500",
  "ProcessId": 4784,
  "Image": "C:\\Windows\\System32\\conhost.exe",
  "FileVersion": "10.0.17763.4377 (WinBuild.160101.0800)",
  "Description": "Console Window Host",
  "Product": "Microsoft® Windows® Operating System",
  "Company": "Microsoft Corporation",
  "OriginalFileName": "CONHOST.EXE",
  "CommandLine": "\\??\\C:\\Windows\\system32\\conhost.exe 0xffffffff -ForceV1",
  "CurrentDirectory": "C:\\Windows",
  "User": "WIN-MUOPDL92HGD\\Administrator",
  "LogonGuid": "a9c6006c-f166-645b-79a7-070000000000",
  "LogonId": "0x7a779",
  "TerminalSessionId": 2,
  "IntegrityLevel": "High",
  "Hashes": "SHA256=70239DE81CE78C19510FAB109B86A1E8A4002F2229A69D935249BC2E7E435510",
  "ParentProcessGuid": "a9c6006c-f361-645b-2802-000000000500",
  "ParentProcessId": 4712,
  "ParentImage": "C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
  "ParentCommandLine": "\"powershell.exe\" & {Remove-Item -Path \\\"\"HKLM:\\SOFTWARE\\Microsoft\\AMSI\\Providers\\{2781761E-28E0-4109-99FE-B9D127C57AFE}\\\"\" -Recurse}",
  "ParentUser": "WIN-MUOPDL92HGD\\Administrator"
}
...
```
