## Eventlog Analyze using LogParser
 Basically Microsoft LogParser is run in the command line mode.
 so you can use it in the command line mode. but in my case, the Visual Logparser was better than command line mode.
 (Whatever you choose, it's your decision. There is no difference.)
 This section i will explain with the Visual Log Parser.

 - Usage of the Visual Log Parser.

 It's very simple. write quey in query section and just run.

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/visual_logparser_uasge.png">

 If you ready to analyze, what will do? You must have the parse rules.
 If possible, each rule must have based on the 5w1h.
 And you have to know event id's knowledge as much as possible.

 - A few tips about event id knowledge

 **1. At least you should know these event id. 
(you can get .xls file <a href="https://www.microsoft.com/en-us/download/details.aspx?id=35753">here)</a>**


Category | Subcategory | Event ID | Message Summary
--------- | --------- | --------- | --------- 
System | Security System Extension | 4610 | An authentication package has been loaded by the Local Security Authority.
System | Security System Extension | 4611 | A trusted logon process has been registered with the Local Security Authority.
System | Security System Extension | 4622 | A security package has been loaded by the Local Security Authority.
System | Security System Extension | 4697 | A service was installed in the system.
System | Other System Events | 5024 | The Windows Firewall Service has started successfully.
System | Other System Events | 5025 | The Windows Firewall Service has been stopped.
System | Other System Events | 5034 | The Windows Firewall Driver has been stopped.
System | Other System Events | 5035 | The Windows Firewall Driver failed to start.
System | Other System Events | 5037 | The Windows Firewall Driver detected critical runtime error. Terminating.
Policy Change | MPSSVC Rule-Level Policy Change | 4946 | A change has been made to Windows Firewall exception list. A rule was added.
Policy Change | MPSSVC Rule-Level Policy Change | 4947 | A change has been made to Windows Firewall exception list. A rule was modified.
Policy Change | MPSSVC Rule-Level Policy Change | 4948 | A change has been made to Windows Firewall exception list. A rule was deleted.
Policy Change | MPSSVC Rule-Level Policy Change | 4949 | Windows Firewall settings were restored to the default values.
Policy Change | Audit Policy Change | 4719 | System audit policy was changed.
Policy Change | Audit Policy Change | 4817 | Auditing settings on an object were changed.
Logon/Logoff | Logon | 4624 | An account was successfully logged on.
Logon/Logoff | Logon | 4625 | An account failed to log on.
Logon/Logoff | Logon | 4648 | A logon was attempted using explicit credentials.
Logon/Logoff | Logoff | 4634 | An account was logged off.
Logon/Logoff | Logoff | 4647 | User initiated logoff.
Logon/Logoff | Other Logon/Logoff Events | 4649 | A replay attack was detected.
Logon/Logoff | Other Logon/Logoff Events | 4778 | A session was reconnected to a Window Station.
Logon/Logoff | Other Logon/Logoff Events | 4779 | A session was disconnected from a Window Station.
Logon/Logoff | Other Logon/Logoff Events | 4800 | The workstation was locked.
Logon/Logoff | Other Logon/Logoff Events | 4801 | The workstation was unlocked.
Logon/Logoff | Other Logon/Logoff Events | 4802 | The screen saver was invoked.
Logon/Logoff | Other Logon/Logoff Events | 4803 | The screen saver was dismissed.
Detailed Tracking | Process Creation | 4688 | A new process has been created.
Detailed Tracking | Process Termination | 4689 | A process has exited.
Object Access | Other Object Access Events | 4698 | A scheduled task was created.
Object Access | Other Object Access Events | 4699 | A scheduled task was deleted.
Object Access | Other Object Access Events | 4700 | A scheduled task was enabled.
Object Access | Other Object Access Events | 4701 | A scheduled task was disabled.
Object Access | Other Object Access Events | 4702 | A scheduled task was updated.
Object Access | File Share | 5140 | A network share object was accessed.
Object Access | File Share | 5142 | A network share object was added.
Object Access | File Share | 5143 | A network share object was modified.
Object Access | File Share | 5144 | A network share object was deleted.
Account Management | User Account Management | 4720 | A user account was created.
Account Management | User Account Management | 4722 | A user account was enabled.
Account Management | User Account Management | 4723 | An attempt was made to change an account's password.
Account Management | User Account Management | 4724 | An attempt was made to reset an account's password.
Account Management | User Account Management | 4725 | A user account was disabled.
Account Management | User Account Management | 4726 | A user account was deleted.
Account Management | User Account Management | 4738 | A user account was changed.
Account Management | User Account Management | 4740 | A user account was locked out.
Account Management | User Account Management | 4781 | The name of an account was changed:
Account Management | Other Account Management Events | 4782 | The password hash an account was accessed.
Account Management | Other Account Management Events | 4793 | The Password Policy Checking API was called.
Security | . | 1102 | The audit log was cleared
system | . | 104 | The Application(system) log file was cleared.
System | . | 7036 | System Service running status (start/stop)
System | . | 7040 | Modify System Service status (manual/stop/disable)
System | . | 7045 | A new Service installed in the system


 **2. A various of Logon type**


Logon Type | Description
--------- | ---------
2 | Logon at the console of system. but mstsc /console command or some of RDP tool use this logon sometimes.
3 | Network Logon (Based on NTLM Authentication. shared folder,IIS, Default share, psexec) If the logon 3 event increased, you can guess ID/PW bruteforce attack
4 | Batch Logon (Like a scheduled task)
5 | Windows Service Logon (Account information logon log set in windows service)
7 | Unlock (When connect to RDP idle session, session protected with password by screen saver)
8 | NetworkCleartext (Similar 3 logon type but Logon credential sent in the plain text. Most often indicates a logon to IIS or FTP with "basic authentication")
9 | Like a runas /netonly command log. this ocurs when the program use runas command with account switch for remote connection.  
10 | RemoteInteractive (Terminal Services, Remote Desktop or Remote Assistance)
11 | CachedInteractive (logon with cached logon) If system is in the active directory, when network connecction problems occurs, system use cached logon.


 **3. Identify same account user**

 How can we identify and analyze same account user logon logs?

 Just simple. you can use Logon ID value expressed with Hexadecimal in the event log as identify.
 you can also use source Network Address value but sometimes this value crafted by hacker. And if the system infected with malware and if malware run as reverse proxy, some of malware connect RDP as localhost to pass the firewall. in the result of these cases, you had better not use source Network Address value as identify.

 **But the Logon ID value is dfficult to crafting.**

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/logon_id.png">

 When user logon, system create to user unique Logon ID value expressed with Hexadecimal and It use until user initiated logoff.
 system will change the value new one every time when user logon. 

 **By Logon ID you can trace everything of user's activity.**

 **Do you use Microsoft sysmon? Logon ID value is same. You can do correlation search together.**

<img src="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/logon_id_example.png">

 - Log parse rules.

 Next, you have to plan Log parse rules. As I mentioned before, each rule had better based on the 5w1h.

 Below are a few examples that has been used actually.


 1. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/Terminal_Service_Logon_Statistics..txt">Terminal Service Logon statistics.</a>
 2. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/process_creation.txt">Process Creation Tracking</a>
 3. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/Logon_Statistics.txt">Logon Account Success/Failure statistics.</a>
 4. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/schedule_task_tracking.txt">Registed schedule Task Tracking.</a>
 5. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/event_log_removal.txt">EventLog removal Tracking.</a>
 6. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/detail_logon_tracking.txt">Detail Logon Tracking.</a>
 7. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/possible_pth.txt">Possbile PTH Attack</a>
 8. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/bruteforce_login_fail_count.txt">Login Fail Count</a>
 9. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/account_creation_tracking.txt">Account Creation Tracking</a>
 10. <a href="https://github.com/password123456/window_eventlog_parse/blob/master/logparse/parsing_example/account_removal_tracking.txt">Account Removal Tracking</a>

 There's one thing to remember. Log Parser does not support JOIN or similar grammer completely.
 so sometimes you may not parse as you wants. but most of the cases it will satisfy.
 If you have SIEM or Correlation tools, it will be more powerful when use together.
 

 
 