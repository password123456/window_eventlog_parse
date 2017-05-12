## Eventlog parse using splunk
 In this section, i'll explain how to parse eventlog using splunk.
 If you don't have a splunk, i recommend try it. you can use Free version that has a few limit.
 but there is no shortage of to learn. if you learn it, i'm certainly sure, it will be helpful and powerful on your career.

 - Get the event log selectively to Splunk using splunkforwarder.
 You can set as you want to get logs by event id. you don't have to bring all of event log.

 Here is configuration. you have to choose and set event id number using whitelist directive.

```sh
inputs.conf

[default]
host = WIN-CEF0N1SQMV8


[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index=winevt
sourcetype=sysmon
CheckpointInterval=5
current_only=0
disabled=0
evt_resolve_ad_obj=1
start_from=oldest

[WinEventLog://Security]
index=winevt
sourcetype=security
CheckpointInterval=5
current_only=0
disabled=0
evt_resolve_ad_obj=1
start_from=oldest
whitelist=4610,4611,4622,4697,4608,5024,5025,5034,5035,4946-4949,4719,4624,4625,4648,4634,4647,4649,4778,4779,4800-4803,4688,4689,4698-4702,5140-5144,4720,4722,4723,4724,4725,4726,4738,4740,4781,4782,4793,1102

[WinEventLog://Application]
index=winevt
sourcetype=Application
CheckpointInterval=5
current_only=0
disabled=0
evt_resolve_ad_obj=1
start_from=oldest

[WinEventLog://System]
index=winevt
sourcetype=System
CheckpointInterval=5
current_only=0
disabled=0
evt_resolve_ad_obj=1
start_from=oldest
whitelist=104,7036,7040,7045
```
 

 
 