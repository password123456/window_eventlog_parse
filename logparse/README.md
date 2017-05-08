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

 - some tips for event id knowledge
 1. Event ids you have to know
 - you can get <a href="http://download.microsoft.com/download/2/d/f/2df05493-32e0-465b-8be2-78afed9ee456/windows%207%20and%20windows%20server%202008%20r2%20security%20event%20descriptions.xls">here</a>


Category | Subcategory | Event ID | Message Summary
--------- | --------- | --------- | --------- 
System | Security System Extension | 4610 | An authentication package has been loaded by the Local Security Authority.

