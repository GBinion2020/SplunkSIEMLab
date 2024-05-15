<h1>Investigation of a reported unknown/unexpected file in employee registry</h1>

<h2>Investigation Scenario</h2>
One of our IT technicians has reported an unexpected file in the registry of an employee they were assisting. You have been tasked with investigating
using various log sources and OSINT to understand if the file is legitimate or malicious, and what it is doing.
<br />

<h2>WHat we know!</h2>

- <b>File name: osk.exe</b>
- <b>EventID #7 logs contain the hash values of files </b>
- <b>Suricata format: event_type, src_ip, dest_ip</b>
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
In this case the xmlwineventlog sourcelog and "osk.exe".  <br/>
<img src="https://i.imgur.com/RYXOzr5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
From the results we can see that the file is indeed NOT stored in the C:\Windows\system32\ folder, which means this file
  is hiding and trying to look legitimate: <br/>
<img src="https://i.imgur.com/oq4efND.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next we're going to find any network connections assosicated with the malicious file. First scroll down to the 'image' section and click on
  the file path for osk.exe, Then click on the DistinationPort section and select the value with the most traffic "6892" Selecting both items will add them to our search:  <br/>
<img src="https://i.imgur.com/we0Dkr5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/qXr8ecT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Up untill now our search should look like this: index="botsv1" sourcetype=xmlwineventlog osk.exe Image="C:\\Users\\bob.smith.WAYNECORPINC\\AppData\\Roaming\\{35ACA89F-933F-6A5D-2776-A3589FB99832}\\osk.exe" DestinationPort=6892, Next we're going to see how many IP addresses the file has connected to. To do so lets add " | stats count by destinationIp":  <br/>
<img src="https://i.imgur.com/C1RIFvF.png" alt="Disk Sanitization Steps"/>
<br />
<br />
From our search we can see that the file has connected to 16,384 IP addresses outside of the network. Well thats not good..:  <br/>
<img src="https://i.imgur.com/cCv9jAQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Next lets find the hash value of the file to see if virsustotal.com can tell us anything about it. We're going to change our search query to specifically show hash values that include the file name, which looks like "index="botsv1" sourcetype=xmlwineventlog EventID=7 ImageLoaded=*osk.exe". From this search we can see the SHA1, MD5, and SHA256 hash values:  <br/>
<img src="https://i.imgur.com/m2t4XK4.png"80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
By entering the files SHA256 hash into virustotal.com we see some very concerning details. It looks like 54 security vendors have flagged the file as malicious, and from the analysis section we see the file is most likely a trojan horse virus: 
  <br/>
<img src="https://i.imgur.com/dV9F8eV.png" width="80%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
Now that we know for a fact the file is malicious. Lets investigate more into the network traffic associated with the file, to gather more informaiton on what the file is trying to do. Lets change our search query to access the fortigate firewall logs and the associated destination port that we found from earlier. Our new search should look like "index="botsv1" sourcetype=fortigate_utm dest_port=6892: 
<img src="https://i.imgur.com/KzKOdEC.png" width="80%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
From the above screenshot, we see Foritgate names the malware inside oske.exe as 'Cerber.Botnet" and values the applications risk as "critical". From a simple google search, we see that Cerber is a type of ransomware  :/
<img src="https://i.imgur.com/shWqlHp.png" width="80%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
Lastly, we're going to see if Suricata (the IDS) has this activity alerted already. To do so we're going to invesitage the single connection on port 80 http traffic from our earlier search to find the destination and source IP.
<br/>
<img src="https://i.imgur.com/Op6bVnu.png" width="80%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
Looking at the single event we see the destinationIP 54.148.194.58 and sourceIP 192.168.250.100. Using the formatting already known we can create the search query to identify the suspicious port 80 HTTP traffic
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
