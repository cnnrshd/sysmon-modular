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