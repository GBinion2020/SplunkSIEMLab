<h1>Investigation of a Brute Force Attack</h1>

<h2>Investigation Scenario</h2>
Alongside the vulnerability scan our security tools have alerted us to a malicious actor that is brute forcing accounts for the website. We need you to investigate, find out where the atack is coming from, if they were successful, and if they were, what did they do with their access. 




<br />


<h2>What we know!</h2>

- Administration URL ishttps://imreallynotbatman.com/joomla/administrator/index.php
  <br />
- The usernames and passwords will be submitted in an HTTP POST request <b>(http_method=POST)<b/>

<h2>Environments Used </h2>

- <b>Splunk</b>

<h2>Investigation walk-through:</h2>

<p align="center">
Using the information we know, we enter the following search query to narrow the data that we are looking for: <br/>
<img src="https://i.imgur.com/h1mgpKm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
We scroll down to 'src_ip' and click to see the following information. We see two source IP addresses, but 23.22.63.114 has more traffic so we click to add to our search query. :  <br/>
<img src="https://i.imgur.com/drom3E3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/eC1nTZY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
For fun lets find the webservers IP address, so we scroll down until we find 'dest_ip' and click the only option 192.168.250.70, which we can add to our search query but in this case we won't :): <br/>
<img src="https://i.imgur.com/AB5K6fq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
To see more closely whats going on, we can add "| table timestamp,form_data" to see the username and password attempts under the form_data section by the source IP (Malicious Actor) :  <br/>
<img src="https://i.imgur.com/4Nk4RkN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<h2>Summary </h2>
From looking at the events
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
