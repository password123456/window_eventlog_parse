## preparation
 - [set the audit policy](#audit-policy)
 - [set the process command line logging policy](#commandline-policy)
 - [get logparser](#get-the-logparser)

## audit-policy

## commandline-policy
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

remember, process command line is very important. 
you can also use it when correation search, find out abnormal activities like malware process's informations.


## get the logparser