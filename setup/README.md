## preparation
 - [Set the Audit Policy](#Audit-policy)
 - [Set the Process Command Line logging Policy](#Process-command-line-policy)
 - [Get the Logparser](#Get-the-logparser)

## Audit-policy
 When you want parse the event logs, you have to set the audit logs policies.
 If not configure the audit log policies, system make the event logs by default value.
 And you have to set the event log size enough.

## Process Command Line Policy
 In the security event id 4688 "Process Creation" log can offer prcoess creation activity. 
 so you must analyse the event id 4688 when incident occurs. 
 but the defaults doesn't offers process command line arguments.
 you can enable "the process command line entity" by additional registry key applying.

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/process_cmdline1.png">

 - additional registry key applying
<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/process_cmdline2.png">

 - here is key
```sh
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit]
"ProcessCreationIncludeCmdLine_Enabled"=dword:00000001
```
 - tips.
```sh
Event id 4688 is only created when process creation is success.
It means failed process doesn't make the logs.
In other words, you don't have to worry about not creating, missed events.
```

Remember, process command line is very important. 
You can also use it when correation search, find out abnormal activities like malware process's informations.
Most of people doesn't know this. if you use on defaults, configure immediately.

## Get the logparser