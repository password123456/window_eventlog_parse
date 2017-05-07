## Preparation
 - [Set the Audit Policy](#audit-policy)
 - [Set the Process Command Line logging Policy](#process-command-line-policy)
 - [Get the Logparser](#get-the-logparser)

## Audit-policy
 When you want parse the event logs, you have to set the audit logs policies.
 If not configure the audit log policies, system make the event logs by default value.
 And you have to set the event log size enough.

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/audit_policy1.png">

 - start > run > secpol.msc
 - configure all audit policies as success, failure

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/audit_policy2.png">

 - If you want set the policy selectively, you can use advanced audit policy configuration. 
 - Through this you can set the audit policy items individual. so you can prevent unnecessary event creation and reduce the event log size.
 - If you have active directory you can make these as GPO and deploy all of server.

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/advanced_audit_policy1.png">
<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/advanced_audit_policy2.png">

 - Below are recommended advanced audit policy configuration.
```sh
Account Management
- Audit Security Group Management
- Audit User Account Management

Detailed Tracking
- Audit Process Creation
- Audit Process Temination

Logon/Logoff
- Audit Account Lockout
- Audit Logoff
- Audit Logon
- Audit Other Logon/Logoff Events
- Audot Special Logon

Object Access
- Audit Detailed File Share
- Audit File Share
- Audit Other Object Access Events
- Audit SAM

Policy Change
- Audit Audit Policy Change
- Audit Authentication Policy Change
- Audit MPSSVC Rule-Level Policy Change

Privilege Use
- Audit Sensitive Privilege Use

System
- Audit Security State Change
- Audit Security System Extension
```
 - Next, set the event log size. Default is 20MB. i recommend at least 200MB.
 - If you have active directory you can also set this as GPO.
 - Belows are sample script written in vbscript
```sh
'--------------------------------------------------------
' set windows eventlog size
'--------------------------------------------------------
Function SET_EVENTLOG_SIZE(eventlog, log_size)
    dim objFSO
    dim objWSHShell, strCMD
    dim strSystemRoot, strWEVUTIL
    dim ret

    Set objWSHShell = CreateObject("WScript.Shell")
    Set objFSO= CreateObject("Scripting.FileSystemObject")
    
    strSystemRoot = objWSHShell.ExpandEnvironmentStrings( "%SystemRoot%" )        
    strWEVUTIL = strSystemRoot & "\system32\wevtutil.exe"
    
    If objFSO.FileExists( strWEVUTIL ) Then        
        strCMD = "%comspec% /c %SYSTEMROOT%\system32\"'strCMD header
        ret = objWSHShell.run(strCMD & "wevtutil sl " & eventlog & " /ms:" & log_size & """",0,True)
        wscript.sleep 1000
        ret = ret + ret
        
        if ret <> 0 then
            ret = 1
        else
            wscript.echo "  [+] '" & eventlog & " Log' set to " & log_size & " bytes."
            ret = 0
        end if        
        SET_EVENTLOG_SIZE = ret            
    else
        ret = 2
        SET_EVENTLOG_SIZE = ret
    end if
end function

Function SET_EVENT_LOG_SET_SIZE(TITLE)
    dim strComputer, i
    dim objWMIService, colItems, objitem
    dim EVENTLOG_LIST
    dim eventname, set_logsize, eventname2
    dim list, log_list, eventlist
    dim retSetlog
    
    strComputer = "."
    Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\CIMV2")
    Set colItems = objWMIService.ExecQuery( _
        "SELECT * FROM Win32_NTEventLogFile",,48)

    EVENTLOG_LIST=array("Application","System","Security")
    
    for each objItem in colItems
        for i=Lbound(EVENTLOG_LIST) to Ubound(EVENTLOG_LIST)
            if StrComp(lcase(EVENTLOG_LIST(i)), lcase(objItem.LogFileName), 1) = 0 Then
                objItem.MaxFileSize = Round(objItem.MaxFileSize / 1024, 1)
                if objItem.MaxFileSize < 307200 Then
                       eventname = eventname & objItem.LogFileName & ","
                end if
            end if
        next
    next
      
    set_logsize = 314572800
    list=Split(eventname,",")

    for each log_list in list
        If not len(log_list) = 0 Then
            'wscript.echo "vul eventlog : " & log_list
            retSetlog = SET_EVENTLOG_SIZE(log_list,set_logsize)
        end if
    next
    
    if retSetlog = 2 then
        wscript.echo "[-] ERROR " & TITLE & " wevtutil.exe not found."
    elseif retSetlog = 0 then
        wscript.echo " [OK] SET: " & TITLE
    else
        wscript.echo " [FAIL] SET: " & TITLE
    end if        
end Function    
```

## Process Command line Policy
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

Remember, process command line entity is very important. 
You can use it when correation search, find out abnormal activities like malware process's informations.
Most of people doesn't know this. if you use on defaults, configure immediately.

## Get the logparser
 It is a difficult to Log analyzing just using event viewer in the tens thousands of logs. 
 You have to get and use logparse tool. there are many kinds of log analyzer.
 I recommend to you Microsoft LogParser and analyzer.
 It's free and portable, no special enviroment. just install and use.

- Install the MS LogParser.<a href="https://www.microsoft.com/en-us/download/details.aspx?id=24659">(get here)</a>
- Get the LogParse. I recommend two. Just download and run.
```sh
1. Visual Logparser (https://visuallogparser.codeplex.com/)
2. MS LogParser Studio (https://gallery.technet.microsoft.com/Log-Parser-Studio-cd458765)
```

- After installation, run the logparser
<img src="https://github.com/password123456/window_eventlog_parse/blob/master/setup/visual_logparser.PNG">



