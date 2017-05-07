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

 - defaults
<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/process_cmdline1.png">



## get the logparser