---
layout: post
title: Write to Windows event log
tags: [vb]
---

This VBScript accepts three command line parameters:
an integer, a string naming a utility, and a message to
be written into the Windows Application Event Log.

If the integer is zero, then the named utility was
successful in execution. Any non-zero integer means
that it failed. The script then writes the pass or
fail result to the Windows Application Event Log,
along with the passed message string.

If the passed parameter is non-zero, an error message
(Event ID = 1) is written to Windows Event Log.

If the passed parameter is zero, an information
message (Event ID = 4) is written to the Log.

```
Option Explicit
On Error Resume Next
 
Dim strDescription, utilName, message
Dim eventID
Dim passFail
'
' check for all the arguments...
'
If WScript.Arguments.Count <> 3 Then
'Wscript.Echo "Usage:  WriteToEventLog.vbs  Integer  String  String"
eventID = 1
' 1 means Error

strDescription = _
"Script WriteToEventLog.vbs encountered an error. " & vbCrLf & _
"Failed to receive all arguments."

' Write to NT Event Log
Call WriteAppEventLog(eventID, strDescription)
' local function

' quit and return an error code...
WScript.Quit(1)
End If


passFail = WScript.Arguments(0)
utilName = WScript.Arguments(1)
message  = WScript.Arguments(2)
'
' 1st parameter means success or fail....
'
If(passFail = 0) Then
' success...

'Wscript.Echo utilName + ": Operation was successful"
eventID = 4
' 4 means Information

strDescription = utilName + " executed successfully. " + message

' Write to NT Event Log
Call WriteAppEventLog(eventID, strDescription)

Else
' oops...failure....

'Wscript.Echo utilName + ": Operation was NOT successful"
eventID = 1
' 1 means Error

strDescription = utilName + " encountered an error. " + message

' Write to NT Event Log
Call WriteAppEventLog(eventID, strDescription)
End If 

'quit and return a 0 as error code...
WScript.Quit(0)


'...................................................................
'
' Write to the Windows Application Event Log on local machine.
'
Function WriteAppEventLog(eventID, strDescription)
Dim objShell
Set objShell = Wscript.CreateObject("Wscript.Shell")
objShell.LogEvent eventID, strDescription
End Function
```
