<h1>Investigation of a reported unknown/unexpected file in employee registry</h1>

<h2>Investigation Scenario</h2>
One of our IT technicians has reported an unexpected file in the registry of an employee they were assisting. You have been tasked with investigating
using various log sources and OSINT to understand if the file is legitimate or malicious, and what it is doing.
<br />

<h2>WHat we know!</h2>

- <b>File name: osk.exe/b> 
<h2>Environments Used </h2>

- <b>Splunk</b>

<h2>Investigation walk-through:</h2>

<p align="center">
First I did a simple google search of the unexpected file, the following results seems to not initially be a cause for concern
  as osk.exe is a legitimate file found in C:\Windows\System32. Although we must continue our investiagtion.<br/>
<img src="https://i.imgur.com/C1PHivR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To further our investigation, we will enter our source type and string to narrow the events that include the file needed.
  In this case the xmlwineventlog sourcelog and "osk.exe":  <br/>
<img src="https://i.imgur.com/RYXOzr5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
From the results we can see that the file is indeed NOT stored in the C:\Windows\system32\ folder, which means this file
  is hiding and trying to look legitimate: <br/>
<img src="https://i.imgur.com/oq4efND.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next we're going to find any network connections assosicated with the malicious file. First scroll down to the 'image' section and click on
  the file path for osk.exe:  <br/>
<img src="https://i.imgur.com/we0Dkr5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
